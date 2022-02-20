(matlab)=
# MATLAB
MATLAB (Matrix Laboratory) is a licenced tool. It is the best tool when dealing with large matrices and matrix manipulations. It allows examining the content of data quickly in a built-in docked or undocked window within the tool to gain an overview of the pattern and structures presented in the data. This tool is helpful because many data types, for example, large image files and large tabular data, can be converted into matrices and analysed efficiently in MATLAB. MATLAB provides an easy-to-use environment with interactive applications, which is excellent for novel programmers. 

As a licensed tool MATLAB might not be available to other researchers and collaborators, so even if you are producing data with MATLAB, avoid saving the data as `.mat` files, and use the best alternative open source format instead.

(nco)=
# NCO - NetCDF Operators
[NetCDF Operators](http://nco.sourceforge.net/) is a toolkit of command-line operators to both handle and perform analysis on netCDF files. It is the tool of choice to add, rename, and modify attributes and variables. It can add internal compression to netCDF4 files and convert between different formats. It is also useful to concatenate files, performing averages and other simple mathematical operations on an entire variable, extracting, or deleting variables. Results will be automatically saved in a netCDF file and NCO can also be used to set chunking and compression.
You can use multiple parallel threads in NCO with ncks --thr_nbr <nthreads> ...

(cdo)=
# CDO - Climate Data Operators
[CDO](https://code.mpimet.mpg.de/projects/cdo/), like NCO, is a large command-line tool set to handle and analyse climate and weather data. CDO can also work with GRIB files, in fact, it is a useful tool to convert from GRIB to netCDF and vice versa. CDO can also be used to compress, convert, and concatenate files, often in conjunction with another operation.

One of the strengths of CDO is its ability to combine operations in succession of steps without creating intermediate files, using little additional memory in the process.

CDO is useful to calculate climatologies, regrid datasets and select subsets both spatially and temporally. Like NCO, it can be used to perform simple transformations across an entire variable. It is useful to handle time axis operations such as going from unlimited to limited dimension and setting a new reference time. CDO can integrate with other languages such as python using the `cdo` module.

For large datasets, if you can process each file independently you can parallelise using e.g., GNU Parallel or a Python multiprocessing.Pool.map.
You can use multiple parallel threads in CDO with cdo -P <nthreads> ...
Resources
cdo --operators will give a list of all available commands
cdo --help COMMAND will show detailed help

Limitations: specific versions can have issues with threading, meaning, chained commands are not always safe. CDO **cannot** be built in threadsafe mode due to underpinning HDF dependencies which means, some versions simply are not reliable and can cause random segfaults when using chained operations.

(fortran)=
# Fortran/C
...

(MPI)=
# MPI - Message Passing Interface
[MPI](https://en.wikipedia.org/wiki/Message_Passing_Interface) is a generic method of parallelising to more than one computer, it's commonly used in HPC to enable programs running on different compute nodes to communicate.
MPI is a low-level library, which makes it flexible to a lot of use cases but also more difficult to set up. Numerical models often use MPI with a program running on each compute node computing part of the model grid, the boundaries between domains being synchronised with MPI messages.
Resources
* [MPI function list](https://www.open-mpi.org/doc/current/)
* [MPI standard](https://www.mpi-forum.org/docs/)

We tried to list most of libraries and packages we know of that are used in climate science. There are many other potentially useful tools not included here.<br>

If you have suggestions for other libraries we can list here please let us know by [opening a ticket](https://github.com/ACDguide/BigData/issues/new).
