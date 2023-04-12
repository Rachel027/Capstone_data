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
library(dplyr)
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

**SKIM DATA**

``` r
skim(hurricane_damage)
```

|                                                  |                  |
|:-------------------------------------------------|:-----------------|
| Name                                             | hurricane_damage |
| Number of rows                                   | 1233             |
| Number of columns                                | 5                |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |                  |
| Column type frequency:                           |                  |
| character                                        | 3                |
| numeric                                          | 2                |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |                  |
| Group variables                                  | None             |

Data summary

**Variable type: character**

| skim_variable | n_missing | complete_rate | min | max | empty | n_unique | whitespace |
|:--------------|----------:|--------------:|----:|----:|------:|---------:|-----------:|
| code          |         0 |             1 |   5 |   6 |     0 |       57 |          0 |
| state         |         0 |             1 |   2 |   2 |     0 |        7 |          0 |
| town          |         0 |             1 |   8 |  24 |     0 |      455 |          0 |

**Variable type: numeric**

| skim_variable | n_missing | complete_rate |   mean |     sd |  p0 | p25 | p50 | p75 | p100 | hist  |
|:--------------|----------:|--------------:|-------:|-------:|----:|----:|----:|----:|-----:|:------|
| gis           |         0 |             1 | 566.34 | 525.63 |   2 | 198 | 393 | 613 | 1689 | ▇▅▁▁▃ |
| f.scale       |         0 |             1 |   1.18 |   0.65 |  -1 |   1 |   1 |   2 |    3 | ▁▂▇▃▁ |

``` r
skim(hurricane_data)
```

|                                                  |                |
|:-------------------------------------------------|:---------------|
| Name                                             | hurricane_data |
| Number of rows                                   | 67             |
| Number of columns                                | 10             |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |                |
| Column type frequency:                           |                |
| character                                        | 4              |
| Date                                             | 2              |
| numeric                                          | 4              |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |                |
| Group variables                                  | None           |

Data summary

**Variable type: character**

| skim_variable | n_missing | complete_rate | min | max | empty | n_unique | whitespace |
|:--------------|----------:|--------------:|----:|----:|------:|---------:|-----------:|
| code          |         0 |          1.00 |   5 |   6 |     0 |       67 |          0 |
| name          |        50 |          0.25 |   3 |   7 |     0 |       15 |          0 |
| f.max         |         0 |          1.00 |   2 |   2 |     0 |        6 |          0 |
| track         |         0 |          1.00 |   1 |   1 |     0 |        3 |          0 |

**Variable type: Date**

| skim_variable | n_missing | complete_rate | min        | max        | median     | n_unique |
|:--------------|----------:|--------------:|:-----------|:-----------|:-----------|---------:|
| start.date    |         0 |             1 | 1635-08-25 | 1996-09-02 | 1893-06-18 |       67 |
| end.date      |         0 |             1 | 1635-08-25 | 1996-09-02 | 1893-06-18 |       67 |

**Variable type: numeric**

| skim_variable | n_missing | complete_rate |  mean |    sd |   p0 |  p25 |  p50 |   p75 |  p100 | hist  |
|:--------------|----------:|--------------:|------:|------:|-----:|-----:|-----:|------:|------:|:------|
| number        |        25 |          0.63 |  5.07 |  3.47 |  1.0 |  2.0 |  4.0 |   7.0 |  15.0 | ▇▇▃▁▂ |
| ss            |         0 |          1.00 |  1.81 |  0.68 |  1.0 |  1.0 |  2.0 |   2.0 |   3.0 | ▆▁▇▁▂ |
| rm            |         0 |          1.00 | 76.49 | 18.40 | 50.0 | 75.0 | 75.0 | 100.0 | 100.0 | ▅▁▇▁▅ |
| b             |         0 |          1.00 |  1.30 |  0.00 |  1.3 |  1.3 |  1.3 |   1.3 |   1.3 | ▁▁▇▁▁ |

``` r
skim(hurricane_tracks)
```

|                                                  |                  |
|:-------------------------------------------------|:-----------------|
| Name                                             | hurricane_tracks |
| Number of rows                                   | 1809             |
| Number of columns                                | 11               |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |                  |
| Column type frequency:                           |                  |
| character                                        | 1                |
| numeric                                          | 9                |
| POSIXct                                          | 1                |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |                  |
| Group variables                                  | None             |

Data summary

