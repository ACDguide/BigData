# Data storage methods for big datasets

Putting information about data storage techniques here.

## About large-scale data

NetCDF, Zarr and why we use them

### NetCDF
[NetCDF](https://www.unidata.ucar.edu/software/netcdf/) is the data format most commonly used in climate science. It is an open data format with full self-described metadata, though it is up to the data creator to ensure the metadata meets any relevant standards, such as the [CF Convention](http://cfconventions.org/Data/cf-conventions/cf-conventions-1.7/cf-conventions.html).

### Zarr
[Zarr](https://zarr.readthedocs.io/en/stable/) is a new data format that is optimised for cloud interaction with data. Zarr is a portmanteau of "zipped archive", it is used by python's Xarray to more performantly store data, by writing a separate file object for each data chunk. This makes data indexing and access much than on a monolithic netCDF file, and performance improvements are seen on local HPC storage as well as cloud.

### NetCDF: what's under the hood?
NetCDF is traditionally backed by [HDF5](https://www.hdfgroup.org/solutions/hdf5/), a self-describing metadata open data format which is extensible and underpins many large-scale data formats, e.g. HDFITS 
An alternative storage back-end for netCDF has recently been released enabling a netCDF to be written as a zarr.

### Other large-scale data formats

Also, Grib, GeoTIFF (COG), zip, others?

## Advice on writing large datasets efficiently

CLEX CMS team have produced [some useful advice](http://climate-cms.wikis.unsw.edu.au/NetCDF_Compression_Tools) about storage of netCDF data. That page details deflation, shuffling, chunking, and command line tools to control file storage structure (some are specific to NCI).

Also consider things like data and metadata standards and ease of use for other researchers or data consumers.
Possible links to elsewhere in this book.

## NetCDF-Zarr

Since [netCDF v4.8.0](https://www.unidata.ucar.edu/blogs/developer/entry/overview-of-zarr-support-in), support for zarr as a storage backend has been added.

Write about what we found. (it kind of works, but not in a practical sense)
