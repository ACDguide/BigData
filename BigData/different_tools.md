# Identifying which languages/tools are best suited to specific tasks

This page contains:
- [Python](#python)
- [R](#r)
- [MATLAB](#matlab)
- [NCO](#nco-netcdf-operators)
- [CDO](#cdo-climate-data-operators)

Other languages and tools exist which can work with netCDF data (e.g. C, FORTRAN, ArcGIS, QGIS, paraview, panoply, Ferret, as well as the deprecated NCL), but on this page we focus on tools commonly used for *analysis* of large scale (tyipcally netCDF) climate data.

## Python 
This is a free, open-source language that is a standard tool used in many organisations and industries. It interfaces with other programs and tools like ArcGIS. Packages like xarray are great for analysing large gridded time-series data in climate and environmental science fields. Python creates beautiful plots.

`os, sys, glob` - to handle directories and files

`numpy` - numerical python

`matplotlib` - to create plots

`cartopy` - plots maps from geospatial data

Other plotting packages: https://mode.com/blog/python-data-visualization-libraries/  - `plotly, seaborn, holoviews, bokeh`

`pandas` - timeseries, integrates with numpy 

`xarray` - gridded data, integrates with pandas, include basic plotting capabilities

`dask` - to parallelise tasks and manage memory more efficiently , integrates with xarray

`calendar, datetime` - to handle calendars and time information

`netcdf4` - to handle netCDF files, usually integrated in tools like xarray, pandas

`hdf5, hdf4, h4netcdf, hdfeos2, hdfeos5, h5py, pyhdf` - to handle various HDF formats they have different advantages

`pygrib` - to handle GRIB file

`requests` - download/upload from/to website (not specifically analysis but can be useful for data handling)

`csv` - to handle CSV files

`json` - to handle JSON files (often useful to store table information and pass schema, vocabularies and other dictionary style information to programs)

`yaml` - to handle yaml files - often use to handle program configurations

`rasterio, rasterstats, rio-xarray, geopandas, fiona` - to handle raster and shapefiles

`gdal` - useful for reprojecting data and interfacing with geoTIFFs

`scipy` - scientific python tools

`zarr` - to read and write datasets as zarr archives 

### Specific tools:

`Iris` - MetOffice tool for working with CF-compliant netCDF data 

`cfcheker.py` - checks netCDF files against CF and ACDD conventions

`marineHeatwaves` / `xmhw` - calculate MHW statistics

`CleF` - discovering ESGF datasets at NCI

`ClimTas` - makes it easier to apply and extend dask functions

`Xclim` - …

`Cosima cookbook` - various python libraries for ocean and sea ice

`cdo` - to call cdo operators (Scott has a regridding function that exploits this)

`Wrf-python` -

`Siphon` - to query and navigate THREDDS servers

`Xesmf` - regridding tool 

`udunits2` - Library used to interpret units of measurement

`Eofs` - 

`Eccodes` - 

`Earthpy` -

`xgcm` - work with offset grids

### Specific toolsets:

[Anaconda](https://www.anaconda.com/): Contains pretty much all the python libraries you'd want to get started, great for newcomers but takes up a lot of space. Not recommended on shared systems with quotas but good on local laptops. Includes Spyder, a Matlab-like programming environment (IDE).

[miniconda](https://docs.conda.io/en/latest/miniconda.html): A lightweight version of anaconda which by default only includes core libraries, good for building specific environments for data analysis. This underpins the `conda` modules in the `hh5` project at NCI.

[Pangeo](https://pangeo.io/): A community for analysis of large scale climate data. Built on tools like python, xarray, dask, iris, cartopy.

## R
This is a free, open-source statistical programming language. It is used mainly in research, but it is also a standard tool in many organisations. This tool is great for statistical analysis.

`dplyr, tidyr, tidyverse` - Dataframe manipulation

`ggplot2` - creating graphics

`purrr` - data wrangling

`rio` - data import/export

`Shiny` - report results, e.g., build interactive web apps

`Mlr` - machine learning tasks

`Leaflet` - mapping and working on interactive maps

`tidymodels` - modeling and machine learning

`sp, maptools` - processing spatial data

`zoo,xls` - for time series data

`climpact` - https://github.com/ARCCSS-extremes/climpact Heatwave/extremes statistics

Recommended list of packages: https://support.rstudio.com/hc/en-us/articles/201057987-Quick-list-of-useful-R-packages

### Specific toolsets

[Rstudio](https://support.rstudio.com/hc/en-us) IDE

## MATLAB
MATLAB (Matrix Laboratory) is a licenced tool. It is a good tool when dealing with large matrices and matrix manipulations. It allows examining the content of data quickly in a built-in docked or undocked window within the tool to gain an overview of the pattern and structures presented in the data. This tool is helpful because many data types, for example, large image files and large tabular data, can be converted into matrices and analysed efficiently in MATLAB. MATLAB provides an easy-to-use environment with interactive applications, which is excellent for novice programmers. MATLAB also has excellent help resources and a useful online community. 

As a licensed tool MATLAB might not be available to other researchers and collaborators, so even if you are producing data with Matlab, it is best to avoid saving the data as `.mat` files, use the best alternative open source format instead.

## NCO - NetCDF Operators
[NetCDF Operators](http://nco.sourceforge.net/) is a toolkit of command-line operators to both handle and perform analysis on netCDF files. It is the tool of choice to add, rename, and modify attributes and variables. It can add internal compression to netCDF4 files and convert between different formats. It is also useful to concatenate files, performing averages and other simple mathematical operations on an entire variable, extracting or deleting variables. The advantage is that the results will be automatically saved in a netCDF file.


## CDO - Climate Data Operators
[CDO](https://code.mpimet.mpg.de/projects/cdo/), like NCO, is a large command-line tool set to handle and analyse climate and weather data. CDO can also work with GRIB files, in fact it is a useful tool to convert from GRIB to netCDF and vice versa. CDO can also be used to compress, convert and concatenate files, often in conjunction with another operation.

One of the strengths of CDO is its ability to combine operations in succession of steps without creating intermediate files, using little additional memory in the process.

CDO is useful to calculate climatologies, regrid datasets, select subset both spatially and temporally. It can be used to perform simple transformations across an entire variable as for NCO. It is useful to handle time axis operations as going from unlimited to limited dimension and setting a new reference time. CDO can integrate with other languages such as python using the `cdo` module.

Limitations: specific versions can have issues with threading, meaning chained commands are not always safe. CDO **cannot** be built in threadsafe mode due to underpinning HDF dependencies which means some versions simply are not reliable and can cause random segfaults when using chained operations.
