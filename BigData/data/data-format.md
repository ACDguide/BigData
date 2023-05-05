# Climate data formats

Traditionally, climate data have been stored as GRIB (GRIdded Binary) or NetCDF (Network Common Data Format) files. These file formats support large array data and metadata. NetCDF files which follow appropriate data standards (e.g. the Climate & Forecasting (CF) Conventions) can be subset to extract data over specified dimension ranges, including remotely over the internet using the [OPeNDAP protocol](https://www.opendap.org/) via THREDDS, Hyrax or PyDAP. NetCDF4 supports data "chunking" to optimise performance for particular read patterns.</br>
**See page on [netCDF](data-netcdf.md)**.

As the volume of climate datasets grows, a need for data formats that can perform more optimally with parallelised access patterns on cloud platforms and HPC has emerged. Zarr (zipped archive) is an alternate way to store climate data, whereby each netCDF "chunk" is written to a unique "object", thereby permitting much higher levels of efficient parallelised access. However, on a traditional filesystem, each chunk is written as an individual file, which can affect quota limits. To avoid this problem, the Zarr representation of a dataset may be stored in a zip file so they are reduced to a single file-object but can still perform in parallel.

Zarr is thus a format which we are particularly interested in, but its lack of widespread tool support at this time means it may limit reach if we only store data in this format.
An interesting recent development is support for zarr as a storage back-end for netCDF, thereby potentially offering the best of both worlds.</br>
**See page on [Zarr](data-zarr.md)**.

## Large-scale data formats used in climate & weather

There are a few other file formats often used for large-scale data storage that we come across in climate science.

| Format | Comment |
|--------|---------|
| [netCDF](https://www.unidata.ucar.edu/software/netcdf/) | Commonly used open format for sharing climate & weather data, supports a number of data and metadata standards |
| [GRIB](https://en.wikipedia.org/wiki/GRIB) | Common for distribution of weather and reanalysis data |
| [Zarr](https://zarr.readthedocs.io/en/stable/) | A cloud-optimised format well suited to storing large climate datasets, best used with Python |
| [GeoTIFF/CoG](https://www.cogeo.org/) | Cloud-optimised GeoTIFF, commonly used for satellite data, enables remote access and subsetting of GeoTIFF data |
| .zip/.tar/.tar.gz | 'zip' or 'tar' compression are often used to make data more movable |
| .pp* | Raw output from some climate models including ACCESS which is typically [converted to NetCDF](http://climate-cms.wikis.unsw.edu.au/Analysing_UM_outputs) for analysis |
