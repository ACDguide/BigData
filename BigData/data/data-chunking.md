# NetCDF chunking and compression 

## Chunking

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

## Compression

The NetCDF4 format also introduced the ability of adding internal compression to the files. 

### Why compress NetCDF files?

**Saving storage**<br>
  Data if often stored on a shared filesystem, so it is each user's responsibility to manage the available storage efficiently. Compressing NetCDF data files can shrink them to one third of their original size. This is the equivalent of being given three times as much disk space.

**NetCDF compression is lossless**<br>
 The data is exactly as it was when read from disk. It can still be read using the same programming interface. As long as the program reading the data has been compiled with the latest NetCDF library (version 4) then the task of decompressing the data is handled by the library and as far as the programs are concerned there is no difference in the data. The usual tools, such as ncdump, can be used to examine the variables contained within the NetCDF file.

Compression with tools such as gzip is possible but is not recommended except for archival purposes. It has the disadvantage that the file must be decompressed to be read and then recompressed again when finished, which can be time consuming, not to mention the data in question will take up much more room while it is being analysed.

### How does it work?
The NetCDF library has several options for compressing data, which all compression programs will use, as they all use the underlying library to perform the compression. There is a more detailed explanation if you wish to understand more, but briefly:

**Deflate level**

This is an integer value from ranging from 0 to 9. A value of 0 means no compression, and 9 is the highest level of compression possible. The higher this value the smaller your file will be once compressed. However, there is a trade-off, the higher the deflate level, the longer it will take to compress, particularly so with very high deflate levels. At deflate level 9 it can take six times longer to compress the data, with only a few percent improvement in compression. The recommended deflate level is 5. This combines good compression with a small increase in compression time.

**Shuffle**

Turn shuffle on. Simple. It usually results in a smaller compressed file with little performance overhead.

Finally `chunking` also plays a part in the compression process, in order to use NetCDF compression the data must be chunked. Many tools just adopt the NetCDF library default chunking strategy. For many NetCDF versions the default strategy has been to create chunks that are simply the same size as the grid dimensions of the variable and using size 1 for `time` when present. This can be a disastrous choice in terms of performance if the data is also compressed, as the entire variable/grid must be read into memory to be uncompressed even if only a single slice is required.

```{warning}
COMMENT: to be fixed!!!
If dealing with a file with contiguous storage, it is necessary to first chunk the file and then apply compression. Otherwise the compression operation will fail.
The error messag ewill vary depending on the tool used, for example with nccopy
`NetCDF: Bad chunk sizes.`
cdo -f nc4 -z zip_5 input-file output-file  might work as cdo will impose chunking before compressing
Another way around this is to use xarry to rewrite the file with the desire encoding, getting a better control of chunking and compression at the same time
```

## Compression tools

There are several tools that can be used to compress NetCDF data, and compression can also be added when creating a fileusing the interfaces to the NetCDF library available for common programming languages like Python, fortran and MatLab.

**nccopy**

One of the standard tools included in a NetCDF installation is [nccopy](https://docs.unidata.ucar.edu/nug/current/netcdf_utilities_guide.html#guide_nccopy). `nccopy` can compress files and define the chunking using a command line argument (-c). nccopy is a good option if your data file structure changes little, so a chunking scheme can be decided upon and hard coded into scripts. It is not so useful if the dimensions and variables change. Another major limitation is that the chunking is defined by dimensions, not variables. If your data file has variables that share dimensions, but have different combinations or numbers of dimensions it is not possible to determine an optimal chunking strategy for each variable.

```{code}
nccopy -k nc4 -d 5 -s input.nc output.nc 
```
**NCO**

The [NetCDF Operator](http://nco.sourceforge.net/nco.html#Compression) program suite can compress NetCDF files and has recently included the option to choose different [chunking strategies](http://nco.sourceforge.net/nco.html#Chunking). NCO provides lots of strategies for both chunking and compressing, refer to their doucmentation for more details. 
COMMENT this might not be anymore the case!!!
However, it is not possible to use their optimised chunking strategy for variables with four dimensions or more.

```{code}
ncks -D 5 input.nc output.nc
```

**CDO**

[Climate Data Operators](https://code.mpimet.mpg.de/projects/cdo/embedded/index.html#x1-70001.2.1) can also compress NetCDF, `shuffle` is automatically enabled and it offers limited chunking options: auto (default), grid or lines.

```{code}
cdo -f nc4 -z zip_5 {-k grid} copy inpput.nc output.nc
```

**nccompress**

The `nccompress` package is available at NCI, as part of the CLEX-CMS conda environment. At present it consists of four python-based programs: `ncfind`, `nc2nc`, `nccompress` and `ncvarinfo`. nccompress can copy NetCDF files with compression and an optimised chunking strategy that has reasonable performance for many datasets. Its two main limitations: it is slower than the other programs, and it can only compress NetCDF3 or NetCDF4 classic format. There is more detail in the following sections.
The `ncfind` utility is used to find NetCDF files and discriminate between compressed and uncompressed ones.
For more information refer to the [original documentation](http://climate-cms.wikis.unsw.edu.au/NetCDF_Compression_Tools#General_guidelines)




