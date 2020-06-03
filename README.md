# FEDECOOP maps, distances, and abalone model


## Repository structure 

```
-- fedecoop.Rproj
-- raw_data
   |__conapesca.rds
   |__spatial
-- README.md
-- renv
-- renv.lock
   |__activate.R
   |__library
-- results
   |__distance_matrix.csv
   |__distance_table.csv
   |__img
-- scripts
   |__1_map_files
   |__1_map.md
   |__1_map.Rmd
   |__2_distances_files
   |__2_distances.md
   |__2_distances.Rmd
   |__3_abalone_fishery_files
   |__3_abalone_fishery.md
   |__3_abalone_fishery.Rmd
   |__4_abalone_model_files
   |__4_abalone_model.md
   |__4_abalone_model.Rmd
```

## Requested things

All the results, including pdf versions of the figures, can be found in the [results](results) folder.

### Map of the Cooperatives

To see more maps, go to [here](scripts/1_map.md).

![](results/img/fedecoop_map.png)

### Distance between cooperatives

![](results/img/distance_heatmap.png)

### Fishery data

An overview of landings data from all FEDECOOP cooperatives can be found [here](scripts/3_abalone_fishery.md). Their [website](https://www.fedecoop.com.mx/) states that aggregate landed green abalone (*H. fulgens*) meat is about 200 tones per year.

The figure below shows aggregate landings of *H. fulgens* for nine cooperative sin FEDECOOP. The solid hosiontal line shows the long-term mean, dashed lines represent $\pm$ 1 SD.

![](results/img/landings_timeseries.png)

### Biology of abalone

A full description of an abalone model following Rossetto et al 2015 can be found [here](scripts/4_abalone_model.md).

Recoding the model and running it 1000 times gives the following intrinsic growth rate.

![](results/img/r_dens.png)

### Relevant references

= Rossetto, M., Micheli, F., Saenz-Arroyo, A., Montes, J. A. E., & De Leo, G. A. (2015). No-take marine reserves can enhance population persistence and support the fishery of abalone. Canadian Journal of Fisheries and Aquatic Sciences, 72(10), 1503-1517.

--------- 

<a href="https://orcid.org/0000-0003-1245-589X" target="orcid.widget" rel="noopener noreferrer" style="vertical-align:top;"><img src="https://orcid.org/sites/default/files/images/orcid_16x16.png" style="width:1em;margin-right:.5em;" alt="ORCID iD icon">orcid.org/0000-0003-1245-589X</a>
