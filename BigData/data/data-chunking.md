# Chunking

## NetCDF chunking
Since NetCDF4, netCDF data supports compression on disk, as well as breaking the storage of the data down into logical "chunks". This means instead of data being written from first to last dimension (for example all longitudes for each latitude for each time step), data can rather be written with a specified chunking which should align with expected most common read patterns. 

### What is data chunking?
Large data arrays are composed of smaller units which are called *chunks*. This is why some software, like xarray, can load data lazily, i.e. load into memory only the data chunks it needs to perfom a specific operation. 
All data stored in netcdf files have been written in chunks, following some chunking strategy. NCO has a list of different chunking policies that you can apply to write a netcdf file. The most common and default approach is to prioritise accessing the data as a grid, so that retrieving all grid points at one timestep will require loading only 1 or few chunks at one time. On the other side this strategy means that often time has a chunk size of 1, i.e. each timestep is on a different chunk, which means that when we want to analysis a timeseries from the same data we will be loading the entire dataset when we are expecting to only loading one grid point.<br> 
As an example, if a file has dimensions `(time=744, lat=180, lon=360)`, the default approach would result in chunks like e.g. `(1, 180, 360)`, so that each disk read extracts an area at a single time step. If this file is expected to be used for timeseries analysis   better chunking strategy would be `(744, 1, 1)`, so that each disk read extracts all of time steps at each point location. 
For data where mixed mode analysis is required, it is best to find a chunking scheme that balances these two approaches, and results in chunk sizes that are broadly commensurate with typical on-board memory. In other words, we might pick a chunking approach like `(100, 180, 360)` which would result in chunks that are approximately `25MB`. This is reasonably computationally efficient, though could be bigger. General advice is to aim for chunks between 100-500MB, to minimise file reads while balancing with typical available memory sizes (say 8GB).
Dask has a comprehensive but accessible [blog introducing chunks](https://blog.dask.org/2021/11/02/choosing-dask-chunk-sizes), including how to choose an optimal chunk size in dask and how to align chunks to the original file chunks.


### Why chunking matters?
Chunks allow to manage memory more efficiently, and to create optimal parallelisation configurations. However, if the chunking is done in a suboptimal way, it can sometimes lead to slower computations or other negative performance outputs.
For more details on why chunking can have significant implications for performance of both data reading/writing and data computation, see [this article](https://www.unidata.ucar.edu/blogs/developer/en/entry/chunking_data_why_it_matters).

{ref}`cdo` and {ref}`nco` also offer some useful.

http://nco.sourceforge.net/nco.html#Timeseries-Reshaping-mode_002c-aka-Splitting

```{admonition} If rechunking use a multiple of original chunks
Some tools like `xarray` can re-chunk on the fly on reading data, and if the `chunks` option is passed to `xr.open_dataset` then `dask` will be used under the hood to load the data in parallel. This seems like a great idea (!) and indeed it is, however a **note of caution** is that when specifying chunking, it is important to make sure the xarray chunk specification is a multiple of that used in the file, if they are a complete mis-match performance can end up worse than a serial load! To check the size of chunks stored on disk, use `ncdump -hs`.
```

Further information: See the [Computations](https://acdguide.github.io/BigData/computations.html) section of this book.

