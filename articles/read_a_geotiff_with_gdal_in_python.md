# Reading GeoTiff raster images in python3

With the constant increase in the volume of earth observation data from satellites and also from aerial platforms like aircrafts and drones it is inevitable that we need use a programming language to automate some of the repetitive tasks. In the recent years python has become the defacto language for data science and machine learning mainly due to its simple structre and a very rich library collections, so in the following article I tried to explain the first step towards automating a raster processing workflow (i.e) reading a raster data.

## Using GDAL

```python
import gdal

# open the raster file
# read the contents as numpy array
# read the metadata for transformation information
```

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
