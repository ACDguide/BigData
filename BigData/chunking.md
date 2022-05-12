# Data Chunking

The previous section about [computations with large datasets](https://acdguide.github.io/BigData/computations.html) shows diagrams with various operations computed on chunks of data. Here, we will go into more depth on what chunking is, why it matters, and some real-world examples.

## What is data chunking?

Large data arrays are composed of smaller units which are called *chunks*. This is why some software, like xarray, can load data lazily, i.e. load into memory only the data chunks it needs to perfom a specific operation. 
All data stored in netcdf files have been written in chunks, following some chunking strategy. NCO has a list of different chunking policies that you can apply to write a netcdf file. The most common and default approach is to prioritise accessing the data as a grid, so that retrieving all grid points at one timestep will require loading only 1 or few chunks at one time. On the other side this strategy means that often time has a chunk size of 1, i.e. each timestep is on a different chunk, which means that when we want to analysis a timeseries from the same data we will be loading the entire dataset when we are expecting to only loading one grid point. 
Dask has a comprehensive but accessible [blog introducing chunks](https://blog.dask.org/2021/11/02/choosing-dask-chunk-sizes), including how to choose an optimal chunk size in dask and how to align chunks to the original file chunks.

## Why chunking matters

Chunks allow to manage memory more efficiently, and to create optimal parallelisation configurations. However, if the chunking is done in a suboptimal way, it can sometimes lead to slower computations or other negative performance outputs.
For more details on why chunking can have significant implications for performance of both data reading/writing and data computation, see [this article](https://www.unidata.ucar.edu/blogs/developer/en/entry/chunking_data_why_it_matters). 

## Chunking in the real world

Examples will be added here soon!

### Simple function to retrieve file chunks

[This blog](https://climate-cms.org/posts/2021-07-29-coarsen_climatology.html) includes a simple function to retrieve a netcdf file chunks.

### Using map_blocks

Dask provides the dask.array.map_blocks() function that allows you to run a function on every chunk of an array.
The last section of [this blog](https://climate-cms.org/posts/2021-11-24-api.html?highlight=chunk#pure-dask-advanced) shows an example of how to use map_blocks()

### Chunks effects on parallel computations with dask

This [parallel training](https://coecms-training.github.io/parallel/dask-intro.html) has many references to chunks and their effects on computation in its dask and case studies sections.
!!!Any more example on map_blocks would be brillinat as it is hard to find them!
