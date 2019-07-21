# Analysing the gender pay gap

This notebook uses gender pay gap data to demonstrate a number of techniques in R, specifically:

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

```{r fetch csv}
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
data1920 <- read.csv(url1920, stringsAsFactors = F)
str(data1920)
```

## Get an overview of our data

We can also use the `summary()` function to get a different overview: it will show us the smallest and largest values, the mean and median averages, and quartiles for any numerical columns in the data.

```{r}
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
table(data1920$SicCodes)
```
