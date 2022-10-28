# Analysing the gender pay gap: time and dates

Let's say you wanted to work out how close to the deadline companies tended to wait before submitting - or filter to the ones who were earliest or latest. To do that you need to be able to work with times and dates. 

This notebook details some processes for dealing with times and dates in R. It's worth having the [dates and times section of Hadley Wickham's book R for Data Science](https://r4ds.had.co.nz/dates-and-times.html) open alongside this, and reading to familiarise yourself further with these techniques.

## Get the data first

In a separate notebook we have downloaded gender pay gap files from https://gender-pay-gap.service.gov.uk/viewing/download. If you need to recreate those files, run the code in the notebook 'downloadgenderpaygap', or just run the line below to load the .rds file that was saved at the end:

```{r load rds}
#Import data exported in previous notebook, store in variable 'dataallyears'
dataallyears <- readRDS("dataallyears.rds")
```

## Installing a package to deal with dates: `lubridate`

The `lubridate` package is designed to deal with dates, so we should install that. 

```{r Install lubridate}
#Install lubridate
install.packages("lubridate")
```

And then activate it:

```{r activate lubridate}
#Activate lubridate package
library(lubridate)
```

## Converting text to dates, times and date-times

Let's remind ourselves which columns in the data have dates in them:

```{r overview of data frame}
#Use str to show column names and types
str(dataallyears)
```

It's the two with 'date' in them, not surprisingly - these are character columns that look like this:

`"27/03/2018 11:42:49"`

The `lubridate` package has a number of functions that take a string like this and convert accordingly. Examples include:

* For dates: `ymd`, `mdy`, `dmy`, `ydm` and other variations of those three letters, which expect a year, day and month in the order specified. The great thing about `lubridate` is that it doesn't matter if those dates use slashes, dashes, spaces or even text (e.g. 21st Jan) - it will still **parse** (interpret and convert) them as a date.
* For times: `hms` and `ms` for hours:minutes:seconds or minutes:seconds, as well as `hours`, `minutes` and `seconds` for those individually
* For combinations of dates and times (called **date-times**) you can use a combination of those separated by an underscore, like so: `ymd_hms`, `mdy_hms`, etc.

To work out which one we need, we need to look at our date string and identify the order the dates and times are in. So we have:

* Day, followed by
* Month, then
* Year
* Hour
* Minutes
* Seconds

So we need `dmy_hms()`

Let's test this by storing one date string in a variable and see what we can do with it:

```{r create testdate}
#Store date
testdate <- "27/03/2018 11:42:49"
#Use 3 different functions to show what time of object it is:
str(testdate)
typeof(testdate)
class(testdate)
#Use lubridate's dmy_hms() function to parse into a new object
dmyhmstest <- lubridate::dmy_hms(testdate)
#Use 3 different functions to show what time of object it is:
str(dmyhmstest)
typeof(dmyhmstest)
class(dmyhmstest)
#Print the two variables
print(testdate)
print(dmyhmstest)
```

By printing the date before and after it was converted, you can see the difference. Although both still *look* like strings, the converted version has `UTC` to indicate a timezone (which we didn't specify, but that's fine), and the date has been ordered slightly differently. 

If you check with `str` or `typeof` or `class` you will also see a difference: a quick google on `POSIXt` will tell you that [it is a timedate type (class) of object](https://astrostatistics.psu.edu/su07/R/html/base/html/DateTimeClasses.html).

*Note: I have added `lubridate::` before `dmy_hms()` just to indicate where the function is from, but you don't need to do that - `dmy_hms()` will still work on its own as long as you've activated the package first. Another reason for adding `lubridate::` is that it allows you to use functions from that package without activating the package.*

## Extracting dates from date-times

We can also extract from this new object, like so:

```{r get date only}
#Convert date-time to date only
lubridate::as_date(dmyhmstest)
```

## Applying this to a whole column

Now that we've tested this, let's convert the whole column accordingly:

```{r create new datetime column}
#Convert one column to a datetime and put in a new column
dataallyears$submitteddatetime <- lubridate::dmy_hms(dataallyears$DateSubmitted)
```

We could have simply replaced the column like so: 

`dataallyears$DueDate <- lubridate::dmy_hms(dataallyears$DueDate)`

But it's a good idea to keep the original data just in case you need to check any problems.

Now if we try to use the `min()` and `max()` functions, or `summary()`, we should get the correct results:

```{r check min and max}
#Show minimum, maximum and statistical summary of new column
min(dataallyears$submitteddatetime)
max(dataallyears$submitteddatetime)
summary(dataallyears$submitteddatetime)
```

Let's repeat this again to just get the dates, as the times aren't too important

```{r create date column}
#Extract date from date-time column, put in new column
dataallyears$submitteddate <- lubridate::as_date(dataallyears$submitteddatetime)
#Show statistical summary of new column
summary(dataallyears$submitteddate)
```

We can repeat the process for the DueDate column...

```{r repeat for duedate}
dataallyears$duedatetime <- lubridate::dmy_hms(dataallyears$DueDate)
summary(dataallyears$duedatetime)
table(dataallyears$duedatetime)
```

## Calculating how early or late each company submitted

Now that we have converted both dates to a date-time, we can calculate a difference between the two (how early or late did each company submit). We couldn't have done this before with character strings.

```{r calculate difference between dates}
#Subtract submitted date from due date to get difference
dataallyears$datediff <- dataallyears$duedatetime - dataallyears$submitteddatetime
#Show min and max
min(dataallyears$datediff)
max(dataallyears$datediff)
```

Note that the min and max functions generate a 'Time difference of' rather than just a number. This is because this column is a **timediff** object, created when you make a calculation with `lubridate` datetimes.

Note that the timediff object cannot be summarised in the same way as normal numbers can:

```{r summary timediff}
summary(dataallyears$datediff)
```

...but it can be averaged:

```{r average timediff}
mean(dataallyears$datediff)
median(dataallyears$datediff)
```

Because this is a timediff object you can use other lubridate functions to work with it. For example the `as.duration()` function will show the timediff as both seconds and another unit of measurement (in this case, it's chosen days):

```{r as.duration}
#Show the median of the datediff column - as a 'duration'
as.duration(median(dataallyears$datediff))
```

We could also simply multiply the column to get different measurements. For example, let's create a new column that shows minutes, not seconds:

```{r create other duration columns}
#Divide seconds by 60 to get minutes
dataallyears$mindiff <- dataallyears$datediff / 60
#Divide minutes by 60 to get hours
dataallyears$hrdiff <- dataallyears$mindiff / 60
#Divide hours by 24 to get days
dataallyears$daydiff <- dataallyears$hrdiff / 24
#And by 7 to get weeks
dataallyears$wkdiff <- dataallyears$daydiff / 7

```

