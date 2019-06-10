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



## Using pivot tables in Google Sheets

[I've written a guide to using pivot tables with the gender pay gap data here](https://github.com/paulbradshaw/MED7373-Data-Journalism/blob/master/1basics/gsheetspivot.md).
