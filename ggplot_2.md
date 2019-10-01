visualization\_ggplot1
================
DuDu
9/26/2019

``` r
library(tidyverse)
```

    ## ── Attaching packages ──────────────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 3.2.1     ✔ purrr   0.3.2
    ## ✔ tibble  2.1.3     ✔ dplyr   0.8.3
    ## ✔ tidyr   1.0.0     ✔ stringr 1.4.0
    ## ✔ readr   1.3.1     ✔ forcats 0.4.0

    ## ── Conflicts ─────────────────────────────────────────────────── tidyverse_conflicts() ──
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
#install.packages("devtools")
devtools::install_github("thomasp85/patchwork")
```

    ## Skipping install of 'patchwork' from a github remote, the SHA1 (36b49187) has not changed since last install.
    ##   Use `force = TRUE` to force installation

``` r
library(patchwork)
```

``` r
## cache=TRUE means we do not need to download the dataset from the website everytime, it will save the dataset
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
#weather_df
```

adding labels, caption will show below the plot add scales and its
association labels

``` r
## adding labels, caption will show below the plot
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax,color=name)) + 
  geom_point(alpha = .5)+
  labs(
    title = "Temperature plot",
    x = "Minimum daily temperature (C)",
    y = "Maxiumum daily temperature (C)",
    caption = "Data from the rnoaa package"
  )+
  scale_x_continuous(
    breaks=c(-15,0,15),
    labels = c("-15º C", "0", "15")
  )+
    scale_y_continuous(
    trans = "sqrt", 
    position = "right")
```

    ## Warning in self$trans$transform(x): NaNs produced

    ## Warning: Transformation introduced infinite values in continuous y-axis

    ## Warning: Removed 90 rows containing missing values (geom_point).

![](ggplot_2_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

``` r
## trans = 可以进行转换，直接画图的时候转换，不用在之前用函数转了再画图
```

trans = 可以进行转换，直接画图的时候转换，不用在之前用函数转了再画图，也可以用+ scale\_y\_sqrt()

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax,color=name)) + 
  geom_point(alpha = .5)+
  labs(
    title = "Temperature plot",
    x = "Minimum daily temperature (C)",
    y = "Maxiumum daily temperature (C)",
    caption = "Data from the rnoaa package"
  )+
  scale_x_continuous(
    breaks=c(-15,0,15),
    labels = c("-15º C", "0", "15")
  )+
scale_y_sqrt()
```

    ## Warning in self$trans$transform(x): NaNs produced

    ## Warning: Transformation introduced infinite values in continuous y-axis

    ## Warning: Removed 90 rows containing missing values (geom_point).

![](ggplot_2_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

scale\_color\_hue 可以更改scale的label，还有点的颜色

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax,color=name)) + 
  geom_point(alpha = .5)+
  labs(
    title = "Temperature plot",
    x = "Minimum daily temperature (C)",
    y = "Maxiumum daily temperature (C)",
    caption = "Data from the rnoaa package"
  )+
  scale_color_hue(
    name="location",
    h = c(50,200)
  )
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](ggplot_2_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

viridis::scale\_color\_viridis 是一个换颜色的函数，同时可以改scale的label

``` r
gg_base=
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax,color=name)) + 
  geom_point(alpha = .5)+
  labs(
    title = "Temperature plot",
    x = "Minimum daily temperature (C)",
    y = "Maxiumum daily temperature (C)",
    caption = "Data from the rnoaa package"
  )+
  viridis::scale_color_viridis(
    name = "Location", 
    discrete = TRUE
  )
```

theme 函数可以更改general的设置， theme\_minimal()/theme\_bw()
用来更改背景，如果写在后面的话不能发挥作用哦

``` r
gg_base+
  theme(legend.position = "bottom")
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](ggplot_2_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

``` r
gg_base+
  theme_minimal()+
  theme(legend.position = "bottom")
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](ggplot_2_files/figure-gfm/unnamed-chunk-6-2.png)<!-- -->

``` r
gg_base+
  theme(legend.position = "bottom")+
  theme_minimal()
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](ggplot_2_files/figure-gfm/unnamed-chunk-6-3.png)<!-- -->

more than one dataset

geom\_line 可以从另一个dataset的里面调用数据画在同一幅图中

``` r
central_park = 
  weather_df %>% 
  filter(name == "CentralPark_NY")

waikiki = 
  weather_df %>% 
  filter(name == "Waikiki_HA")

ggplot(data = waikiki, aes(x = date, y = tmax, color = name)) + 
  geom_point() 
```

    ## Warning: Removed 3 rows containing missing values (geom_point).

![](ggplot_2_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

``` r
ggplot(data = waikiki, aes(x = date, y = tmax, color = name)) + 
  geom_point(aes(size=prcp)) + 
  geom_line(data = central_park)
```

    ## Warning: Removed 3 rows containing missing values (geom_point).

![](ggplot_2_files/figure-gfm/unnamed-chunk-7-2.png)<!-- -->

(brief aisde about colors) 如果想手动改成某个颜色，在geom\_point里改成“blue”，不能在ggplot里改

``` r
waikiki %>% 
  ggplot(aes(x=date,y=tmax ))+
  geom_point(color="blue")
