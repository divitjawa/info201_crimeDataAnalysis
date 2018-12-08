# R for Police Data Analysis
### Interesting Tools for Crime Data Analysis
Divit Jawa
12-07-2018

## Setup

#### Required Software(s)
+ R/RStudio

#### Required Libraries:
`
if(!require(dplyr)) install.packages("dplyr")
`
`
if(!require(plotly)) install.packages("plotly")
`
`
if(!require(ggplot2)) install.packages("ggplot2")
`

## Background & Objectives
I am working as a Cadet with the [University of Washington Police Department](http://police.uw.edu/), and will soon
be working in their research department. I have been reading some academic articles and blogs on Crime and Police data
analysis, and I realized that the most important task is to find trends in data. These
trends can help find a pattern that may prevent a crime from happening.

# The data

The dataset I worked with was the [Seattle 911 Calls dataset](Info201_Final_Repository/data/Seattle_PD_data.bz2). It had information about call id, lat, long, call type, call subtype, resolution type(ticket, arrest etc.), date, and several other columns for the years 2010 - 2017. The dataset contains about 1.4 million rows, so it might be a little slow to work with.

Here is a screenshot of the datset.
![Screenshot of Seattle Police 911 dataset](https://imgur.com/ozk6EbZ.png)

# Data Processing
## Reading the data
This dataset is **huge**, so it might take about 30 seconds for the dataset to load. Be patient.
`
police_data  <-
  data.table::fread("./data/Seattle_PD_data.bz2, header = FALSE, sep = ",")
`
If you feel the dataset is too big, you may want to trim it down by using this command. Beware, you need to follow this command **AFTER** you have done the first one, stated above.
`
police_data <- sample_n(police_data, 25000)
`
This will trim the dataset to 25,000 rows from 1.4 million. A much more usable dataset.

# Data Analysis

One thing that you would definitely want to do is summarize the data in the dataset to get basic information like how much crime of each category was committed in each year, and similar questions.

```
data_2013 <-
  group_by(police_data, V6) %>% dplyr::filter(grepl('2013', V8)) %>% summarise(freq = n()) 
ggplot(data = data_2013, aes(
  x = reorder(V6, freq),
  y = freq,
  width = .5,
  fill = V6
)) +
  geom_bar(stat = 'identity',
           position = 'dodge',
           width = 400) +
  coord_flip() + labs(title = "Crime rates in 2013", x = "Crime Groups", y = "Frequency")

```

This creates a crime report for the data of 2012 & 2013. Here is what they look like:

![Crime report of Seattle for 2012](https://imgur.com/QZzK3PN.png)
![Crime report of Seattle for 2013](https://imgur.com/0KljJVp.png)

This is the plot created for the entire dataset consisting of the 1.4 million rows. I used the full dataset, and created summary plots for 2012 and 2013 and found something shocking!

## The Revelation:

When I analyzed the entire dataset for the years 2012 and 2013, I found that there was a **HUGE** difference in the crime rates for the 2 years. An interesting event that happened between 2012 and 2013 was that the [i-502](https://en.wikipedia.org/wiki/Washington_Initiative_502) intitiative was passed, and Marijuana was legalized.

Look at how the scales have changed between 2012 and 2013! The highest crime rate for any category in 2012 is 40000+, while it is just 6000+ for 2013. I don't know if this is related to the fact that Marijuana was legalized in November 2012, but I certainly found this interesting.

# Conclusion

This is a basic approach to crime data analysis. You look for significant events between the years that your dataset consists of and try looking for patterns. You can also begin by simply analyzing the trends by creating a summary plot of your dataset.

Hope you found this helpful. Thank you!
