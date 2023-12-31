Vis part 2
================
Longyi Zhao
2023-10-03

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.3     ✔ readr     2.1.4
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.0
    ## ✔ ggplot2   3.4.3     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.2     ✔ tidyr     1.3.0
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
knitr::opts_chunk$set(
  fig.width = 6,
  fig.asp = .6,
  out.width = "90%"
)
```

``` r
weather_df = 
  rnoaa::meteo_pull_monitors(
    c("USW00094728", "USW00022534", "USS0023B17S"),
    var = c("PRCP", "TMIN", "TMAX"), 
    date_min = "2021-01-01",
    date_max = "2022-12-31") |>
  mutate(
    name = recode(
      id, 
      USW00094728 = "CentralPark_NY", 
      USW00022534 = "Molokai_HI",
      USS0023B17S = "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) |>
  select(name, id, everything())
```

    ## using cached file: /Users/longyizhao/Library/Caches/org.R-project.R/R/rnoaa/noaa_ghcnd/USW00094728.dly

    ## date created (size, mb): 2023-09-28 10:19:30.11192 (8.524)

    ## file min/max dates: 1869-01-01 / 2023-09-30

    ## using cached file: /Users/longyizhao/Library/Caches/org.R-project.R/R/rnoaa/noaa_ghcnd/USW00022534.dly

    ## date created (size, mb): 2023-09-28 10:20:46.67367 (3.83)

    ## file min/max dates: 1949-10-01 / 2023-09-30

    ## using cached file: /Users/longyizhao/Library/Caches/org.R-project.R/R/rnoaa/noaa_ghcnd/USS0023B17S.dly

    ## date created (size, mb): 2023-09-28 10:20:53.320358 (0.994)

    ## file min/max dates: 1999-09-01 / 2023-09-30

## same plot from last time

``` r
weather_df |>
  # filter (tmax >= 20, tmax <=30)
  ggplot(aes(x = tmin, y = tmax, color = name)) +
  geom_point(alpha = 0.5) +
  labs(
    title = "Temperature plot", 
    x = "min daily temp (Degree C)", 
    y = "max daily temp",
    color = "location", 
    caption = "max vs min daily temperature in three locations"
  ) +
  scale_x_continuous(
    breaks = c(-15,0,15),
    labels = c( "-15C", "0", "15C")
  ) +
  scale_y_continuous(
    position = "right", 
    # limits = c(0,30) #zoom in 
    trans = "sqrt"
  )
```

    ## Warning in self$trans$transform(x): NaNs produced

    ## Warning: Transformation introduced infinite values in continuous y-axis

    ## Warning: Removed 142 rows containing missing values (`geom_point()`).

<img src="vis_part_2_files/figure-gfm/unnamed-chunk-3-1.png" width="90%" />

what about color

``` r
weather_df |>
  # filter (tmax >= 20, tmax <=30)
  ggplot(aes(x = tmin, y = tmax, color = name)) +
  geom_point(alpha = 0.5) +
  labs(
    title = "Temperature plot", 
    x = "min daily temp (Degree C)", 
    y = "max daily temp",
    color = "location", 
    caption = "max vs min daily temperature in three locations"
  ) +
  # scale_color_hue(h = c(150,300)) # change color 
  viridis::scale_color_viridis(discrete = TRUE) # this is encouraged
```

    ## Warning: Removed 17 rows containing missing values (`geom_point()`).

<img src="vis_part_2_files/figure-gfm/unnamed-chunk-4-1.png" width="90%" />

## Themes
