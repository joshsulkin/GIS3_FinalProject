# The 2022, 2017, and 2012 French Presidential Elections

Want to better understand the 2022 French Presidential Election? How about putting in context with other French presidential elections? Here is how I created some maps to address these questions!

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

# 
