(python)=
# Python
This is a free, open-source language that is a standard tool used in many organisations and industries. Python is easy to learn and read, hence is popularity. It also interfaces with many other programs and tools. Compared to other languages python is slow and has high memory usage, this can become a challenge when working with big datasets. 

## Integrated Development Environments
An integrated development environment (IDE) is a tool that helps managing your workspace when working on a software code. At its most basic an IDE is an editor that understand and can highlight the programming language syntax. They can haveintegration with testing packages, version control and other developers tools. Some can be setup to work remotely (jupyterlab, VSCode). 

```{glossary}

[jupyter](https://jupyter-notebook.readthedocs.io/en/stable/)
    Jupyter Notebook is an open-source web application that allows you to create and share documents that contain live code, equations, visualizations and narrative text. Uses include data cleaning and transformation, numerical simulation, statistical modelling, data visualization, machine learning, and much more. While jupyter is a python package it supports more than 40 languages including R, Julia and of course python.

[jupyterlab](https://jupyterlab.readthedocs.io/en/stable/) 
    JupyterLab is a web-based interactive development environment for Jupyter notebooks, code, and data. JupyterLab is flexible: configure and arrange the user interface to support a wide range of workflows in data science, scientific computing, and machine learning. JupyterLab is extensible and modular: write plugins that add new components and integrate with existing ones. 

[spyder](https://www.spyder-ide.org)
    Spyder is a graphical user interface that provides an interface similar to matlab. 

[PyCharm](https://www.jetbrains.com/pycharm/)
    PyCharm is a python IDE, not all the versions are free, but a free license is available for single accademic use for the PyCharm Community editon and the [Educational](https://www.jetbrains.com/pycharm-edu/) edition. The educational edition includes python training modules. 

[VSCode](https://code.visualstudio.com)
    VSCode is a source code editor which is available for Windows, macOS and Linux. You can edit code locally, or use plugins to remotely connect to servers over SSH. It also integrates with Anaconda, letting you run Python programs in different environments. VSCode is designed to be lightweight and adaptable, so has just basic functionalities out of the box and you need to install extensions to add more. In particular, useful extensions for python are: [Python](https://marketplace.visualstudio.com/items?itemName=donjayamanne.python-environment-manager), [Pylance](https://marketplace.visualstudio.com/items?itemName=ms-python.vscode-pylance) and [Jupyter](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.jupyter).

```

## Package and environment management

A package manager is a collection of tools that automates the configuration, installation, upgrades and removal of software packages and handles dependencies. Some package managers are also environment managers as they allow users to create separate environments and handle potential conflicts between packages belonging to the same environment. An environment manager will also keep track of all the packages and versions installed, so that it's easier to reproduce the same environment again in a consistent manner.<br>
Here we cover some of the package and environment managers most used for Python. For a full list, check the Python documentation. Some managers are Python specific, such as venv, virtualenv and pipenv. The conda managers can also be used for R, Julia and many other analysis softwares.

```{warning}
Always check if a pre-defined software environment is available already! See here for {ref}`examples<condaenvs>`
```

```{glossary}

[conda](https://conda.io)
    Conda is an open source package management system and environment management system that runs on Windows, macOS and Linux. Conda quickly installs, runs and updates packages and their dependencies. Conda easily creates, saves, loads and switches between environments on your local computer. It was created for Python programs, but it can package and distribute software for any language.
    
[Anaconda](https://www.anaconda.com/)
    Anaconda contains pretty much all the python libraries you would want to get started, great for newcomers but takes up a lot of space. Not recommended on shared systems with quotas but good on local laptops. Includes Spyder, a Matlab-like programming environment (IDE).

[miniconda](https://docs.conda.io/en/latest/miniconda.html)
     A lightweight version of anaconda which by default only includes core libraries, good for building specific environments for data analysis. This underpins the `conda` modules in the `hh5` project at NCI.

[mamba](https://mamba.readthedocs.io/en/latest/)
    A reimplementation of the conda package manager in C++, mamba is a fast, robust, and cross-platform package manager.

[virtualenv](https://virtualenv.pypa.io/en/latest/) 
   Virtualenv allows to create isolated Python environments. Since Python 3.3, a basic version (venv module) is integrated into the standard library.

[pipenv](https://pipenv-fork.readthedocs.io/en/latest/)
  Pipenv creates and manages separate virtualenv in a project-based way. The project specific requirements are listed in the Pipfile, of which a locked version is automatically created once the packages are installed. Pipenv works well on Windows, which can be sometimes problematic for other tools.


COMMENT this and the community list could be removed from here and be only in the analysis-ready-data page!
If doing that, we could still leave a warning in place as above

```
It is good practice, where possible, to use existing/provided analysis environments in order to avoid generating large numbers of duplicate files. Before installing conda, for example, it's a good idea to check whether a shared conda installation and environment that serves your needs doesn't already exist. 
Some examples of managed analysis environments include:

