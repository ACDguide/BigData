# Troubleshooting NetCDF files


All of the factors described in the previous pages, can have massive impacts on file performance for both regular access but in particular parallelised access using `dask` (e.g. with `xarray`). Data will be most performant with a tool like `dask` if it is structured appropriately for the read patterns, and chunk arguments supplied to `dask` must align with the chunk sizes the data is physically stored in, otherwise you can end up with *worse* performance. See also the [Tools](https://acdguide.github.io/BigData/tools/intro.html) section of this book for further information.


## Issues to look out for

While NetCDF tools are improving constantly, there are still some discrepancies between what the format supports, and what tools reading the data support.

A classic example is that since the move to HDF5 as a back end, NetCDF4 data can support "groups", which is sort of like a directory structure inside the file in which like variables can be stored. Almost no tools can automatically deal with data stored in this way! The same goes for "ragged arrays" where different variables have different lengths (relevant to observed track data, e.g.). 

Other issues can arise where a file has ***multiple similarly named dimensions*** (again, often around `time`). For example, [`xarray` is sometimes unable to open files](https://github.com/pydata/xarray/issues/2368) that are entirely standards compliant for this reason.

Another problem can occur ***when dimensions are reordered*** for performance - for example we may wish to write a file in which `time` is the fastest changing variable instead of the slowest, that is, convert from `(time, lat, lon)` to `(lat, lon, time)`. This does not explicitly violate any standards, however it's commonly assumed that time is the first dimension, and so some tools (e.g. `CDO`) make this assumption and [produce an error when data is structured this way](https://github.com/ACDguide/BigData/issues/15).

Files **created using MatLab** can sometimes produce unexpected results. Make sure you are aware of the defaults applied by the [`nccreate` and `ncwriteschema`](https://au.mathworks.com/help/matlab/network-common-data-form.html?s_tid=CRUX_lftnav) functions. If using the low level NetCDF library interface, you can set explicitly the format and chunksizes when calling the [`netcdf.create`](https://au.mathworks.com/help/matlab/ref/netcdf.create.html) function, if not, Matlab will use the default format. To change the latter you can use `netcdf.setDefaultFormat`.

Finally, visualisation tools like the Godiva2 ncWMS viewer in THREDDS servers occasionally don't cope with data which is not standards compliant (particulary around `standard_name`).

*Other fun "gotchas" with NetCDF data?* 

We've probably missed a few common issues here, if so, please [open an issue](https://github.com/ACDguide/BigData/issues) and if it's a common one we'll add it to this page.
