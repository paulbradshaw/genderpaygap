# Analysing the gender pay gap

[This notebook](https://github.com/paulbradshaw/genderpaygap/blob/master/r/downloadgenderpaygap.Rmd) uses gender pay gap data to demonstrate a number of techniques in R, specifically:

* Importing live data directly from the web
* Understanding different object types in R (character/strings, factors, numeric, data frames)
* Combining data

Gender pay gap is published in a series of annual (financial year) files at https://gender-pay-gap.service.gov.uk/viewing/download

As well as the end-of-year data, that page also includes the latest data for the current year. 

This is a good candidate for using R because the **data will change regularly**. 

In this context, it makes sense to write some code that 'records' all the steps we want to perform, so we can just run that on the latest dataset - performing the downloading and analysis in a few minutes, rather than having to spend hours going through all the steps again.

## Fetching data from a URL

We can use R to fetch the latest version directly from the data URL.

This has two steps: 

* Firstly, we store the URL as a character variable; 
* Then, we use `read.csv()` to read the CSV file from that URL, and store it in a data frame variable.

*Note: variable names in R never begin with numbers, so we put the year at the end of the variable name to avoid this*

```{r}
#Store the URL
url1920 <- "https://gender-pay-gap.service.gov.uk/viewing/download-data/2019"
#Read a CSV from that URL, and store in a variable called 'csv1920'
data1920 <- read.csv(url1920)
```

You should now see two variables in the *Global Environment* area in the upper right corner of RStudio. We can use `typeof()` or `str()` to find out more about them:

```{r}
#Find out what type of variable url1920 is
typeof(url1920)
```

The `typeof()` function is best for small objects. The `str()` function gives more information and is best for data frames where you want to see a bit more:

```{r}
#Find out about the columns in csv1920
str(data1920)
```

This shows us that the data frame columns are either numeric (`num`) or *Factor*. 

We don't actually want those columns to be factors (factors are like categories and have a particular use in R that we don't have a need for right now) - we want them to be character columns.

To correct this, we import again, this time *specifying* that we don't want to treat strings as factors:

```{r}
#Read in the CSV again, this time adding an instruction not to default to factors
data1920 <- read.csv(url1920, stringsAsFactors = FALSE)
#Check the object has numeric and character fields now
str(data1920)
```

## Get an overview of our data

We can also use the `summary()` function to get a different overview: it will show us the smallest and largest values, the mean and median averages, and quartiles for any numerical columns in the data.

```{r}
#Get summary statistics for the specified data frame
summary(data1920)
```

This is one advantage of using R over Excel: to get this overview in Excel you would have to perform about a dozen different sorts, and type a whole bunch of formulae.

A quick look at those summary statistics highlights some potential avenues for inquiry. For example, that the *DiffMeanBonusPercent* column has one company with a -512.6% gender pay gap. 

## Getting a summary of category columns

For category columns the `table()` function is useful to get an overview.

In this case you need to specify the table *and* the column within that. This is done by adding a `$` sign after the table name, and then typing the column name.

RStudio will suggest columns as soon as you type the `$`, so you don't need to remember the name, just select it from the list.

```{r}
#Show a table of values in the EmployerSize column
table(data1920$EmployerSize)
```

The `table()` function isn't so helpful for columns where there are lots of different values:

```{r}
#Show a table of values in the SicCodes column
table(data1920$SicCodes)
```

## Combining the data

Another thing that R is particularly useful for is combining datasets. In this case, we've downloaded the latest data but there are other years as well. If we want to make a year-by-year comparison we'll need to combine the data.

First we need to get the other datasets into R, which we do with `read.csv()` again, remembering to set `stringsAsFactors` to `FALSE`:

```{r}
#Store the URLs of the 2 datasets
url1819 <- "https://gender-pay-gap.service.gov.uk/viewing/download-data/2018"
url1718 <- "https://gender-pay-gap.service.gov.uk/viewing/download-data/2017"
#Read the CSV from each URL into a new variable
data1819 <- read.csv(url1819, stringsAsFactors = FALSE)
data1718 <- read.csv(url1718, stringsAsFactors = FALSE)
```

You should now see the new variables in your RStudio environment (upper right corner). 

This is a good opportunity to point out something: in data frames rows are called 'observations' and columns are called 'variables'. The data for 2017-18, then, has 10,559 rows (observations) and 25 columns (variables).

If you want to combine data frames from different years (or other timeframes) in R, you need to make sure they have the same number of columns (variables) - otherwise you will get an error.

In this case you should see that all three data frames have 25 columns, so that's OK. 

Now, to merge data we can use the `rbind()` function (think of it as 'binding rows' together), like so:

```{r}
#Combine the data frames for all 3 years
dataallyears <- rbind(data1718, data1819, data1920)
```

The `rbind(data1718, data1819, data1920)` part specifies which data frames we want to combine with `rbind()` (each one separated by a comma), and we *assign* the results of that to a new variable: `dataallyears`.

You should now see that new variable in the environment, and notice that it has a number of rows (observations) equal to the other 3 added up.

You can now get a summary of this new combined data:

```{r}
#summarise the new data frame
summary(dataallyears)
```


By the way, there is *another* way of combining data, when you want to bring in *extra* data on a particular *aspect* of your existing data. 

For example, if you wanted to combine one dataset on *crimes* by police force, with a second dataset giving the *populations* of each police force. We will cover this in another notebook. 

## What we've learned so far

We will use another notebook to move onto analysing the data. Before then, let's summarise some of the key points:

* Create a variable to hold information in R using the `<-` operator
* You can import a CSV directly from a URL using the `read.csv()` function, and assign it to a variable. This will be a data frame.
* Remember to add `, stringsAsFactors = FALSE` inside that function, otherwise character fields will be converted to factors (which we don't want)
* Use `str()` or `typeof()` to find out what types of data are inside a data frame, or what type of variable something is
* Use `summary()` to get summary statistics for a data frame - especially useful for numeric fields (columns)
* Use `table()` to get a summary of a category column - you will need to name the column by adding `$` to the data frame name, and then the name of the column

## Next steps

Now, pick a story you'd like to pursue in this data. 

Some things to consider:

* Numerical columns allow us to look for extremes (outliers) and patterns (averages)
* Categories allow us to look at patterns or differences between categories
* Addresses allow us to find geographical patterns, or focus on a particular area
* Dates allow us to find patterns in time, or focus on a particular period
* Codes allow us to pull in information from elsewhere, such as other categories which we can filter on
* Names can be indicators of other things. Have a think what...
* Text can be split, counted, measured, searched and matched.

And of course, remember outside of the data there are always considerations about topicality and audience: what about this data might be *relevant to your audience*, or *more relevant right now* (in terms of the news agenda)?

Now look at the other notebooks in this repo for examples of some of the techniques you can use to get the story you've thought of...