**Variable type: character**

| skim_variable | n_missing | complete_rate | min | max | empty | n_unique | whitespace |
|:--------------|----------:|--------------:|----:|----:|------:|---------:|-----------:|
| code          |         0 |             1 |   5 |   6 |     0 |       67 |          0 |

**Variable type: numeric**

| skim_variable | n_missing | complete_rate |    mean |    sd |     p0 |    p25 |    p50 |    p75 |   p100 | hist  |
|:--------------|----------:|--------------:|--------:|------:|-------:|-------:|-------:|-------:|-------:|:------|
| year          |         0 |             1 | 1919.15 | 65.95 | 1635.0 | 1893.0 | 1936.0 | 1960.0 | 1996.0 | ▁▁▁▃▇ |
| month         |         0 |             1 |    8.73 |  0.97 |    6.0 |    8.0 |    9.0 |    9.0 |   12.0 | ▁▅▇▂▁ |
| day           |         0 |             1 |   16.38 |  8.44 |    1.0 |    9.0 |   17.0 |   23.0 |   31.0 | ▆▇▇▇▆ |
| hour          |         0 |             1 |    9.14 |  6.69 |    0.0 |    6.0 |   12.0 |   15.0 |   22.0 | ▇▇▇▁▇ |
| minute        |         0 |             1 |    0.00 |  0.00 |    0.0 |    0.0 |    0.0 |    0.0 |    0.0 | ▁▁▇▁▁ |
| lat           |         0 |             1 |   31.93 | 11.44 |   10.1 |   22.7 |   30.7 |   41.3 |   61.8 | ▅▇▆▅▁ |
| long          |         0 |             1 |  -63.58 | 14.39 |  -95.2 |  -73.2 |  -68.1 |  -58.4 |   -9.1 | ▁▇▂▁▁ |
| vm.m          |         0 |             1 |   36.97 | 14.59 |    5.1 |   25.7 |   36.0 |   46.3 |   82.3 | ▃▆▇▃▁ |
| vm.k          |         0 |             1 |   71.87 | 28.36 |   10.0 |   50.0 |   70.0 |   90.0 |  160.0 | ▅▇▇▃▁ |

**Variable type: POSIXct**

| skim_variable | n_missing | complete_rate | min                 | max                 | median              | n_unique |
|:--------------|----------:|--------------:|:--------------------|:--------------------|:--------------------|---------:|
| datetime      |         0 |             1 | 1635-08-24 18:00:00 | 1996-09-06 18:00:00 | 1936-09-22 12:00:00 |     1738 |

``` r
skim(hurricane_site)
```

|                                                  |                |
|:-------------------------------------------------|:---------------|
| Name                                             | hurricane_site |
| Number of rows                                   | 114            |
| Number of columns                                | 4              |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |                |
| Column type frequency:                           |                |
| character                                        | 2              |
| numeric                                          | 2              |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |                |
| Group variables                                  | None           |

Data summary

**Variable type: character**

| skim_variable | n_missing | complete_rate | min | max | empty | n_unique | whitespace |
|:--------------|----------:|--------------:|----:|----:|------:|---------:|-----------:|
| code          |         0 |             1 |   5 |   6 |     0 |       57 |          0 |
| site          |         0 |             1 |   9 |  10 |     0 |        2 |          0 |

**Variable type: numeric**

| skim_variable | n_missing | complete_rate |  mean |     sd |  p0 |   p25 | p50 |    p75 |  p100 | hist  |
|:--------------|----------:|--------------:|------:|-------:|----:|------:|----:|-------:|------:|:------|
| f.scale       |        14 |          0.88 |   1.0 |   0.63 |   0 |  0.58 |   1 |   1.42 |   2.5 | ▇▇▇▃▂ |
| w.dir         |        14 |          0.88 | 114.3 | 105.29 |   0 | 35.50 |  80 | 145.50 | 360.0 | ▇▅▂▁▂ |

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

**DATA ANALYSIS**

