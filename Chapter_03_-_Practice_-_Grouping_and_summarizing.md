---
title: "DataCamp - Introduction to the Tidyverse"
author: "[Luka Ignjatovi&#263;](https://github.com/LukaIgnjatovic)"
output:
  html_document:
    highlight: tango
    theme: united
    toc: yes
    toc_depth: 4
    keep_md: true
  md_document:
    toc: true
    toc_depth: 4
---

## Grouping and summarizing

So far you've been answering questions about individual country-year pairs, but we may be interested in aggregations of the data, such as the average life expectancy of all countries within each year. Here you'll learn to use the group by and summarize verbs, which collapse large datasets into manageable summaries.

<div align="middle">

> **Document:** ["Slides - Grouping and summarizing"](./Slides/Chapter 03 - Grouping and summarizing.pdf)

</div>

<div align="middle">

<video width="80%" controls src="./Videos/Chapter 03 - Lecture 01 - The summarize verb.mp4" type="video/mp4"/>

</div>

### Summarizing the median life expectancy

You've seen how to find the mean life expectancy and the total population across a set of observations, but `mean()` and `sum()` are only two of the functions R provides for summarizing a collection of numbers. Here, you'll learn to use the `median()` function in combination with `summarize()`.

By the way, `dplyr` displays some messages when it's loaded that we've been hiding so far. They'll show up in red and start with:

    Attaching package: 'dplyr'

    The following objects are masked from 'package:stats':

This will occur in future exercises each time you load `dplyr`: it's mentioning some built-in functions that are overwritten by `dplyr`. You won't need to worry about this message within this course.

#### Instructions
*Use the* `median()` *function within a* `summarize()` *to find the median life expectancy. Save it into a column called* `medianLifeExp`*.*

```r
library(gapminder)
library(dplyr)
```

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
# Summarize to find the median life expectancy
gapminder %>%
  summarize(medianLifeExp = median(lifeExp))
```

```
## # A tibble: 1 x 1
##   medianLifeExp
##           <dbl>
## 1          60.7
```

**That's right! Note that this is the median across all countries and all years in the dataset.**

### Summarizing the median life expectancy in 1957

Rather than summarizing the entire dataset, you may want to find the median life expectancy for only one particular year. In this case, you'll find the median in the year 1957.

#### Instructions
*Filter for the year 1957, then use the* `median()` *function within a* `summarize()` *to calculate the median life expectancy into a column called* `medianLifeExp`*.*

```r
library(gapminder)
library(dplyr)

# Filter for 1957 then summarize the median life expectancy
gapminder %>%
  filter(year == 1957) %>%
  summarize(medianLifeExp = median(lifeExp))
```

```
## # A tibble: 1 x 1
##   medianLifeExp
##           <dbl>
## 1          48.4
```

**Great! Just like in Chapter 1, this chapter will often involve performing multiple dplyr steps in a row.**

### Summarizing multiple variables in 1957

The `summarize()` verb allows you to summarize multiple variables at once. In this case, you'll use the `median()` function to find the median life expectancy and the `max()` function to find the maximum GDP per capita.

#### Instructions
*Find both the median life expectancy (*`lifeExp`*) and the maximum GDP per capita (*`gdpPercap`*) in the year 1957, calling them* `medianLifeExp` *and* `maxGdpPercap` *respectively. You can use the* `max()` *function to find the maximum.*

```r
library(gapminder)
library(dplyr)

# Filter for 1957 then summarize the median life expectancy and the maximum GDP per capita
gapminder %>%
  filter(year == 1957) %>%
  summarize(medianLifeExp = median(lifeExp),
            maxGdpPercap = max(gdpPercap))
