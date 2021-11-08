
# TRmaps <a href='https://github.com/htastan/TRmaps'><img src='man/img/hex1.png' align="right"  width="200"/></a>

The purpose of this package is to provide data sets to draw maps of
Turkey at four levels: - NUTS-Level 1: Broad geographical regions (there
are 12) - NUTS-Level 2: Geographical sub-regions (there are 26) -
NUTS-Level 3: Provinces (there are 81 as of 2021, these are also the
largest administrative units) - District level: there are 973 districts
as of 2021

## Installation

The development version can be installed from
[GitHub](https://github.com/) using

``` r
# devtools is required, install if you don't already have it
# install.packages("devtools")
devtools::install_github("htastan/TRmaps")
```

## Basic Usage

### NUTS-1

``` r
library(turkeymaps)
library(tidyverse)
#> -- Attaching packages --------------------------------------- tidyverse 1.3.1 --
#> v ggplot2 3.3.5     v purrr   0.3.4
#> v tibble  3.1.2     v dplyr   1.0.6
#> v tidyr   1.1.3     v stringr 1.4.0
#> v readr   1.4.0     v forcats 0.5.1
#> -- Conflicts ------------------------------------------ tidyverse_conflicts() --
#> x dplyr::filter() masks stats::filter()
#> x dplyr::lag()    masks stats::lag()
library(sf)
#> Linking to GEOS 3.9.0, GDAL 3.2.1, PROJ 7.2.1
```

``` r
data("tr_nuts1")
tr_nuts1
#> Simple feature collection with 12 features and 2 fields
#> Geometry type: MULTIPOLYGON
#> Dimension:     XY
#> Bounding box:  xmin: 25.66521 ymin: 35.81946 xmax: 44.82037 ymax: 42.10502
#> Geodetic CRS:  WGS 84
#> First 10 features:
#>           NUTS1_name NUTS1_code                       geometry
#> 1     Bati Karadeniz        TR8 MULTIPOLYGON (((35.5137 41....
#> 2           Istanbul        TR1 MULTIPOLYGON (((29.86563 41...
#> 3       Bati Marmara        TR2 MULTIPOLYGON (((28.16478 40...
#> 4  Kuzeydogu Anadolu        TRA MULTIPOLYGON (((43.46964 41...
#> 5     Dogu Karadeniz        TR9 MULTIPOLYGON (((42.51517 41...
#> 6  Güneydogu Anadolu        TRC MULTIPOLYGON (((41.38055 38...
#> 7                Ege        TR3 MULTIPOLYGON (((29.74218 39...
#> 8       Bati Anadolu        TR5 MULTIPOLYGON (((32.55482 40...
#> 9            Akdeniz        TR6 MULTIPOLYGON (((37.63867 37...
#> 10      Dogu Marmara        TR4 MULTIPOLYGON (((30.35469 41...
```

To draw a simple map, run the following code chunk:

``` r
ggplot(tr_nuts1) + 
  geom_sf()
```

<img src="man/img/usage-ex1-1.png" width="100%" />

``` r
ggplot(tr_nuts1) + 
  geom_sf(mapping = aes(fill = NUTS1_name))
```

<img src="man/img/usage-ex2-1.png" width="100%" />

### NUTS-2

``` r
ggplot(tr_nuts2) + 
  geom_sf()
```

<img src="man/img/usage-ex3-1.png" width="100%" />

### NUTS-3

``` r
ggplot(tr_nuts3) + 
  geom_sf()
```

<img src="man/img/usage-ex4-1.png" width="100%" />

### District level

``` r
ggplot(tr_ilce) + 
  geom_sf()
```

<img src="man/img/usage-ex5-1.png" width="100%" />

## Merging with your data set

``` r
# load example data set 
data("trdata2015")
# select happiness level and province key
tr_happiness <- trdata2015 %>% 
  select(province, happiness_level)
# merge with geo-spatial data 
tr_happiness2 <- left_join(tr_nuts3, tr_happiness, by = c("name_tr" = "province"))
ggplot(tr_happiness2) + 
  geom_sf(aes(fill = happiness_level))
```

<img src="man/img/usage-ex6-1.png" width="100%" />

``` r
# fixed and interactive maps using {tmap} package
library(tmap)
tmap_mode("plot")
#> tmap mode set to plotting
tm_shape(tr_happiness2) +
  tm_polygons("happiness_level") +
  tm_layout(legend.outside = TRUE)
```

<img src="man/img/usage-unnamed-chunk-5-1.png" width="100%" />

``` r
# uncomment for interactive map
# tmap_mode("view")
# tm_shape(tr_happiness2) +
#  tm_polygons("happiness_level")
```
