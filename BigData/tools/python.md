# Python
This is a free, open-source language that is a standard tool used in many organisations and industries. It interfaces with other programs and tools like ArcGIS. Packages like xarray are great for analysing large gridded time-series data in climate and environmental science fields. Python creates beautiful plots.

## Analysis

The three main python packages used in climate science are numpy, pandas and xarray. Lots of the other analysis are based of them, xarray is itself based on pandas which is based on numpy.

```` {dropdown} [**Numpy**](https://numpy.org/doc/stable/): numerical math 
NumPy is the fundamental package for scientific computing in Python. It is a Python library that provides a multidimensional array object, various derived objects (such as masked arrays and matrices), and an assortment of routines for fast operations on arrays, including mathematical, logical, shape manipulation, sorting, selecting, I/O, discrete Fourier transforms, basic linear algebra, basic statistical operations, random simulation and much more.

At the core of the NumPy package, is the ndarray object. This encapsulates n-dimensional arrays of homogeneous data types, with many operations being performed in compiled code for performance. There are several important differences between NumPy arrays and the standard Python sequences:

NumPy arrays have a fixed size at creation, unlike Python lists (which can grow dynamically). Changing the size of an ndarray will create a new array and delete the original.
The elements in a NumPy array are all required to be of the same data type, and thus will be the same size in memory. The exception: one can have arrays of (Python, including NumPy) objects, thereby allowing for arrays of different sized elements.
NumPy arrays facilitate advanced mathematical and other types of operations on large numbers of data. Typically, such operations are executed more efficiently and with less code than is possible using Python’s built-in sequences.
A growing plethora of scientific and mathematical Python-based packages are using NumPy arrays; though these typically support Python-sequence input, they convert such input to NumPy arrays prior to processing, and they often output NumPy arrays. In other words, in order to efficiently use much (perhaps even most) of today’s scientific/mathematical Python-based software, just knowing how to use Python’s built-in sequence types is insufficient - one also needs to know how to use NumPy arrays.<br>
Extract from [numpy documentation](https://numpy.org/doc/stable/user/whatisnumpy.html)
````

```` {dropdown} [**Pandas**](https://pandas.pydata.org/docs/index.html):
Built to work with tabular and timeseries data and make use of relational and label infomration associated with the main data. Pandas is useful to slice, index, group data in complex ways. It has timeseries specific functionalities that makes working with time axis much easier. Pandas is based on numpy, iit is itself the base for xarray and integrates well with other libraries. Pandas has two main data structures Series which is a 1-dimensional homogeneous array, and Dataframe which is a 2-dimensional tabular format. 
````

```` {dropdown} [**Xarray**](http://xarray.pydata.org/en/stable/#)
xarray (formerly xray) is an open source project and Python package that makes working with labelled multi-dimensional arrays simple, efficient, and fun!

Xarray introduces labels in the form of dimensions, coordinates and attributes on top of raw NumPy-like arrays, which allows for a more intuitive, more concise, and less error-prone developer experience. The package includes a large and growing library of domain-agnostic functions for advanced analytics and visualization with these data structures.

Xarray is inspired by and borrows heavily from pandas, the popular data analysis package focused on labelled tabular data. It is particularly tailored to working with netCDF files, which were the source of xarray’s data model, and integrates tightly with dask for parallel computing.
Extract from [http://xarray documentation](http://xarray.pydata.org/en/stable/)
Xarray dependes on numpy and pandas, it can use several other python packages as optional dependencies. A full list is available on the [installation page](http://xarray.pydata.org/en/stable/getting-started-guide/installing.html) of its documentation. As long as these packages are already installed they can be used directly from xarray. Examples are matplotlib, dask and netcdf4. The hh5 conda enviroments available on the NCI servers will have all these dependencies with the exception of PyNIO and pseudonetcdf. 
````
When to use numpy vs pandas vs xarray:
Numpy is at the base of many other scientific analysis packages (as pandas, xarray etc) so you are often dealing with numpy arrays when using them. If you are delaing with complex object then using pandas (for tabular data and/or timeseries) or xarray for geonspatial grid data is better. Pandas and xarray add overhead for smaller object, as they add a complexity which is not encessarily useful. Occasionally you might find yourself moving from one to the other to perfom a specific numerical oprationnin numpy 

Multi-dimensional (a.k.a. N-dimensional, ND) arrays (sometimes called “tensors”) are an essential part of computational science. They are encountered in a wide range of fields, including physics, astronomy, geoscience, bioinformatics, engineering, finance, and deep learning. In Python, NumPy provides the fundamental data structure and API for working with raw ND arrays. However, real-world datasets are usually more than just raw numbers; they have labels which encode information about how the array values map to locations in space, time, etc.
Xarray doesn’t just keep track of labels on arrays – it uses them to provide a powerful and concise interface. For example:

* Apply operations over dimensions by name: x.sum('time').
* Select values by label (or logical location) instead of integer location: x.loc['2014-01-01'] or x.sel(time='2014-01-01').
* Mathematical operations (e.g., x - y) vectorize across multiple dimensions (array broadcasting) based on dimension names, not shape.
* Easily use the split-apply-combine paradigm with groupby: x.groupby('time.dayofyear').mean().
* Database-like alignment based on coordinate labels that smoothly handles missing values: x, y = xr.align(x, y, join='outer').
* Keep track of arbitrary metadata in the form of a Python dictionary: x.attrs.

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

## Packages for specific analysis


marineHeatwaves / xmhw - calculate MHW statistics

CleF - discovering ESGF datasets at NCI

ClimTas - makes it easier to apply and extend dask functions

Xclim - …

Cosima cookbook

## Data handling

`os, sys, glob` - to handle directories and files


`json` - to handle json files (often useful to store table information and pass schema, vocabularies and other dictionary style information to programs)

`yaml` -  to handle yaml files - often use to handle program configurations

`rasterio, rasterstats, rio-xarray, geopandas, fiona` - to handle raster and shapefiles


## Packages to work with NetCDF

`Iris` - 

[Netcdf4](http://unidata.github.io/netcdf4-python/) - to handle netcdf files, it is an optional dependecy for xarray 

`Zarr` - 

`cfcheker.py` - checking against CF and ACDD conventions

## Other formats

`datetime` - to handle date and time information
`calendar` - to handle calendars
`hdf5, hdf4, h4netcdf, hdfeos2, hdfeos5,` 
[pytables](https://www.pytables.org/index.html) and [h5py](https://docs.h5py.org/en/stable/), pyhdf - to handle various hdf formats they have different advantages

Pygrib -m to handle grib file


Csv - to handle csv files

## Web related packages

`siphon` - to navigate thredds servers

`requests` - download/upload from/to website

## Database related packages

`SQLAlchemy`

`sqlite3`


## Plotting
Matplotlib: to create plots

Other plotting packages: https://mode.com/blog/python-data-visualization-libraries/: plotly, seaborn, holoviews


## Other
CDO - to call cdo operators (Scott has a regridding function that exploit this)

Wrf-python -


Xesmf - 

Udunits2 -

Eofs - 

Eccodes - 

Earthpy -

xgcm - work with offset grids

## Specific toolsets

[Anaconda](https://www.anaconda.com/): Contains pretty much all the python libraries you'd want to get started, great for newcomers but takes up a lot of space. Not recommended on shared systems with quotas but good on local laptops. Includes Spyder, a Matlab-like programming environment (IDE).

[miniconda](https://docs.conda.io/en/latest/miniconda.html): A lightweight version of anaconda which by default only includes core libraries, good for building specific environments for data analysis. This underpins the `conda` modules in the `hh5` project at NCI.

[Pangeo](https://pangeo.io/): A community for analysis of large scale climate data. Built on tools like python, xarray, dask, iris, cartopy.

scipy