``` r
stormsurge %>%
  group_by(Country = "US") %>%
  group_by(State = "FL")
```

    ## # A tibble: 745 × 23
    ## # Groups:   State [1]
    ##     Year Storm N…¹ Storm…² Time    Reg Sub R…³ Country State Locat…⁴   Lat   Lon
    ##    <dbl> <chr>     <chr>   <chr> <dbl>   <dbl> <chr>   <chr> <chr>   <dbl> <dbl>
    ##  1  1897 Typhoon … 12-Oct  <NA>      1       1 US      FL    Hernan…  11.4  126.
    ##  2  1908 Typhoon … 13-Oct  <NA>      1       1 US      FL    Aparri   18.4  122.
    ##  3  1908 <NA>      Sep 17… <NA>      1       1 US      FL    <NA>     NA     NA 
    ##  4  1912 Typhoon … Oct 14… <NA>      1       1 US      FL    <NA>     10.4  125.
    ##  5  1912 Typhoon … Sep 27… <NA>      1       1 US      FL    <NA>     NA     NA 
    ##  6  1912 Typhoon … Nov 24… <NA>      1       1 US      FL    <NA>     NA     NA 
    ##  7  1914 Typhoon … Jun 15… <NA>      1       1 US      FL    <NA>     12.3  125.
    ##  8  1916 Typhoon … Jan 10… <NA>      1       1 US      FL    <NA>     11.2  124.
    ##  9  1968 Didang    <NA>    <NA>      1       1 US      FL    Barrio…  17.4  120.
    ## 10  1970 <NA>      <NA>    <NA>      1       1 US      FL    <NA>     14.5  120.
    ## # … with 735 more rows, 12 more variables: Surge_m <chr>, Surge_ft <dbl>,
    ## #   Storm_Tide_m <dbl>, Storm_Tide_ft <dbl>, Storm_Tide_Waves_m <dbl>,
    ## #   Storm_Tide_Waves_ft <dbl>, Datum <chr>, `Type of Obs` <chr>,
    ## #   Tropical <chr>, Confidence <dbl>, `Surge ID` <dbl>, `Storm ID` <dbl>, and
    ## #   abbreviated variable names ¹​`Storm Name`, ²​`Storm Dates`, ³​`Sub Reg`,
    ## #   ⁴​Location

``` r
stormsurge %>%
  group_by(Country = "US") %>%
  group_by(State = "TX")
```

    ## # A tibble: 745 × 23
    ## # Groups:   State [1]
    ##     Year Storm N…¹ Storm…² Time    Reg Sub R…³ Country State Locat…⁴   Lat   Lon
    ##    <dbl> <chr>     <chr>   <chr> <dbl>   <dbl> <chr>   <chr> <chr>   <dbl> <dbl>
    ##  1  1897 Typhoon … 12-Oct  <NA>      1       1 US      TX    Hernan…  11.4  126.
    ##  2  1908 Typhoon … 13-Oct  <NA>      1       1 US      TX    Aparri   18.4  122.
    ##  3  1908 <NA>      Sep 17… <NA>      1       1 US      TX    <NA>     NA     NA 
    ##  4  1912 Typhoon … Oct 14… <NA>      1       1 US      TX    <NA>     10.4  125.
    ##  5  1912 Typhoon … Sep 27… <NA>      1       1 US      TX    <NA>     NA     NA 
    ##  6  1912 Typhoon … Nov 24… <NA>      1       1 US      TX    <NA>     NA     NA 
    ##  7  1914 Typhoon … Jun 15… <NA>      1       1 US      TX    <NA>     12.3  125.
    ##  8  1916 Typhoon … Jan 10… <NA>      1       1 US      TX    <NA>     11.2  124.
    ##  9  1968 Didang    <NA>    <NA>      1       1 US      TX    Barrio…  17.4  120.
    ## 10  1970 <NA>      <NA>    <NA>      1       1 US      TX    <NA>     14.5  120.
    ## # … with 735 more rows, 12 more variables: Surge_m <chr>, Surge_ft <dbl>,
    ## #   Storm_Tide_m <dbl>, Storm_Tide_ft <dbl>, Storm_Tide_Waves_m <dbl>,
    ## #   Storm_Tide_Waves_ft <dbl>, Datum <chr>, `Type of Obs` <chr>,
    ## #   Tropical <chr>, Confidence <dbl>, `Surge ID` <dbl>, `Storm ID` <dbl>, and
    ## #   abbreviated variable names ¹​`Storm Name`, ²​`Storm Dates`, ³​`Sub Reg`,
    ## #   ⁴​Location

