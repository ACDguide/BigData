# Other software

(matlab)=
## MATLAB
MATLAB (Matrix Laboratory) is a licenced tool. It is the best tool when dealing with large matrices and matrix manipulations. It allows examining the content of data quickly in a built-in docked or undocked window within the tool to gain an overview of the pattern and structures presented in the data. This tool is helpful because many data types, for example, large image files and large tabular data, can be converted into matrices and analysed efficiently in MATLAB. MATLAB provides an easy-to-use environment with interactive applications, which is excellent for novel programmers. 
As a licensed tool MATLAB might not be available to other researchers and collaborators, so even if you are producing data with MATLAB, avoid saving the data as `.mat` files, and use the best alternative open source format instead. 

### Data handling with MATLAB
MATLAB has two sets of functions to read and write netCDF files, the high-level functions simplify the process, while the low-level functions allow more control on the way the data is imported from or written to a file. 
While with recent versions of MATLAB you can import and export netCDF and OPeNDAP files without needing any external package, there are some limitations in the way netCDF support is implemented.
One of these is that MATLAB does not automatically apply scale factors and offsets, which can cause confusion with some data.
It also chooses a different HDF back end to save netCDF files according to how large they are, meaning dimension ordering and performance can be inconsistent for downstream users. 
---COMMENT this part needs still a lot of work, I'm not a Matlabl user so i put down here some of the comments, but they need to be verified. I also add this as data handling in matlab as it might be worth to cover some detail on hdf and/or other formats as we didd for python. Finally, I also found this https://www.unidata.ucar.edu/software/netcdf/software.html#CSIRO-MATLAB I'm not sure how outdated this might be, but might potentially help with the reading part at least ---- 

(julia)=
## Julia
Julia is a recent addition to the programming languages used in climate science. Julia was designed from the beginning for high performance and parallelism. It can integrate with Python, R and other languages, has a machine learning, visualization, dataframe and netcdf packages.
In Australia is becoming popular with the oceanographic community. 

```{glossary}

[DataFrames.jl](https://dataframes.juliadata.org/stable/)
   DataFrames allows tabular data manipulation with Julia, its functionality is similar to Pandas (python) and dplyr (R).

[JuliaPy](https://github.com/JuliaPy)
   JuliaPy includes interfaces to Pythin and some of its most common packages as Pandas and pyplot.

[MLJ.jl](https://alan-turing-institute.github.io/MLJ.jl/dev/)
   MLJ is a machine learning framework for Julia which includes the most commo machine learning models. 

[NetCDF.jl](https://github.com/JuliaGeo/NetCDF.jl)
    NetCDF su[port for Julia

[Plots.jl](https://docs.juliaplots.org/stable/)
   A visualization ecosystem for Julia

[RJuliaCall](https://cran.r-project.org/web/packages/JuliaCall/JuliaCall.pdf)
   JuliaCall is a R package that allows to call Julia from R 

```

(nco)=
## NCO - NetCDF Operators
[NetCDF Operators](http://nco.sourceforge.net/) is a toolkit of command-line operators to both handle and perform analysis on netCDF files. It is the tool of choice to add, rename, and modify attributes and variables. It can add internal compression to netCDF4 files and convert between different formats. It is also useful to concatenate files, performing averages and other simple mathematical operations on an entire variable, extracting, or deleting variables. Results will be automatically saved in a netCDF file and NCO can also be used to set chunking and compression.
Each NCO operation appends a line to the history attribute, meaning provenance of data created with NCO is clear and explicit. It can make the history content unwieldy, however, so it may require manual editing with the nco tools (ncatted) after other operations are performed.
You can use multiple parallel threads in NCO with ncks --thr_nbr <nthreads> ...

(cdo)=
## CDO - Climate Data Operators
[CDO](https://code.mpimet.mpg.de/projects/cdo/), like NCO, is a large command-line tool set to handle and analyse climate and weather data. CDO can also work with GRIB files, in fact, it is a useful tool to convert from GRIB to netCDF and vice versa. CDO can also be used to compress, convert, and concatenate files, often in conjunction with another operation.

One of the strengths of CDO is its ability to combine operations in succession of steps without creating intermediate files, using little additional memory in the process.

CDO is useful to calculate climatologies, regrid datasets and select subsets both spatially and temporally. Like NCO, it can be used to perform simple transformations across an entire variable. It is useful to handle time axis operations such as going from unlimited to limited dimension and setting a new reference time. CDO can integrate with other languages such as python using the `cdo` module.

For large datasets, if you can process each file independently you can parallelise using e.g., GNU Parallel or a Python multiprocessing.Pool.map.
You can use multiple parallel threads in CDO with cdo -P <nthreads> ...
Resources
cdo --operators will give a list of all available commands
cdo --help COMMAND will show detailed help

Limitations: specific versions can have issues with threading, meaning, chained commands are not always safe. CDO **cannot** be built in threadsafe mode due to underpinning HDF dependencies which means, some versions simply are not reliable and can cause random segfaults when using chained operations. This can cause data loss, so use chained operations with caution.

(fortran)=
## Fortran/C
Fortran and C can read, write, and manipulate netCDF files using their respective netCDF APIs.
To use VSCode with fortran install [Modern Fortran extension](https://marketplace.visualstudio.com/items?itemName=krvajalm.linter-gfortran).

(MPI)=
## MPI - Message Passing Interface
[MPI](https://en.wikipedia.org/wiki/Message_Passing_Interface) is a generic method of parallelising to more than one computer, it's commonly used in HPC to enable programs running on different compute nodes to communicate.
MPI is a low-level library, which makes it flexible to a lot of use cases but also more difficult to set up. Numerical models often use MPI with a program running on each compute node computing part of the model grid, the boundaries between domains being synchronised with MPI messages.
Resources
* [MPI function list](https://www.open-mpi.org/doc/current/)
* [MPI standard](https://www.mpi-forum.org/docs/)

We tried to list most of libraries and packages we know of that are used in climate science. There are many other potentially useful tools not included here.<br>
If you have suggestions for other libraries we can list here please let us know by [opening a ticket](https://github.com/ACDguide/BigData/issues/new).
