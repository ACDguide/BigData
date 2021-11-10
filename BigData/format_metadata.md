# Data formats, variables, and metadata

When using self-describing data formats (such as netCDF), it is important to understand the various attributes contained in the metadata, how to interact with them, and potential issues of which to remain aware.

### Simple tools for viewing netCDF data and metadata 
The `ncdump` command (contained within the standard netcdf module) is the most common way of probing netCDF files, particularly metadata attributes.

- Dimensions are listed first in ncdump, and descibe the overall structures of the data within. These dimensions are not self-described, and therefore must also exist as a variable whose values and attributes then describe the associated dimension. Dimensions can be 'real' dimensions (such as time, lat, lon), or 'pseudo' dimensions (such as land-use tiles, or spectral bands), and can be described using values or strings.
E.g. TBA

- Variables contain the bulk of the information with a netCDF file. Each is defined along one or more dimension, and is self-described with associated attributes. The attributes can technically include and be titled anything, however there are some common standards to which most data adheres (the most common of which is the CF conventions \[link\]), including bounds, units, standard_name, etc.

- Global attributes are those that descibe the entire dataset. While these are typically chosen according to the use case of the data and can vary significantly between modelling realms or scientific need, standards also exist for these. Common global attributes include data provenance information (i.e. where the data came from), license, and contact information.

The `ncview` tool, while very simple, can easily display netCDF data and highlight metadata issues. This is because `ncview` looks for standard metadata attributes to quickly decide how to plot the data. Files that are missing vital attributes, or are otherwise lacking adequate description will quickly break `ncview`, allowing for a fast diagnostic tool for both metadata quality and the data itself.


### Common metadata issues
#### Time dimension
- UNLIMITED/record dimensions
- Calendars
- Units

#### Missing values


#### Coordinates and grids


#### Scaling & offsets


### Chunking



### Metadata standards
Information about metadata standards and conventions can be found in the [Climate Data Guidelines](https://acdguide.github.io/Governance/concepts/conventions.html).
