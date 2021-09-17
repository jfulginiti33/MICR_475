HW\_5
================
John
9/17/2021

``` r
library(tidyverse)
```

    ## Warning: package 'tidyverse' was built under R version 4.1.1

    ## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --

    ## v ggplot2 3.3.5     v purrr   0.3.4
    ## v tibble  3.1.3     v dplyr   1.0.7
    ## v tidyr   1.1.3     v stringr 1.4.0
    ## v readr   2.0.0     v forcats 0.5.1

    ## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
glimpse(diamonds)
```

    ## Rows: 53,940
    ## Columns: 10
    ## $ carat   <dbl> 0.23, 0.21, 0.23, 0.29, 0.31, 0.24, 0.24, 0.26, 0.22, 0.23, 0.~
    ## $ cut     <ord> Ideal, Premium, Good, Premium, Good, Very Good, Very Good, Ver~
    ## $ color   <ord> E, E, E, I, J, J, I, H, E, H, J, J, F, J, E, E, I, J, J, J, I,~
    ## $ clarity <ord> SI2, SI1, VS1, VS2, SI2, VVS2, VVS1, SI1, VS2, VS1, SI1, VS1, ~
    ## $ depth   <dbl> 61.5, 59.8, 56.9, 62.4, 63.3, 62.8, 62.3, 61.9, 65.1, 59.4, 64~
    ## $ table   <dbl> 55, 61, 65, 58, 58, 57, 57, 55, 61, 61, 55, 56, 61, 54, 62, 58~
    ## $ price   <int> 326, 326, 327, 334, 335, 336, 336, 337, 337, 338, 339, 340, 34~
    ## $ x       <dbl> 3.95, 3.89, 4.05, 4.20, 4.34, 3.94, 3.95, 4.07, 3.87, 4.00, 4.~
    ## $ y       <dbl> 3.98, 3.84, 4.07, 4.23, 4.35, 3.96, 3.98, 4.11, 3.78, 4.05, 4.~
    ## $ z       <dbl> 2.43, 2.31, 2.31, 2.63, 2.75, 2.48, 2.47, 2.53, 2.49, 2.39, 2.~

``` r
sample_frac(diamonds, 0.01, replace = TRUE)
```

    ## # A tibble: 539 x 10
    ##    carat cut       color clarity depth table price     x     y     z
    ##    <dbl> <ord>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1  1.15 Very Good H     SI2      63.2    58  4767  6.64  6.68  4.21
    ##  2  1.03 Ideal     F     VS2      62      57  6902  6.4   6.48  3.99
    ##  3  1.03 Premium   G     SI2      59.9    60  4413  6.56  6.5   3.91
    ##  4  0.38 Ideal     F     IF       62.2    54  1212  4.64  4.69  2.9 
    ##  5  0.3  Ideal     F     SI1      62.8    56   641  4.26  4.24  2.67
    ##  6  0.9  Ideal     F     SI2      61.8    56  4593  6.18  6.21  3.83
    ##  7  0.73 Premium   F     VS1      62.7    56  2961  5.75  5.73  3.6 
    ##  8  1.02 Very Good I     SI2      63.2    60  4155  6.37  6.41  4.04
    ##  9  2.11 Very Good I     SI2      63.8    54 12937  8.13  8.19  5.21
    ## 10  0.43 Good      D     SI1      63.4    55   774  4.78  4.84  3.05
    ## # ... with 529 more rows

``` r
group_by(diamonds, clarity)
```

    ## # A tibble: 53,940 x 10
    ## # Groups:   clarity [8]
    ##    carat cut       color clarity depth table price     x     y     z
    ##    <dbl> <ord>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1  0.23 Ideal     E     SI2      61.5    55   326  3.95  3.98  2.43
    ##  2  0.21 Premium   E     SI1      59.8    61   326  3.89  3.84  2.31
    ##  3  0.23 Good      E     VS1      56.9    65   327  4.05  4.07  2.31
    ##  4  0.29 Premium   I     VS2      62.4    58   334  4.2   4.23  2.63
    ##  5  0.31 Good      J     SI2      63.3    58   335  4.34  4.35  2.75
    ##  6  0.24 Very Good J     VVS2     62.8    57   336  3.94  3.96  2.48
    ##  7  0.24 Very Good I     VVS1     62.3    57   336  3.95  3.98  2.47
    ##  8  0.26 Very Good H     SI1      61.9    55   337  4.07  4.11  2.53
    ##  9  0.22 Fair      E     VS2      65.1    61   337  3.87  3.78  2.49
    ## 10  0.23 Very Good H     VS1      59.4    61   338  4     4.05  2.39
    ## # ... with 53,930 more rows

``` r
top_100 <- diamonds %>%
  group_by(clarity) %>%
  slice_max(carat, n=100) %>%
  summarise(mean(carat), na.rm = TRUE)
```

Plot for Y vs X

``` r
ggplot(data = diamonds, aes(x = x, y = y)) + geom_point()
```

![](HW_5_files/figure-gfm/unnamed-chunk-3-1.png)<!-- --> Plot for Z vs X

``` r
ggplot(data = diamonds, aes(x = x, y = z)) + geom_point()
```

![](HW_5_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

``` r
diamonds2 <- diamonds %>%
  filter(x >= 3 & x <= 11 & y >= 3 & y <= 11 & z <= 10)
ggplot(data = diamonds2, aes(x = x, y = y)) + geom_point()
```

![](HW_5_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

``` r
ggplot(data = diamonds2, aes(x = x, y = z)) + geom_point()
```

![](HW_5_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->
