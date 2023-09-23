# Getting Started with Remote Sensing Data

In this article, we will use remote sensing (here on refered to as RS) data to conduct a simple analysis. Out of many satellites that orbit earth collecting RS data, here we will use the data from [LANDSAT](https://landsat.gsfc.nasa.gov/landsat-8/) satellite which is freely available and can be downloaded from [Earth Explorer](https://earthexplorer.usgs.gov). 

## Downloading data
We will download the data for Bangalore, India. As shown in the screenshot below, we search for the area of interest which in our case is Bangalore. Once you select from the given suggestions, latitude and ongitudes details will be filled in automatiallyand you can select date range for the time you want to download the data (Figure2). We download images for November 1999 and October 2018. Next we need to click on Datasets to select which satellite data we want to download, for us it is LANDSAT 7. While downloading the data, we ensure that the coud cover is less than 10% by adding criteria in Additional Criteria tab. By clicking on footprint (little icon of footnear image thumbnail), we can see the footprint of the scene (the geographic area covered by the image) on the map as shown in Figure 3. Once you have selected the data, we order it (Since the data is freely available you won't be charged for it :)). We might have to wait a little while before data can be downloaded (You will receive an email from USGS when the data is ready for download). 

Figure 1: Searching for area of interest in Earth Explorer
<img src = "/screenshots/search_location.png">

Figure 2: Latitude Longitude of location
<img src = "/screenshots/footprint.PNG">

Figure 3: Footprint of selected scene
<img src = "/screenshots/footprint.PNG">

## Downloading Q-GIS
Now, that we have downloaded our data, we can analyse it in a software named [Q-GIS](https://qgis.org/en/site/forusers/download.html). This is an opensource and freely available version, the other proprietary options can be ArcGIS, ENVI, Erdas Imagine.
 
## Working with the data
We will analyse the forest area of Bangalore and see how much change has occured between 1999 and 2018. For this we will compute NDVI - Normalized Difference Vegetation Index. NDVI can be computed using the formula, (NIR - RED) / (NIR + RED). The values of NDVI are between -1 and 1. Since, vegetation has high reflectance in NIR band and low reflectance in Red band, the NDVI values are high for the area with high vegetation and forest. Similarly, water has no reflectance in NIR band and very low in RED band resulting in negative values of NDVI. The soil and urban area have values in between these two in the range of approx. 0-0.5. Using this information we classify our NDVI images into three classes namely water, urban area or soil, forest or vegetation. 

It is very to easy load both raster data and vector data in Q-GIS by simply using drag and drop. The downloaded zip files of LANDSAT data contains a separate image for each band. For computation of NDVI, we require Red Band (Band-3 in Landsat 7) and Near Infra Red Band (Band-4 in LANDSAT 7). You can read more information on the bands of landsat 7 [here](https://www.usgs.gov/land-resources/nli/landsat/landsat-7?qt-science_support_page_related_con=0#qt-science_support_page_related_con). The NDVI computation can be performed using Raster Calculator tool in Q-GIS under the menu item Raster. In Figure 4 and 5, we see the NDVI images of 1999 and 2018 for the Bangalore area.


Figure 4: NDVI for Bangalore, 1999
<img src = "/screenshots/ndvi_1999.PNG">

Figure 5: NDVI for Bangalore, 2018
<img src = "/screenshots/ndvi_2018.PNG">

It is clear from the first glance that the forest area in 2018 (Figure 5) is quite smaller compared to that of 1999 (Figure 4). Based on the images, we can also understand that there is significant growth in urban area. We also notice a decrease in the number of lakes around Bangalore from 1999 to 2018. This shows that the city has seen a very rapid urban expansion in two decades and as a consequence there is loss of forest and water area in the surrounding region. 
Thus with very basic tools, we are able to analyse the changes that have taken place in the surroundings of Bangalore.
