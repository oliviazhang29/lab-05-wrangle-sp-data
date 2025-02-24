Lab 05 - La Quinta is Spanish for next to Denny’s, Pt. 2
================
Olivia Zhang
02/23/2025

### Load packages and data

``` r
library(tidyverse) 
library(dsbox) 
```

``` r
states <- read_csv("data/states.csv")
```

### Exercise 1

``` r
dn_ak <- dennys %>%
  filter(state == "AK")
nrow(dn_ak)
```

    ## [1] 3

There are 3 Denny’s locations in Alaska.

``` r
lq_ak <- laquinta %>%
  filter(state == "AK")
nrow(lq_ak)
```

    ## [1] 2

There are 2 La Quinta locations in Alaska.

### Exercise 2

``` r
dn_ak$establishment <- "dn"
lq_ak$establishment <- "lq"

dn_lq_ak1 <- 
  full_join(dn_ak, lq_ak, by = join_by(address, city, state, zip, longitude, latitude, establishment))
```

``` r
#dn1
dn1_lq1 <- sqrt((dn_lq_ak1$longitude[1] - dn_lq_ak1$longitude[4])^2 + (dn_lq_ak1$latitude[1] - dn_lq_ak1$latitude[4])^2)
dn1_lq2 <- sqrt((dn_lq_ak1$longitude[1] - dn_lq_ak1$longitude[5])^2 + (dn_lq_ak1$latitude[1] - dn_lq_ak1$latitude[5])^2)

#dn2
dn2_lq1 <- sqrt((dn_lq_ak1$longitude[2] - dn_lq_ak1$longitude[4])^2 + (dn_lq_ak1$latitude[2] - dn_lq_ak1$latitude[4])^2)
dn2_lq2 <- sqrt((dn_lq_ak1$longitude[2] - dn_lq_ak1$longitude[5])^2 + (dn_lq_ak1$latitude[2] - dn_lq_ak1$latitude[5])^2)

#dn3
dn3_lq1 <- sqrt((dn_lq_ak1$longitude[3] - dn_lq_ak1$longitude[4])^2 + (dn_lq_ak1$latitude[3] - dn_lq_ak1$latitude[4])^2)
dn3_lq2 <- sqrt((dn_lq_ak1$longitude[3] - dn_lq_ak1$longitude[5])^2 + (dn_lq_ak1$latitude[3] - dn_lq_ak1$latitude[5])^2)
```

There are six pairings.

### Exercise 3

Well, I guess I did in my own way above. I’ll follow the steps now.

``` r
dn_lq_ak <- full_join(dn_ak, lq_ak, 
                      by = "state")
```

    ## Warning in full_join(dn_ak, lq_ak, by = "state"): Detected an unexpected many-to-many relationship between `x` and `y`.
    ## ℹ Row 1 of `x` matches multiple rows in `y`.
    ## ℹ Row 1 of `y` matches multiple rows in `x`.
    ## ℹ If a many-to-many relationship is expected, set `relationship =
    ##   "many-to-many"` to silence this warning.

``` r
dn_lq_ak
```

    ## # A tibble: 6 × 13
    ##   address.x  city.x state zip.x longitude.x latitude.x establishment.x address.y
    ##   <chr>      <chr>  <chr> <chr>       <dbl>      <dbl> <chr>           <chr>    
    ## 1 2900 Dena… Ancho… AK    99503       -150.       61.2 dn              3501 Min…
    ## 2 2900 Dena… Ancho… AK    99503       -150.       61.2 dn              4920 Dal…
    ## 3 3850 Deba… Ancho… AK    99508       -150.       61.2 dn              3501 Min…
    ## 4 3850 Deba… Ancho… AK    99508       -150.       61.2 dn              4920 Dal…
    ## 5 1929 Airp… Fairb… AK    99701       -148.       64.8 dn              3501 Min…
    ## 6 1929 Airp… Fairb… AK    99701       -148.       64.8 dn              4920 Dal…
    ## # ℹ 5 more variables: city.y <chr>, zip.y <chr>, longitude.y <dbl>,
    ## #   latitude.y <dbl>, establishment.y <chr>

### Exercise 4

There are 6 observations in the joined dn_lq_ak data frame. The names
are address.x, city.x, state, zip.x, longitude.x, latitude.x,
establishment.x, address.y, city.y, zip.y, longitude.y, latitude.y,
establishment.y.

### Exercise 5

### Exercise 6

``` r
dn_lq_ak$distance <- haversine(dn_lq_ak$longitude.x, dn_lq_ak$latitude.x, dn_lq_ak$longitude.y, dn_lq_ak$latitude.y, round = 3)
```

### Exercise 7

``` r
dn_lq_ak_mindist <- dn_lq_ak %>%
  group_by(address.x) %>%
  summarize(closest = min(distance))
```

### Exercise 8

``` r
dn_lq_ak1 %>%
  ggplot(mapping = aes(
    x = longitude,
    y = latitude,
    color = establishment
    )) +
  geom_point() +
  labs(
  title = "Danny's and La Quinta Locations",
  subtitle = "in Alaska",
  x = "Longitude of establishements", 
  y = "Latitude of establishements", 
  color = "Establishment"
     )
```

![](lab-05_files/figure-gfm/ak-vis-1.png)<!-- --> As shown in the graph,
in Alaska, La Quinta locations are all near Denny’s. The distance
between the nearest Denny’s and La Quinta are 5.197, 2.035, 5.998.