``` r
stormsurge %>%
  group_by(Country = "US") %>%
  group_by(State = "LA")
```

    ## # A tibble: 745 × 23
    ## # Groups:   State [1]
    ##     Year Storm N…¹ Storm…² Time    Reg Sub R…³ Country State Locat…⁴   Lat   Lon
    ##    <dbl> <chr>     <chr>   <chr> <dbl>   <dbl> <chr>   <chr> <chr>   <dbl> <dbl>
    ##  1  1897 Typhoon … 12-Oct  <NA>      1       1 US      LA    Hernan…  11.4  126.
    ##  2  1908 Typhoon … 13-Oct  <NA>      1       1 US      LA    Aparri   18.4  122.
    ##  3  1908 <NA>      Sep 17… <NA>      1       1 US      LA    <NA>     NA     NA 
    ##  4  1912 Typhoon … Oct 14… <NA>      1       1 US      LA    <NA>     10.4  125.
    ##  5  1912 Typhoon … Sep 27… <NA>      1       1 US      LA    <NA>     NA     NA 
    ##  6  1912 Typhoon … Nov 24… <NA>      1       1 US      LA    <NA>     NA     NA 
    ##  7  1914 Typhoon … Jun 15… <NA>      1       1 US      LA    <NA>     12.3  125.
    ##  8  1916 Typhoon … Jan 10… <NA>      1       1 US      LA    <NA>     11.2  124.
    ##  9  1968 Didang    <NA>    <NA>      1       1 US      LA    Barrio…  17.4  120.
    ## 10  1970 <NA>      <NA>    <NA>      1       1 US      LA    <NA>     14.5  120.
    ## # … with 735 more rows, 12 more variables: Surge_m <chr>, Surge_ft <dbl>,
    ## #   Storm_Tide_m <dbl>, Storm_Tide_ft <dbl>, Storm_Tide_Waves_m <dbl>,
    ## #   Storm_Tide_Waves_ft <dbl>, Datum <chr>, `Type of Obs` <chr>,
    ## #   Tropical <chr>, Confidence <dbl>, `Surge ID` <dbl>, `Storm ID` <dbl>, and
    ## #   abbreviated variable names ¹​`Storm Name`, ²​`Storm Dates`, ³​`Sub Reg`,
    ## #   ⁴​Location

``` r
stormsurge %>%
  group_by(Country = "US") %>%
  group_by(State = "MS")
```

    ## # A tibble: 745 × 23
    ## # Groups:   State [1]
    ##     Year Storm N…¹ Storm…² Time    Reg Sub R…³ Country State Locat…⁴   Lat   Lon
    ##    <dbl> <chr>     <chr>   <chr> <dbl>   <dbl> <chr>   <chr> <chr>   <dbl> <dbl>
    ##  1  1897 Typhoon … 12-Oct  <NA>      1       1 US      MS    Hernan…  11.4  126.
    ##  2  1908 Typhoon … 13-Oct  <NA>      1       1 US      MS    Aparri   18.4  122.
    ##  3  1908 <NA>      Sep 17… <NA>      1       1 US      MS    <NA>     NA     NA 
    ##  4  1912 Typhoon … Oct 14… <NA>      1       1 US      MS    <NA>     10.4  125.
    ##  5  1912 Typhoon … Sep 27… <NA>      1       1 US      MS    <NA>     NA     NA 
    ##  6  1912 Typhoon … Nov 24… <NA>      1       1 US      MS    <NA>     NA     NA 
    ##  7  1914 Typhoon … Jun 15… <NA>      1       1 US      MS    <NA>     12.3  125.
    ##  8  1916 Typhoon … Jan 10… <NA>      1       1 US      MS    <NA>     11.2  124.
    ##  9  1968 Didang    <NA>    <NA>      1       1 US      MS    Barrio…  17.4  120.
    ## 10  1970 <NA>      <NA>    <NA>      1       1 US      MS    <NA>     14.5  120.
    ## # … with 735 more rows, 12 more variables: Surge_m <chr>, Surge_ft <dbl>,
    ## #   Storm_Tide_m <dbl>, Storm_Tide_ft <dbl>, Storm_Tide_Waves_m <dbl>,
    ## #   Storm_Tide_Waves_ft <dbl>, Datum <chr>, `Type of Obs` <chr>,
    ## #   Tropical <chr>, Confidence <dbl>, `Surge ID` <dbl>, `Storm ID` <dbl>, and
    ## #   abbreviated variable names ¹​`Storm Name`, ²​`Storm Dates`, ³​`Sub Reg`,
    ## #   ⁴​Location
