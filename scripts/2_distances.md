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
  library(startR)
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
matrix <- round(
  CreateDistMatrix(knownpts = turfs, unknownpts = turfs) / 1e3, # Divide by a thousand to convert to km
  2)                                                            # Keep only two decimals
```

``` r
knitr::kable(matrix)
```

|                           | Bahia de Tortugas | Buzos y Pescadores | California de San Ignacio | Emancipacion | La Purisima | Leyes de Reforma | Progreso | Punta Abreojos | Isla Cedros |
| ------------------------- | ----------------: | -----------------: | ------------------------: | -----------: | ----------: | ---------------: | -------: | -------------: | ----------: |
| Bahia de Tortugas         |              0.00 |              34.37 |                     97.16 |        48.20 |       42.89 |           129.07 |   158.70 |         200.70 |       72.45 |
| Buzos y Pescadores        |             34.37 |               0.00 |                    131.27 |        82.27 |       57.56 |           163.30 |   192.98 |         234.95 |       45.48 |
| California de San Ignacio |             97.16 |             131.27 |                      0.00 |        49.00 |      107.89 |            32.24 |    62.05 |         103.82 |      166.91 |
| Emancipacion              |             48.20 |              82.27 |                     49.00 |         0.00 |       66.84 |            81.07 |   110.81 |         152.73 |      119.07 |
| La Purisima               |             42.89 |              57.56 |                    107.89 |        66.84 |        0.00 |           136.11 |   163.55 |         204.77 |       68.14 |
| Leyes de Reforma          |            129.07 |             163.30 |                     32.24 |        81.07 |      136.11 |             0.00 |    29.81 |          71.66 |      197.79 |
| Progreso                  |            158.70 |             192.98 |                     62.05 |       110.81 |      163.55 |            29.81 |     0.00 |          42.03 |      226.69 |
| Punta Abreojos            |            200.70 |             234.95 |                    103.82 |       152.73 |      204.77 |            71.66 |    42.03 |           0.00 |      268.59 |
| Isla Cedros               |             72.45 |              45.48 |                    166.91 |       119.07 |       68.14 |           197.79 |   226.69 |         268.59 |        0.00 |

Now, lets convert the matrix into a table just in case this is useful.

``` r
distance_as_table <- matrix %>%
  as_tibble() %>% 
  mutate(from = colnames(.)) %>% 
  gather(to, value, -from) %>% 
  mutate(value = value)
```

Now let’s visualize and export this matrix.

``` r
heatmap <- ggplot(data = distance_as_table,
       mapping = aes(x = from, y = to, fill = value)) +
  geom_tile(color = "black", size = 0.5) +
  coord_equal() +
  ggtheme_plot() +
  theme(axis.text.x = element_text(angle = 90, hjust = 1)) +
  guides(fill = guide_colorbar(ticks.colour = "black",
                               frame.colour = "black")) + 
  labs(x = "", y = "") +
  scale_fill_viridis_c(name = "Distance (km)") +
  scale_x_discrete(expand = c(0, 0)) +
  scale_y_discrete(expand = c(0, 0))

heatmap
```

![](2_distances_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

We can now export both, the matrix and table, as well as the figure.

``` r
write.csv(x = matrix,
          file = here::here("results", "distance_matrix.csv"))

write.csv(x = distance_as_table,
          file = here::here("results", "distance_table.csv"),
          row.names = F)

lazy_ggsave(plot = heatmap,
            filename = "distance_heatmap",
            width = 17,
            height = 17)
```
