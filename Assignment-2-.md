Assignment 2
================
Catherine Kagemann
4/8/2020

``` r
library(tidyverse)
```

    ## ── Attaching packages ───────────────────────────────── tidyverse 1.3.0 ──

    ## ✓ ggplot2 3.3.0     ✓ purrr   0.3.3
    ## ✓ tibble  2.1.3     ✓ dplyr   0.8.5
    ## ✓ tidyr   1.0.2     ✓ stringr 1.4.0
    ## ✓ readr   1.3.1     ✓ forcats 0.5.0

    ## ── Conflicts ──────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(knitr)

economist_data <- read_csv("https://raw.githubusercontent.com/nt246/NTRES6940-data-science/master/datasets/EconomistData.csv")
```

    ## Warning: Missing column names filled in: 'X1' [1]

    ## Parsed with column specification:
    ## cols(
    ##   X1 = col_double(),
    ##   Country = col_character(),
    ##   HDI.Rank = col_double(),
    ##   HDI = col_double(),
    ##   CPI = col_double(),
    ##   Region = col_character()
    ## )

1.1 Show the first few rows of economist\_data.
===============================================

``` r
head(economist_data)
```

    ## # A tibble: 6 x 6
    ##      X1 Country     HDI.Rank   HDI   CPI Region           
    ##   <dbl> <chr>          <dbl> <dbl> <dbl> <chr>            
    ## 1     1 Afghanistan      172 0.398   1.5 Asia Pacific     
    ## 2     2 Albania           70 0.739   3.1 East EU Cemt Asia
    ## 3     3 Algeria           96 0.698   2.9 MENA             
    ## 4     4 Angola           148 0.486   2   SSA              
    ## 5     5 Argentina         45 0.797   3   Americas         
    ## 6     6 Armenia           86 0.716   2.6 East EU Cemt Asia

1.2 Expore the relationship between human development index (HDI) and corruption perception index (CPI) with a scatter plot as the following.
=============================================================================================================================================

``` r
ggplot(economist_data, aes(x=HDI, y=CPI)) + geom_point()
```

![](Assignment-2-_files/figure-markdown_github/unnamed-chunk-3-1.png)

1.3 Make of color of all points in the previous plot red.
=========================================================

``` r
ggplot(economist_data, aes(x=HDI, y=CPI)) + geom_point(color="red")
```

![](Assignment-2-_files/figure-markdown_github/unnamed-chunk-4-1.png)

1.4 Color the points in the previous plot according to the Region variable, and set the size of points to 2.
============================================================================================================

``` r
ggplot(economist_data, aes(x=HDI, y=CPI, size=2, color=Region)) + geom_point()
```

![](Assignment-2-_files/figure-markdown_github/unnamed-chunk-5-1.png)

1.5 Set the size of the points proportional to HDI.Rank
=======================================================

``` r
ggplot(economist_data, aes(x=HDI, y=CPI, size=HDI.Rank, color=Region)) + geom_point()
```

![](Assignment-2-_files/figure-markdown_github/unnamed-chunk-6-1.png)

1.6 Fit a smoothing line to all the data points in the scatter plot from Excercise 1.4
======================================================================================

``` r
ggplot(economist_data, aes(x=HDI, y=CPI, size=2, color=Region)) + geom_point() + geom_smooth(method=lm)
```

    ## `geom_smooth()` using formula 'y ~ x'

![](Assignment-2-_files/figure-markdown_github/unnamed-chunk-7-1.png)

1.7 Fit a separate straight line for each region instead, and turn off the confidence interval.
===============================================================================================

``` r
ggplot(economist_data, aes(x=HDI, y=CPI, size=2, color=Region)) + geom_point() + geom_smooth(method=lm, se=FALSE)
```

    ## `geom_smooth()` using formula 'y ~ x'

![](Assignment-2-_files/figure-markdown_github/unnamed-chunk-8-1.png)

1.8 Building on top of the previous plot, show each Region in a different facet.
================================================================================

``` r
ggplot(economist_data, aes(x=HDI, y=CPI, size=2, color=Region)) + geom_point() + geom_smooth(method=lm, se=FALSE) +
facet_wrap( ~ Region, nrow=2)
```

    ## `geom_smooth()` using formula 'y ~ x'

![](Assignment-2-_files/figure-markdown_github/unnamed-chunk-9-1.png)

1.9 Show the distribution of HDI in each region using density plot. Set the transparency to 0.5
===============================================================================================

``` r
ggplot(economist_data, aes(x=HDI, fill=Region)) + geom_density(alpha=0.5)
```

![](Assignment-2-_files/figure-markdown_github/unnamed-chunk-10-1.png)

1.10 Show the distribution of HDI in each region using histogram and facetting.
===============================================================================

``` r
ggplot(economist_data, aes(x=HDI, color=Region, fill=Region)) +
  geom_histogram() +
  facet_wrap( ~ Region, nrow=2)
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](Assignment-2-_files/figure-markdown_github/unnamed-chunk-11-1.png)

