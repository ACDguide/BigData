# Data formats, variables, and metadata

When using self-describing data formats (such as netCDF), it is important to understand the various attributes contained in the metadata, how to interact with them, and potential issues of which to remain aware.

### Simple tools for viewing netCDF data and metadata 
The `ncdump` command (contained within the standard netcdf library) is the most common way of probing netCDF files, particularly metadata attributes.

- Dimensions are listed first in ncdump, and descibe the overall structure of the data within. These dimensions are not self-described, and therefore must also exist as a variable whose values and attributes then describe the associated dimension. Dimensions can be 'real' dimensions (such as time, lat, lon), or 'pseudo' dimensions (such as land-use tiles, or spectral bands), and can be cardinal, continous or nominal, i.e. described using values or strings (such as a numbered or named list, or floating point values).

- Variables contain the bulk of the information with a netCDF file. Each is defined along one or more dimension, and is self-described with associate attributes. The variable attributes can technically include and be titled anything, however there are some common standards to which most data adheres (the most common of which is the [CF conventions](http://cfconventions.org/)), including bounds, units, standard_name, etc.

- Global attributes descibe the entire dataset. While these are typically chosen according to the use case of the data and can vary significantly between modelling realms or scientific need, standards also exist for these. Common global attributes include title, data provenance information (i.e. where the data came from), license, and contact information, as well as any conventions implemented in the file.

The `ncview` tool, while very simple, can easily display netCDF data and highlight metadata issues. This is because `ncview` looks for standard metadata attributes to quickly decide how to plot the data. Files that are missing vital attributes, or are otherwise lacking adequate description will quickly break `ncview`, allowing for a fast diagnostic tool for both metadata quality and the data itself. Note that being a basic tool, it can not handle reprojections of non-cartesian grids, so plots may not *look* right, this tool is used to sanity check data, not assess its quality.


### Common metadata issues
#### Time dimension
- UNLIMITED/record dimensions

Not all file formats support unlimited dimensions, and conversely netCDF4 supports exactly one unlimited dimension (the underlying HDF5 format supports multiple unlimited dimensions). Zarr does not support unlimited dimensions so conversion between the two can be an issue if this capability is important.

- Calendars

Calendar issues are possibly the most common cause of frustration with climate data! Not all models use a gregorian calendar, which can make comparing data across models tricky. Furthermore not all calendars found in netCDF files are necessarily recognised by the datetime libraries commonly used in tools. It may sometimes be necessary to resample data into a recognised calendar, and tools like `xarray` support a `calendar` argument where a non-standard calendar is used.

- Units

The CF convention requires that time be defined with respect to a reference date, using unambiguous units. That is, "seconds since", "days since", "years since" as appropriate. Some time units are not valid, for example "months since" is not a valid time unit as it is ambiguous whether a month is a fixed number of days, or always refers to a particular date in a month. Nevertheless, some seasonal data does use this unit, which can cause issues.

- Long timespans

Even if a calendar are units are clearly defined, not all datetime libraries are capable of dealing with dates far in the past or future. In this case it may be necessary to explicitly use "CFtime" when decoding a file in python, for example (e.g. in `xarray`, using `decode_cf=True`).

#### Missing values



#### Coordinates and grids


#### Scaling & offsets


### Chunking



### Metadata standards
Information about metadata standards and conventions can be found in the [Climate Data Guidelines](https://acdguide.github.io/Governance/concepts/conventions.html).