```{dropdown} **hh5 conda environment at NCI**
The CLEX-CMS (http://climate-cms.wikis.unsw.edu.au/Conda) team maintains a [conda environment]((http://climate-cms.wikis.unsw.edu.au/Conda) on NCI that includes a wide variety of climate and weather related libraries. This includes mostly python based packages, but also community software and some custom-built NCI related command line programs, for a [full list](https://github.com/coecms/conda-envs/blob/analysis3/environment.yml). They also provide instructions on how to create your own custom conda environment using their conda installation. Additional packages can be requested via the cws_help-at-nci.org.au helpdesk.
```
```{dropdown} **dk92 conda environment at NCI**
The NCI team manage an open project, [dk92](https://opus.nci.org.au/pages/viewpage.action?pageId=134742126), that provides a module for data analysis that includes Python, and , in the latest version, R and Julia packages. This environment is updated every 3 months. It is also possible to clone notebooks with related analysis examples.
```
```{dropdown} **miniconda environment for Petrichor (CSIRO)**
Petrichor users (CSIRO employees only) can `module load miniconda3` to use a conda installation managed by IM&T. Custom conda environments and packages can be installed to a preferred location using the [`.condarc` configuration file](https://docs.conda.io/projects/conda/en/latest/user-guide/configuration/use-condarc.html#specify-env-directories)
```

## Community  

There are a few community based projects that aim to provide stacks of python packages selected for climate or related fields analysis. They often also provides examples of how to use these packages in the forms of notebooks and/or tutorials.
  
