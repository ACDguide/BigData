# Identifying which languages/tools are best suited to specific tasks
## Python
This is a free, open-source language that is a standard tool used in many organisations and industries. It interfaces with other programs and tools like ArcGIS. Packages like xarray are great for analysing large gridded time-series data in climate and environmental science fields. Python creates beautiful plots.
Os, sys, glob: to handle directories and files
Numpy: numerical math
Matplotlib: to create plots
Other plotting packages: https://mode.com/blog/python-data-visualization-libraries/: plotly, seaborn, holoviews
Pandas: timeseries, integrates with numpy 
Xarray: gridded data, integrates with pandas, include basic plotting capabilities
Dask: to parallelise tasks and manage memory more efficiently , integrates with xarray
Calendar: to handle calendars and time information
Netcdf4 - to handle netcdf files, usually integrated in tools like xarray, pandas
hdf5, hdf4, h4netcdf, hdfeos2, hdfeos5, h5py, pyhdf - to handle various hdf formats they have different advantages
Pygrib -m to handle grib file
Requests: download/upload from/to website (not specifically analysis but can be useful for data handling)
Csv - to handle csv files
Json - to handle json files (often useful to store table information and pass schema, vocabularies and other dictionary style information to programs)
Yaml -  to handle yaml files - often use to handle program configurations
Rasterio, rasterstats, rio-xarray, geopandas, fiona - to handle raster and shapefiles
Zarr - 
Specific tools:
Iris - 
Cfcheker.py - checking against CF and ACDD conventions
marineHeatwaves / xmhw - calculate MHW statistics
CleF - discovering ESGF datasets at NCI
ClimTas - makes it easier to apply and extend dask functions
Xclim - …
Cosima cookbook
Cdo - to call cdo operators (Scott has a regridding function that exploit this)
Wrf-python -
Siphon - to navigate thredds servers
Xesmf - 
Udunits2 -
Eofs - 
Eccodes - 
Earthpy -
xgcm - work with offset grids

Specific distributions:
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
