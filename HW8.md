HW8
================
John
11/23/2021

``` r
library(tidyverse)
```

    ## Warning: package 'tidyverse' was built under R version 4.1.1

    ## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --

    ## v ggplot2 3.3.5     v purrr   0.3.4
    ## v tibble  3.1.6     v dplyr   1.0.7
    ## v tidyr   1.1.4     v stringr 1.4.0
    ## v readr   2.1.0     v forcats 0.5.1

    ## Warning: package 'tibble' was built under R version 4.1.2

    ## Warning: package 'tidyr' was built under R version 4.1.2

    ## Warning: package 'readr' was built under R version 4.1.2

    ## Warning: package 'dplyr' was built under R version 4.1.1

    ## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(modelr)
data("diamonds")

ggplot(data = diamonds) +
  geom_point(aes(x = carat, y = price, color = color))
```

![](HW8_files/figure-gfm/unnamed-chunk-1-1.png)<!-- -->

``` r
ggplot(data = diamonds) +
  geom_line(aes(x = carat, y = price))
```

![](HW8_files/figure-gfm/unnamed-chunk-1-2.png)<!-- -->

Question 2, developing a square-root model

``` r
library(nls2)
```

    ## Warning: package 'nls2' was built under R version 4.1.2

    ## Loading required package: proto

    ## Warning: package 'proto' was built under R version 4.1.2

``` r
data("DNase")

fst_mod <- formula(density ~ beta_1 * sqrt(conc) + beta_0)

summary(fst_mod)
```

    ##  Length   Class    Mode 
    ##       3 formula    call

``` r
by_run <- DNase %>% 
  group_by(Run) %>% 
  nest()

run_model <- function(DNase) {
  nls2(fst_mod, 
                    data = DNase, 
                    start = list(beta_1 = 0.5, beta_0 = 0.1))
}


by_run1 <- by_run %>% 
  mutate(model = map(data, run_model))


glance <- by_run1 %>% 
  mutate(glance = map(model, broom::glance)) %>% 
  unnest(glance)

glance %>%
  arrange(AIC) %>%
  head()
```

    ## # A tibble: 6 x 12
    ## # Groups:   Run [6]
    ##   Run   data          model  sigma isConv     finTol logLik   AIC   BIC deviance
    ##   <ord> <list>        <lis>  <dbl> <lgl>       <dbl>  <dbl> <dbl> <dbl>    <dbl>
    ## 1 7     <tibble [16 ~ <nls> 0.0748 TRUE      1.53e-7   19.8 -33.7 -31.4   0.0784
    ## 2 1     <tibble [16 ~ <nls> 0.0771 TRUE      6.90e-8   19.4 -32.7 -30.4   0.0833
    ## 3 6     <tibble [16 ~ <nls> 0.0798 TRUE      7.55e-8   18.8 -31.6 -29.3   0.0891
    ## 4 11    <tibble [16 ~ <nls> 0.0799 TRUE      3.88e-8   18.8 -31.6 -29.3   0.0895
    ## 5 4     <tibble [16 ~ <nls> 0.0809 TRUE      5.99e-8   18.6 -31.2 -28.9   0.0915
    ## 6 3     <tibble [16 ~ <nls> 0.0817 TRUE      1.07e-7   18.4 -30.9 -28.6   0.0935
    ## # ... with 2 more variables: df.residual <int>, nobs <int>

``` r
ggplot(glance, aes(x=Run, y=AIC)) + 
  geom_boxplot() + 
  geom_jitter(width = 0.3, height = 0)
```

![](HW8_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

``` r
snd_mod <- formula(density ~ (conc * d_max) / (conc + k))

summary(snd_mod)
```

    ##  Length   Class    Mode 
    ##       3 formula    call

``` r
by_run <- DNase %>% 
  group_by(Run) %>% 
  nest()

run2_model <- function(DNase) {
  nls2(snd_mod, 
                    data = DNase, 
                    start = list(d_max = 0.5, k = 0.1))
}

by_run2 <- by_run %>% 
  mutate(model = map(data, run2_model))


glance <- by_run2 %>% 
  mutate(glance = map(model, broom::glance)) %>% 
  unnest(glance)

glance %>%
  arrange(AIC) %>%
  head()
```

    ## # A tibble: 6 x 12
    ## # Groups:   Run [6]
    ##   Run   data          model  sigma isConv     finTol logLik   AIC   BIC deviance
    ##   <ord> <list>        <lis>  <dbl> <lgl>       <dbl>  <dbl> <dbl> <dbl>    <dbl>
    ## 1 4     <tibble [16 ~ <nls> 0.0138 TRUE      3.20e-6   46.9 -87.9 -85.6  0.00265
    ## 2 5     <tibble [16 ~ <nls> 0.0138 TRUE      7.90e-8   46.9 -87.9 -85.6  0.00265
    ## 3 2     <tibble [16 ~ <nls> 0.0162 TRUE      8.62e-7   44.3 -82.6 -80.3  0.00369
    ## 4 1     <tibble [16 ~ <nls> 0.0197 TRUE      7.58e-7   41.2 -76.4 -74.1  0.00542
    ## 5 9     <tibble [16 ~ <nls> 0.0238 TRUE      5.77e-7   38.2 -70.3 -68.0  0.00794
    ## 6 8     <tibble [16 ~ <nls> 0.0266 TRUE      8.05e-6   36.4 -66.8 -64.5  0.00988
    ## # ... with 2 more variables: df.residual <int>, nobs <int>

``` r
ggplot(glance, aes(x=Run, y=AIC)) + 
  geom_boxplot() + 
  geom_jitter(width = 0.3, height = 0)
```

![](HW8_files/figure-gfm/unnamed-chunk-3-1.png)<!-- --> The monod model
is more negative than the square root model. This means that monod model
is more effctive since it yields a lower AIC

**Extra Credit**

``` r
ggplot(data = DNase) +
  geom_point(aes(x = conc, y = density, color = Run))
```

![](HW8_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

``` r
DNase %>% 
  ggplot(aes(conc, density)) + 
  geom_line() + 
  ggtitle("Full data = ")
```

![](HW8_files/figure-gfm/unnamed-chunk-4-2.png)<!-- -->

``` r
by_run_ex <- by_run1 %>% 
  mutate(predict = map2(data, model, add_predictions))

predicts <- unnest(by_run_ex, predict)


predicts %>%
ggplot(aes(conc, density)) +
    geom_line(aes(group = pred), alpha = 1 / 3) + 
    geom_smooth(se = FALSE)
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](HW8_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

\`\`\`
