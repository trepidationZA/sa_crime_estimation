# sa_crime_estimation
Estimating crime density in South Africa using SAPS data and 2011 Census data from StatsSA. 
This is for interest's sake. Please do not use this information to make decisions - it could be incomplete, inaccurate, and/or invalid. 

# Crime Density Estimation in South Africa

We aim to estimate crime density in South Africa and showcase it on a per capita basis. The data to do this quite disagregated and not in a format that makes it easy to compare places in the country. We thought taking the actual crimes and dividing it by the number of people in that police precinct could give us a more accurate visualisation of where crime in the country is.  

To compile these estimates we used publicly downloadable data generated by Stats SA (Statistics South Africa) and SAPS (South African Police Service). For demographic data, we utilized the 2011 census numbers and spatial maps (we will update for the 2022 census). For the crime data - we used SAPS reported crime for the past 10 years and aggregated it into different time buckets (1 year, 2 years, 5 years, and 10 years) to observe trends. 

The crime data refers to all:
- Reported crime that was perpetrated within the borders of South Africa. 
- Reported at any police station in the country in any of the 1 144 police stations (although we counted 1 158).
- Reported by anyone, whenever.
- Are in the CAS/ICDMS system (Case Docket Management System).
- Full methodology for compiling crime statistics: https://static.pmg.org.za/170531SAPS.pdf

## Data Sources

1. **Stats SA Data**:
   - This data can be sourced here: https://www.datafirst.uct.ac.za/dataportal/index.php/catalog/485
   - We obtained the ".dta" files from UCT's Datafirst repository which included information for 84,907 small areas, 21,589 unique sub-places, 12,299 unique main places, 232 municipalities, 52 districts, and 9 provinces. We mainly used the population, gender, and age stats.

2. **SAPS Crime Stats**:
   - This data can be sourced here: https://www.saps.gov.za/services/crimestats.php
   - We obtained crime statistics from the SAPS website in the form of the "Crime-Statistics-2021_2022-latest.xlsx" file. This file is password protected and data can't be easily extracted out. 
   - Using VBA code, we extracted the crime data per "police cluster" (the reporting area/policing boundry around a police station), resulting in a total of 1,158 police stations with 31 crime categories each, leading to 35,898 records per year over 10 years.
   - We noted that the SAPS database listed "LAERSDRIFT" twice: one in Mpumalanga and another in Limpopo. Since the Limpopo entry had all crime entries as zero and no related data, we assumed it was included in error and excluded it - there doesn't appear to be a Laersdrift police station in Limpopo (https://www.saps.gov.za/contacts/provdetails.php?pid=2).


3. **Spatial Data**:
   - SAPS spatial data can be found here: https://www.saps.gov.za/services/boundary.php and
   - Demographic spatial data can be found here: https://www.datafirst.uct.ac.za/dataportal/index.php/catalog/485
   - We downloaded the shape files of boundaries at a small place level from Stats SA and the station boundary and points shape file from SAPS (the station boundry and station points have to be downloaded seperately, the feature to download both at once doesn't work).
   - Geometry issues in the shape files (specifically the SAL_SA_2011 shape file) were fixed using GQIS.
   - Where the StatsSA small place boundry overlapped with the station boundries we recorded an intersection relationship. There is a lot of uncertainty in this process because:
      - some small areas sit in between multiple station boundries but we allocated one station per small area (the max is 1 small area sitting in 10 police boundries)
      - the SAL_SA_2011 shape file had some very big small areas (i.e. the ones that sit in 10 police boundries) either representing the actual state, errors in the original shape file, errors in our intersection file, or errors in the allocation process.
   - An uncertainty level was assigned based on the relationship between the small place and the number of intersecting police stations. An uncertainty rating of 0 indicates confirmation, while 1:1 and 1:10 ratios result in uncertainty ratings of 1 and 10, respectively.
   - Some police stations in the SAPS Station Boundary data had no related stats, so we ignored them.

5. **Demographic Data**:
   - Police clusters were assigned to demographic data, and the population per police cluster was calculated. This population figure allows us to estimate the rate of crime per 20,000 people, as it is similar to the estimated population in the last place where crime was experienced.

6. **Open Street Map (OSM) Data**:
   - Open Street Map places were downloaded and joined by location to our crime data.
   - Certain locations were not allocated a place on OSM, but this does not pose a problem for this project. A list of all coordinates for these places is available, and updating OSM data is planned for a future project.

## Map Layers

The map generated by this project consists of four layers:

1. **Base Layer**:
   - This layer showcases the names of towns and the geographical structure of South Africa.

2. **Police Bounds**:
   - This layer displays crimes in certain police clusters, allowing us to overlay the Stats SA data and estimate crime density or crimes per 20,000 people.

3. **Police Points**:
   - These are geographical locations of all police stations. They provide access to actual crime statistics for each police station.

4. **Small Area Bounds**:
   - These boundaries represent small areas as defined by Stats SA and are the smallest geographical units used to split communities in South Africa. We utilize this layer to display population, gender, and age data.

## Limitations and Disclaimer

Please note the following limitations and disclaimer regarding this data:

1. **Data for Decision Making**: This data should not be used for decision-making purposes as it may contain errors or inaccuracies.

2. **Estimate Accuracy**: The demographic data used for population allocation is an estimate, and there is no guarantee of complete accuracy or validity in this allocation process.

3. **Spatial Data Issues**: While we attempted to resolve geometry issues, there may still be potential discrepancies or uncertainties in the spatial data used.

4. **Missing Data**: Certain locations were not allocated a place on Open Street Map, and their data might not be available for analysis.

5. **Outdated Data**: The crime data and demographic data used in this project might become outdated over time.

## Conclusion

This project presents an estimation of crime density in South Africa using a combination of publicly available data from Stats SA and SAPS. The generated map showcases per capita crime data at different time intervals and can provide valuable insights into crime trends. However, it is essential to understand the limitations of the data and exercise caution when using it for any decision-making purposes. Future updates to demographic data and spatial data may improve the accuracy and reliability of crime density estimations in South Africa.
