---
title: Reading Geotiff raster images in python3
permalink: /articles/read_a_geotiff_with_gdal_in_python.md
collection: articles
author: [Gokul Ganesan](https://www.linkedin.com/in/gokul-ganesan/)
---

# Reading GeoTiff raster images in python3

With the constant increase in the volume of earth observation data from satellites and also from aerial platforms like aircrafts and drones it is inevitable that we need use a programming language to automate some of the repetitive tasks. In the recent years python has become the defacto language for data science and machine learning mainly due to its simple structre and a very rich library collections, so in the following article I tried to explain the first step towards automating a raster processing workflow (i.e) reading a raster data.

## Using GDAL

```python
try:
    import gdal
except:
    from osgeo import gdal
gdal.UseExceptions()
dataset = gdal.Open("<path/to/file.tif", gdal.GA_ReadOnly) # open the raster file
print(f'{dataset.RasterXSize}, {dataset.RasterYSize}, {dataset.RasterCount}')
print(f'{dataset.GetProjection()}') # reading the metadata
data_array = dataset.ReadAsArray() # read the contents as numpy array
print(data_array.shape)
```

In the above code snippet GDAL official python API is used to open a raster dataset, read it as a numpy array and print some of the information about the dataset. Here the use of GDAL python API's sometimes forces the user to write non-pythonic code (e.g) raising Exceptions when an error occurs should be activated by default in python by in GDAL python API user has to call `gdal.UseExceptions()` to activate Exceptions. Also the API is close to GDAL's C++ API so it will be easy to switch to a different programming language if needed.

## Using Rasterio

```python
import rasterio as rio

# open the raster file
with rio.open('<path/to/file.tiff>') as src_file:
    # Read the image as numpy array
    src_array = src_file.read() # read as a ndarray
    # read the meatadata
    src_metadata = src_file.meta # metadata containing transform, image size, dtype etc
```

In this code snippet rasterio is used to read a GeoTiff raster file. Rasterio is created to overcome the pitfals of GDAL python API which is heavily depended on the GDAL C++ API (i.e) the python program run and behave like a C++ program. So, rasterio provides a way to interact with the raster data in python with all the features and idioms of python language ([more about rasterio philosophy](https://rasterio.readthedocs.io/en/stable/intro.html)). Rasterio is depended on GDAL for its format drivers and it also includes a minimal GDAL library.

Once the GeoTiff raster data is read as a numpy array, the data can be manipulated/processed at speeds close to C using Numpy.
