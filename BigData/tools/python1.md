(python)=
# Python
This is a free, open-source language that is a standard tool used in many organisations and industries. Python is easy to learn and read, hence is popularity. It also interfaces with many other programs and tools. Compared to other languages python is slow and has high memory usage, this can become a challenge when working with big datasets. 

## Running environment
(jupyter)=
`jupyter` - The Jupyter Notebook is an open-source web application that allows you to create and share documents that contain live code, equations, visualizations and narrative text. Uses include data cleaning and transformation, numerical simulation, statistical modelling, data visualization, machine learning, and much more.
While jupyter is a python package it supports more than 40 languages including R, Julia and of course python.
`jupyterlab` - JupyterLab is a web-based interactive development environment for Jupyter notebooks, code, and data. JupyterLab is flexible: configure and arrange the user interface to support a wide range of workflows in data science, scientific computing, and machine learning. JupyterLab is extensible and modular: write plugins that add new components and integrate with existing ones. 

## Specific toolsets

[Anaconda](https://www.anaconda.com/): Contains pretty much all the python libraries you'd want to get started, great for newcomers but takes up a lot of space. Not recommended on shared systems with quotas but good on local laptops. Includes Spyder, a Matlab-like programming environment (IDE).

[miniconda](https://docs.conda.io/en/latest/miniconda.html): A lightweight version of anaconda which by default only includes core libraries, good for building specific environments for data analysis. This underpins the `conda` modules in the `hh5` project at NCI.

(pangeo)=
[Pangeo](https://pangeo.io/): A community for analysis of large scale climate data. Built on tools like python, xarray, dask, iris, cartopy.

## GUI
[spyder](https://www.spyder-ide.org) - Spyder is a graphical user interface that provides an interface similar to matlab. 

--COMMENT: other GUI?? pycharm??

(pyanalysis)=
## Analysis
The three main python packages used in climate science are numpy, pandas and xarray. Lots of the other analysis are based of them, xarray is itself based on pandas which is based on numpy.

:::: {dropdown} [**Numpy: numerical math**](https://numpy.org/doc/stable/)
NumPy is the fundamental package for scientific computing in Python. It is a Python library that provides a multidimensional array object, various derived objects (such as masked arrays and matrices), and an assortment of routines for fast operations on arrays, including mathematical, logical, shape manipulation, sorting, selecting, I/O, discrete Fourier transforms, basic linear algebra, basic statistical operations, random simulation and much more.

At the core of the NumPy package, is the ndarray object. This encapsulates n-dimensional arrays of homogeneous data types, with many operations being performed in compiled code for performance. There are several important differences between NumPy arrays and the standard Python sequences:

* NumPy arrays have a fixed size at creation, unlike Python lists (which can grow dynamically). Changing the size of a ndarray will create a new array and delete the original.
* The elements in a NumPy array are all required to be of the same data type, and thus will be the same size in memory. The exception: one can have arrays of (Python, including NumPy) objects, thereby allowing for arrays of different sized elements.
* NumPy arrays facilitate advanced mathematical and other types of operations on large numbers of data. Typically, such operations are executed more efficiently and with less code than is possible using Python’s built-in sequences.
A growing plethora of scientific and mathematical Python-based packages are using NumPy arrays; though these typically support Python-sequence input, they convert such input to NumPy arrays prior to processing, and they often output NumPy arrays. In other words, in order to efficiently use much (perhaps even most) of today’s scientific/mathematical Python-based software, just knowing how to use Python’s built-in sequence types is insufficient - one also needs to know how to use NumPy arrays.<br>
Extract from [numpy documentation](https://numpy.org/doc/stable/user/whatisnumpy.html)
::::

:::: {dropdown} [**Pandas: tabular labeled data**](https://pandas.pydata.org/docs/index.html)
Built to work with tabular and timeseries data and make use of relational and label infomration associated with the main data. Pandas is useful to slice, index, group data in complex ways. It has timeseries specific functionalities that makes working with time axis much easier. Pandas is based on numpy, it is itself the base for xarray and integrates well with other libraries. Pandas has two main data structures Series which is a 1-dimensional homogeneous array, and Dataframe which is a 2-dimensional tabular format. 
::::

:::: {dropdown} [**Xarray: multidimensional labeled data**](http://xarray.pydata.org/en/stable/#)
xarray (formerly xray) is an open source project and Python package that makes working with labeled multi-dimensional arrays simple, efficient, and fun!

Xarray introduces labels in the form of dimensions, coordinates, and attributes on top of raw NumPy-like arrays, which allows for a more intuitive, more concise, and less error-prone developer experience. The package includes a large and growing library of domain-agnostic functions for advanced analytics and visualization with these data structures.

Xarray is inspired by and borrows heavily from pandas, the popular data analysis package focused on labeled tabular data. It is particularly tailored to working with netCDF files, which were the source of xarray’s data model, and integrates tightly with dask for parallel computing.
Extract from [http://xarray documentation](http://xarray.pydata.org/en/stable/)
Xarray depends on numpy and pandas, it can use several other python packages as optional dependencies. A full list is available on the [installation page](http://xarray.pydata.org/en/stable/getting-started-guide/installing.html) of its documentation. As long as these packages are already installed, they can be used directly from xarray. Examples are matplotlib, dask and netcdf4. The hh5 conda environments available on the NCI servers will have all these dependencies except for PyNIO and pseudonetcdf. 
::::

When to use numpy vs pandas vs xarray?<br>
As most big climate data is multidimensional and stored as netCDF files, xarray is usually the best tool to base your analysis. Still numpy and pandas can be faster than xarray for certain operations. For example pandas is faster for groupby operation and generally querying data.<br> 
As numpy is the base of the other two packages even when your analysis is not fully based on numpy, you are often dealing with numpy arrays and operations when using them.<br> 
Xarray provides several ways to convert your arrays to and from pandas dataframes and the arrays values are numpy arrays. So these three packages can and are often used interchangeably in the same analysis code.<br> 
The table below provides a schematic of the main differences, more on the reletionship between xarray and pandas is also available from the [xarray FAQ page](http://xarray.pydata.org/en/stable/getting-started-guide/faq.html).<br>

```{list-table}
:header-rows: 1
:name: np-pd-xr-table

* -  
  - Numpy
  - Pandas
  - Xarray
* - Best use
  - numerical computations
  - tabular data analysis
  - multidim labeled data analysis
* - Data structure
  - homogeneous array
  - Series (columns), Dataframes (table)
  - Labelled data arrays and datasets
* - Data input/output 
  - Read from csv, txt and simple binary files. Needs other libraries to input/output formats like netcdf, hdf5 and zarr. Can output binary, csv, txt files
  - Read/write [many formats](https://pandas.pydata.org/docs/user_guide/io.html), including hdf5, for netcdf you need other libraries
  - best tool for netcdf, including multiple files at once, includes support for openDAP and compression, chunks can easily convert arrays to pandas and numpy (http://xarray.pydata.org/en/stable/user-guide/io.html)
* - Vectorised operations
  - Yes 
  - Yes
  - Yes
* - Dimensions
  - multi-dimensional
  - 2D with multi-index support
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


As xarray gains popularity there are more and more xarray based packages.
A list of the major ones is provided by on the [xarray documentation](http://xarray.pydata.org/en/stable/ecosystem.html).
Some extends the formats supported by xarray, others are used for specific analysis.

(iris)=
### Iris
[Iris](https://scitools-iris.readthedocs.io/en/stable) is an alternative general analysis package to xarray. It is developed by the UK MetOffice as part of their [SciTools](https://scitools.org.uk) stack, as {term}`cartopy` and {term}`cfunits`. Iris is specifically designed for weather, climate and ocean data, so has a lot of relevant functions and examples. Iris requires CF compliance for netCDF files, as it uses these conventions as a data model.
Iris can also handle both grib (1 and 2) formats and pp binary files. These last are specific to the UK MetOffice and is what the UM atmospheric uses as a output format. 
[SciPy](https://scipy.github.io/devdocs/index.html) is a collection of mathematical algorithms and convenience functions built on NumPy. SciPy is still a core dependency of Iris but it is not as often used now for analysis on its own

---- Work in progress!!! ----
The following two paragraph with lists could be unified or separated in a more coherent way
This is because I'm trying to move further into separating packages based on functionality rather than origin. However, I would still like to put somewhere a "NB" this is a development package for stuff like xmhw
----- --------

### Packages for specific analysis

```{glossary}

[xrft](https://xrft.readthedocs.io/en/latest/)
    xrft is used for taking the discrete Fourier transform (DFT) on xarray and dask arrays 

[eof](https://ajdawson.github.io/eofs/latest/)
    eof is used for EOF (empirical orthogonal functions) analysis. NB eof has also an interface for Iris.

[metpy](https://unidata.github.io/MetPy/latest/index.html)
    MetPy is a collection of tools in Python for reading, visualizing, and performing calculations with weather data.

[Py-ART](https://arm-doe.github.io/pyart/)
    Py-ART the Python ARM Radar Toolkit is a collection of weather and radar utilities

[gsw-Python](https://teos-10.github.io/GSW-Python/)
    gsw-Python is an implementation of the Thermodynamic Equation of Seawater 2010 (TEOS-10). It is based primarily on numpy ufunc wrappers of the GSW-C implementation.
    It aims to replace python-gsw which is purely python based.
 
[climate-indices](https://climate-indices.readthedocs.io/en/latest/)
    climate-indices provides implementations of various climate index algorithms in python.

[windspharm](https://ajdawson.github.io/windspharm/latest/)
    windspharm is a package for performing computations on global wind fields in spherical geometry.

```
### Packages developed by climate community for specific analysis

These packages were developed for very specific tasks from members of the climate community. While their scope is limited and the packages might be maintained on a need base, they still can be very useful as they have been tailored to the community needs.

```{glossary}
[CleF](https://clef.readthedocs.io/en/stable/)
     CleF Climate Finder is a python based command line tool to discover ESGF datasets at NCI, the functions can also be loaded and called in a interactive session or script.

[climpred](https://climpred.readthedocs.io/en/stable/)
    climpred is a library that helps assessing weather and climate forecast output against a validation product.

[ClimTas](https://climtas.readthedocs.io/en/latest/)
     Climtas is a package for working with large climate analyses. It focuses on the time domain with custom functions for Xarray and Dask data.

[cmip6-preprocessing](https://cmip6-preprocessing.readthedocs.io/)
    cmip6-preprocessing offers tools for cleaning/standardization of the metadata associated with CMIP6 data files

[Cosima cookbook](https://github.com/COSIMA/cosima-cookbook)
    Cosima Cookbook is a framework for analysing model output from the ACCESS-OM2 suite of ocean-sea ice models. The model output itself is usually available at NCI where the cookbook is centrally installed in the hh5 project conda environments (possibly also the NCI python env?)

[ESMValTool](https://docs.esmvaltool.org/)
    ESMValTool is community-based a framework to perform diagnostics and performance metrics for the evaluation of CMIP models

[IOOS Compliance Checker](https://github.com/ioos/compliance-checker)
    to check netcdf files metadata against CF and ACDD conventions

[marineHeatWaves](http://www.marineheatwaves.org)
    calculate MHW statistics for one grid point, mostly numpy based

[PMP - PCMDI Metrics Package](http://pcmdi.github.io/pcmdi_metrics/index.html)
    PMP provides a variety of metrics to quickly produce summary statistics comparing climate model simulations and available observations

[xclim](https://xclim.readthedocs.io/en/stable/)
    xclim is a library of functions to compute climate indices from observations or model simulations.

[xmhw](https://github.com/coecms/xmhw)
    as marineHeatWaves but based on xarray and dask, taking care of MHW detection and statistics for a multidimensional grid.

[wrf-python](https://wrf-python.readthedocs.io/en/latest/)
    wrf-python is a collection of diagnostic and interpolation routines for use with output of the Weather Research and Forecasting (WRF-ARW) Model.

```

----COMMENT: we haven't listed yet anything in regard to machine learning

----COMMENT!!! This section could be moved elsewhere? I'm not sure what's the best placement maybe after dealing with data formats and visualization? hence it appears twice ----

## Working in parallel
(dask)=
### Dask
[dask](https://docs.dask.org/en/stable/) to parallelise tasks and manage memory more efficiently, integrates with numpy, pandas and xarray. In fact, dask arrays are built on numpy arrays and the dask dataframe is based on Pandas dataframe. Dask is a general purpose parallel programming solution.
Dask allows to scale up your code and notebooks to a cluster, 
Dask usually can work with your existing code with just small modifications, it will try to work out the best scaling based on the memory and cpus it detects on the system. You can tune dask performance using the thread/process mixture to deal with GIL-holding computations (which are rare in Numpy/Pandas/Scikit-Learn workflows)
Partition size, like if should you have 100 MB chunks or 1 GB chunks
dask support several data formats among which: HDF5, netCDF, Zarr, GRIB 
Dask is a library for working with larger-than memory arrays and parallel data analysis transparently. Xarray can use Dask arrays as a backend when opening a netCDF file with the chunks attribute, and Dask has its own Pandas-like DataFrame implementation.
Dask splits an array up into chunks. When doing operations on a Dask array, rather than evaluating the operation immediately Dask will create a task graph of what operations need to be run to create the output array chunks from the input array chunks. The task graph is only evaluated when results are needed (e.g. by saving to a file or creating a plot), and different chunks can be evaluated in parallel.
It is best to start a [dask.distributed.Client]{https://docs.dask.org/en/latest/how-to/deploy-dask/single-distributed.html} to allow Dask to process data in parallel with multiple processes.

### Other libraries

```{glossary}
[multiprocessing](https://docs.python.org/3/library/multiprocessing.html)
     The built-in Python multiprocessing library has low-level tools for parallel computing. You can create a 'pool' of processes, then given a function and a list of arguments it can run that function on each argument in parallel. 

[mpi4py](https://mpi4py.readthedocs.io/en/stable/)
    mpi4py is an implementation of the MPI library for Python, from which you can create parallel methods the same way as Fortran sending data between processes via messages.

[xarray-beam](https://xarray-beam.readthedocs.io/en/latest/)
    xarray-beam is a library for writing Apache Beam pipelines consisting of xarray Dataset objects. This is a new module in development as part of the PyAOS stack. The main aim of xarray-beam is to provide an alternative to dask in climate data analysis cases where applying dask is not suitable or efficient. Xarray-beam aims to facilitate data transformations and analysis on large-scale multi-dimensional labeled arrays. (provide examples???)
```
