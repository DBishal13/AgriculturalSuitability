# Agricultural Land Suitability Analysis Based on Physical Factors: A Case Study of Makwanpur District, Nepal

## Purpose Statement
This study aims to analyze cropland's suitability in the Makwanpur district based on physical factors using different freely available raster and raster data, map algebra, and tools available on the ArcGIS desktop.

## Introduction
The Makwanpur district is located in the south Asian country, Nepal, at an elevation ranging between 166 m to 2586 m from MSL with latitude 27° 24' 59.99" N and longitude 85° 01' 60.00" E. It has a hilly landscape, with slightly more than half of the population practicing subsistence agriculture (Adhikari S. P. et al., 2018). Precipitation is typically unpredictable due to the high dependency on the weather (Regmi, H. R. 2007; Malla, G. 2008). Rainfed farming is problematic because of the variability in rainfall distribution, especially when cultivating crops with a fair market value, such as rice, maize, wheat, and vegetables (Gurung, G. B. et al., 2009). Located inside the Narayani zone, Makwanpur has many stream networks distributed all over the district, significantly contributing to irrigation. In Makawanpur district, cropland makes up 25.15 percent of the total area, rivers and lakes make up 6.83 percent, and pasture and bare ground make up 9 percent (DFO., 2002).

Many factors determine cropland suitability, including biophysical, socioeconomic, and ecological factors (Olaniyi A. et al., 2015). Considering only the physical characteristics, I have assessed the suitability of agricultural land based on slope, aspect, and proximity to stream networks. The slope is a crucial element in agriculture. The degree of slope directly affects various soil properties, including soil depth, susceptibility to erosion, soil tillage, irrigation, and plant adaption. The soil and water lost in a region are directly impacted by the slope's steepness, length, and sharpness (Everest T. et al., 2021). Aspect is another factor that determines the crop yield. Sunlight is necessary for plants to function physically and metabolically (Everest T. et al., 2021). Plants that get daylight begin to sprout, develop, and produce fruit (Bale et al., 1998; Yimer et al., 2006). Apart from slope and aspect, the distribution of the stream network around the study area also significantly influences agricultural productivity. Landscapes that are closer to the stream network have more access to irrigation. Also, if the elevation of the land surface is less than that of the water source, it is easier to transport water than if the height is more than that of the water source by using canals or pipes. However, all these factors do not always equally contribute to the suitability. Their weight on the final suitability raster also differs based on their significance. Public and expert opinions exist on how these weights are assigned to each parameter.

