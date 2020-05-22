Abalone Fishery
================
Juan Carlos Villaseñr-Derbez

# Intro

This documents uses landings data from CONAPESCA (2000 - 2014). I will
create a monthly time series of landings and values for each
cooperative.

## Set up

### Load packages

### Load data

    ## Reading layer `fedecoop_polygons' from data source `/Users/juancarlosvillasenorderbez/GitHub/fedecoop/raw_data/spatial/fedecoop_polygons.gpkg' using driver `GPKG'
    ## Simple feature collection with 9 features and 9 fields
    ## geometry type:  POLYGON
    ## dimension:      XY
    ## bbox:           xmin: 614359.1 ymin: 2896921 xmax: 873150.6 ymax: 3172240
    ## CRS:            32611

## Prepare data

Thes are some very messy data. We’ll first have to filter it to keep
only the nince cooperatives that are part of FEDECOOP. Then, we’ll keep
only data on abalone, and then filter again to keep only data on abalone
where weight is reported without accounting for abalone shell. Then I
will modify the coperative names so that they match the spatial dataset.

Once all the filters and modifications are ready, I’ll then group data
at the coop-year level and create a time series of landings. Landings
data will be reported in metric tones. The resulting table contins the
total annual landings at the coop level. A preview of this panel is
this:

| year | coop                      | landed\_weight |
| ---: | :------------------------ | -------------: |
| 2000 | Bahia de Tortugas         |       64.22668 |
| 2000 | Buzos y Pescadores        |       63.16996 |
| 2000 | California de San Ignacio |       46.96692 |
| 2000 | Emancipacion              |        9.32246 |
| 2000 | Isla Cedros               |       15.55568 |
| 2000 | Leyes de Reforma          |        9.52238 |
| 2000 | Progreso                  |       41.45722 |
| 2000 | Punta Abreojos            |       53.30962 |
| 2001 | Bahia de Tortugas         |       66.67808 |
| 2001 | Buzos y Pescadores        |       68.71060 |
| 2001 | California de San Ignacio |       39.98638 |
| 2001 | Emancipacion              |       14.28000 |
| 2001 | Isla Cedros               |       28.44338 |
| 2001 | Leyes de Reforma          |       13.05668 |
| 2001 | Progreso                  |       38.31086 |
| 2001 | Punta Abreojos            |       49.98000 |
| 2002 | Bahia de Tortugas         |       70.43848 |
| 2002 | Buzos y Pescadores        |       65.16440 |
| 2002 | California de San Ignacio |       39.98400 |
| 2002 | Emancipacion              |       20.45848 |
| 2002 | Isla Cedros               |       25.90154 |
| 2002 | Leyes de Reforma          |       12.81154 |
| 2002 | Progreso                  |       37.62066 |
| 2002 | Punta Abreojos            |       55.91572 |

Landings (kg), revenue (MXP), and prices (MXP / Kg) for the abalone
fishery in 9 cooperatives belonging to FEDECOOP.

# Tables and Figures

## Total landings

Let’s create a table that shows the total landings for each cooperative
on any given year. We’ll also add row total and column
totals.

