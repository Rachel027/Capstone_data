What Impacts do Artificial Oyster Reef have on Land Erosion Caused by
Storm Surges in the Gulf Coast?
================
2023-03-30

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.1     ✔ readr     2.1.4
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.0
    ## ✔ ggplot2   3.4.1     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.2     ✔ tidyr     1.3.0
    ## ✔ purrr     1.0.1     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the ]8;;http://conflicted.r-lib.org/conflicted package]8;; to force all conflicts to become errors

``` r
library(readr)
library(skimr)
```

# Import Data Manually

*Hurricane Dataset*

``` r
hurricane_data <- read_csv("Hurricane Dataset/hf011-01-hurricanes.csv")
```

    ## Rows: 67 Columns: 10
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (4): code, name, f.max, track
    ## dbl  (4): number, ss, rm, b
    ## date (2): start.date, end.date
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
hurricane_damage <- read_csv("Hurricane Dataset/hf011-02-damage.csv")
```

    ## Rows: 1233 Columns: 5
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (3): code, state, town
    ## dbl (2): gis, f.scale
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
hurricane_tracks <- read_csv("Hurricane Dataset/hf011-03-tracks.csv")
```

    ## Rows: 1809 Columns: 11
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (1): code
    ## dbl  (9): year, month, day, hour, minute, lat, long, vm.m, vm.k
    ## dttm (1): datetime
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
hurricane_site <- read_csv("Hurricane Dataset/hf011-04-site.csv")
```

    ## Rows: 114 Columns: 4
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): code, site
    ## dbl (2): f.scale, w.dir
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

*Storm Surge Dataset*

``` r
stormsurge <- read_csv("globalpeaksurgedb.csv")
```

    ## Rows: 745 Columns: 23
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (10): Storm Name, Storm Dates, Time, Country, State, Location, Surge_m, ...
    ## dbl (13): Year, Reg, Sub Reg, Lat, Lon, Surge_ft, Storm_Tide_m, Storm_Tide_f...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

*Oyster Modeling data*

``` r
oyster_model_tb1 <- read_csv("Oyster Model Data/ofr20211063_table_1.1 (1).csv")
```

    ## New names:
    ## Rows: 36 Columns: 5
    ## ── Column specification
    ## ──────────────────────────────────────────────────────── Delimiter: "," chr
    ## (5): Table 1.1. Discrete water-quality data sources., ...2, ...3, ...4, ...
    ## ℹ Use `spec()` to retrieve the full column specification for this data. ℹ
    ## Specify the column types or set `show_col_types = FALSE` to quiet this message.
    ## • `` -> `...2`
    ## • `` -> `...3`
    ## • `` -> `...4`
    ## • `` -> `...5`

``` r
oyster_model_tb2 <- read_csv("Oyster Model Data/ofr20211063_table_2.1 (2).csv")
```

    ## New names:
    ## Rows: 30 Columns: 4
    ## ── Column specification
    ## ──────────────────────────────────────────────────────── Delimiter: "," chr
    ## (4): Table 2.1. Modeled water quality and physical data sources., ...2, ...
    ## ℹ Use `spec()` to retrieve the full column specification for this data. ℹ
    ## Specify the column types or set `show_col_types = FALSE` to quiet this message.
    ## • `` -> `...2`
    ## • `` -> `...3`
    ## • `` -> `...4`

``` r
oyster_model_tb3 <- read_csv("Oyster Model Data/ofr20211063_table_3.1 (1).csv")
```

    ## New names:
    ## Rows: 41 Columns: 14
    ## ── Column specification
    ## ──────────────────────────────────────────────────────── Delimiter: "," chr
    ## (14): Table 3.1. Oyster model inventory., ...2, ...3, ...4, ...5, ...6, ...
    ## ℹ Use `spec()` to retrieve the full column specification for this data. ℹ
    ## Specify the column types or set `show_col_types = FALSE` to quiet this message.
    ## • `` -> `...2`
    ## • `` -> `...3`
    ## • `` -> `...4`
    ## • `` -> `...5`
    ## • `` -> `...6`
    ## • `` -> `...7`
    ## • `` -> `...8`
    ## • `` -> `...9`
    ## • `` -> `...10`
    ## • `` -> `...11`
    ## • `` -> `...12`
    ## • `` -> `...13`
    ## • `` -> `...14`

*Oyster Data*

``` r
Oyster_data_NC <-  read_csv("Oyster Data/bcodmo_dataset_704333_712b_5843_9069.csv")
```

    ## Rows: 10 Columns: 14
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (14): reef_ID, habitat, location, latitude, longitude, bucket, weight_tt...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

**Storm surge**

``` r
skim(stormsurge)
```

|                                                  |            |
|:-------------------------------------------------|:-----------|
| Name                                             | stormsurge |
| Number of rows                                   | 745        |
| Number of columns                                | 23         |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |            |
| Column type frequency:                           |            |
| character                                        | 10         |
| numeric                                          | 13         |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |            |
| Group variables                                  | None       |

Data summary

**Variable type: character**

| skim_variable | n_missing | complete_rate | min | max | empty | n_unique | whitespace |
|:--------------|----------:|--------------:|----:|----:|------:|---------:|-----------:|
| Storm Name    |        25 |          0.97 |   3 |  37 |     0 |      385 |          0 |
| Storm Dates   |       148 |          0.80 |   3 |  17 |     0 |      526 |          0 |
| Time          |       738 |          0.01 |   5 |   8 |     0 |        7 |          0 |
| Country       |         1 |          1.00 |   2 |   5 |     0 |       38 |          0 |
| State         |       255 |          0.66 |   2 |   6 |     0 |       29 |          0 |
| Location      |        42 |          0.94 |   4 |  58 |     0 |      502 |          0 |
| Surge_m       |       374 |          0.50 |   1 |   7 |     0 |      166 |          0 |
| Datum         |       663 |          0.11 |   3 |  46 |     0 |       35 |          0 |
| Type of Obs   |       718 |          0.04 |   4 |  48 |     0 |       18 |          0 |
| Tropical      |         0 |          1.00 |   1 |   1 |     0 |        2 |          0 |

**Variable type: numeric**

| skim_variable       | n_missing | complete_rate |    mean |     sd |      p0 |     p25 |     p50 |     p75 |    p100 | hist  |
|:--------------------|----------:|--------------:|--------:|-------:|--------:|--------:|--------:|--------:|--------:|:------|
| Year                |         0 |          1.00 | 1969.44 |  34.76 | 1737.00 | 1953.00 | 1976.00 | 1998.00 | 2014.00 | ▁▁▁▃▇ |
| Reg                 |         1 |          1.00 |    3.64 |   1.84 |    1.00 |    2.00 |    5.00 |    5.00 |   11.00 | ▆▇▁▁▁ |
| Sub Reg             |         9 |          0.99 |    1.72 |   1.08 |    1.00 |    1.00 |    1.00 |    2.00 |    6.00 | ▇▂▁▁▁ |
| Lat                 |       158 |          0.79 |   20.54 |  17.50 |  -33.33 |   20.05 |   26.77 |   30.11 |   52.34 | ▁▁▁▇▁ |
| Lon                 |       158 |          0.79 |  -16.32 |  98.61 | -179.05 |  -88.16 |  -80.16 |  110.17 |  179.20 | ▁▇▁▁▃ |
| Surge_ft            |       467 |          0.37 |    7.79 |   5.49 |    0.94 |    4.00 |    6.48 |   10.00 |   44.95 | ▇▃▁▁▁ |
| Storm_Tide_m        |       374 |          0.50 |    2.91 |   1.96 |    0.20 |    1.60 |    2.38 |    3.78 |   13.70 | ▇▅▁▁▁ |
| Storm_Tide_ft       |       451 |          0.39 |    9.45 |   6.28 |    0.72 |    4.94 |    7.11 |   13.18 |   31.50 | ▇▅▃▁▁ |
| Storm_Tide_Waves_m  |       736 |          0.01 |    6.03 |   1.88 |    3.40 |    5.00 |    6.00 |    7.60 |    9.20 | ▇▇▇▇▃ |
| Storm_Tide_Waves_ft |       740 |          0.01 |   22.50 |   5.28 |   16.40 |   19.68 |   21.33 |   24.93 |   30.18 | ▃▇▁▃▃ |
| Confidence          |       305 |          0.59 |    2.37 |   0.60 |    1.00 |    2.00 |    2.00 |    3.00 |    4.00 | ▁▇▁▆▁ |
| Surge ID            |       402 |          0.46 |  268.54 | 100.25 |    2.00 |  204.50 |  289.00 |  339.60 |  429.00 | ▁▃▃▇▅ |
| Storm ID            |       402 |          0.46 |  268.48 | 100.26 |    2.00 |  204.50 |  289.00 |  339.50 |  429.00 | ▁▃▃▇▅ |

``` r
skim (Oyster_data_NC)
```

|                                                  |                |
|:-------------------------------------------------|:---------------|
| Name                                             | Oyster_data_NC |
| Number of rows                                   | 10             |
| Number of columns                                | 14             |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |                |
| Column type frequency:                           |                |
| character                                        | 14             |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |                |
| Group variables                                  | None           |

Data summary

**Variable type: character**

| skim_variable     | n_missing | complete_rate | min | max | empty | n_unique | whitespace |
|:------------------|----------:|--------------:|----:|----:|------:|---------:|-----------:|
| reef_ID           |         0 |             1 |   3 |   8 |     0 |       10 |          0 |
| habitat           |         0 |             1 |   4 |   8 |     0 |        4 |          0 |
| location          |         0 |             1 |   1 |   8 |     0 |        4 |          0 |
| latitude          |         0 |             1 |   7 |  13 |     0 |        9 |          0 |
| longitude         |         0 |             1 |   8 |  12 |     0 |       10 |          0 |
| bucket            |         0 |             1 |   1 |   8 |     0 |       10 |          0 |
| weight_ttl        |         0 |             1 |   3 |   9 |     0 |       10 |          0 |
| weight_liveOyster |         0 |             1 |   3 |   9 |     0 |       10 |          0 |
| weight_shell      |         0 |             1 |   3 |   9 |     0 |       10 |          0 |
| count             |         0 |             1 |   1 |   5 |     0 |       10 |          0 |
| density           |         0 |             1 |   3 |  15 |     0 |       10 |          0 |
| length_avg        |         0 |             1 |   5 |  11 |     0 |       10 |          0 |
| length_stDev      |         0 |             1 |   4 |   8 |     0 |       10 |          0 |
| length_median     |         0 |             1 |   2 |  11 |     0 |        9 |          0 |

``` r
skim(oyster_model_tb1)
```

|                                                  |                  |
|:-------------------------------------------------|:-----------------|
| Name                                             | oyster_model_tb1 |
| Number of rows                                   | 36               |
| Number of columns                                | 5                |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |                  |
| Column type frequency:                           |                  |
| character                                        | 5                |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |                  |
| Group variables                                  | None             |

Data summary

**Variable type: character**

| skim_variable                                   | n_missing | complete_rate | min | max | empty | n_unique | whitespace |
|:------------------------------------------------|----------:|--------------:|----:|----:|------:|---------:|-----------:|
| Table 1.1. Discrete water-quality data sources. |         1 |          0.97 |   6 | 210 |     0 |       35 |          0 |
| …2                                              |         3 |          0.92 |   3 |  14 |     0 |       31 |          0 |
| …3                                              |         3 |          0.92 |   5 |  36 |     0 |        9 |          0 |
| …4                                              |         3 |          0.92 |   5 | 107 |     0 |       24 |          0 |
| …5                                              |         3 |          0.92 |   4 | 126 |     0 |       33 |          0 |

``` r
skim(oyster_model_tb2)
```

|                                                  |                  |
|:-------------------------------------------------|:-----------------|
| Name                                             | oyster_model_tb2 |
| Number of rows                                   | 30               |
| Number of columns                                | 4                |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |                  |
| Column type frequency:                           |                  |
| character                                        | 4                |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |                  |
| Group variables                                  | None             |

Data summary

**Variable type: character**

| skim_variable                                               | n_missing | complete_rate | min | max | empty | n_unique | whitespace |
|:------------------------------------------------------------|----------:|--------------:|----:|----:|------:|---------:|-----------:|
| Table 2.1. Modeled water quality and physical data sources. |         0 |          1.00 |   7 | 728 |     0 |       30 |          0 |
| …2                                                          |         1 |          0.97 |   3 |  34 |     0 |       24 |          0 |
| …3                                                          |         1 |          0.97 |   9 |  99 |     0 |       27 |          0 |
| …4                                                          |         1 |          0.97 |   9 | 107 |     0 |       13 |          0 |

``` r
skim(oyster_model_tb3)
```

|                                                  |                  |
|:-------------------------------------------------|:-----------------|
| Name                                             | oyster_model_tb3 |
| Number of rows                                   | 41               |
| Number of columns                                | 14               |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |                  |
| Column type frequency:                           |                  |
| character                                        | 14               |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |                  |
| Group variables                                  | None             |

Data summary

**Variable type: character**

| skim_variable                      | n_missing | complete_rate | min | max | empty | n_unique | whitespace |
|:-----------------------------------|----------:|--------------:|----:|----:|------:|---------:|-----------:|
| Table 3.1. Oyster model inventory. |         0 |          1.00 |  13 | 474 |     0 |       25 |          0 |
| …2                                 |         6 |          0.85 |   9 |  81 |     0 |       35 |          0 |
| …3                                 |         6 |          0.85 |   8 | 222 |     0 |       28 |          0 |
| …4                                 |         6 |          0.85 |  31 | 705 |     0 |       35 |          0 |
| …5                                 |         6 |          0.85 |   8 | 334 |     0 |       34 |          0 |
| …6                                 |         6 |          0.85 |  47 | 566 |     0 |       35 |          0 |
| …7                                 |         6 |          0.85 |  17 | 231 |     0 |       18 |          0 |
| …8                                 |         6 |          0.85 |  18 | 296 |     0 |       31 |          0 |
| …9                                 |         6 |          0.85 |  19 | 290 |     0 |       35 |          0 |
| …10                                |         6 |          0.85 |  20 | 249 |     0 |       34 |          0 |
| …11                                |         6 |          0.85 |  14 | 312 |     0 |       29 |          0 |
| …12                                |         6 |          0.85 |  14 | 340 |     0 |       22 |          0 |
| …13                                |         6 |          0.85 |  48 | 738 |     0 |       32 |          0 |
| …14                                |         6 |          0.85 |  15 | 313 |     0 |       32 |          0 |
