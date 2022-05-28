# GIS3_FinalProject

Want to better understand the 2022 French Presidential Election? How about putting in context with other French presidential elections? Here is how I created some maps to address these questions!

# PART 1: 2022 PRESIDENTIAL ELECTION (MACRON V. LE PEN)

# Extract

## Download data from the French Government's website

I downloaded the Excel file entitled "resultats-par-niveau-dpt-t2-france-entiere.xlsx" from the French Government's [data website]([url](https://www.data.gouv.fr/fr/datasets/election-presidentielle-des-10-et-24-avril-2022-resultats-definitifs-du-2nd-tour/#resources)).

## Download boundary data from 

I downloaded the boundary data matching the level of scale of the French Government's data (the _département_) from the [European Environmental Agency]([url](https://www.eea.europa.eu/data-and-maps/data/external/france-administrative-boundaries)).

## Merge government and boundary data

While the French government provided data for the election results by département, it only included the data as an excel file without any boundary data. I therefore joined the CSV data to boundary data I found elsewhere on the French government's website and performed a spatial join in QGIS before importing into R. I ensured that there was a join key, "Departement" and "Name_2".