```

```
## # A tibble: 1 x 2
##   medianLifeExp maxGdpPercap
##           <dbl>        <dbl>
## 1          48.4      113523.
```

**That's right! Think about what other kinds of information about countries you might want to summarize within one year.**

<div align="middle">

<video width="80%" controls src="./Videos/Chapter 03 - Lecture 02 - The group_by verb.mp4" type="video/mp4"/>

</div>

### Summarizing by year

In a previous exercise, you found the median life expectancy and the maximum GDP per capita in the year 1957. Now, you'll perform those two summaries within each year in the dataset, using the `group_by` verb.

#### Instructions
*Find the median life expectancy (*`lifeExp`*) and maximum GDP per capita (*`gdpPercap`*) **within each year**, saving them into* `medianLifeExp` *and* `maxGdpPercap`*, respectively.*

```r
library(gapminder)
library(dplyr)

# Find median life expectancy and maximum GDP per capita in each year
gapminder %>%
  group_by(year) %>%
  summarize(medianLifeExp = median(lifeExp),
            maxGdpPercap = max(gdpPercap))
```

```
## # A tibble: 12 x 3
##     year medianLifeExp maxGdpPercap
##    <int>         <dbl>        <dbl>
##  1  1952          45.1      108382.
##  2  1957          48.4      113523.
##  3  1962          50.9       95458.
##  4  1967          53.8       80895.
##  5  1972          56.5      109348.
##  6  1977          59.7       59265.
##  7  1982          62.4       33693.
##  8  1987          65.8       31541.
##  9  1992          67.7       34933.
## 10  1997          69.4       41283.
## 11  2002          70.8       44684.
## 12  2007          71.9       49357.
```

**Great! Interesting: notice that median life expectancy across countries is generally going up over time, but maximum GDP per capita is not.**

### Summarizing by continent

You can group by any variable in your dataset to create a summary. Rather than comparing across time, you might be interested in comparing among continents. You'll want to do that within one year of the dataset: let's use 1957.

#### Instructions
*Filter the* `gapminder` *data for the year 1957. Then find the median life expectancy (*`lifeExp`*) and maximum GDP per capita (*`gdpPercap`*) within each continent, saving them into* `medianLifeExp` *and* `maxGdpPercap`*, respectively.*

```r
library(gapminder)
library(dplyr)

# Find median life expectancy and maximum GDP per capita in each continent in 1957
gapminder %>%
  filter(year == 1957) %>%
  group_by(continent) %>%
  summarize(medianLifeExp = median(lifeExp),
            maxGdpPercap = max(gdpPercap))
```

```
## # A tibble: 5 x 3
##   continent medianLifeExp maxGdpPercap
##   <fct>             <dbl>        <dbl>
## 1 Africa             40.6        5487.
## 2 Americas           56.1       14847.
## 3 Asia               48.3      113523.
## 4 Europe             67.6       17909.
## 5 Oceania            70.3       12247.
```

**Great work! Which continent had the highest median life expectancy in 1957?**

### Summarizing by continent and year

Instead of grouping just by year, or just by continent, you'll now group by both continent and year to summarize within each.

#### Instructions
*Find the median life expectancy (*`lifeExp`*) and maximum GDP per capita (*`gdpPercap`*) within each combination of continent and year, saving them into* `medianLifeExp` *and* `maxGdpPercap`*, respectively.*

```r
library(gapminder)
library(dplyr)

# Find median life expectancy and maximum GDP per capita in each year/continent combination
gapminder %>%
  group_by(continent, year) %>%
  summarize(medianLifeExp = median(lifeExp),
            maxGdpPercap = max(gdpPercap))
```

```
## # A tibble: 60 x 4
## # Groups:   continent [?]
##    continent  year medianLifeExp maxGdpPercap
##    <fct>     <int>         <dbl>        <dbl>
##  1 Africa     1952          38.8        4725.
##  2 Africa     1957          40.6        5487.
##  3 Africa     1962          42.6        6757.
##  4 Africa     1967          44.7       18773.
##  5 Africa     1972          47.0       21011.
##  6 Africa     1977          49.3       21951.
##  7 Africa     1982          50.8       17364.
##  8 Africa     1987          51.6       11864.
##  9 Africa     1992          52.4       13522.
## 10 Africa     1997          52.8       14723.
## # ... with 50 more rows
```

**Excellent! In the next chapter, you'll learn to turn this data into an informative graph.**

<div align="middle">

<video width="80%" controls src="./Videos/Chapter 03 - Lecture 03 - Visualizing summarized data.mp4" type="video/mp4"/>

</div>

### Visualizing median life expectancy over time

In the last chapter, you summarized the gapminder data to calculate the median life expectancy within each year. This code is provided for you, and is saved (with `<-`) as the `by_year` dataset.

Now you can use the `ggplot2` package to turn this into a visualization of changing life expectancy over time.

#### Instructions
*Use the* `by_year` *dataset to create a scatter plot showing the change of median life expectancy over time, with* `year` *on the x-axis and medianLifeExp on the y-axis. Be sure to add* `expand_limits(y = 0)` *to make sure the plot's y-axis includes zero.*

```r
library(gapminder)
library(dplyr)
library(ggplot2)

