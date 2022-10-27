# About large-scale data

When working with climate data, we are often interested in datasets which are many terabytes (TB) in size, up to even the petabyte (PB) scale. Computers typically have only GBs of RAM, and so even when working on clusters and HPCs, we need to consider how to structure our data for optimal read performance by others.
In this chapter we are covering the most commonly used data formats for climate data, with a focus on NetCDF and advice on how to write and handle NetCDF files efficiently.

**Index**

* [Climate data formats](data-format.md)
   - NetCDF and Zarr
* [](data-advice.md)
   - [NetCDF metadata](data-metadata.md)
   - [Chunking and compression](data-chunking.md)
   - [](data-troubleshooting.md) 
