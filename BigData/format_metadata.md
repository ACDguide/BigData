# Data formats, variables, and metadata

When using self-describing data formats (such as netCDF), it is important to understand the various attributes contained in the metadata, how to interact with them, and potential issues of which to remain aware.

## Simple tools for viewing netCDF data and metadata 
The `ncdump` command (contained within the standard netcdf library) is the most common way of probing netCDF files, particularly metadata attributes.

- Dimensions are listed first in ncdump, and descibe the overall structure of the data within. These dimensions are not self-described, and therefore must also exist as a variable whose values and attributes then describe the associated dimension. Dimensions can be 'real' dimensions (such as time, lat, lon), or 'pseudo' dimensions (such as land-use tiles, or spectral bands), and can be cardinal, continous or nominal, i.e. described using values or strings (such as a numbered or named list, or floating point values).

- Variables contain the bulk of the information with a netCDF file. Each is defined along one or more dimension, and is self-described with associate attributes. The variable attributes can technically include and be titled anything, however there are some common standards to which most data adheres (the most common of which is the [CF conventions](http://cfconventions.org/)), including bounds, units, standard_name, etc.

- Global attributes descibe the entire dataset. While these are typically chosen according to the use case of the data and can vary significantly between modelling realms or scientific need, standards also exist for these. Common global attributes include title, data provenance information (i.e. where the data came from), license, and contact information, as well as any conventions implemented in the file.

The `ncview` tool, while very simple, can easily display netCDF data and highlight metadata issues. This is because `ncview` looks for standard metadata attributes to quickly decide how to plot the data. Files that are missing vital attributes, or are otherwise lacking adequate description will quickly break `ncview`, allowing for a fast diagnostic tool for both metadata quality and the data itself. Note that being a basic tool, it can not handle reprojections of non-cartesian grids, so plots may not *look* right, this tool is used to sanity check data, not assess its quality.


## Common metadata issues
### **Time dimension**
- **UNLIMITED/record dimensions**

Unlimited dimensions provide a data creator the opportunity to extend their dataset by appending new content to the file without having to rewrite the entire file (assuming the tools used permit `append` mode). This is particularly useful for datasets where we might want to add new observational data at later points in time, for example. Not all file formats support unlimited dimensions, and conversely netCDF4 supports exactly one unlimited dimension (the underlying HDF5 format supports multiple unlimited dimensions). Zarr does not support unlimited dimensions so conversion between the two can be an issue if this capability is important. It is common for netCDF files to contain `time` as an unlimited dimension (see [our netCDF-Zarr testing](https://acdguide.github.io/BigData/nczarr_test.html) for an example).

- **Calendars**

Calendar issues are possibly the most common cause of frustration with climate data! Not all models use a gregorian calendar, which can make comparing data across models tricky. Furthermore not all calendars found in netCDF files are necessarily recognised by the datetime libraries commonly used in tools. It may sometimes be necessary to resample data into a recognised calendar, and tools like `xarray` support a `calendar` argument where a non-standard calendar is used.

- **Units**

The CF convention requires that time be defined with respect to a reference date, using unambiguous units. That is, "seconds since", "days since", "years since" as appropriate. Some time units are not valid, for example "months since" is not a valid time unit as it is ambiguous whether a month is a fixed number of days, or always refers to a particular date in a month. Nevertheless, some seasonal data does use this unit, which can cause issues.

- **Long timespans**

Even if calendar units are clearly defined, not all datetime libraries are capable of dealing with dates far in the past or future. In this case it may be necessary to explicitly use "CFtime" when decoding a file in python, for example (e.g. in `xarray`, using `decode_cf=True`).

### **Missing values**

NetCDF files should not contain "`NaN`" values. Indeed, each variable should include a `_FillValue` attribute which defines the numerical value of NaN in this dataset. Common values used are `-999` and `1e+20`. By setting this variable attribute, netCDF-aware tools are able to decode files appropriately restoring a NaN mask where these values are recorded in the file, meanwhile the file itself contains only valid numerical values.

Some datasets, for example those derived from observations, may differentiate between NaN values, and missing values. Sometimes the value `0` is recorded for "no data" which can cause problems for variables like precipitation. It is important that the user is aware of how these concerns are handled in the dataset they are using.

### **Coordinates and grids**

While often climate models are run on a regular cartesian grid, sometimes other grids are used in models which can cause confusion, both in terms of plotting and the coordinates used. A few examples follow.

- **Tripolar grids**
Ocean models are often run on a tripolar grid to avoid the existence of a singularity at the North Pole. Instead a global grid is constructed with 3 poles all under continental land masses to permit non-ambiguous hydrodynamic modelling throughout the ocean basins. It may be necessary to "regrid" such data to a cartesian grid if it is to be combined with atmospheric data. Tools to do this include `xESMF` in python, as well as `NCO`, `CDO` and `GDAL`.
Further information: See the [Computations](https://acdguide.github.io/BigData/computations.html) section of this book.

- **Unstructured grids**
Some models use mesh grids which conform to coastlines to permit higher resolution in areas of particular interest without needing to run the whole model at high resolution. In this case the `UGRID` convention may also be used, in which the file dimensions are required to specify mesh nodes, edges and faces and their connectivity. When plotting data on an unstructured grid, reprojection may be required, though tools like `cartopy` typically make this task straightforward.

- **Coordinates of non-cartesian grids**
While a climate model using cartesian coordinates will usually have both dimensions and variables of "latitude" and "longitude", for other grid types it is typical to have other dimensions which are then mapped in 2 dimensions to latitude and longitude. There are many nomenclatures used for this, but it is not uncommon for a model to use dimensions like `i`, `j`, `nx`, `ny`, etc to define grid position, and then define variables `latitude(i,j)` to be a mapping of the coordinates onto projectable values.

### **Scaling & offsets**

NetCDF has long supported using scale factors and offsets to reduce required precision in data storage, i.e., reducing required disk space. These capabilities are rarely used these days, but some tools will still automatically write netCDFs that optimise disk use through applying a `scale_factor` (multiplicative correction) and `add_offset` (additive correction) in order to store data using less bits. Most tools like `NCO` and python's `xarray` are netCDF-aware and apply these corrections on loading, however MATLAB's netCDF operators do not automatically account for these corrections and they need to be applied manually.

The scale_factor and add_offset form a linear equation of the form `y=mx+c`, where the true value of the data `dt` is found by the stored value `ds` multiplied by the `scale_factor`, and to this value we add the `add_offset`. 

It is uncommon to need to worry about these corrections, but the hint that they may exist and not be applied on data load is if a sanity check of the data produces values that seem wildly wrong and often by a consistent factor.

**NOTE** this is NOT used to convert between Celsius and Farenheit or Kelvin temperatures (although it could be!) so when working with data for which multiple common units exist, the cause of a failed sanity check is more likely that the units are not those expected.

## Chunking

Since netCDF v4, netCDF data supports compression on disk, as well as breaking the storage of the data down into logical "chunks". This means instead of data being written from first to last dimension (for example all longitudes for each latitude for each time step), data can rather be written with a specified chunking which should align with expected most common read patterns. 

Data with dimensions `(time, lat, lon)` that is expected to be used for timeseries analysis most often should be stored with chunks like e.g. `(744, 1, 1)`, so that each disk read extracts a lot of time steps at each point location. Conversely data that is used for spatial analysis is better stored with chunks like e.g. `(1, 180, 360)`, so that each disk read extracts an area at a single time step. For data where mixed mode analysis is required, it is best to find a chunking scheme that balances these two approaches, and results in chunk sizes that are broadly commensurate with typical on-board memory. In other words, we might pick a chunking approach like `(100, 180, 360)` which would result in chunks that are approximately `25MB`. This is reasonably computationally efficient, though could be bigger. General advice is to aim for chunks between 100-500MB, to minimise file reads while balancing with typical available memory sizes (say 8GB).

Some tools like `xarray` can re-chunk on the fly on reading data, and if the `chunks` option is passed to `xr.open_dataset` then `dask` will be used under the hood to load the data in parallel. This seems like a great idea (!) and indeed it is, however a **note of caution** is that when specifying chunking, it is important to make sure the xarray chunk specification is a multiple of that used in the file, if they are a complete mis-match performance can end up worse than a serial load! To check the size of chunks stored on disk, use `ncdump -hs`.

Further information: See the [Computations](https://acdguide.github.io/BigData/computations.html) section of this book.

## Metadata standards
Information about metadata standards and conventions can be found in the [Climate Data Guidelines](https://acdguide.github.io/Governance/concepts/conventions.html).

## Other issues to look out for

While netCDF tools are improving constantly, there are still some discrepancies between what the format supports, and what tools reading the data support.

A classic example is that since the move to HDF5 as a back end, netCDF4 data can support "groups", which is sort of like a directory structure inside the file in which like variables can be stored. Almost no tools can automatically deal with data stored in this way! The same goes for "ragged arrays" where different variables have different lengths (relevant to observed track data, e.g.). 

Other issues can arise where a file has ***multiple similarly named dimensions*** (again, often around `time`). For example, [`xarray` is sometimes unable to open files](https://github.com/pydata/xarray/issues/2368) that are entirely standards compliant for this reason.

Another problem can occur ***when dimensions are reordered*** for performance - for example we may wish to write a file in which `time` is the fastest changing variable instead of the slowest, that is, convert from `(time, lat, lon)` to `(lat, lon, time)`. This does not explicitly violate any standards, however it's commonly assumed that time is the first dimension, and so some tools (e.g. `CDO`) make this assumption and [produce an error when data is structured this way](https://github.com/ACDguide/BigData/issues/15).

Files **created using MatLab** can sometimes produce unexpected results. Make sure you are aware of the defaults applied by the [`nccreate` and `ncwriteschema`](https://au.mathworks.com/help/matlab/network-common-data-form.html?s_tid=CRUX_lftnav) functions. If using the low level NetCDF library interface, you can set explicitly the format and chunksizes when calling the [`netcdf.create`](https://au.mathworks.com/help/matlab/ref/netcdf.create.html) function, if not, Matlab will use the default format. To change the latter you can use `netcdf.setDefaultFormat`.

Finally, visualisation tools like the Godiva2 ncWMS viewer in THREDDS servers occasionally don't cope with data which is not standards compliant (particulary around `standard_name`).

*Other fun "gotchas" with netCDF data?* 

We've probably missed a few common issues here, if so, please [open an issue](https://github.com/ACDguide/BigData/issues) and if it's a common one we'll add it to this page.
