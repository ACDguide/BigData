# Climate data formats

## NetCDF
[NetCDF](https://www.unidata.ucar.edu/software/netcdf/) is the data format most commonly used in climate science. It is an open data format with full self-described metadata, though it is up to the data creator to ensure the metadata meets any relevant standards, such as the [CF Convention](http://cfconventions.org/Data/cf-conventions/cf-conventions-1.7/cf-conventions.html).

### NetCDF: what's under the hood?
NetCDF4 is traditionally backed by [HDF5](https://www.hdfgroup.org/solutions/hdf5/), a self-describing metadata open data format which is extensible and underpins many large-scale data formats, e.g. HDFITS.
NetCDF is the most co monly used format in climate science and related disciplines, hence it is covered in detail in this chapter.

### Impact of file structure and metadata on performance

When using self-describing data formats (such as netCDF), it is important to understand the various attributes contained in the metadata, how to interact with them, and potential issues of which to remain aware.
In particular, deflate level and chunking attributes define the way the data is stored in memory and written to disk, these can have a big impact on an analysis efficiency.
The Governance book contains guidelines on how to create netCDF files and includes lots of useful advice about [compression](_Compression_Tools) with a comprehensive list of tools to apply compression and chunking to files, some of these are specific to [NCI](https://nci.org.au/).

All of these things can have massive impacts on file performance for both regular access but in particular parallelised access using `dask` (e.g. with `xarray`). Data will be most performant with a tool like `dask` if it is structured appropriately for the read patterns, and chunk arguments supplied to `dask` must align with the chunk sizes the data is physically stored in, otherwise you can end up with *worse* performance.

We are covering some aspects of metadata [later in this section](data-metadata.md), but a lot of useful information on [metadata standards](https://acdguide.github.io/Governance/concepts/conventions.html) and [how to apply them](https://acdguide.github.io/Governance/tech/conventions.html) when creating your own netCDF files is also available in the Governance book.

## Zarr
An alternative storage back-end for NetCDF has recently been released (since v4.8.0) enabling a NetCDF to be written as a zarr.
[Zarr](https://zarr.readthedocs.io/en/stable/) is a new data format that is optimised for cloud interaction with data. Zarr is a portmanteau of "zipped archive", it is used by python's `xarray` to more performantly store data, by writing a separate file object for each data chunk. This makes data indexing and access much faster than on a monolithic NetCDF file, and performance improvements are seen on local HPC storage as well as cloud.

Tools (that we know of at this time) for converting files to zarr format include:
* `xarray`, data can be saved to NetCDF or Zarr format using the [`xarray.Dataset.to_netcdf()`](https://docs.xarray.dev/en/latest/generated/xarray.Dataset.to_netcdf.html) and [`xarray.Dataset.to_zarr()`](https://docs.xarray.dev/en/latest/generated/xarray.Dataset.to_zarr.html) functions respectively;
* [GDAL](https://gdal.org/drivers/raster/zarr.html#examples);
* `ncgen` (see NetCDF-zarr below).


### NetCDF-Zarr

Since [NetCDF v4.8.0](https://www.unidata.ucar.edu/blogs/developer/entry/overview-of-zarr-support-in), support for zarr as a storage backend has been added.

* When NetCDF is built with zarr enabled, it is possible to write a NetCDF file out in Zarr format. 
* At this stage, reading of Zarr files is not yet enabled by the python NetCDF library, but that will be added in due course.
* NetCDF 4.8.0 does not (yet) support for zip output, so at this stage, writing a NetCDF file out in zarr format creates many file objects (one per chunk)
* "nczarr" does not support unlimited dimensions like HDF5 does, as such NetCDFs must have all dimensions of defined length. 

For a description of the tests performed, see [In-depth `nczarr` testing](https://acdguide.github.io/BigData/nczarr_test.html). 

In summary, the library is functional but currently lacking, but it will be worth monitoring updates to the library in the coming years as additional capability is added.
NetCDF remains the standards-compliant file format for climate data which is broadly supported by many tools, but lacks optimal parallel performance. As this library matures it should be possible eventually to store our data in standards- and tools-compliant NetCDF format but with zarr performance.

## Other large-scale data formats

There are a few other file formats often used for large-scale data storage that we come across in climate science.

| format | comment |
|--------|---------|
| [GRIB](https://en.wikipedia.org/wiki/GRIB) | Common for distribution of weather data |
| [GeoTIFF/CoG](https://www.cogeo.org/) | Cloud optimised geoTIFF, common in satellite data |
| .zip/.tar/.tar.gz | 'zip' or 'tar' compression are often used to make data more movable |
| .pp* | Raw output from some climate models including ACCESS which is typically [converted to NetCDF](http://climate-cms.wikis.unsw.edu.au/Analysing_UM_outputs) for analysis |

