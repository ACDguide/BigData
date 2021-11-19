# Identifying which languages/tools are best suited to specific tasks
## Python
This is a free, open-source language that is a standard tool used in many organisations and industries. It interfaces with other programs and tools like ArcGIS. Packages like xarray are great for analysing large gridded time-series data in climate and environmental science fields. Python creates beautiful plots.

### Data handling

Os, sys, glob: to handle directories and files


Json - to handle json files (often useful to store table information and pass schema, vocabularies and other dictionary style information to programs)

Yaml -  to handle yaml files - often use to handle program configurations

Rasterio, rasterstats, rio-xarray, geopandas, fiona - to handle raster and shapefiles

### Packages to work with NetCDF

Iris - 

[Netcdf4](http://unidata.github.io/netcdf4-python/) - to handle netcdf files, it is an optional dependecy for xarray 

Zarr - 

Cfcheker.py - checking against CF and ACDD conventions

### Other formats

Calendar: to handle calendars and time information
hdf5, hdf4, h4netcdf, hdfeos2, hdfeos5, 
[pytables](https://www.pytables.org/index.html) and [h5py](https://docs.h5py.org/en/stable/), pyhdf - to handle various hdf formats they have different advantages

Pygrib -m to handle grib file


Csv - to handle csv files

### Web resoources

Siphon - to navigate thredds servers

Requests: download/upload from/to website

SQLAlchemy

sqlite3


### Analysis

The three main python packages used in climate science are numpy, pandas and xarray. Lots of the other analysis are based of them, xarray is itself based on pandas which is based on numpy.

```` {dropdown} [**Numpy**](https://numpy.org/doc/stable/): numerical math 
NumPy is the fundamental package for scientific computing in Python. It is a Python library that provides a multidimensional array object, various derived objects (such as masked arrays and matrices), and an assortment of routines for fast operations on arrays, including mathematical, logical, shape manipulation, sorting, selecting, I/O, discrete Fourier transforms, basic linear algebra, basic statistical operations, random simulation and much more.

At the core of the NumPy package, is the ndarray object. This encapsulates n-dimensional arrays of homogeneous data types, with many operations being performed in compiled code for performance. There are several important differences between NumPy arrays and the standard Python sequences:

NumPy arrays have a fixed size at creation, unlike Python lists (which can grow dynamically). Changing the size of an ndarray will create a new array and delete the original.
The elements in a NumPy array are all required to be of the same data type, and thus will be the same size in memory. The exception: one can have arrays of (Python, including NumPy) objects, thereby allowing for arrays of different sized elements.
NumPy arrays facilitate advanced mathematical and other types of operations on large numbers of data. Typically, such operations are executed more efficiently and with less code than is possible using Python’s built-in sequences.
A growing plethora of scientific and mathematical Python-based packages are using NumPy arrays; though these typically support Python-sequence input, they convert such input to NumPy arrays prior to processing, and they often output NumPy arrays. In other words, in order to efficiently use much (perhaps even most) of today’s scientific/mathematical Python-based software, just knowing how to use Python’s built-in sequence types is insufficient - one also needs to know how to use NumPy arrays.<br>
Extract from https://numpy.org/doc/stable/user/whatisnumpy.html
````

```` {dropdown} [**Pandas**](https://pandas.pydata.org/docs/index.html):
 built to work with tabular and timeseries data and make use of relational and label infomration associated with the main data. Pandas is useful to slice, index, group data in complex ways. It has timeseries specific functionalities that makes working with time axis much easier. Pandas is based on numpy, iit is itself the base for xarray and integrates well with other libraries. Pandas has two main data structures Series which is a 1-dimensional homogeneous array, and Dataframe which is a 2-dimensional tabular format. 
````

```` {dropdown} [**Xarray**](http://xarray.pydata.org/en/stable/#)
xarray (formerly xray) is an open source project and Python package that makes working with labelled multi-dimensional arrays simple, efficient, and fun!

Xarray introduces labels in the form of dimensions, coordinates and attributes on top of raw NumPy-like arrays, which allows for a more intuitive, more concise, and less error-prone developer experience. The package includes a large and growing library of domain-agnostic functions for advanced analytics and visualization with these data structures.

Xarray is inspired by and borrows heavily from pandas, the popular data analysis package focused on labelled tabular data. It is particularly tailored to working with netCDF files, which were the source of xarray’s data model, and integrates tightly with dask for parallel computing.
Extract from http://xarray.pydata.org/en/stable/
: xarray is used to analyse gridded data,
Xarray dependes on numpy and pandas, it can use several other python packages as optional dependencies. A full list is available on the [installation page](http://xarray.pydata.org/en/stable/getting-started-guide/installing.html) of its documentation. As long as these packages are already installed they can be used directly from xarray. Examples are matplotlib, dask and netcdf4. The hh5 conda enviroments available on the NCI servers will have all these dependencies with the exception of PyNIO and pseudonetcdf. 
````
When to use numpy vs pandas vs xarray:
Numpy is at the base of many other scientific analysis packages (as pandas, xarray etc) so you are often dealing with numpy arrays when using them. If you are delaing with complex object then using pandas (for tabular data and/or timeseries) or xarray for geonspatial grid data is better. Pandas and xarray add overhead for smaller object, as they add a complexity which is not encessarily useful. Occasionally you might find yourself moving from one to the other to perfom a specific numerical oprationnin numpy 

xarray
Multi-dimensional (a.k.a. N-dimensional, ND) arrays (sometimes called “tensors”) are an essential part of computational science. They are encountered in a wide range of fields, including physics, astronomy, geoscience, bioinformatics, engineering, finance, and deep learning. In Python, NumPy provides the fundamental data structure and API for working with raw ND arrays. However, real-world datasets are usually more than just raw numbers; they have labels which encode information about how the array values map to locations in space, time, etc.

Xarray doesn’t just keep track of labels on arrays – it uses them to provide a powerful and concise interface. For example:

Apply operations over dimensions by name: x.sum('time').
Select values by label (or logical location) instead of integer location: x.loc['2014-01-01'] or x.sel(time='2014-01-01').
Mathematical operations (e.g., x - y) vectorize across multiple dimensions (array broadcasting) based on dimension names, not shape.
Easily use the split-apply-combine paradigm with groupby: x.groupby('time.dayofyear').mean().
Database-like alignment based on coordinate labels that smoothly handles missing values: x, y = xr.align(x, y, join='outer').
Keep track of arbitrary metadata in the form of a Python dictionary: x.attrs.

Instead of axis labels, xarray uses named dimensions, which makes it easy to select data and apply operations over dimensions.
NumPy array can only have one data type, while xarray can hold heterogeneous data in an ND array. It also makes NaN handling easier.
Keep track of arbitrary metadata on your object with obj.attrs .
Data structures
Xarray has two data structures:
DataArray — for a single data variable
Dataset — a container for multiple DataArrays (data variables)
There’s a distinction between data variables and coordinates, according to CF conventions. Xarray follows these conventions, but it mostly semantic and you don’t have to follow it. I see it like this: a data variable is the data of interest, and a coordinate is a label to describe the data of interest. For example latitude, longitude and time are coordinates while the temperature is a data variable. This is because we are interested in measuring temperatures, all the rest is describing the measurement (the data point). In xarray docs, they say:
Coordinates indicate constant/fixed/independent quantities, unlike the varying/measured/dependent quantities that belong in data.
O

```{list-table}
:header-rows: 1
:name: np-pd-xr-table

* - 
  - Numpy
  - Pandas
  - Xarray
* - Best use
  - numerical computions
  - tabular data analysis
  - multidim labelled data analysis
* - Data structure
  - homogeneous array
  - Series (columns), Dataframes (table)
  - Labelled data arrays and datasets
* - Vectorised operations
  - Yes 
  - Yes
  - Yes
* - Dimensions
  - multi-dimensional
  - 2D with multiindex support
  - multi-dimensional
* - Time handling
  - No
  - Yes
  - Yes
* - Speed
  - Faster < 50K elements, fast indexing
  - Faster > 500K rows
  -
* - Based on
  - C uses multiple functionalities
  - R provides similar functions
* - memory use
  - more efficient
  - uses more memory
  - uses more memory
* - Plotting
  - No
  - Yes
  - Yes 
* - Attributes
  - No
  - Yes
  - Yes
* - Labels selection
  - No
  - Yes
  - Yes
```
### Xarray based packages

A list of the major ones is provided by on the [xarray documentation](http://xarray.pydata.org/en/stable/ecosystem.html)


Dask: to parallelise tasks and manage memory more efficiently , integrates with xarray

### Packages for specific analysis


marineHeatwaves / xmhw - calculate MHW statistics

CleF - discovering ESGF datasets at NCI

ClimTas - makes it easier to apply and extend dask functions

Xclim - …

Cosima cookbook


### Plotting
Matplotlib: to create plots

Other plotting packages: https://mode.com/blog/python-data-visualization-libraries/: plotly, seaborn, holoviews


### Other
CDO - to call cdo operators (Scott has a regridding function that exploit this)

Wrf-python -


Xesmf - 

Udunits2 -

Eofs - 

Eccodes - 

Earthpy -

xgcm - work with offset grids

### Specific distributions

Anaconda

miniconda

Pangeo

scipy


## R
This is a free, open-source statistical programming language. It is used mainly in research, but it is also a standard tool in many organisations. This tool is great for statistical analysis.

Dplyr, tidyr, tidyverse - Dataframe manipulation

ggplot2 - Creating graphics

purrr - data wrangling

rio - data import/export

Shiny - report results, e.g., build interactive web apps

Mlr - machine learning tasks

Leaflet - mapping and working on interactive maps

tidymodels - modeling and machine learning

sp, maptools - processing spatial data

Zoo,xls - for time series data

climpact - https://github.com/ARCCSS-extremes/climpact heatwave/extremes statistics

https://support.rstudio.com/hc/en-us/articles/201057987-Quick-list-of-useful-R-packages

## MATLAB
MATLAB (Matrix Laboratory) is a licenced tool. It is the best tool when dealing with large matrices and matrix manipulations. It allows examining the content of data quickly in a built-in docked or undocked window within the tool to gain an overview of the pattern and structures presented in the data. This tool is helpful because many data types, for example, large image files and large tabular data, can be converted into matrices and analysed efficiently in MATLAB. MATLAB provides an easy-to-use environment with interactive applications, which is excellent for novel programmers. 

As a licensed tool matlab might not be available to other researchers and collaborators, so even if you are producing data with matlab, avoid saving the data as ‘mat’ files, use the best alternative open source format instead.

## NCO - NetCDF Operators
NetCDF Operators toolkit of command-line operators to both handle and perform analysis on netCDF files. It is the tool of choices to add, rename, modified attributes and variables. It can add internal compression to netcdf4 files and convert between different formats. It is also useful to concatenate files, performing averages and other simple mathematical operations on an entire variable, extracting or deleting variables. The advantage is that the results will be automatically saved in a netcdf file.
Limitations: memory? File size?

## CDO - Climate Data Operators
CDO, like NCO is a large tool set to handle and analyse climate and weather data. CDO can also work with grib files, in fact it is a useful tool to convert from grib to netcdf and vice versa. CDO can also be used to compress, convert and concatenate files. However this is usually in conjunction with another operation.

One of the strengths of CDO is its ability to combine operations in succession of steps without creating intermediate files.
CDO is useful to calculate climatologies, regrid datasets, select subset both spatially and temporally. It can be used to perform simple transformations across an entire variable as for NCO. It is useful to handle time axis operations as going from unlimited to limited dimension and setting a new reference time. CDO can integrate with other languages such as python using the ‘cdo’ module.

Limitations: specific versions can have issues with threading
