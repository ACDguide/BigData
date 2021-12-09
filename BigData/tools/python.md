# Python
This is a free, open-source language that is a standard tool used in many organisations and industries. Python is easy to learn and read, hence is popularity. It also interfaces with many other programs and tools. Compared to other languages python is slow and has high memory usage, this can become a challenge when working with big datasets. 

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

```` {dropdown} [**Pandas: tabular labeled data**](https://pandas.pydata.org/docs/index.html):
Built to work with tabular and timeseries data and make use of relational and label infomration associated with the main data. Pandas is useful to slice, index, group data in complex ways. It has timeseries specific functionalities that makes working with time axis much easier. Pandas is based on numpy, iit is itself the base for xarray and integrates well with other libraries. Pandas has two main data structures Series which is a 1-dimensional homogeneous array, and Dataframe which is a 2-dimensional tabular format. 
````

```` {dropdown} [**Xarray: multidimensional labeled data**](http://xarray.pydata.org/en/stable/#)
xarray (formerly xray) is an open source project and Python package that makes working with labeled multi-dimensional arrays simple, efficient, and fun!

Xarray introduces labels in the form of dimensions, coordinates and attributes on top of raw NumPy-like arrays, which allows for a more intuitive, more concise, and less error-prone developer experience. The package includes a large and growing library of domain-agnostic functions for advanced analytics and visualization with these data structures.

Xarray is inspired by and borrows heavily from pandas, the popular data analysis package focused on labeled tabular data. It is particularly tailored to working with netCDF files, which were the source of xarray’s data model, and integrates tightly with dask for parallel computing.
Extract from [http://xarray documentation](http://xarray.pydata.org/en/stable/)
Xarray dependes on numpy and pandas, it can use several other python packages as optional dependencies. A full list is available on the [installation page](http://xarray.pydata.org/en/stable/getting-started-guide/installing.html) of its documentation. As long as these packages are already installed they can be used directly from xarray. Examples are matplotlib, dask and netcdf4. The hh5 conda enviroments available on the NCI servers will have all these dependencies with the exception of PyNIO and pseudonetcdf. 
````
When to use numpy vs pandas vs xarray?<br>
As most big climate data is multidimensional and stored as NetCDF files, xarray is usually the best tool to base your analysis. Still numpy and pandas can be faster than xarray for certain operations. For example pandas is faster for groupby operation and generally querying data.<br> 
As numpy is the base of the other two packages even when your analysis is not fully based on numpy, you are often dealing with numpy arrays and operations when using them.<br> 
Xarray provides several ways to convert your arrays to and from pandas dataframes and the arrays values are numpy arrays. So these three packages can and are often used interchangeably in the same analysis code.<br> 
The table below provides a schematic of the main differences, more on the releationship between xarray and pandas is also available from the [xarray FAQ page](http://xarray.pydata.org/en/stable/getting-started-guide/faq.html).<br>

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
  - multidim labeled data analysis
* - Data structure
  - homogeneous array
  - Series (columns), Dataframes (table)
  - Labelled data arrays and datasets
* - Data input/output 
  - Read from csv, txt and simple binary files. Needs other libraries to input/output formats like netcdf, hdf5 and zarr. Can output binary, csv, txt files
  - Read/write [many formats](https://pandas.pydata.org/docs/user_guide/io.html), including hdf5, for netcdf you ened other libraries
  - best tool for netcdf, including multiple files at once, includes support for openDAP and compression, chunks can easily convert arrays to pandas and numpy (http://xarray.pydata.org/en/stable/user-guide/io.html)
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
  - pandas
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

A list of the major ones is provided by on the [xarray documentation](http://xarray.pydata.org/en/stable/ecosystem.html).

Some extends the formats supported by xarray, others are used for specific analysis:

`rioxarray` - [rioxarray](https://corteva.github.io/rioxarray/stable/) is a xarray extension powered by rasterio to load raster data.

`xgrads` - [xgrads](https://github.com/miniufo/xgrads) parses the ctl descriptor of GrADS binary data to load data as a xarray dataset.<br>

`xgcm` - [xgcm]() extends the xarray data model to finite volume grid cells (common in General Circulation Models) and provides interpolation and difference operations for such grids.

`xclim` - [xclim](https://xclim.readthedocs.io/en/stable/) is a library of functions to compute climate indices from observations or model simulations.  

`xESMF` - [xESMF](https://pangeo-xesmf.readthedocs.io/en/latest/) is a universal regridder for geospatial data. It is part of the Pangeo ecosystem.

`eof` - [eof](https://ajdawson.github.io/eofs/latest/) is used EOF (empirical orthogonal functions) analysis. NB eof has also an interface for Iris.

`wrf-python` - [wrf-python](https://wrf-python.readthedocs.io/en/latest/) is a collection of diagnostic and interpolation routines for use with output of the Weather Research and Forecasting (WRF-ARW) Model.

### Iris
Iris is an alternative to xarray ...

## Working in parallel
`dask` - [dask](https://docs.dask.org/en/stable/) to parallelise tasks and manage memory more efficiently , integrates with numpy, pandas and xarray. In fact, dask arrays are built on numpy arrays and the dask dataframe is based on Pandas dataframe. Dask is a general purpose parallel programming solution.
Dask allows to scale up your code and notebooks to a cluster, 
Dask usually can work with your existing code with just small modifications, it will try to work out the best scaling based on the memory and cpus it detects on the system. You can tune dask performance using The thread/process mixture to deal with GIL-holding computations (which are rare in Numpy/Pandas/Scikit-Learn workflows)
Partition size, like if should you have 100 MB chunks or 1 GB chunks
dask support several data formats among which: HDF5, NetCDF, Zarr, GRIB 

`multiprocessing` - 

## running environment
`jupyter` - The Jupyter Notebook is an open-source web application that allows you to create and share documents that contain live code, equations, visualizations and narrative text. Uses include: data cleaning and transformation, numerical simulation, statistical modeling, data visualization, machine learning, and much more.
While jupyter is a python package it supports more than 40 languages including R, Julia and of course python.
`jupyterlab` - JupyterLab is a web-based interactive development environment for Jupyter notebooks, code, and data. JupyterLab is flexible: configure and arrange the user interface to support a wide range of workflows in data science, scientific computing, and machine learning. JupyterLab is extensible and modular: write plugins that add new components and integrate with existing ones. 


## Data handling

`os, sys, glob` - to handle directories and files

`datetime` - to handle date and time information

`calendar` - to handle calendars

`csv` - to handle csv files

`json` - to handle json files (often useful to store table information and pass schema, vocabularies and other dictionary style information to programs)

`yaml` -  to handle yaml files - often use to handle program configurations

`siphon` - to navigate thredds servers

## Geospatial data handling

`rasterio, rasterstats, rio-xarray, geopandas, fiona` - to handle raster and shapefiles

[Netcdf4](http://unidata.github.io/netcdf4-python/) - to handle netcdf files, it is an optional dependecy for xarray 

`Zarr` - 

`hdf5, hdf4, h4netcdf, hdfeos2, hdfeos5,` 
[pytables](https://www.pytables.org/index.html) and [h5py](https://docs.h5py.org/en/stable/), pyhdf - to handle various hdf formats they have different advantages

Pygrib -m to handle grib file


## Other packages not analysis but still useful


`requests` - download/upload from/to website

`tlc`




## Plotting
`matplotlib` - to create plots

Other plotting packages: https://mode.com/blog/python-data-visualization-libraries/: plotly, seaborn, holoviews


## Interfaces to other software 
CDO - to call cdo operators (Scott has a regridding function that exploit this)

Wrf-python -
`SQLAlchemy`

`sqlite3`

## Random

Udunits2 -

Eofs - 

Eccodes - 

Earthpy -

`xgcm` - work with offset grids

## GUI
 `spyder` - good for reformed matlab users as it provides a very similar interface
## Specific toolsets

[Anaconda](https://www.anaconda.com/): Contains pretty much all the python libraries you'd want to get started, great for newcomers but takes up a lot of space. Not recommended on shared systems with quotas but good on local laptops. Includes Spyder, a Matlab-like programming environment (IDE).

[miniconda](https://docs.conda.io/en/latest/miniconda.html): A lightweight version of anaconda which by default only includes core libraries, good for building specific environments for data analysis. This underpins the `conda` modules in the `hh5` project at NCI.

[Pangeo](https://pangeo.io/): A community for analysis of large scale climate data. Built on tools like python, xarray, dask, iris, cartopy.


`scipy` - obsolete?


## Packages developed by climate community for specific analysis
Thes epackages were developed for very specific tasks from members of the climate community. While their scope is limited and the packages might be maintained on a need base, they still can be very useful as they have been tailored to the community needs.

`marineHeatwaves` / `xmhw` - calculate MHW statistics

`CleF` - discovering ESGF datasets at NCI

`ClimTas` - makes it easier to apply and extend dask functions

`Cosima cookbook`

`cfcheker.py` - checking against CF and ACDD conventions

