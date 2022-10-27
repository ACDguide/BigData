# In-depth `nczarr` testing

This page documents tests performed to understand the newly released [netCDF v4.8.0](https://www.unidata.ucar.edu/blogs/developer/entry/overview-of-zarr-support-in) library with Zarr back-end support.

* When netCDF is built with Zarr enabled, it is possible to write a netCDF file out in Zarr format. 
* At this stage, reading of Zarr files is not yet enabled by the python netCDF library, but that will be added in due course.
* netCDF 4.8.0 does not (yet) support zip output, so at this stage, writing a netCDF file out in Zarr format creates many file objects (one per chunk)
* "nczarr" does not support unlimited dimensions like HDF5 does, as such netCDFs must have all dimensions of defined length. 

**TL;DR: It kind of works, but not in a practical sense. It will be worth montioring updates to the netCDF Zarr library over the next couple of years.**

## Description of tests performed

We tested `nczarr` on NCI's Gadi system.

I took an existing netCDF file and attempted to convert it to Zarr so we could test the conversion mechanism and performance.
This test was done in project `p66`, members of that project should be able to see it: `/scratch/p66/ct5255/nczarr`. 

Thanks to Dale Roberts at NCI for installing a new version of netCDF on Gadi to support these tests and rebuilding with a bunch of different flags as I tried and failed at things in this process!

*I attempted to write an existing netCDF file, which I knew would be fully compliant with all relevant standards, to Zarr format.* 

1. Could not generate a Zarr directly from the netCDF because it was not consistent with current nczarr limitations. <br>
```ncgen -4 -lb -o "file:///scratch/p66/ct5255/nczarr/tas_Amon_ACCESS-CM2_historical_r1i1p1f1_gn_185001-201412_test.ncz#mode=nczarr,file" /g/data/fs38/publications/CMIP6/CMIP/CSIRO-ARCCSS/ACCESS-CM2/historical/r1i1p1f1/Amon/tas/gn/v20191108/tas_Amon_ACCESS-CM2_historical_r1i1p1f1_gn_185001-201412.nc``` <br>
fails (you can test this yourself, just `module load netcdf/4.8.0`).

2. I dumped the input netCDF file to its plain ASCII representation (.cdl), <br>
```ncdump /g/data/fs38/publications/CMIP6/CMIP/CSIRO-ARCCSS/ACCESS-CM2/historical/r1i1p1f1/Amon/tas/gn/v20191108/tas_Amon_ACCESS-CM2_historical_r1i1p1f1_gn_185001-201412.nc > tas_Amon_hist_ACCESS-CM2_test.cdl``` <br>
This needed to be edited to change the time dimension from `UNLIMITED` to the actual length of the time dimension in that file (`1980` in this case).

3. I then used `ncgen` to convert it to Zarr: <br>
```ncgen -4 -lb -o "file:///scratch/p66/ct5255/nczarr/tas_Amon_ACCESS-CM2_historical_r1i1p1f1_gn_185001-201412_test.ncz#mode=nczarr,file" tas_Amon_hist_ACCESS-CM2_test.cdl``` <br> (per vague instructions at https://www.unidata.ucar.edu/blogs/developer/en/entry/overview-of-zarr-support-in). <br>
This produces a directory which looks like a filename <br>
```drwxr-s--- 10 ct5255 p66     16384 Jul 16 16:58 tas_Amon_ACCESS-CM2_historical_r1i1p1f1_gn_185001-201412_test.ncz```<br>
and contains each dimension and variable as subdirectories, which in turn contain numbered files representing each data chunk.<br>
This can (I believe), be read with `xarray.open_zarr` 

4. It seems that it is possible to handle zipped Zarr files (at least on read), presumably also on write? However, I found this was not supported, and indeed NCI investigated the build for this module and it seemed like it couldn't be enabled in the current version.
So this is not possible <br>
`ncgen -4 -lb -o "file:///scratch/p66/ct5255/nczarr/tas_Amon_ACCESS-CM2_historical_r1i1p1f1_gn_185001-201412_test2.ncz#mode=nczarr,zip" tas_
Amon_hist_ACCESS-CM2_test.cdl`</br>
`ncgen: netCDF: Attempt to use feature that was not turned on when netCDF was built. 
(../../ncgen/genbin.c:genbin_netcdf:63)`

## Findings: 
* It is not viable to translate existing netCDF to Zarr-backed netCDF, because we liberally use unlimited time dimensions.
* It is not a good idea to write much data to Zarr format using this tool at the moment because it doesn't support the zip archiving, so uses a lot of inodes, which will affect quotas.
* It is important to watch this library as it (rapidly) evolves to better supoort Zarr read/write.


## Other people's tests

* IOOS compliance checker support for NCzarr [Git pull request](https://github.com/ioos/compliance-checker/pull/884).
