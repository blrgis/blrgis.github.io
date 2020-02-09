# Getting Started with Remote Sensing Data

In this article, we will use remote sensing (here on refered to as RS) data to conduct simple analysis. Out of many satellites that orbit earth collecting RS data, here we will use the data from [LANDSAT](https://landsat.gsfc.nasa.gov/landsat-8/) satellite which is freely available and can be downloaded from [Earth Explorer](https://earthexplorer.usgs.gov). 

## Downloading data
We will download the data for Bangalore, India. As shown in the screenshot below, we search for the area of interest which in our case is Bangalore. After giving the date range (for us we download images from November 1999 and Ocotber 2018) as shown in Figure 2, we move on to select the datasets. Here, we need to select which satellite data we want to download, for us it is LANDSAT 7. By clicking on footprint, we can see the footprint of the scene (the geographic area covered by the image) on the map as shown in Figure 2. Once you have selected the data we order it (Since the data is freely available you won't be charged for it :)). We might have to wait a little while before data can be downloaded (You will receive an email from USGS informing you when the data is ready for download). While downloading the data we ensure that the coud cover is less than 10% by adding criteria inAdditional Criteria tab.

<img src = "/screenshots/search_location.png">
Figure 1: Searching for area of interest in Earth Explorer


<img src = "/screenshots/footprint.PNG">
Figure 2: Footprint of selected scene

## Downloading in Q-GIS
Now, that we have downloaded our data, we can analyse it in a software named [Q-GIS](https://qgis.org/en/site/forusers/download.html). This is an opensource and freely available version, the other proprietary options can be ArcGIS, ENVI, Erdas Imagine.
 
## Working with the data
We will analyse the forest area of Bangalore and see how much change has occured between 1999 and 2018. For this we will compute NDVI - Normalized Difference Vegetation Index. 

It is very to easy load both raster data and vector data in Q-GIS by simply using drag and drop. We see the NDVI images of 1999 and 2018 for the Bangalore area. The downloaded zip files of LANDSAT data contains a separate image for each band. For computation of NDVI, we require Red Band (Band-3 in Landsat 7) and Near Infra Red Band (Band-4 in LANDSAT 7). You can read more on bands on landsat [here](https://www.usgs.gov/land-resources/nli/landsat/landsat-7?qt-science_support_page_related_con=0#qt-science_support_page_related_con). The NDVI computation can be performed using Raster Calculator tool in Q-GIS under the menu item Raster using the following formula, (NIR - RED) / (NIR + RED). The values of NDVI are between -1 and 1. Since, vegetation has high reflectance in NIR band and low reflectance in Red band, the NDVI values are high for the area with high vegetation and forest. Similarly, water has no reflectance in NIR band and very low in RED band resulting in negative values of NDVI. The soil and urban area have values in between these two in the range of approx. 0-0.4. Using this information we classify our NDVI images into three classes namely water, urban area or soil, forest or vegetation. 

It is clear from the first glance that the forest area around Bangalore district has decreased drastically from 1999 (Figure 3) to 2018 (Figure 4). Based on the images, we can also understand that there is significant growth in urban area. This shows that the city has seen a very rapid growth in two decades and as a result the forestarein the surrounding has gone down as well. Thus with very basic tools, we are able to analyse the changes that have taken place in the surroundings of Bangalore. 



<img src = "/screenshots/ndvi_1999.PNG">
Figure 3: NDVI for Bangalore, 1999

<img src = "/screenshots/ndvi_2018.PNG">
Figure 4: NDVI for Bangalore, 2018