| year  | Bahia de Tortugas | Buzos y Pescadores | California de San Ignacio | Emancipacion | Isla Cedros | La Purisima | Leyes de Reforma |  Progreso | Punta Abreojos |     Total |
| :---- | ----------------: | -----------------: | ------------------------: | -----------: | ----------: | ----------: | ---------------: | --------: | -------------: | --------: |
| 2000  |          64.22668 |           63.16996 |                  46.96692 |      9.32246 |    15.55568 |          NA |          9.52238 |  41.45722 |       53.30962 |  303.5309 |
| 2001  |          66.67808 |           68.71060 |                  39.98638 |     14.28000 |    28.44338 |          NA |         13.05668 |  38.31086 |       49.98000 |  319.4460 |
| 2002  |          70.43848 |           65.16440 |                  39.98400 |     20.45848 |    25.90154 |          NA |         12.81154 |  37.62066 |       55.91572 |  328.2948 |
| 2003  |          83.55466 |           63.34608 |                  39.97448 |     28.55048 |    28.16492 |          NA |         14.86072 |  42.84714 |       61.28500 |  362.5835 |
| 2004  |          83.32142 |           63.14854 |                  34.86224 |     36.64962 |    28.95032 |          NA |         17.83096 |  39.70316 |       62.42026 |  366.8865 |
| 2005  |          89.64746 |           67.65150 |                  46.38144 |     31.10660 |    33.32000 |          NA |         23.30020 |  28.60998 |       75.20800 |  395.2252 |
| 2006  |          91.61572 |           66.55194 |                  47.08354 |     32.07288 |    38.06572 |          NA |         29.79760 |  32.61076 |       79.73000 |  417.5282 |
| 2007  |          94.01952 |           66.89704 |                  47.99984 |     36.90904 |    39.22240 |     54.4230 |         28.53144 |  38.35132 |       90.44000 |  496.7936 |
| 2008  |          97.39436 |           67.91092 |                  48.03554 |     36.88524 |    39.15100 |          NA |         28.56000 |  38.07048 |       90.44000 |  446.4475 |
| 2009  |          78.45908 |           11.92618 |                  50.89868 |     39.21288 |    41.43104 |          NA |         12.98290 |  44.75828 |       91.39438 |  371.0634 |
| 2010  |          56.87962 |           12.49976 |                  56.19418 |     35.39536 |    34.00306 |      4.0160 |          2.00872 |  31.63734 |       90.09252 |  322.7266 |
| 2011  |          42.67578 |            4.91232 |                  60.42344 |     41.53100 |    24.61158 |          NA |          0.43316 |  33.19386 |       83.30000 |  291.0811 |
| 2012  |          32.58696 |                 NA |                  62.93672 |     44.06332 |    26.11336 |          NA |               NA |  35.71428 |       92.82476 |  294.2394 |
| 2013  |           6.24274 |                 NA |                  68.96288 |     47.48100 |    27.29384 |      3.4391 |               NA |  40.05778 |      100.73826 |  294.2156 |
| 2014  |                NA |                 NA |                  42.74956 |     25.94200 |    29.17880 |          NA |               NA |  42.31640 |       96.48758 |  236.6743 |
| Total |         957.74056 |          621.88924 |                 733.43984 |    479.86036 |   459.40664 |     61.8781 |        193.69630 | 565.25952 |     1173.56610 | 5246.7367 |

Total landings (Kg) for each cooperative through time. NAs represent
missing data for a given year / cooperative combination.

The table above is shown in the figures below. The first figure shows
the timseries of landed abalone for each cooperative. The second figure
shows the aggregate abalone landings for all FEDECOOP cooperatives.

![](3_abalone_fishery_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

![](3_abalone_fishery_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

## Mean measures

The tables and figures above show the totals for each year and across
all cooperatives. Let’s create one where we show the mean annual
landings and revenue for each cooperative, and then one where we look at
the grand mean.

| coop                      |     mean |        sd |
| :------------------------ | -------: | --------: |
| Bahia de Tortugas         | 68.41004 | 26.320573 |
| Buzos y Pescadores        | 51.82410 | 25.486544 |
| California de San Ignacio | 48.89599 |  9.554259 |
| Emancipacion              | 31.99069 | 10.757187 |
| Isla Cedros               | 30.62711 |  6.918571 |
| La Purisima               | 20.62603 | 29.270453 |
| Leyes de Reforma          | 16.14136 |  9.866574 |
| Progreso                  | 37.68397 |  4.565258 |
| Punta Abreojos            | 78.23774 | 17.230245 |

Annual Mean and SD landed weigh for each cooperative.

![](3_abalone_fishery_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

![](3_abalone_fishery_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

![](3_abalone_fishery_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

Based on the 2000 - 2014 data, a cooperative’s mean catch is 45.2304884
\(\pm\) 24.7055996 tones (mean \(\pm\) SD). Total annual extraction to
be 349.782444 \(\pm\) 68.2356602 (mean \(\pm\) SD). In FEDECOOPs website
(<https://www.fedecoop.com.mx/>), they report that current total abalone
extraction averages 290 metric tonnes for all cooperatives together.
This value is closer to the more recent value in these data.

However, this analyses des not accout for the temporal trend in which
cooperatives have been forced to reduce fishing effort due to the
diwndling status of stocks. I
