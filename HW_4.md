functions
================

Assigning “2” to “a” and “3” to “b”, printing the sum of “a + b”

``` r
a <- 2
b <- 3
print (a+b)
```

    ## [1] 5

``` r
sum(2+3)
```

    ## [1] 5

Creating graph displaying arrival delay vs departure delay

``` r
library(tidyverse)
library(nycflights13)
data("flights")
AA_flights = filter(flights, carrier == "AA")
ggplot(data = flights) + geom_point(mapping = aes(x=dep_delay, y=arr_delay))
```

![](HW_4_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->