```{glossary}

[Pangeo](https://pangeo.io/)
    Pangeo is a community of people working collaboratively to develop software and infrastructure to enable Big Data geoscience research. A Pangeo environment is made of up of many different open-source software packages for ocean, atmosphere, land and climate science. 

[PyAOS](https://pyaos.github.io)
    PyAOS is a community project that offers a stack of python libraries used by the Atmosphere and Ocean Science communities.

[ProjectPythia](https://projectpythia.org/)
    Project Pythia aims to provide a public, web-accessible training resource that will help educate current, and aspiring, earth scientists to more effectively use both the Scientific Python Ecosystem and Cloud Computing to make sense of huge volumes of numerical scientific data

[EarthPy](http://earthpy.org)
    EarthPy is a collection of IPython notebooks with examples of Earth Science related Python code: tutorials, descriptions of the modules, small scripts, or just tricks. They welcome contributions.

```

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
Extract from the [package documentation](https://numpy.org/doc/stable/user/whatisnumpy.html)
::::

:::: {dropdown} [**Pandas: tabular labeled data**](https://pandas.pydata.org/docs/index.html)
Built to work with tabular and timeseries data and make use of relational and label infomration associated with the main data. Pandas is useful to slice, index, group data in complex ways. It has timeseries specific functionalities that makes working with time axis much easier. Pandas is based on numpy, it is itself the base for xarray and integrates well with other libraries. Pandas has two main data structures Series which is a 1-dimensional homogeneous array, and Dataframe which is a 2-dimensional tabular format. 
::::

:::: {dropdown} [**Xarray: multidimensional labeled data**](http://xarray.pydata.org/en/stable/#)
xarray (formerly xray) is an open source project and Python package that makes working with labeled multi-dimensional arrays simple, efficient, and fun!

Xarray introduces labels in the form of dimensions, coordinates, and attributes on top of raw NumPy-like arrays, which allows for a more intuitive, more concise, and less error-prone developer experience. The package includes a large and growing library of domain-agnostic functions for advanced analytics and visualization with these data structures.

Xarray is inspired by and borrows heavily from pandas, the popular data analysis package focused on labeled tabular data. It is particularly tailored to working with netCDF files, which were the source of xarray’s data model, and integrates tightly with dask for parallel computing.
Extract from the [package documentation](http://xarray.pydata.org/en/stable/)
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
A list of the major ones is provided by on the [package documentation](http://xarray.pydata.org/en/stable/ecosystem.html).
Some extends the formats supported by xarray, others are used for specific analysis.

(iris)=
### Iris
[Iris](https://scitools-iris.readthedocs.io/en/stable) is an alternative general analysis package to xarray. It is developed by the UK MetOffice as part of their [SciTools](https://scitools.org.uk) stack, as {term}`cartopy` and {term}`cfunits`. Iris is specifically designed for weather, climate and ocean data, so has a lot of relevant functions and examples. Iris requires CF compliance for netCDF files, as it uses these conventions as a data model.
Iris can also handle both grib (1 and 2) formats and pp binary files. These last are specific to the UK MetOffice and is what the UM atmospheric uses as a output format. 
[SciPy](https://scipy.github.io/devdocs/index.html) is a collection of mathematical algorithms and convenience functions built on NumPy. SciPy is still a core dependency of Iris but it is not as often used now for analysis on its own

### Machine learning packages

PyTorch and TensorFlow are very similar in terms of features but PyTorch is more used in research environments since it has a better memory optimisation management and allows more fine-grained control of the model structure.

```{glossary}

[aesara](https://aesara.readthedocs.io/en/latest/)
  Aesara, previously know as Theano, is used to define, evaluate and optimize mathematical expressions involving multi-dimensional arrays in an efficient manner. It optimizes the utilization of CPU and GPU and is often used in large-scale computationally intensive scientific projects, but it is simple and approachable enough to be used for smaller projects too. 

[Keras](https://keras.io)
    Keras is a high-level neural networks API for TensorFlow2. It provides essential abstractions and building blocks for developing and shipping machine learning solutions with high iteration velocity.

[PyTorch](https://pytorch.org)
    PyTorch is a ML framework based on the C library Torch. Basic data structure is a tensor. It allows to write highly customized neural network components. 

[scikit-learn](https://scikit-learn.org)
    Scikit-learn is built on top of viz., NumPy and SciPy. Scikit-learn supports most of the classical supervised and unsupervised learning algorithms. Scikit-learn can also be used for data-mining and data-analysis.
    
[TensorFlow](https://www.tensorflow.org)
    TensorFlow is developed by Google to develop and train ML models. The basic data structure is a tensor. TensorFlow can efficiently execute low-level tensor operations on CPU, GPU, TPU.

```
As machine learning is very popular there are plenty of resources available online. The [Realpython website](https://realpython.com/tutorials/machine-learning/), for example has several machine learning related tutorials.

### Climate science related packages
Some of these packages were developed for very specific tasks from members of the climate community. While their scope is limited and the packages might be maintained on a need base, they still can be very useful as they have been tailored to the community needs.

```{glossary}

[climate-indices](https://climate-indices.readthedocs.io/en/latest/)
    climate-indices provides implementations of various climate index algorithms in python.

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

### Other Packages 
---COMMENT still need intro, better title , basically I'm chucking here anything which isn't "exclusive" for climate and it's used for analysis

Also unless we have some specific to say about these packages we could just refer to several existing lists. Provided we don't need to create references to them in other part of the book.

```{glossary}

[eof](https://ajdawson.github.io/eofs/latest/)
    eof is used for EOF (empirical orthogonal functions) analysis. NB eof has also an interface for Iris.

[gsw-Python](https://teos-10.github.io/GSW-Python/)
    gsw-Python is an implementation of the Thermodynamic Equation of Seawater 2010 (TEOS-10). It is based primarily on numpy ufunc wrappers of the GSW-C implementation.
    It aims to replace python-gsw which is purely python based.
 
[metpy](https://unidata.github.io/MetPy/latest/index.html)
    MetPy is a collection of tools in Python for reading, visualizing, and performing calculations with weather data.

[Py-ART](https://arm-doe.github.io/pyart/)
    Py-ART the Python ARM Radar Toolkit is a collection of weather and radar utilities

[windspharm](https://ajdawson.github.io/windspharm/latest/)
    windspharm is a package for performing computations on global wind fields in spherical geometry.

[xrft](https://xrft.readthedocs.io/en/latest/)
    xrft is used for taking the discrete Fourier transform (DFT) on xarray and dask arrays 

```
