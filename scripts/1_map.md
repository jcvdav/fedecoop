Map of nine TURFs
================
Juan Carlos Villaseñr-Derbez

# Intro

The Federation of Cooperatives (FEDECOOP) is a group of 9 fishing
cooperatives that. This Rmd file creates a map of these TURFS

# Coding

## Set up

### Load libraries

### Load data

Let’s load the data that contain the shapefiles for the 9 TURF polygons,
and transform it to longlat
    coordinates.

    ## Reading layer `fedecoop_polygons' from data source `/Users/juancarlosvillasenorderbez/GitHub/fedecoop/raw_data/spatial/fedecoop_polygons.gpkg' using driver `GPKG'
    ## Simple feature collection with 9 features and 9 fields
    ## geometry type:  POLYGON
    ## dimension:      XY
    ## bbox:           xmin: 614359.1 ymin: 2896921 xmax: 873150.6 ymax: 3172240
    ## CRS:            32611

Let’s briefly visualize this data. It contains 9 features (TURFs) and 9
fields (columns). The columns
are:

| objectid | fid\_fedeco | coop                      |        ha | fid\_cedros | id | et\_id | shape\_leng | shape\_area |
| -------: | ----------: | :------------------------ | --------: | ----------: | -: | :----- | ----------: | ----------: |
|        1 |           1 | Bahia de Tortugas         |  48927.53 |         \-1 |  0 | NA     |    135346.8 |   489275313 |
|        2 |           2 | Buzos y Pescadores        |  82244.71 |         \-1 |  0 | NA     |    120709.6 |   822447132 |
|        3 |           3 | California de San Ignacio |  53313.56 |         \-1 |  0 | NA     |    122100.2 |   533135567 |
|        4 |           4 | Emancipacion              | 115689.08 |         \-1 |  0 | NA     |    176635.2 |  1156890844 |
|        5 |           5 | La Purisima               | 246787.77 |         \-1 |  0 | NA     |    230779.0 |  2467877738 |
|        6 |           6 | Leyes de Reforma          | 104477.07 |         \-1 |  0 | NA     |    143783.9 |  1044770741 |
|        7 |           7 | Progreso                  |  77856.35 |         \-1 |  0 | NA     |    138268.8 |   778563505 |
|        8 |           8 | Punta Abreojos            | 312745.47 |         \-1 |  0 | NA     |    226242.2 |  3127454736 |
|        9 |         \-1 | Isla Cedros               | 532470.33 |           1 |  0 | 0      |    293701.3 |  5324703275 |

We see that most columns are not very important for us. But the total
area might come in handy, so we’ll leave that there. Now, let’s rapidly
visualize them, using area to color them.

![](1_map_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

# Map

The first step is to build a publication-quality map. Something that
will probably end up as a supplementary figure.

First, we need the coastline for Mexico and Baja. Let’s get both from
the `rnaturalearth` package.

Now let’s create a map of Mexico to use as reference map.

![](1_map_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

Now let’s create a map of the actual cooperatives. We might want to
include their names and locations too. So lets create a shapefile of
their sentroids
first.

![](1_map_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->![](1_map_files/figure-gfm/unnamed-chunk-9-2.png)<!-- -->

Now, lets combine them into a finalized plot that we can export.

![TURFs for nine fishing cooperatives belonging to the federation of
cooepratives (FEDECOOP) in Baja California and Baja California Sur,
Mexico. Each red dot indicates the centroid of each TURF used to
calculate pairwise distances between TURFs. The lower left corner shows
an insert of Mexico, with a red box showing the general location of
these TURFs.](1_map_files/figure-gfm/unnamed-chunk-10-1.png)

![](1_map_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

Export the figure
