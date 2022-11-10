# Methods of accessing data and metadata

## STAC (SpatioTemporal Asset Catalog)

> "Enabling online search and discovery of geospatial assets"

[STAC](https://stacspec.org) is a specification aimed at standardizing the language around geospatial datasets in order to increase accessability and interoperability across many datasets. It is for use with data stored in the cloud. 

### Making data easily searchable
At its core, `STAC` provides a [JSON](https://www.json.org/json-en.html) file wrapper around any geospatial data (i.e. any data relating to the Earth). The goal of this wrapper file is to contain all relevant information that a user may want to search for when finding a dataset. In this way, `STAC` seeks to make all earth-related cloud-optimized datasets easily searchable, but does not provide the search tools themselves. `STAC` integrates well with a tool like `intake` that can search for and load the desired datasets.

(intake)=
## Intake

> "Taking the pain out of data access and distribution"

[Intake](https://intake.readthedocs.io/en/latest/) is a python package used to find, investigate, load, and disseminate data. 


### Data loading
`Intake` can be used to load many different types of data formats (e.g. tabular data, multi-dimensional data, etc.) into a python notebook or script using familiar containers (e.g. Pandas dataframes, Xarray DataArrays, etc.). Intake can work on local, remote, or cloud computing infrastructures, and is relatively fast due to its ability to integrate distirbuted computing (e.g. Dask).

There are a number of [plugins](https://intake.readthedocs.io/en/latest/plugin-directory.html#plugin-directory) that currently exist for different types of data. Several that may be of particular interest to the climate science community are:

- [intake-esm](https://intake-esm.readthedocs.io/en/latest/): for ESM catalog datasets (including CMIP and CESM datasets)
- [intake-geopandas](https://github.com/intake/intake_geopandas): for reading in geospatial datasets into a [geopandas](https://geopandas.org) dataframe
- [intake-stac](https://intake-stac.readthedocs.io/en/latest/): for SpatioTemporal Asset Catalogs ([STAC](https://stacspec.org)); the [Pangeo Data Catalog](https://pangeo.io/catalog.html) uses STAC to catalog its cloud-based datasets
- [intake-thredds](https://intake-thredds.readthedocs.io/en/latest/): to retrieve data from THREDDS data servers (used by NCAR for example)
- [intake-xarray](https://intake-xarray.readthedocs.io/en/latest/): to load datasets into [Xarray](http://xarray.pydata.org/en/stable/) containers

```{admonition} **Intake data catalogues at NCI**
There are currently two Intake catalogues listing climate data hosted at NCI:
* the [nci-intake-catalogue from CLEX](https://github.com/coecms/nci-intake-catalogue/tree/main/docs) covering CMIP6, CMIP5, CORDEX, ERA5, ERA-Interim and Cosima dataset
* a catalogue for the [Australian Community Reference Climate Data Collection](https://github.com/aus-ref-clim-data-nci/acs-replica-intake/blob/main/acs-replica-demo.ipynb), which includes several climate reference data
Their documentation includes demo on how to use access the data in the catalogue (links above).
```

####Other features
Intake (and many of the plugins) can also be used for a few other tasks:
* cataloging system for listing data sources, metadata, and parameters
* convenience functions that can be used to, among other things, distribute data catalogs
* investigate data sources and create plots using a GUI

#### Working with authorised catalogues
There is no direct way for intake to open and load authorised catalogues. Including the username and password in the URL helps open the catalogue, but further data processing using Dask generates an error message relating to the "nonnumeric port" in the URL. Nikhil Garg (Data61, CSIRO) advises on creating a .netrc file and a .dodsrc file in the home directory to resolve this issue. These files are used by the netCDF4 library; when intake uses Xarray and Dask to access the files, netCDF4 is utilised in the background. The .netrc file contains details about the machine, username, and password. It is a single-line file that can be written in the following format: machine machine_name login user_ID password user_password. The .dodsrc file points to the location of the .netrc file, and can be written as a single-line: HTTP.NETRC=YourHomeDirectory/.netrc. Note, when working on NCI OOD, these files need to be created in the OOD home directory. Hint: According to Nikhil, this method works for any tool (e.g., R, cdo, nco).
#### Intake and distributed client
Intake is not working currently with a distributed client. You can avoid using distributed client or open the data catalogue link with Xarray to work around this issue, e.g.,
ds = xarray.open_dataset("catalogue_link", chunks={'lat': 100, 'lon': 100, 'time': 500})

<div class="alert alert-info" role="alert">
<b>For dataset users:</b> if an intake catalog is already set up on your system, then all you need to know if the code to access the dataset you are interested in!
</div>

<div class="alert alert-warning" role="alert">
  <b>For dataset maintainers:</b> more specialized knowledge will be necessary.
</div>

