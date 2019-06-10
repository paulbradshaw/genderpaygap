# Doing journalism with gender pay gap data

This repo contains tips for finding stories in data on gender pay.

First, [download the gender pay gap data from the Gender Pay Gap Service here](https://gender-pay-gap.service.gov.uk/viewing/download). You should see partial data for the current year (with a smaller number of employers than the others), and full data for past years.

Download the most recent *full* year of data.

Then open the data in Excel or Google Sheets.

## First steps: look at the column headings

The column headings (often called the 'fields' in your data) are the first place to look in any dataset. Most of your ideas will come from those.

The gender pay gap data has 25 columns. Those include:

* Company details: names and addresses, "person responsible" (column T) and a unique identifier: the company number
* Category details: SIC codes, employer size (column U),
* Quantitative information: means and medians for the wage gap, and percentages of women and men in different categories (bonuses, quartiles)
* Links (column S)
* Compliance information (when the data was due, when it was submitted, and if it was submitted after the deadline)

Not all of this will make sense immediately, so you may need to refer elsewhere or do some searching to understand certain terms or abbreviations.

A good start is to look at an individual company report and compare that to the rows in the spreadsheet (especially for that company).

If you search the Gender Pay Gap website for the Telegraph Media Group, for example, you will [find their page](https://gender-pay-gap.service.gov.uk/employer/JqugxufS) and then [view the report for 2018](https://gender-pay-gap.service.gov.uk/Employer/JqugxufS/2018).

That report converts those numbers into a narrative, inserting them into sentences such as this:

> "In this organisation, women earn 77p for every £1 that men earn when comparing median hourly wages. Their median hourly wage is 22.7% lower than men’s."

And generating charts that represent those numbers.

Here are some things you might find out through searching:

* An SIC code [identifies the sector that an organisation operates in](https://www.gov.uk/government/publications/standard-industrial-classification-of-economic-activities-sic). A company can have more than one SIC code if it operates in more than one sector.
* A mean average is calculated by taking all numbers (in this case pay) and dividing by how many numbers there are. A median average is calculated by putting all the numbers into order and then identifying the middlemost value (in other words, the point at which half of numbers are higher, and half lower)

## Sorting to find the story

If you want to find the companies that have the biggest gender pay gaps, find the column that contains that information. That would be column E ('DiffMeanHourlyPercent') or F ('DiffMedianHourlyPercent') depending which measure you want to use.

Make sure you are on one of the cells in that column, and then select the sorting options from the *Data* menu: in Excel these are small buttons marked A-Z (smallest to largest) and Z-A (largest to smallest). Click on one of these and *all* of the data will be sorted by that column.

## Filtering to find the story

If you want to narrow the focus to some dimension of your data - for example, companies in a particular category, or with values in a certain range, or with a certain keyword - then you can use filters to do this.

You can add a filter to all the columns by clicking on the Filter button in the *Data* menu, or selecting *Data > Autofilter*.

This will add a dropdown menu to the top of each column.

Let's say we want to look at universities in the data. Which column tells us whether an organisation is a university or not? Well, there isn't a column that *explicitly* has this as a category, but the name of a university normally contains the word 'University', so we can filter on that.

Click on the dropdown filter at the top of the first column, 'EmployerName'.

You should see a list of all the values, but also above those, an empty search box. Start typing 'university'.

As soon as you type that word you should see the data in the background change so that it only shows those entries that match your search. Click away from the filter and it should still be applied.

You can now copy those filtered results into a new sheet.

To do that, select all with CTRL+A (CMD+A on a Mac), and copy by pressing CTRL+C (or CMD+C).

Create a new sheet in Excel, and paste what you've just copied by pressing CTRL+V (or CMD+V).

You can check that this *only* contains the filtered results by moving to the first column and pressing CTRL and the down arrow. This should take you to the bottom of the table, row 120 or so. Is that the number of universities you expected? Are they all universities? Or has the filter caught other organisations too? Filters are not always completely effective, so make sure you include some checks, and think of other ways to achieve similar results.

Once you do have an effective filter, you can sort it as above, or perform other calculations and analysis.

## Using pivot tables in Google Sheets

[I've written a guide to using pivot tables with the gender pay gap data here](https://github.com/paulbradshaw/MED7373-Data-Journalism/blob/master/1basics/gsheetspivot.md).
