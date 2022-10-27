# About large-scale data

When working with climate data, we are often interested in datasets which are many terabytes (TB) in size, up to even the petabyte (PB) scale. Computers typically have only GBs of RAM, and so even when working on clusters and HPCs, we need to consider how to structure our data for optimal read performance by others.

Traditionally, climate data have been stored as GRIB (GRIdded Binary) or NetCDF (Network Common Data Format) files. These file formats support large array data and metadata. NetCDF files can be subset to extract data over specified dimension ranges, including remotely over the internet using the [OPeNDAP protocol](https://www.opendap.org/) via THREDDS, Hyrax or PyDAP. NetCDF4 supports data "chunking" to optimise performance for particular read patterns.

As the volume of climate datasets grows, a need for data formats that can perform more optimally with parallelised access patterns on cloud platforms and HPC has emerged. Zarr (zipped archive) is an alternate way to store climate data, whereby each netCDF "chunk" is written to a unique "object", thereby permitting much higher levels of efficient parallelised access. However, on a traditional filesystem, each chunk is written as an individual file, which can affect quota limits. To avoid this problem, the Zarr representation of a dataset may be stored in a zip file so they are reduced to a single file-object but can still perform in parallel.

Zarr is thus a format which we are particularly interested in, but its lack of widespread tool support at this time means it may limit reach if we only store data in this format.
An interesting recent development is support for zarr as a storage back-end for netCDF, thereby potentially offering the best of both worlds.

In this chapter we are covering the most commonly used data formats for climate data, with a focus on NetCDF and advice on how to write and handle NetCDF files efficiently.

**Index**

