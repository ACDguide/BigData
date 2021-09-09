# Data storage methods for big datasets

This page describes data storage formats and techniques in the climate realm. 
See content navigation in the right hand menu.

## About large-scale data

When working with climate data, we are often interested in datasets which are many terabytes (TB) in size, up to even the petabyte (PB) scale. Computers typically have only GBs of RAM, and so even when working on clusters and HPCs, we need to consider how to structure our data for optimal read performance by others.

Traditionally, climate data have been stored as GRIB (GRIdded Binary) or NetCDF (Network Common Data Format) files. These file formats support large array data and metadata. NetCDF files can be subset to extract data over specified dimension ranges, including remotely over the internet using the [OPeNDAP protocol](https://www.opendap.org/) via THREDDS, Hyrax or PyDAP. NetCDF4 supports data "chunking" to optimise performance for particular read patterns.

With the rise of cloud computing, the need for data formats that can perform more optimally with parallelised access patterns has emerged. Zarr (zipped archive) is an alternate way to store climate data, whereby each netCDF "chunk" is written to a unique "object", thereby permitting much higher levels of efficient parallelised access. However, on a traditional filesystem, each chunk is written as an individual file, which can affect quota limits. To avoid this problem, Zarr representations of dataset may be stored in a zip file so they are reduced to a single file-object, without loss of performance with tools like `xarray`. 

Zarr is thus a format which we are particularly interested in, but it's lack of widespread tool support at this time means it may limit reach if we only store data in this format.
An interesting recent development is support for zarr as a storage back-end for netCDF, thereby potentially offerring the best of both worlds.

### NetCDF
[NetCDF](https://www.unidata.ucar.edu/software/netcdf/) is the data format most commonly used in climate science. It is an open data format with full self-described metadata, though it is up to the data creator to ensure the metadata meets any relevant standards, such as the [CF Convention](http://cfconventions.org/Data/cf-conventions/cf-conventions-1.7/cf-conventions.html).

### Zarr
[Zarr](https://zarr.readthedocs.io/en/stable/) is a new data format that is optimised for cloud interaction with data. Zarr is a portmanteau of "zipped archive", it is used by python's Xarray to more performantly store data, by writing a separate file object for each data chunk. This makes data indexing and access much than on a monolithic netCDF file, and performance improvements are seen on local HPC storage as well as cloud.

### NetCDF: what's under the hood?
NetCDF is traditionally backed by [HDF5](https://www.hdfgroup.org/solutions/hdf5/), a self-describing metadata open data format which is extensible and underpins many large-scale data formats, e.g. HDFITS 
An alternative storage back-end for netCDF has recently been released (since 4.8.0) enabling a netCDF to be written as a zarr.

### Other large-scale data formats

There are a few other file formats often used for large-scale data storage.

| format | comment |
|--------|---------|
| GRIB | Common in weather data|
| GeoTIFF/CoG | Cloud optimised geoTIFF, common in satellite data |
| Zip | zip or tar are often used to make data more movable |

TO BE EXPANDED

## Advice on writing large datasets efficiently

CLEX CMS team have produced [some useful advice](http://climate-cms.wikis.unsw.edu.au/NetCDF_Compression_Tools) about storage of netCDF data. That page details 
* deflation, 
* shuffling, 
* chunking, and 
* command line tools to control file storage structure (some are specific to [NCI](https://nci.org.au/)).

All of these things can have massive impacts on file performance for both regular access but in particular parallelised access using `dask`. Data will only be performant with a tool like `dask` if it is structured appropriately for the read patterns, and chunk arguments supplied to `xarray` must align with the chunk sizes the data is physically stored in, otherwise you can end up with *worse* performance.

Also consider things like data and metadata standards and ease of use for other researchers or data consumers. For example, data should be [CF-compliant](http://cfconventions.org/Data/cf-conventions/cf-conventions-1.7/cf-conventions.html), but also consider [ACDD](https://wiki.esipfed.org/Attribute_Convention_for_Data_Discovery_1-3) for metadata, [UGRID](https://ugrid-conventions.github.io/ugrid-conventions/) for unstructured data, and [CMOR](https://pcmdi.github.io/cmor-site/) and other data request requirements for Earth System Grid Federation data submission.

**Possible links to elsewhere in this book?**

## NetCDF-Zarr

Since [netCDF v4.8.0](https://www.unidata.ucar.edu/blogs/developer/entry/overview-of-zarr-support-in), support for zarr as a storage backend has been added.

* When netCDF is built with zarr enabled, it is possible to write a netCDF file out in Zarr format. 
* At this stage, reading of Zarr files is not yet enabled by the python NetCDF library, but that will be added in due course.
* netCDF 4.8.0 does not (yet) support for zip output, so at this stage, writing a netCDF file out in zarr format creates many file objects (one per chunk)
* "nczarr" does not support unlimited dimensions like HDF5 does, as such netCDFs must have all dimensions of defined length. 

### What we found
**TL;DR: It kind of works, but not in a practical sense. It will be worth montioring updates to the netCDF zarr library over the next couple of years.**

I took an existing netCDF file and attempted to convert it to zarr so we could test the conversion mechanism and performance.
This test was done in project `p66`, members of that project should be able to see it: `/scratch/p66/ct5255/nczarr`. 

*I attempted to write an existing netCDF file, which I knew would be fully compliant with all relevant standards, to Zarr format.* 

0. Could not generate a zarr directly from the netCDF because it was not consistent with current nczarr limitations.
`ncgen -4 -lb -o "file:///scratch/p66/ct5255/nczarr/tas_Amon_ACCESS-CM2_historical_r1i1p1f1_gn_185001-201412_test.ncz#mode=nczarr,file" /g/data/fs38/publications/CMIP6/CMIP/CSIRO-ARCCSS/ACCESS-CM2/historical/r1i1p1f1/Amon/tas/gn/v20191108/tas_Amon_ACCESS-CM2_historical_r1i1p1f1_gn_185001-201412.nc`
fails (you can test this yourself, just `module load netcdf/4.8.0`).

1. I took a netCDF file, dumped it to its plain ASCII representation (.cdl), 
`ncdump /g/data/fs38/publications/CMIP6/CMIP/CSIRO-ARCCSS/ACCESS-CM2/historical/r1i1p1f1/Amon/tas/gn/v20191108/tas_Amon_ACCESS-CM2_historical_r1i1p1f1_gn_185001-201412.nc > tas_Amon_hist_ACCESS-CM2_test.cdl`
This needed to be edited to change the time dimension from `UNLIMITED` to the actual length of the time dimension in that file (`1980` in this case).

2. I then used `ncgen` to convert it to zarr:
`ncgen -4 -lb -o "file:///scratch/p66/ct5255/nczarr/tas_Amon_ACCESS-CM2_historical_r1i1p1f1_gn_185001-201412_test.ncz#mode=nczarr,file" tas_Amon_hist_ACCESS-CM2_test.cdl` (per vague instructions at https://www.unidata.ucar.edu/blogs/developer/en/entry/overview-of-zarr-support-in).
This produces a directory which looks like a filename 

`drwxr-s--- 10 ct5255 p66     16384 Jul 16 16:58 tas_Amon_ACCESS-CM2_historical_r1i1p1f1_gn_185001-201412_test.ncz` 

which contains each dimension and variable as subdirectories, which in terms contain numbered files representing each data chunk.
This can (as far as I can tell), be read with `xarray.open_zarr` 

3. It seems that it is possible to handle zipped zarr files (at least on read), presumably also on write? However, I found this was not supported, and indeed NCI investigated the build for this module and it seemed like it couldn't be enabled in the current version.
So this is not possible 
`ncgen -4 -lb -o "file:///scratch/p66/ct5255/nczarr/tas_Amon_ACCESS-CM2_historical_r1i1p1f1_gn_185001-201412_test2.ncz#mode=nczarr,zip" tas_
Amon_hist_ACCESS-CM2_test.cdl`
`ncgen: NetCDF: Attempt to use feature that was not turned on when netCDF was built.
        (../../ncgen/genbin.c:genbin_netcdf:63)`

Findings: 
* It is not viable to translate existing netCDF to zarr-backed netCDF, because we liberally use unlimited time dimensions.
* It is not a good idea to write much data to zarr format using this tool at the moment because it doesn't support the zip archiving, so uses a lot of inodes, which will affect quotas.
* It is important to watch this library as it (rapidly) evolves to better supoort zarr read/write.

NetCDF remains the standards-compliant file format for climate data which is broadly supported by many tools, but lack optimal parallel performance. As this library matures it should be possible eventually to store our data in standards- and tools-compliant netCDF format but with zarr performance.

