(python-data)=
# Data handling in Python

The data formats most commonly used in climate science are covered in more detail in the [Large-scale climate data](../data/data-format.md) section of this book.
Both {ref}`xarray<pyanalysis>` and {ref}`iris` can access most of them, provided that the dependencies for the formats are also installed.
A full list and guide of formats accessible via xarray is available from the [package documentation](https://xarray.pydata.org/en/stable/user-guide/io.html). The open_dataset() function can be called with different `engines`.
NetCDF4 is the main library to read netcdf data, another option is to use h5netcdf, which is based on h5py, this can be faster depending on the file structure.
To open grib files in xarray either cfgrib or PyNIO needs to be installed. Xarray also has additional features, sometimes developed by a third-party, that can extend further the list of accessible data formats.
Iris is preferable if you need to access grib files and can also access `pp files`, a binary data format used by the UM model. Main dependencies for iris are  netCDF4 and scipy. 
Other libraries are useful to manage or access dataset collections both local and remote. Siphon and pydap helps accessing remote files on thredds and/or OpenDAP services. {ref}`intake` can be used to build a catalogue to help locate and query local datasets. 

```{glossary}

[netCDF4](http://unidata.github.io/netcdf4-python/) 
     netCDF4 is the Unidata library to handle netcdf files 

[h5netcdf](https://github.com/h5netcdf/h5netcdf)
    h5netcdf is an interface for the netCDF4 file-format that reads and writes local or remote HDF5 files directly via h5py or h5pyd, without need for netCDF4. It can be faster than the latter depending on the actual file structure (any example??)

[Zarr](https://zarr.readthedocs.io/en/stable)
    Zarr is a relatively new format to store chunked, compressed, N-dimensional arrays, optimised for cloud data access
  
[cfgrib](https://github.com/ecmwf/cfgrib)
    cfgrib is a python interface to [ecCodes](https://confluence.ecmwf.int/display/ECC), a set of tools for decoding and encoding grib1 and grib2 files. ecCodes replaced the grib-api. Cfgrib allows to open a grib file with xarray and iris. 

[Pygrib](https://jswhit.github.io/pygrib/)
    pygrib is another high-level interface to ecCodes to read and write grib files

[cfunits](https://ncas-cms.github.io/cfunits/)
    cfunits is an interface the UDUNITS-2 library with CF extensions, to store, combine and compare physical units and convert numeric values to different units.

[cftime](https://unidata.github.io/cftime/)
    cftime is used to decode time units and variable values in a CF compliant netCDF file.

[intake](https://intake.readthedocs.io/en/latest/index.html)
    intake supports building catalogues of datasets, which are easy to naviagte, query and that can be augmented with metadata. We are covering intake and its extensions in more depth {ref}`here<intake>`.

[siphon](https://unidata.github.io/siphon/latest/index.html)
   siphon provides utilities to navigate and download data from remote data services, in particular thredds servers.

[CleF](https://clef.readthedocs.io/en/stable/)
     CleF Climate Finder is a python based command line tool to discover ESGF datasets at NCI, the functions can also be loaded and called in a interactive session or script.

[IOOS Compliance Checker](https://github.com/ioos/compliance-checker)
    to check netcdf files metadata against CF and ACDD conventions

```

## HDF data access
Satellite data is more commonly available as HDF.
There are several packages to handle HDF data what is best it really depends on the specific HDF format, as HDF files can really varies in characteristic and complexity.
Some HDF files can also be read as raster using libraries like rio-xarray.

```{glossary}

[hdf5](https://docs.h5py.org/en/stable/)
   interface specific to the HDF5 data format

[h5py](https://docs.h5py.org/en/stable/)
    h5py is an interface to the HDF5 data format

[pyhdf](https://hdfeos.org/software/pyhdf.php)
    pyhdf (previously known as python-hdf4) is an interface to the HDF4 data format, including the HDF4-EOS format

[pytables](https://www.pytables.org/index.html) 
    pytables allows to manipulate data tables and array objects in a hierarchical structure based on the HDF5 library. 

```

## Other raster data access
While less commonly used in core climate science research, geographical data formats as raster data is not uncommon in meteorology and GIS data is becoming more common in climate adaptation and impact studies.
An example of this is the [Cloud Optimised GeoTIFF](https://www.cogeo.org) (COG)format, which is becoming more popular to serve satellite data on cloud servers. 
As for the hdf format there is a variety of libraries whose usefulness will depend on the exact nature of your raster or GIS data. 

```{glossary}

[xgrads](https://github.com/miniufo/xgrads)
    xgrads parses the ctl descriptor of GrADS binary data to load data as a xarray dataset.

[rasterio](https://rasterio.readthedocs.io)
    raster data library based on GDAL data model

[rio-xarray](https://corteva.github.io/rioxarray/stable/index.html)
    exote4nds xarray to work seamlessly with rasterio

[rasterstats](https://pythonhosted.org/rasterstats/#)
   summarizing geospatial raster datasets based on vector geometries. It includes functions for zonal statistics and interpolated point queries.

[shapely](https://shapely.readthedocs.io/en/stable/)
    shapely is a module to manipulate and analyse geometric objects in the Cartesian plane.

[geopandas](https://geopandas.org/en/stable/)
    geopandas combines the capabilities of pandas and shapely, providing geospatial operations in pandas and a high-level interface to multiple geometries to shapely.

[fiona](https://fiona.readthedocs.io/en/latest/)
    Fiona reads and writes geographic data files; it contains extension modules for GDAL.

[gdal](https://gdal.org/api/python.html)
    gdal python bindings provide a wrapper to the C [Geospatial Data Abstraction Library](https://gdal.org) library. Gdal can manipulate a lot of geospatial data including netcdf, hdf, grib gis data formats and several image format, so it can be useful to convert between formats. 

```


## Grid handling

```{glossary}

[xESMF](https://pangeo-xesmf.readthedocs.io/en/latest/)
    xESMF is a universal regridder for geospatial data. It is part of the Pangeo ecosystem.

[xgcm](https://xgcm.readthedocs.io)
    xgcm extends the xarray data model to finite volume grid cells (common in General Circulation Models) and provides interpolation and difference operations for such grids.

[gridded](https://noaa-orr-erd.github.io/gridded/)
    gridded is a single API for accessing / working with gridded ocean model results on multiple grid types

[pyproj](https://pyproj4.github.io/pyproj/stable/)
    pyproj is an interface to proj a cartographic projections and coordinate transformations library.

```

## General purpose packages 

The following packages are not specific to climate or science but they are really useful to handle generic tasks. `os` and `sys` perform operating system functions and together with `glob` are useful to handle directories and files. Datetime and calendar help managing time related information. The package `csv` is useful to handle tabular ascii data, while `pyyaml` and `json` are often used for configuration files as well as other kind of metadata. 
Finally to pass input parameters to python script you can use sys.argv() function for a basic approach, `argparse` and `click` provide more features. Both packages provides automatically generated help message, you can define input type, default and valid values. The package `click` is also used to create command-line based programs. 
Some of these modules are distributed with the main python library (indicated with *). You still need to import them in a script but there is no need to install them.

---COMMENT: as for all the other introduction this could be massively improved, particularly in terms of highlighting usage of time related libraries.

```{glossary}

[os](https://docs.python.org/3/library/os.html)
    os offers an operative system interface (*)

[sys](https://docs.python.org/3/library/sys.html)
    sys is the interface to system specific parameters and functions, an example is sys.argv() that allows to access input parameters passed to a python script. (*)

[glob](https://docs.python.org/3/library/glob.html)
    glob finds all the pathnames matching a specified pattern according to the rules used by the Unix shell. (*)

[time](https://docs.python.org/3/library/time.html)
     time provides time related functions. (*)

[datetime](https://docs.python.org/3/library/datetime.html)
     datetime is used to manipulate date and time. (*)

[dateutil](https://dateutil.readthedocs.io/en/stable/)
    dateutil is an extension to datetime.

[calendar](https://docs.python.org/3/library/calendar.html#module-calendar)
    calendar provides calendar related functions. (*)

[csv](https://docs.python.org/3/library/csv.html)
     csv is used to read and write csv files. (*)

[json](https://docs.python.org/3/library/json.html) 
    to handle json files which are useful to store table information and pass schema, vocabularies, and other dictionary style information to programs. (*)

[pyyaml](https://pyyaml.org/wiki/PyYAMLDocumentation)
    to load and parse yaml files, these are often used to handle programs/models' configurations

[argparse](https://docs.python.org/3/library/argparse.html)
    argparse is useful to handle inputs and to write user-friendly command-line interfaces. (*)

[click](https://click.palletsprojects.com/en/8.0.x/)
    click allows to create command line interfaces, it is more powerful than argparse.

[sqlite3](https://docs.python.org/3/library/sqlite3.html)
    sqlite3 is an interface to sqlite databases. It is easier to use than other libraries but fairly basic. (*)

[SQLalchemy](http://www.sqlalchemy.org)
    SQLalchemy is an interface to SQL based databases, including postgres, mysql and sqlite. It is very powerful but complex to use.

[requests](https://docs.python-requests.org/en/latest/)
     requests is an HTTP interface which is useful to download data from a website.

[ftplib](https://docs.python.org/3/library/ftplib.html)
    ftplib is an interface to the FTP protocol, it is useful to dowload data from a FTP server. (*)

[BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)
    BeautifulSoup is a very useful library to parse xml/html content. It can be handy when trying to download data from a website.

[tkinter](https://docs.python.org/3/library/tkinter.html)
    The tkinter package (“Tk interface”) is the standard Python interface to the Tcl/Tk GUI toolkit (*)
    
```

# Visualization 
Matplotlib is a popular and useful plotting library, it integrates seamlessly with other packages and the JupyterLab environment. As it is so widely used, there are also a lot of [third-party libraries](https://matplotlib.org/mpl-third-party/) that extend its capabilities.
Cartopy is used to visualise data on accurate maps, cartopy replaced basemap which is now considered obsolete.  
Other packages to consider are seaborn, holoviews, plotly. 

```{glossary}

[matplotlib](https://matplotlib.org)
    matplotlib is a comprehensive library for creating static, animated, and interactive visualizations

[cartopy](https://scitools.org.uk/cartopy/docs/latest/)
    cartopy helps handling and visualising cartographic data 

[cmocean](https://matplotlib.org/cmocean/)
    cmocean provides colourmaps specifically created for common oceanographic variables to use with matplotlib

[seaborn](https://seaborn.pydata.org)
    seaborn is based on matplotlib and is used to make statistical graphics

[plotly](https://plotly.com/python/)
    plotly python allows to make quality interactive graphs, plotly is at the base of [dash](https://dash.plotly.com) a framework to quickly create web data applications.

[bokeh](https://docs.bokeh.org/en/latest/)
    bokeh is a library for creating interactive visualizations for web browsers and dashboards.

[holoviews](https://holoviews.org)
    holoviews helps visualizing data for exploration rather that to create graphs

[hvPlot](https://hvplot.holoviz.org)
   hvPlot provides a high-level plotting API built on HoloViews that provides a general and consistent API for plotting data in a wide variety of formats.
 
[GeoViews]()
    GeoViews makes it easy to explore and visualize geographical, meteorological, and oceanographic datasets. It is built on holoviews

[xhistogram](https://xhistogram.readthedocs.io/en/latest/) 
    xhistogram Xhistogram makes it easier to calculate flexible, complex histograms with multi-dimensional data. It integrates with xarray and dask.

```

----COMMENT: this part is even more a work in progress than others!

# Interfaces to other software 

```{glossary}

[CDO-python](https://code.mpimet.mpg.de/projects/cdo/wiki/Cdo%7Brbpy%7D)
    CDO for python is a wrapper around the CDO binary. It parses method arguments and options, builds a command line and executes it. NB (Scott has a regridding function that exploit this)

[PyNCML](https://github.com/axiom-data-science/pyncml)
    PyNCML is a simple python library to apply NcML logic to netCDF files. (last updates were in 2017 potentially obsolete)

[PyNIO](http://www.pyngl.ucar.edu/Nio.shtml)
    PyNIO is a python interface to NCL, as NCL is currently in maintenance node (last updates were in 2019)

[PyNGL](http://www.pyngl.ucar.edu)
    PyNGL is also an interface to NCL but for visualization

```
----COMMENT!!! This section could be moved elsewhere? I'm not sure what's the best placement maybe after dealing with data formats and visualization? hence it appears twice ----

# Working in parallel
--COMMENT I've copied most of the dask definition from what Scott had in the coputations notebook. Probably, aside fro the first paragraph, the rest could be better used in an introduction/compariosn with the other libraries here.

```{glossary}

[Dask](https://docs.dask.org/en/stable/)
    Dask is a library for working with larger-than memory arrays and parallel data analysis transparently. Xarray can use Dask arrays as a backend when opening a netCDF file with the chunks attribute, and Dask has its own Pandas-like DataFrame implementation. Dask splits an array up into chunks. When doing operations on a Dask array, rather than evaluating the operation immediately Dask will create a task graph of what operations need to be run to create the output array chunks from the input array chunks. The task graph is only evaluated when results are needed (e.g. by saving to a file or creating a plot), and different chunks can be evaluated in parallel. Dask usually can work with your existing code with just small modifications, it will try to work out the best scaling based on the memory and cpus it detects on the system. You can tune dask performance using the thread/process mixture to deal with GIL-holding computations (which are rare in Numpy/Pandas/Scikit-Learn workflows). It is best to start a [dask.distributed.Client]{https://docs.dask.org/en/latest/how-to/deploy-dask/single-distributed.html} to allow Dask to process data in parallel with multiple processes.

[multiprocessing](https://docs.python.org/3/library/multiprocessing.html)
     The built-in Python multiprocessing library has low-level tools for parallel computing. You can create a 'pool' of processes, then given a function and a list of arguments it can run that function on each argument in parallel.

[mpi4py](https://mpi4py.readthedocs.io/en/stable/)
    mpi4py is an implementation of the MPI library for Python, from which you can create parallel methods the same way as Fortran sending data between processes via messages.

[xarray-beam](https://xarray-beam.readthedocs.io/en/latest/)
    xarray-beam is a library for writing Apache Beam pipelines consisting of xarray Dataset objects. This is a new module in development as part of the Pangeo stack. The main aim of xarray-beam is to provide an alternative to dask in climate data analysis cases where applying dask is not suitable or efficient. Xarray-beam aims to facilitate data transformations and analysis on large-scale multi-dimensional labeled arrays. (provide examples???)

```
