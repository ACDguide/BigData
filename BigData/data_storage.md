(data-storage)=
# Data storage methods for big datasets

This page describes data storage formats and techniques in the climate realm. 
See content navigation in the right hand menu.

## About large-scale data

When working with climate data, we are often interested in datasets which are many terabytes (TB) in size, up to even the petabyte (PB) scale. Computers typically have only GBs of RAM, and so even when working on clusters and HPCs, we need to consider how to structure our data for optimal read performance by others.

Traditionally, climate data have been stored as GRIB (GRIdded Binary) or NetCDF (Network Common Data Format) files. These file formats support large array data and metadata. NetCDF files can be subset to extract data over specified dimension ranges, including remotely over the internet using the [OPeNDAP protocol](https://www.opendap.org/) via THREDDS, Hyrax or PyDAP. NetCDF4 supports data "chunking" to optimise performance for particular read patterns.

As the volume of climate datasets grows, a need for data formats that can perform more optimally with parallelised access patterns on cloud platforms and HPC has emerged. Zarr (zipped archive) is an alternate way to store climate data, whereby each netCDF "chunk" is written to a unique "object", thereby permitting much higher levels of efficient parallelised access. However, on a traditional filesystem, each chunk is written as an individual file, which can affect quota limits. To avoid this problem, the Zarr representation of a dataset may be stored in a zip file so they are reduced to a single file-object but can still perform in parallel. 

Zarr is thus a format which we are particularly interested in, but its lack of widespread tool support at this time means it may limit reach if we only store data in this format.
An interesting recent development is support for zarr as a storage back-end for netCDF, thereby potentially offering the best of both worlds.

### NetCDF
[NetCDF](https://www.unidata.ucar.edu/software/netcdf/) is the data format most commonly used in climate science. It is an open data format with full self-described metadata, though it is up to the data creator to ensure the metadata meets any relevant standards, such as the [CF Convention](http://cfconventions.org/Data/cf-conventions/cf-conventions-1.7/cf-conventions.html).

#### NetCDF: what's under the hood?
NetCDF4 is traditionally backed by [HDF5](https://www.hdfgroup.org/solutions/hdf5/), a self-describing metadata open data format which is extensible and underpins many large-scale data formats, e.g. HDFITS.

An alternative storage back-end for netCDF has recently been released (since v4.8.0) enabling a netCDF to be written as a zarr.

### Zarr
[Zarr](https://zarr.readthedocs.io/en/stable/) is a new data format that is optimised for cloud interaction with data. Zarr is a portmanteau of "zipped archive", it is used by python's `xarray` to more performantly store data, by writing a separate file object for each data chunk. This makes data indexing and access much faster than on a monolithic netCDF file, and performance improvements are seen on local HPC storage as well as cloud.

Tools (that we know of at this time) for converting files to zarr format include:
* [xarray](http://xarray.pydata.org/en/stable/generated/xarray.Dataset.to_zarr.html)
* [GDAL](https://gdal.org/drivers/raster/zarr.html#examples)
* `ncgen` (see netCDF-zarr below)

### Other large-scale data formats

There are a few other file formats often used for large-scale data storage that we come across in climate science.

| format | comment |
|--------|---------|
| [GRIB](https://en.wikipedia.org/wiki/GRIB) | Common for distribution of weather data |
| [GeoTIFF/CoG](https://www.cogeo.org/) | Cloud optimised geoTIFF, common in satellite data |
| .zip/.tar/.tar.gz | 'zip' or 'tar' compression are often used to make data more movable |
| .pp* | Raw output from some climate models including ACCESS which is typically [converted to netCDF](http://climate-cms.wikis.unsw.edu.au/Analysing_UM_outputs) for analysis |


## Advice on writing datasets for efficient use

CLEX CMS team have produced [some very useful advice](http://climate-cms.wikis.unsw.edu.au/NetCDF_Compression_Tools) about storage of netCDF data. That page details 
* deflation, 
* shuffling, 
* chunking, and 
* command line tools to control file storage structure (some are specific to [NCI](https://nci.org.au/)).

All of these things can have massive impacts on file performance for both regular access but in particular parallelised access using `dask` (e.g. with `xarray`). Data will be most performant with a tool like `dask` if it is structured appropriately for the read patterns, and chunk arguments supplied to `dask` must align with the chunk sizes the data is physically stored in, otherwise you can end up with *worse* performance. See also the [Tools](https://acdguide.github.io/BigData/tools/intro.html) section of this book for further information.

If working in `xarray`, data can be saved to NetCDF or Zarr format using the [`xarray.Dataset.to_netcdf()`](https://docs.xarray.dev/en/latest/generated/xarray.Dataset.to_netcdf.html) and [`xarray.Dataset.to_zarr()`](https://docs.xarray.dev/en/latest/generated/xarray.Dataset.to_zarr.html) functions respectively.

Also consider things like data and metadata standards and ease of use for other researchers or data consumers. For example, data should be [CF-compliant](http://cfconventions.org/Data/cf-conventions/cf-conventions-1.7/cf-conventions.html), but also consider [ACDD](https://wiki.esipfed.org/Attribute_Convention_for_Data_Discovery_1-3) for metadata, [UGRID](https://ugrid-conventions.github.io/ugrid-conventions/) for unstructured data, and [CMOR](https://pcmdi.github.io/cmor-site/) and other data request requirements for Earth System Grid Federation data submission.

## NetCDF-Zarr

Since [netCDF v4.8.0](https://www.unidata.ucar.edu/blogs/developer/entry/overview-of-zarr-support-in), support for zarr as a storage backend has been added.

* When netCDF is built with zarr enabled, it is possible to write a netCDF file out in Zarr format. 
* At this stage, reading of Zarr files is not yet enabled by the python NetCDF library, but that will be added in due course.
* netCDF 4.8.0 does not (yet) support for zip output, so at this stage, writing a netCDF file out in zarr format creates many file objects (one per chunk)
* "nczarr" does not support unlimited dimensions like HDF5 does, as such netCDFs must have all dimensions of defined length. 

For a description of the tests performed, see [In-depth `nczarr` testing](https://acdguide.github.io/BigData/nczarr_test.html). 

In summary, the library is functional but currently lacking, but it will be worth monitoring updates to the library in the coming years as additional capability is added.
NetCDF remains the standards-compliant file format for climate data which is broadly supported by many tools, but lacks optimal parallel performance. As this library matures it should be possible eventually to store our data in standards- and tools-compliant netCDF format but with zarr performance.

## Pangeo Forge: an open source framework for extraction, transformation, and loading of scientific data

[Pangeo Forge](https://pangeo-forge.readthedocs.io/en/latest/index.html) is a combination of two things, with the ultimate goal of uploading datasets into the cloud in an analysis-ready, cloud-optimized (ARCO) format:

1. Pangeo Forge Recipes - an open source Python package, which allows you to create and run extraction, transformation, and loading pipelines (“recipes”) and run them from your own computer
2. Pangeo Forge Cloud - a cloud-based automation framework which runs these recipes in the cloud from code stored in GitHub

Pangeo Forge is inspired directly by Conda Forge, a community-led collection of recipes for building conda packages (see the [Python Tools page](https://acdguide.github.io/BigData/tools/python1.html#python) for more info on conda). Pangeo Forge seeks to play the same role for datasets.

### When to use Pangeo Forge

Pangeo Forge is useful if you have access to some data, and would like to work with the data on the cloud. It is optimized for multidimensional array data (e.g. NetCDF, GRIB, Zarr) that can be opened with Xarray. To upload a dataset to the cloud via Pangeo Forge, a user should submit a Pull Request to the Pangeo Forge [staged-recipes](https://github.com/pangeo-forge/staged-recipes) GitHub repository, following the [introductory guide in their documentation](https://pangeo-forge.readthedocs.io/en/latest/introduction_tutorial/index.html).