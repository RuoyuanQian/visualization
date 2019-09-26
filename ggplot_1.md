visualization\_ggplot1
================
DuDu
9/26/2019

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 3.2.1     ✔ purrr   0.3.2
    ## ✔ tibble  2.1.3     ✔ dplyr   0.8.3
    ## ✔ tidyr   1.0.0     ✔ stringr 1.4.0
    ## ✔ readr   1.3.1     ✔ forcats 0.4.0

    ## ── Conflicts ────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
#install.packages("ggridges")
#install.packages("rnoaa")
library(rnoaa)
```

    ## Registered S3 method overwritten by 'crul':
    ##   method                 from
    ##   as.character.form_file httr

    ## Registered S3 method overwritten by 'hoardr':
    ##   method           from
    ##   print.cache_info httr

``` r
library(ggridges)
```

    ## 
    ## Attaching package: 'ggridges'

    ## The following object is masked from 'package:ggplot2':
    ## 
    ##     scale_discrete_manual

``` r
## 
weather_df = 
  rnoaa::meteo_pull_monitors(c("USW00094728", "USC00519397", "USS0023B17S"),
                      var = c("PRCP", "TMIN", "TMAX"), 
                      date_min = "2017-01-01",
                      date_max = "2017-12-31") %>%
  mutate(
    name = recode (id, USW00094728 = "CentralPark_NY", 
                       USC00519397 = "Waikiki_HA",
                       USS0023B17S = "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) %>%
  select(name, id, everything())
```

    ## file path:          /Users/ruoyuanqian/Library/Caches/rnoaa/ghcnd/USW00094728.dly

    ## file last updated:  2019-09-26 11:10:51

    ## file min/max dates: 1869-01-01 / 2019-09-30

    ## file path:          /Users/ruoyuanqian/Library/Caches/rnoaa/ghcnd/USC00519397.dly

    ## file last updated:  2019-09-26 11:11:00

    ## file min/max dates: 1965-01-01 / 2019-09-30

    ## file path:          /Users/ruoyuanqian/Library/Caches/rnoaa/ghcnd/USS0023B17S.dly

    ## file last updated:  2019-09-26 11:11:03

    ## file min/max dates: 1999-09-01 / 2019-09-30

``` r
weather_df
```

    ## # A tibble: 1,095 x 6
    ##    name           id          date        prcp  tmax  tmin
    ##    <chr>          <chr>       <date>     <dbl> <dbl> <dbl>
    ##  1 CentralPark_NY USW00094728 2017-01-01     0   8.9   4.4
    ##  2 CentralPark_NY USW00094728 2017-01-02    53   5     2.8
    ##  3 CentralPark_NY USW00094728 2017-01-03   147   6.1   3.9
    ##  4 CentralPark_NY USW00094728 2017-01-04     0  11.1   1.1
    ##  5 CentralPark_NY USW00094728 2017-01-05     0   1.1  -2.7
    ##  6 CentralPark_NY USW00094728 2017-01-06    13   0.6  -3.8
    ##  7 CentralPark_NY USW00094728 2017-01-07    81  -3.2  -6.6
    ##  8 CentralPark_NY USW00094728 2017-01-08     0  -3.8  -8.8
    ##  9 CentralPark_NY USW00094728 2017-01-09     0  -4.9  -9.9
    ## 10 CentralPark_NY USW00094728 2017-01-10     0   7.8  -6  
    ## # … with 1,085 more rows

``` r
## ggplot aes build a blank plot
ggplot(weather_df, aes(x = tmin, y = tmax)) + 
  geom_point()
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](ggplot_1_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

``` r
weather_df %>%
  ggplot(aes(x = tmin, y = tmax)) + 
  geom_point()
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](ggplot_1_files/figure-gfm/unnamed-chunk-3-2.png)<!-- -->

``` r
plot_weather = 
  weather_df %>%
  ggplot(aes(x = tmin, y = tmax)) 

plot_weather + geom_point()
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](ggplot_1_files/figure-gfm/unnamed-chunk-3-3.png)<!-- -->

``` r
ggplot(weather_df, aes(x = tmin, y = tmax)) + 
  geom_point(aes(color = name))
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](ggplot_1_files/figure-gfm/unnamed-chunk-3-4.png)<!-- -->

``` r
## "alpha = .5" make the points a little transparent, readable
## "geom_smooth" draw a curve to see the trend
## "color" in "ggplot", all groups in different color
## "color" in "geom_point", just first group will be colored
## 
ggplot(weather_df, aes(x = tmin, y = tmax)) + 
  geom_point(aes(color = name), alpha = .5) +
  geom_smooth(se = FALSE)
```

    ## `geom_smooth()` using method = 'gam' and formula 'y ~ s(x, bs = "cs")'

    ## Warning: Removed 15 rows containing non-finite values (stat_smooth).
    
    ## Warning: Removed 15 rows containing missing values (geom_point).

![](ggplot_1_files/figure-gfm/unnamed-chunk-3-5.png)<!-- -->

``` r
ggplot(weather_df, aes(x = tmin, y = tmax,color = name)) + 
  geom_point( alpha = .5) +
  geom_smooth(se = FALSE)
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

    ## Warning: Removed 15 rows containing non-finite values (stat_smooth).
    
    ## Warning: Removed 15 rows containing missing values (geom_point).

![](ggplot_1_files/figure-gfm/unnamed-chunk-3-6.png)<!-- -->

``` r
## "facet_grid" split the points based on group into different plots
ggplot(weather_df, aes(x = tmin, y = tmax, color = name)) + 
  geom_point(alpha = .5) +
  geom_smooth(se = FALSE) + 
  facet_grid(. ~ name)
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

    ## Warning: Removed 15 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](ggplot_1_files/figure-gfm/unnamed-chunk-4-1.png)<!-- --> \#\# please
be careful the withden of the
plot\!\!

``` r
## "geom_point(aes(size = prcp), alpha = .5)" add another information variable
## 根据prcp的数值，点可大可小
ggplot(weather_df, aes(x = date, y = tmax, color = name)) + 
  geom_point(aes(size = prcp), alpha = .5) +
  geom_smooth(se = FALSE) + 
  facet_grid(. ~ name)
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

    ## Warning: Removed 3 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 3 rows containing missing values (geom_point).

![](ggplot_1_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

## some extra stuff

``` r
## remove"geom_point" can also output smooth lines without points
ggplot(weather_df, aes(x = date, y = tmax, color = name)) + 
  geom_smooth(size=2,se = FALSE) 
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

    ## Warning: Removed 3 rows containing non-finite values (stat_smooth).

![](ggplot_1_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

``` r
## " geom_hex" scatterplot
# install.packages("hexbin")
ggplot(weather_df, aes(x = tmax, y = tmin)) + 
  geom_hex()
```

    ## Warning: Removed 15 rows containing non-finite values (stat_binhex).

![](ggplot_1_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

# more kinds of plots

``` r
## hist has two kinds of color 
## "color=" just have the fifferent color frame 
## "fill=" fill the different bars with different color
## hist dose not need y aes
weather_df %>%
  ggplot(aes(x=tmax,color=name))+
  geom_histogram()
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

    ## Warning: Removed 3 rows containing non-finite values (stat_bin).

![](ggplot_1_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

``` r
ggplot(weather_df, aes(x = tmax, fill = name)) + 
  geom_histogram()+
    facet_grid(. ~ name)
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

    ## Warning: Removed 3 rows containing non-finite values (stat_bin).

![](ggplot_1_files/figure-gfm/unnamed-chunk-8-2.png)<!-- -->

density plots

``` r
## "geom_density()"exploring the distribution 
ggplot(weather_df, aes(x = tmax, fill = name)) + 
  geom_density(alpha = .4)
```

    ## Warning: Removed 3 rows containing non-finite values (stat_density).

![](ggplot_1_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

boxplot

``` r
ggplot(weather_df, aes(x = name, y = tmax)) + 
  geom_boxplot()
```

    ## Warning: Removed 3 rows containing non-finite values (stat_boxplot).

![](ggplot_1_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

violin plot

``` r
## same kind of information like desity plot
## 越粗代表点越密集
## 可以从很多组中看到分布与其他组别不同的组（e.g. two peaks）
ggplot(weather_df, aes(x = name, y = tmax)) + 
  geom_violin()
```

    ## Warning: Removed 3 rows containing non-finite values (stat_ydensity).

![](ggplot_1_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

ridges
plots

``` r
## like violin plots, can see different distribution in a lot of different groups
ggplot(weather_df, aes(x = tmax, y = name)) + 
  geom_density_ridges(scale = .85)
```

    ## Picking joint bandwidth of 1.84

    ## Warning: Removed 3 rows containing non-finite values (stat_density_ridges).

![](ggplot_1_files/figure-gfm/unnamed-chunk-12-1.png)<!-- --> \#\#
saving a plot

``` r
weather_plot = ggplot(weather_df, aes(x = tmin, y = tmax)) + 
  geom_point(aes(color = name), alpha = .5) 

## "ggave" 会自动储存上面离他最近的的图，但是最好在下面设置一下！
ggsave("weather_plot.pdf", weather_plot)
```

    ## Saving 7 x 5 in image

    ## Warning: Removed 15 rows containing missing values (geom_point).
