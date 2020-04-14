Distance between TURFs
================
Juan Carlos Villaseñr-Derbez

# Intro

The previous Rmd file created a figure to show the location of nine
TURFs that are part of FEDECOOP. This Rmd document will now calculate
the pairwise distances between all TURFs.

## Set up

### Load packages

``` r
suppressPackageStartupMessages({
  library(here)
  library(sf)
  library(SpatialPosition)
  library(tidyverse)
})
```

### Load data

``` r
turfs <- st_read(here("raw_data", "spatial",  "feddecoop_polygons.gpkg")) %>% 
  st_centroid()
```

    ## Reading layer `feddecoop_polygons' from data source `/Users/juancarlosvillasenorderbez/GitHub/fedecoop/raw_data/spatial/feddecoop_polygons.gpkg' using driver `GPKG'
    ## Simple feature collection with 9 features and 9 fields
    ## geometry type:  POLYGON
    ## dimension:      XY
    ## bbox:           xmin: 614359.1 ymin: 2896921 xmax: 873150.6 ymax: 3172240
    ## projected CRS:  WGS 84 / UTM zone 11N

# Calculations

## Calculate distance between TURF centroids

We can now calculate the distance between all combinations of TURFs.
This will produce a 9 X 9 matrix, with a diagonal with value so 0.

``` r
# Assign rownames based on TURF name
rownames(turfs) <- turfs$Coop

# Calculate distance matrix (results are in meters)
matrix <- CreateDistMatrix(knownpts = turfs, unknownpts = turfs)
```

``` r
knitr::kable(matrix)
```

|                           | Bahia de Tortugas | Buzos y Pescadores | California de San Ignacio | Emancipacion | La Purisima | Leyes de Reforma |  Progreso | Punta Abreojos | Isla Cedros |
| ------------------------- | ----------------: | -----------------: | ------------------------: | -----------: | ----------: | ---------------: | --------: | -------------: | ----------: |
| Bahia de Tortugas         |              0.00 |           34366.63 |                  97157.96 |     48201.30 |    42888.52 |        129074.32 | 158701.67 |      200703.34 |    72445.78 |
| Buzos y Pescadores        |          34366.63 |               0.00 |                 131265.32 |     82266.22 |    57564.31 |        163299.56 | 192979.38 |      234954.69 |    45483.91 |
| California de San Ignacio |          97157.96 |          131265.32 |                      0.00 |     49001.18 |   107889.43 |         32242.81 |  62048.39 |      103822.62 |   166909.08 |
| Emancipacion              |          48201.30 |           82266.22 |                  49001.18 |         0.00 |    66842.22 |         81068.72 | 110806.41 |      152729.93 |   119071.65 |
| La Purisima               |          42888.52 |           57564.31 |                 107889.43 |     66842.22 |        0.00 |        136113.19 | 163552.45 |      204773.08 |    68144.50 |
| Leyes de Reforma          |         129074.32 |          163299.56 |                  32242.81 |     81068.72 |   136113.19 |             0.00 |  29805.61 |       71661.65 |   197790.81 |
| Progreso                  |         158701.67 |          192979.38 |                  62048.39 |    110806.41 |   163552.45 |         29805.61 |      0.00 |       42034.51 |   226689.58 |
| Punta Abreojos            |         200703.34 |          234954.69 |                 103822.62 |    152729.93 |   204773.08 |         71661.65 |  42034.51 |           0.00 |   268588.15 |
| Isla Cedros               |          72445.78 |           45483.91 |                 166909.08 |    119071.65 |    68144.50 |        197790.81 | 226689.58 |      268588.15 |        0.00 |

Now, lets convert the matrix into a table just in case this is useful.

``` r
distance_as_table <- matrix %>%
  as_tibble() %>% 
  mutate(from = colnames(.)) %>% 
  gather(to, value, -from) %>% 
  mutate(value = value)
```

Now let’s visualize and export this
matrix.

``` r
ggplot(data = distance_as_table, aes(x = from, y = to, fill = value / 1e3)) +
  geom_tile() +
  coord_equal() +
  theme(axis.text.x = element_text(angle = 90, hjust = 1)) +
  guides(fill = guide_colorbar(ticks.colour = "black", frame.colour = "black")) + 
  labs(x = "", y = "") +
  scale_fill_viridis_c(name = "Distance (km)")
```

![](2_distances_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

We can now export both, the matrix and table.

``` r
write.csv(x = matrix,
          file = here::here("results", "distance_matrix.csv"))

write.csv(x = distance_as_table,
          file = here::here("results", "distance_table.csv"),
          row.names = F)
```
