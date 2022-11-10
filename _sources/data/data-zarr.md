# Zarr
An alternative storage back-end for NetCDF has recently been released (since v4.8.0) enabling a NetCDF to be written as a zarr.
[Zarr](https://zarr.readthedocs.io/en/stable/) is a new data format that is optimised for cloud interaction with data. Zarr is a portmanteau of "zipped archive", it is used by python's `xarray` to more performantly store data, by writing a separate file object for each data chunk. This makes data indexing and access much faster than on a monolithic NetCDF file, and performance improvements are seen on local HPC storage as well as cloud.

Tools (that we know of at this time) for converting files to zarr format include:
* `xarray`, data can be saved to NetCDF or Zarr format using the [`xarray.Dataset.to_netcdf()`](https://docs.xarray.dev/en/latest/generated/xarray.Dataset.to_netcdf.html) and [`xarray.Dataset.to_zarr()`](https://docs.xarray.dev/en/latest/generated/xarray.Dataset.to_zarr.html) functions respectively;
* [GDAL](https://gdal.org/drivers/raster/zarr.html#examples);
* `ncgen` (see NetCDF-zarr below).


## NetCDF-Zarr

Since [NetCDF v4.8.0](https://www.unidata.ucar.edu/blogs/developer/entry/overview-of-zarr-support-in), support for zarr as a storage backend has been added.

* When NetCDF is built with zarr enabled, it is possible to write a NetCDF file out in Zarr format. 
* At this stage, reading of Zarr files is not yet enabled by the python NetCDF library, but that will be added in due course.
* NetCDF 4.8.0 does not (yet) support for zip output, so at this stage, writing a NetCDF file out in zarr format creates many file objects (one per chunk)
* "nczarr" does not support unlimited dimensions like HDF5 does, as such NetCDFs must have all dimensions of defined length. 

For a description of the tests performed, see [In-depth `nczarr` testing](https://acdguide.github.io/BigData/nczarr_test.html). 

In summary, the library is functional but currently lacking, but it will be worth monitoring updates to the library in the coming years as additional capability is added.
NetCDF remains the standards-compliant file format for climate data which is broadly supported by many tools, but lacks optimal parallel performance. As this library matures it should be possible eventually to store our data in standards- and tools-compliant NetCDF format but with zarr performance.

```{admonition} **zarrdump**
`zarrdump` works similarly to ncdump but to display the content of a zarr file from teh command line 
```
