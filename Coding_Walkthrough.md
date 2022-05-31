# Coding Walkthrough

This document explores my process for creating the maps for this project.

# PART 1: 2022 PRESIDENTIAL ELECTION (MACRON V. LE PEN)

# MapBox Interactive Map

## Download data from the French Government's website

I downloaded the Excel file entitled "resultats-par-niveau-dpt-t2-france-entiere.xlsx" from the French Government's data website at https://www.data.gouv.fr/fr/datasets/election-presidentielle-des-10-et-24-avril-2022-resultats-definitifs-du-2nd-tour/#resources

## Download boundary data from the European Environmental Agency

I downloaded the boundary data matching the level of scale of the French Government's data (the _département_) from the European Environmental Agency at https://www.eea.europa.eu/data-and-maps/data/external/france-administrative-boundaries

## Merge government and boundary data

While the French government provided data for the election results by département, it only included the data as an excel file without any boundary data. I therefore joined the CSV data to boundary data I found elsewhere on the French government's website and performed a spatial join in QGIS before importing into R. I ensured that there was a join key, "Departement" and "Name_2".

![Screen Shot](https://user-images.githubusercontent.com/104933711/170835582-a49c0781-05ba-4150-80e3-6a10e417cb56.png)

## Import to MapBox

I then exported the merged data file as a GeoJSON to import it into MapBox, changing the CRS to 4326 instead of the CRS 2154 that minimizes the distortion for France and its colonies. After visualizing the data in MapBox, I made the red signify only the data up to 50% support for Macron, followed by white and then blue, indicating much stronger support for Macron.

![MapBox](https://user-images.githubusercontent.com/104933711/170836397-3751875e-ba0a-449f-a564-2c8f2c0cd17b.png)

## Final Map

My final map for the 2022 French Presidential Election on MapBox can be found here: https://api.mapbox.com/styles/v1/jls2023/cl3q3154s002215qy6vopkvlv.html?title=view&access_token=pk.eyJ1IjoiamxzMjAyMyIsImEiOiJjbDM2Y2k5YmMwYjhqM2pudmNhd3ZqNnIzIn0.Wg-1UgUi2U3QuSPMN1-BUQ&zoomwheel=true&fresh=true#6.54/47.959/3.026

# Population-Weighted Cartogram

Importantly, the French Presidential election does not depend on departments or any other geographic element to help determine the relative voting power of one constituency in France over another. In other words, _départements_ are not like U.S. states with an equivalent to the Electoral College. French voters are weighted equally. However, in the interest of providing context for just how many people live in each of the departments that I used in the MapBox maps, I sought to create a population-weighted cartogram for each presidential election year.

## Data Source

It should be noted that the 2022 population data are estimates from https://www.insee.fr/fr/statistiques/1893198. All of the data besides the 2022 data are directly from the French Government, as it notes on the website.

## How I created it

I merged the population data with my CSV file containing the _département_ boundaries in QGIS, exported as a GeoJSON, imported into R using the st_read function, and then viewed the data to see the column headings.

```{r}
Election2022pop <- st_read("/Users/jls/GEOG28602/Final\ Project/new2022macronNpop.geojson")
head(Election2022pop)
```

Seeing that the data was not in a uniform CRS and that R did not allow for the CRS to be 4326, I changed the CRS to EPSG:2154 because it is optimized for France and its colonies.

```{r}
FrenchDepts <- st_transform(Election2022pop, 2154)
```

Then, after hours of struggling, I made a contiguous cartogram for French _départements_ using the latest and greatest population estimates for 2022.

```{r}
Cartogram2022 <- cartogram_cont(FrenchDepts, "Population", itermax=5)
tm_shape(Cartogram2022) + tm_polygons("Population") + 
  tm_compass(type = "arrow", position = c("left", "top")) +
  tm_scale_bar(breaks = c(0, 100, 200), text.size = 1, position = "left") +
  tm_layout(frame = FALSE, 
            legend.outside = TRUE,
            legend.outside.position = 'right', 
            legend.title.size = 0.9,
            main.title = '2022 Cartogram of Population by Department, by Josh Sulkin',
            main.title.size = 0.9,
            aes.palette = list(seq = "-plasma"))
```

## Final Map

![Screen Shot 2022-05-30 at 12 08 11 AM](https://user-images.githubusercontent.com/104933711/170920846-4c615554-f710-4311-8042-970c0e38a8f1.png)

# PART 2: 2017 PRESIDENTIAL ELECTION (MACRON V. LE PEN)

# MapBox Interactive Map

## Data source

I also got the data from the 2017 French Presidential Election from the French Government's website: https://www.data.gouv.fr/fr/datasets/election-presidentielle-des-23-avril-et-7-mai-2017-resultats-definitifs-du-2nd-tour/#resources

## How I made the map

I performed the same steps as in Part 1, including joining the boundaries and excel files, importing to Mapbox, and so on. However, I simply made a copy of the first MapBox map and replaced the data. While I had to adjust the choropleth settings again, it was much easier this time since I had already decided on the colors scheme I liked, and I wanted to ensure that the maps were the same stylistically to make comparisons between the maps easier.

![MapBox P2](https://user-images.githubusercontent.com/104933711/170837943-63381a69-f7a6-48c3-b600-7f409878100e.png)

## Final map

My final map for the 2017 Presidential Election on MapBox can be found here: https://api.mapbox.com/styles/v1/jls2023/cl3q6jlb2000n15l30cp8x5uo.html?title=view&access_token=pk.eyJ1IjoiamxzMjAyMyIsImEiOiJjbDM2Y2k5YmMwYjhqM2pudmNhd3ZqNnIzIn0.Wg-1UgUi2U3QuSPMN1-BUQ&zoomwheel=true&fresh=true#6.54/47.959/3.026

# Population-Weighted Cartogram

## Data Source

I got the data from https://www.insee.fr/fr/statistiques/1893198, which are directly from the French Government, as it notes on the website.

## How I created it

I merged the population data with my CSV file containing the _département_ boundaries in QGIS, exported as a GeoJSON, imported into R using the st_read function, and then viewed the data to see the column headings.

```{r}
Election2017pop <- st_read("/Users/jls/GEOG28602/Final\ Project/2017macronNpop.geojson")
head(Election2017pop)
```

Then, I made a contiguous cartogram for French _départements_ using the data for 2017.

```{r}
names(Election2017pop)[13] <- 'Population'
FrenchDepts2017 <- st_transform(Election2017pop, 2154)
Cartogram2017 <- cartogram_cont(FrenchDepts2017, "Population", itermax=5)
tm_shape(Cartogram2017) + tm_polygons("Population") + 
  tm_compass(type = "arrow", position = c("left", "top")) +
  tm_scale_bar(breaks = c(0, 100, 200), text.size = 1, position = "left") +
  tm_layout(frame = FALSE, 
            legend.outside = TRUE,
            legend.outside.position = 'right',
            legend.title.size = 0.9,
            main.title = '2017 Cartogram of Population by Department, by Josh Sulkin',
            main.title.size = 0.9,
            aes.palette = list(seq = "-plasma"))
```

## Final Map

![Screen Shot 2022-05-30 at 12 51 49 AM](https://user-images.githubusercontent.com/104933711/170925689-48eae465-5da0-4fc7-9383-6cebaafa74dd.png)

# PART 3: 2012 PRESIDENTIAL ELECTION (HOLLANE V. SARKOZY)

In 2012, François Hollande ran and won as the first Socialist president of France against incumbent President Nicolas Paul Stéphane Sarközy de Nagy-Bocsa, a member of the conservative party. I am including Hollande's victory to demonstrate how far to the right France has come since electing a socialist president in 2012.

# MapBox Interactive Map

## Data source

The 2012 data were extremely difficult to find. The French Government's data site said it provided the data, but in reality, it was not downloadable. I eventually found some data by department, but I had to manually input all of the data into a spreadsheet from this website (https://www.lexpress.fr/actualite/politique/elections/presidentielle-2012/resultats-elections/france.html). 

Since I do not want anyone to have to do that again, the data I used with the percentage of voters who cast a ballot for François Hollande can be downloaded here (https://uchicagoedu-my.sharepoint.com/:x:/g/personal/josh23_uchicago_edu/EVarHfFvm2dJiDrUyVvsBh8BbQsLngCN3j0-6MbAJDJrlA?e=5BDs38). 

## How I made the map

I performed the same steps as in Part 1, including joining the boundaries and excel files, importing to Mapbox, and so on. However, I simply made a copy of the first MapBox map and replaced the data. While I had to adjust the choropleth settings again, it was much easier this time since I had already decided on the colors scheme I liked, and I wanted to ensure that the maps were the same stylistically to make comparisons between the maps easier.

![MapBox 3](https://user-images.githubusercontent.com/104933711/170840700-88882354-610d-4562-b8c2-748af99a02ac.png)

## Final map

My final map for the 2012 Presidential Election on MapBox can be found here: https://api.mapbox.com/styles/v1/jls2023/cl3q74ldk005614rw3a5qmf4p.html?title=view&access_token=pk.eyJ1IjoiamxzMjAyMyIsImEiOiJjbDM2Y2k5YmMwYjhqM2pudmNhd3ZqNnIzIn0.Wg-1UgUi2U3QuSPMN1-BUQ&zoomwheel=true&fresh=true#6.01/46.907/3.349

# Population-Weighted Cartogram

## Data Source

I got the data from https://www.insee.fr/fr/statistiques/1893198, which are directly from the French Government, as it notes on the website.

## How I created it

I merged the population data with my CSV file containing the _département_ boundaries in QGIS, exported as a GeoJSON, imported into R using the st_read function, and then viewed the data to see the column headings.

```{r}
Election2012pop <- st_read("/Users/jls/GEOG28602/Final\ Project/2012macronNpop.geojson")
head(Election2012pop)
```

Then, I made a contiguous cartogram for French _départements_ using the data for 2012.

```{r}
names(Election2012pop)[13] <- 'Population'
FrenchDepts2012 <- st_transform(Election2012pop, 2154)
Cartogram2012 <- cartogram_cont(FrenchDepts2012, "Population", itermax=5)
tm_shape(Cartogram2012) + tm_polygons("Population") + 
  tm_compass(type = "arrow", position = c("left", "top")) +
  tm_scale_bar(breaks = c(0, 100, 200), text.size = 1, position = "left") +
  tm_layout(frame = FALSE, 
            legend.outside = TRUE,
            legend.outside.position = 'right',
            legend.title.size = 0.9,
            main.title = '2012 Cartogram of Population by Department, by Josh Sulkin',
            main.title.size = 0.9,
            aes.palette = list(seq = "-plasma"))
```

## Final Map

![Screen Shot 2022-05-30 at 1 04 22 AM](https://user-images.githubusercontent.com/104933711/170927226-74126e6f-0f92-4185-95f1-b001e8865a38.png)