by_year <- gapminder %>%
  group_by(year) %>%
  summarize(medianLifeExp = median(lifeExp),
            maxGdpPercap = max(gdpPercap))

# Create a scatter plot showing the change in medianLifeExp over time
ggplot(by_year, aes(x = year, y = medianLifeExp)) + 
  geom_point() + 
  expand_limits(y = 0)
```

![](Chapter_03_-_Practice_-_Grouping_and_summarizing_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

**Great! It looks like median life expectancy across countries is increasing over time.**

### Visualizing median GDP per capita per continent over time

In the last exercise you were able to see how the median life expectancy of countries changed over time. Now you'll examine the median GDP per capita instead, and see how the trend differs among continents.

#### Instructions
* *Summarize the gapminder dataset by continent and year, finding the median GDP per capita (*`gdpPercap`*) within each and putting it into a column called* `medianGdpPercap`*. Use the assignment operator* `<-` *to save this summarized data as* `by_year_continent`*.*
* *Create a scatter plot showing the change in* `medianGdpPercap` *by continent over time. Use color to distinguish between continents, and be sure to add* `expand_limits(y = 0)` *so that the y-axis starts at zero.*

```r
library(gapminder)
library(dplyr)
library(ggplot2)

# Summarize medianGdpPercap within each continent within each year: by_year_continent
by_year_continent <- gapminder %>%
  group_by(continent, year) %>%
  summarize(medianGdpPercap = median(gdpPercap))

# Plot the change in medianGdpPercap in each continent over time
ggplot(by_year_continent, aes(x = year, y = medianGdpPercap, color = continent)) + 
  geom_point() + 
  expand_limits(y = 0)
```

![](Chapter_03_-_Practice_-_Grouping_and_summarizing_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

**Great! You might be wondering how you can connect these points with lines. You'll learn that in Chapter 4!**

### Comparing median life expectancy and median GDP per continent in 2007

In these exercises you've generally created plots that show change over time. But as another way of exploring your data visually, you can also use ggplot2 to plot summarized data to compare continents within a single year.

#### Instructions
* *Filter the gapminder dataset for the year 2007, then summarize the median GDP per capita and the median life expectancy **within each continent**, into columns called* `medianLifeExp` *and* `medianGdpPercap`*. Save this as* `by_continent_2007`*.*
* *Use the* `by_continent_2007` *data to create a scatterplot comparing these summary statistics for continents in 2007, putting the median GDP per capita on the x-axis to the median life expectancy on the y-axis. Color the scatter plot by* `continent`*. You don't need to add* `expand_limits(y = 0)` *for this plot.*

```r
library(gapminder)
library(dplyr)
library(ggplot2)

# Summarize the median GDP and median life expectancy per continent in 2007
by_continent_2007 <- gapminder %>%
  filter(year == 2007) %>%
  group_by(continent) %>%
  summarize(medianLifeExp = median(lifeExp),
            medianGdpPercap = median(gdpPercap))

# Use a scatter plot to compare the median GDP and median life expectancy
ggplot(by_continent_2007, aes(x = medianGdpPercap, y = medianLifeExp, color = continent)) + 
  geom_point()
```

![](Chapter_03_-_Practice_-_Grouping_and_summarizing_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

**Great work! Scatter plots are a very flexible tool for examining relationships.**
**You have finished the chapter "Grouping and summarizing"!**