1.11 Show the distribution of HDI in each region using a box plot. Set the transparency of these boxes to 0.5 and do not show outlier points with the box plot. Instead, show all data points for each country in the same plot. (Hint: geom\_jitter() or position\_jitter() might be useful.)
==============================================================================================================================================================================================================================================================================================

``` r
ggplot(economist_data, aes(x=Region, y=HDI, fill=Region, color=Region)) +
  geom_boxplot(alpha=0.5) +
  geom_jitter()
```

![](Assignment-2-_files/figure-markdown_github/unnamed-chunk-12-1.png)

1.12 Show the count of countries in each region using a bar plot.
=================================================================

``` r
p<-ggplot(data=economist_data, aes(x=Region, y=as.factor(Region))) +
  geom_bar(stat="identity") 
p +labs(x ="Region", y = "Count")
```

![](Assignment-2-_files/figure-markdown_github/unnamed-chunk-13-1.png)

1.13 You have now created a variety of different plots of the same dataset. Which of your plots do you think are the most informative? Describe briefly the major trends that you see in the data.
==================================================================================================================================================================================================

I believe that the scatterplots were the most informative with a large amount of data. Histograms and boxplots can get very cluttered (as seen in the homework) if there's too much data that is being presented
================================================================================================================================================================================================================

Exercise 2
==========

This excercise uses the dataset economics from the ggplot2 package. It was produced from US economic time series data available from <http://research.stlouisfed.org/fred2>. It descibes the number of unemployed persons (unemploy), among other variables, in the US from 1967 to 2015.
=========================================================================================================================================================================================================================================================================================

``` r
head(economics) %>% kable()
```

| date       |    pce|     pop|  psavert|  uempmed|  unemploy|
|:-----------|------:|-------:|--------:|--------:|---------:|
| 1967-07-01 |  506.7|  198712|     12.6|      4.5|      2944|
| 1967-08-01 |  509.8|  198911|     12.6|      4.7|      2945|
| 1967-09-01 |  515.6|  199113|     11.9|      4.6|      2958|
| 1967-10-01 |  512.2|  199311|     12.9|      4.9|      3143|
| 1967-11-01 |  517.4|  199498|     12.8|      4.7|      3066|
| 1967-12-01 |  525.1|  199657|     11.8|      4.8|      3018|

2.1 Plot the trend in number of unemployed persons (unemploy) though time using the economics dataset shown above. And for this question only, hide your code and only show the plot.
=====================================================================================================================================================================================

![](Assignment-2-_files/figure-markdown_github/unnamed-chunk-15-1.png)

2.2 Edit the plot title and axis labels of the previous plot appropriately. Make y axis start from 0. Change the background theme to what is shown below. (Hint: search for help online if needed)
==================================================================================================================================================================================================

``` r
p <- ggplot(data=economics, aes(x=date, y=unemploy)) +
  geom_line() +
  ggtitle("Trend of Unemployed Persons")
p +labs(x ="Year", y = "# of Unemployed Persons (in thousands)") + theme_minimal()
```

![](Assignment-2-_files/figure-markdown_github/unnamed-chunk-16-1.png)
