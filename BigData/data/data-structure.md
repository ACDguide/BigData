# Data structure on disk

The way data is written on and read from disk can influence the efficiency and speed of data analysis.
When looking at climate data the three main components of a file structure are:

* data blocks
* headers
* encoding/conventions 

## Blocks
Data is stored physically on disk in smaller units which we will refer to as `blocks`.
Depending on how the file is structured a variable might be stored on a different number of blocks and when accessing a subset of it, the blocks to be read might or not be stored contiguously.
When working on big data arrays it is important to be aware of their underlying structure, more detail on how to do this will be shown when looking at the [netCDF format](data-netcdf.md).

```{admonition} **Chunks**
Data blocks are usually referred to as `chunks` in netCDF, dask and xarray
```

Data blocks and the way they are stored in memory, can have massive impacts on file performance in particular when accessing data and/or performing analysis in parallel. Software like `xarray`, can load data lazily, i.e., load into memory only the data chunks it needs to perform a specific operation. Data will be most performant with a tool like `dask` if it is structured appropriately for the read patterns, and chunk arguments supplied to `dask` must align with the chunk sizes the data is physically stored in, otherwise you can end up with *worse* performance.

## Headers
Some of the file formats used for climate data are self-describing, i.e. they have a header storing metadata information, like units, variable names etc. This is used by software developers to recognise dimensions and different aspects of the data and treat them accordingly.

## Encoding and conventions
For the headers to be effective and useful, metadata conventions have been developed to have consistent data definitions. For example, netCDF uses the CF conventions, GRIB files have their own encoding of which the current version is GRIB2.