## Datasets Used:
The following datasets were used for this project:
- **Digital elevation data**: The 30m spatial resolution SRTM topography data was downloaded from the USGS EarthExplorer website (https://earthexplorer.usgs.gov/). The slope and aspect parameters were calculated from this data.
- **Nepal political boundary**: The feature was downloaded from the OpenStreetMap database using the QuickOSM plugin in QGIS.
- **Stream network shapefile**: The waterways feature was also downloaded from the OSM database using the QuickOSM plugin in QGIS.
- **Land Use Land Cover data**: For the land use data, the sentinel 2, 10 m land use land cover map was downloaded from Esri Atlas.

## Steps and Procedures
### 1. Data Preparation
- All the datasets were uploaded in ArcGIS Pro.
- The LULC data was resampled to 30m spatial resolution.
- All mandatory projections and transformations were performed to maintain a uniform coordinate system, datum, and projection of each data layer using geoprocessing tools such as Define Projection, Project, and Project Raster. I have used the projected coordinate system WGS 1984 UTM Zone 44N and Transverse Mercator Projection.
- Data were clipped using the extent of the Makwanpur district, which was exported from the political boundary shapefile of Nepal.

### 2. Calculation of Slope and Aspect
- Slope and aspect were calculated using Spatial Analyst tools. The highly suitable areas were given a new value of 4, with a slope under 2 degrees. Similarly, the areas were classified as moderately suitable (value 3) with a slope ranging from 2 to 6 degrees, followed by the marginally suitable areas (value 2) with a slope of 6 to 12 degrees. They remained classified as not suitable (value 1) areas. The output raster was saved as rec_slope.

| Slope Value      | Suitability         |
|------------------|---------------------|
| Less than 2 degrees | Highly Suitable (4) |
| 2 to 6 degrees      | Moderately Suitable (3) |
| 6 to 12 degrees     | Marginally Suitable (2) |
| More than 12 degrees| Not Suitable (1) |

- In the case of aspect, the east-facing areas and north-facing areas were considered the most suitable landscape. I reclassified the aspect based on the values 1 to 4 mean the same thing as in the slope case, i.e., 4 means highly suitable, and 1 means not suitable. The output raster was saved as rec_aspect.

| Aspect Value    | Suitability         |
|-----------------|---------------------|
| 45 to 135 degrees  | Highly Suitable (4) |
| 135 to 225 degrees | Moderately Suitable (3) |
| 225 to 315 degrees | Marginally Suitable (2) |
| 315 to 45 degrees  | Not Suitable (1)  |

### 3. Calculation of Distance Raster
A spatial analyst tool called Euclidean Distance calculates a distance raster based on the stream feature. For the environment settings, the coordinate system, snap raster, and output extent were used as the DEM. It was then classified into five classes: the most suitable landscape between 10m and 100m. A distance of less than 10m was excluded to remove the right-of-way area of the river. The final layer was saved as rec_dist_ras.

### 4. Calculation of Stream Elevation Raster
A stream raster was created using the feature to raster tool in ArcGIS using a cell size the same as that of the DEM. It was then reclassified into the values of 0 and 1, such that 1 represents the stream and all other parts as zero. The stream raster's elevation was calculated using the raster calculator.

Moving forward, the Focal statistics tool was used to calculate the maximum elevation of the stream raster. A rectangular 5x5 neighborhood was used, and the environments were set as that of the DEM to get the FocalStat raster.

The Raster calculator was used to find the areas above or below the stream. If the DEM is subtracted from the FocalStat raster, a value from -2600m to 150m was received, which was then reclassified based on the assumption that the land surface below the stream is more suitable for farming than that above the streams and saved as rec_strmdem.

### 6. Calculation of Suitability Raster using Linear Average Method
Finally, the weighted linear average of all the above calculated and reclassified raster was computed using the raster calculator tool. The slope was given the most weight based on the assumption that it impacts crop suitability the most. The least weight was assigned to aspect because of its distribution around the study area, which means it doesn’t have a more significant contribution. The distance to the stream and elevation from the stream were each given a weight of 0.2 such that the whole weight sums up to 1.

## Results and Discussions
If we consider each parameter separately, the following outputs are obtained from our calculations, and inferences are made accordingly.

### a. Suitability Based on Slope
| Slope Value        | % Of Land | Suitability         |
|--------------------|-----------|---------------------|
| Less than 2 degrees| 5.6 %     | Highly Suitable     |
| 2 to 6 degrees     | 9.7 %     | Moderately Suitable |
| 6 to 12 degrees    | 12.5 %    | Marginally Suitable |
| More than 12 degrees| 72.2 %   | Not Suitable        |

[![Slope](https://github.com/DBishal13/AgriculturalSuitability/blob/main/Outputs/Slope.jpg)](https://github.com/DBishal13/AgriculturalSuitability/blob/main/Outputs/Slope.jpg)
**Fig 1. Suitability Map based on Slope.**

### b. Suitability Based on Aspect
| Aspect Value         | % Of Land | Suitability         |
|----------------------|-----------|---------------------|
| 45 to 135 degrees    | 24.5 %    | Highly Suitable     |
| 135 to 225 degrees   | 24.7 %    | Moderately Suitable |
| 225 to 315 degrees   | 24.5 %    | Marginally Suitable |
| 315 to 45 degrees    | 26.3 %    | Not Suitable        |

[![Aspect](https://github.com/DBishal13/AgriculturalSuitability/blob/main/Outputs/Aspect.jpg)](https://github.com/DBishal13/AgriculturalSuitability/blob/main/Outputs/Aspect.jpg)
**Fig 2. Suitability Map based on Aspect.**

### c. Suitability Based on Distance from Stream
| Distance From Stream | % Of Land | Suitability         |
|----------------------|-----------|---------------------|
| Between 10m to 100m  | 16.3 %    | Highly Suitable     |
| Between 100m to 500m | 43.5 %    | Moderately Suitable |
| Between 500 to 1000m | 22.2 %    | Marginally Suitable |
| More than 1000m      | 18 %      | Not Suitable        |

[![Distance](https://github.com/DBishal13/AgriculturalSuitability/blob/main/Outputs/Distance.jpg)](https://github.com/DBishal13/AgriculturalSuitability/blob/main/Outputs/Distance.jpg)
**Fig 3. Suitability Map based on distance from the stream.**

### d. Suitability Based on Elevation from Stream
| Elevation from Stream | % Of Land | Suitability         |
|-----------------------|-----------|---------------------|
| More than 250m        | 18 %      | Not Suitable        |
| Up to 250m            | 22.5 %    | Marginally Suitable |
| Below 50m             | 28.4 %    | Moderately Suitable |
| Below 50m to 150m     | 31.1 %    | Highly Suitable     |

[![Elevation](https://github.com/DBishal13/AgriculturalSuitability/blob/main/Outputs/Elevation.jpg)](https://github.com/DBishal13/AgriculturalSuitability/blob/main/Outputs/Elevation.jpg)
**Fig 4. Suitability Map based on elevation from the stream.**

### Final Suitability Data
| % Of Land  | Suitability         |
|------------|---------------------|
| 7.2 %      | Highly Suitable     |
| 23.75 %    | Moderately Suitable |
| 65.1 %     | Marginally Suitable |
| 3.95 %     | Not Suitable        |

[![Final Suitability](https://github.com/DBishal13/AgriculturalSuitability/blob/main/Outputs/Final_suitability.jpg)](https://github.com/DBishal13/AgriculturalSuitability/blob/main/Outputs/Final_suitability.jpg)
**Fig 5. Final Suitability Map**

Looking at the final suitability map and data, we can see that around 7.2 % of the land is highly suitable for agriculture, and about one-third was found marginally suitable. We can see that a meager portion of the land is not ideal for agriculture. However, this assumes that they have specific weights. If we change the importance of each parameter, different results could be obtained.

Finally, if we compare the final map with the land use land cover map, we can see that some very suitable agricultural areas are occupied by built-up areas. Also, the places found to be Moderately and Marginally suitable are mostly covered with trees, Bare land, and Range Lands. Hence, there is a higher possibility of agricultural expansion; however, increasing population resulting in highly built-up areas will likely limit agricultural land growth soon.

[![LULC](https://github.com/DBishal13/AgriculturalSuitability/blob/main/Outputs/LULC.jpg)](https://github.com/DBishal13/AgriculturalSuitability/blob/main/Outputs/LULC.jpg)
**Fig 12. Land Use Land Cover Map of 2022**

## References
1. Adhikari, S. P., Timsina, K. P., & Lamichhane, J. (2018). Adoption and impact of rainwater harvesting technology on rural livelihoods: The case of Makwanpur district, Nepal. Rural Extension and Innovation Systems Journal, 14(1), 34–40. https://search.informit.org/doi/10.3316/informit.563360644027139
2. Regmi, H. R. (2007). Effect of unusual weather on cereal crop production and household food security. Journal of Agriculture and Environment, 8, 20–29. https://doi.org/10.3126/aej.v8i0.723 
3. Malla, G. (2009). Climate Change and Its Impact on Nepalese Agriculture. Journal of Agriculture and Environment, 9, 62-71. 
4. Gurung, G.B., & Bhandari, D. (2009). Integrated approach to climate change adoption. Journal of Forest and Livelihood, 8(1), 91-99.
5. DFO. 2002. Records of traded NTCPs. District Forest Office, Makwanpur, Nepal, 43p.
6. Olaniyi, A., Ajiboye, A.J., Abdullah, A., Ramli, M. & Sood, A. (2015). Agricultural land use suitability assessment in Malaysia. Bulgarian Journal of Agricultural Science, 21, 560-572. 
7. Everest, T., Sungur, A. & Özcan, H. (2021). Determination of agricultural land suitability with a multiple-criteria decision-making method in Northwestern Turkey. Int. J. Environ. Sci. Technol. 18, 1073–1088. https://doi.org/10.1007/s13762-020-02869-9
8. Bale C.L., Williams J.B., Charley J.L. (1998). The impact of aspect on forest structure and floristics in some Eastern Australian sites. Forest Ecology and Management, 110(1–3), 363–377.
9. Yimer F., Ledin S., Abdelkadir A. (2006). Soil property variations in relation to topographic aspect and vegetation community in the southeastern highlands of Ethiopia. Forest Ecology Management, 232(1–3), 90–99.