```

    ## Warning: Removed 3 rows containing missing values (geom_point).

![](ggplot_2_files/figure-gfm/unnamed-chunk-8-1.png)<!-- --> \#
muti-panel plots 要先加载patchwork

install.packages(“devtools”)
devtools::install\_github(“thomasp85/patchwork”) library(patchwork)

``` r
gg_scatter=
  weather_df %>% 
  ggplot(aes(x=tmin,y=tmax ))+
  geom_point()

gg_density=
   weather_df %>% 
  ggplot(aes(x=tmin ))+
  geom_density()

ggp_box=
     weather_df %>% 
  ggplot(aes(x=tmin,y=tmax ))+
  geom_boxplot()

# 直接加图的名字
ggp_box/(gg_scatter + gg_density) 
```

    ## Warning: Continuous x aesthetic -- did you forget aes(group=...)?

    ## Warning: Removed 15 rows containing missing values (stat_boxplot).

    ## Warning: Removed 15 rows containing missing values (geom_point).

    ## Warning: Removed 15 rows containing non-finite values (stat_density).

![](ggplot_2_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

``` r
tmax_tmin_p = 
  weather_df %>% 
  ggplot(aes(x = tmax, y = tmin, color = name)) + 
  geom_point(alpha = .5) +
  theme(legend.position = "none")

prcp_dens_p = 
  weather_df %>% 
  filter(prcp > 0) %>% 
  ggplot(aes(x = prcp, fill = name)) + 
  geom_density(alpha = .5) + 
  theme(legend.position = "none")

tmax_date_p = 
  weather_df %>% 
  ggplot(aes(x = date, y = tmax, color = name)) + 
  geom_point(alpha = .5) +
  geom_smooth(se = FALSE) + 
  theme(legend.position = "bottom")

(tmax_tmin_p + prcp_dens_p) / tmax_date_p
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

    ## Warning: Removed 3 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 3 rows containing missing values (geom_point).

![](ggplot_2_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

更改组别的顺序，默认按首字母排列 reorder 按分布从低到高排列 relevel 按照自己给定的组别顺序排列
前提：先把variable变成factor

``` r
weather_df %>%
  mutate(name = factor(name),
         name = fct_reorder(name,tmax)) %>% 
  ggplot(aes(x = name, y=tmax,color=name))+
  geom_boxplot()
```

    ## Warning: Removed 3 rows containing non-finite values (stat_boxplot).

![](ggplot_2_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

``` r
weather_df %>%
  mutate(name = factor(name),
         name = fct_relevel(name,"Waterhole_WA","Waikiki_HA","Centralpark")) %>% 
  ggplot(aes(x = name, y=tmax,color=name))+
  geom_boxplot()
```

    ## Warning: Unknown levels in `f`: Centralpark
    
    ## Warning: Removed 3 rows containing non-finite values (stat_boxplot).

![](ggplot_2_files/figure-gfm/unnamed-chunk-11-2.png)<!-- -->

``` r
weather_df %>%
  mutate(name = factor(name),
         name = fct_reorder(name,tmax)) %>% 
  ggplot(aes(x = name, y=tmax,color=name))+
  geom_boxplot()
```

    ## Warning: Removed 3 rows containing non-finite values (stat_boxplot).

![](ggplot_2_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

\#\#restructure then plot facet\_grid 可以把曲线按照“name（group）”分开
前提，要先用pivot\_longer处理数据

``` r
weather_df %>% 
  pivot_longer(
    tmax:tmin,
    names_to = "observation",
    values_to = "tempareture"
  ) %>% 
  ggplot(aes(x=tempareture,fill=observation))+
           geom_density(alpha=.5)+
  facet_grid(~name)+
  theme()
```

    ## Warning: Removed 18 rows containing non-finite values (stat_density).

![](ggplot_2_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

``` r
pup_data = 
  read_csv("./data/FAS_pups.csv", col_types = "ciiiii") %>%
  janitor::clean_names() %>%
  mutate(sex = recode(sex, `1` = "male", `2` = "female")) 

litter_data = 
  read_csv("./data/FAS_litters.csv", col_types = "ccddiiii") %>%
  janitor::clean_names() %>%
  select(-pups_survive) %>%
  separate(group, into = c("dose", "day_of_tx"), sep = 3) %>%
  mutate(wt_gain = gd18_weight - gd0_weight,
         day_of_tx = as.numeric(day_of_tx))

fas_data = left_join(pup_data, litter_data, by = "litter_number") 
```

reorder 可按照纵坐标从低到高排列几幅图 relevel 可按自己定义的组的顺序排
先处理好数据，按照着median从低到高排列好再画图，也使用reorder排列

``` r
fas_data %>% 
  select(sex, dose, day_of_tx, pd_ears:pd_walk) %>% 
  pivot_longer(
    pd_ears:pd_walk,
    names_to = "outcome", 
    values_to = "pn_day") %>% 
  drop_na() %>% 
  mutate(outcome = forcats::fct_reorder(outcome, day_of_tx, median)) %>% 
  ggplot(aes(x = dose, y = pn_day)) + 
  geom_violin() + 
  facet_grid(day_of_tx ~ outcome)
```

![](ggplot_2_files/figure-gfm/unnamed-chunk-15-1.png)<!-- -->
