# Analysis ready data 


## Local data collections and tools

data projects on gadi and related tool

(condaenvs)=
### Pre-defined conda environments
It is good practice, where possible, to use existing/provided analysis environments in order to avoid generating large numbers of duplicate files. Before installing conda, for example, it's a good idea to check whether a shared conda installation and environment that serves your needs doesn't already exist.
Some examples of managed analysis environments include:

```{dropdown} **hh5 conda environment at NCI**
The CLEX-CMS (http://climate-cms.wikis.unsw.edu.au/Conda) team maintains a [conda environment]((http://climate-cms.wikis.unsw.edu.au/Conda) on NCI that includes a wide variety of climate and weather related libraries. This includes mostly python based packages, but also community software and some custom-built NCI related command line programs, for a [full list](https://github.com/coecms/conda-envs/blob/analysis3/environment.yml). They also provide instructions on how to create your own custom conda environment using their conda installation. Additional packages can be requested via the cws_help-at-nci.org.au helpdesk.
```
```{dropdown} **dk92 conda environment at NCI**
The NCI team manage an open project, [dk92](https://opus.nci.org.au/pages/viewpage.action?pageId=134742126), that provides a module for data analysis that includes Python, and , in the latest version, R and Julia packages. This environment is updated every 3 months. It is also possible to clone notebooks with related analysis examples.
```
```{dropdown} **miniconda environment for Petrichor (CSIRO)**
Petrichor users (CSIRO employees only) can `module load miniconda3` to use a conda installation managed by IM&T. Custom conda environments and packages can be installed to a preferred location using the [`.condarc` configuration file](https://docs.conda.io/projects/conda/en/latest/user-guide/configuration/use-condarc.html#specify-env-directories)
```

### Cosima cookbook
...

### STAC collection example?
COMMENT we're mentioning STAC in previous pages but it's not clear to me where it is used in climate science.

### NCI and acs-replica intake catalogues
...

## Remote resources

### Thredds catalogues with opendap access

### Community projects

There are a few community based projects that aim to provide stacks of python packages selected for climate or related fields analysis. They often also provides examples of how to use these packages in the forms of notebooks and/or tutorials.

```{glossary}

[Pangeo](https://pangeo.io/)
    Pangeo is a community of people working collaboratively to develop software and infrastructure to enable Big Data geoscience research. A Pangeo environment is made of up of many different open-source software packages for ocean, atmosphere, land and climate science.

[PyAOS](https://pyaos.github.io)
    PyAOS is a community project that offers a stack of python libraries used by the Atmosphere and Ocean Science communities.

[ProjectPythia](https://projectpythia.org/)
    Project Pythia aims to provide a public, web-accessible training resource that will help educate current, and aspiring, earth scientists to more effectively use both the Scientific Python Ecosystem and Cloud Computing to make sense of huge volumes of numerical scientific data

[EarthPy](http://earthpy.org)
    EarthPy is a collection of IPython notebooks with examples of Earth Science related Python code: tutorials, descriptions of the modules, small scripts, or just tricks. They welcome contributions.
```

### Pangeo Forge: an open source framework for extraction, transformation, and loading of scientific data

[Pangeo Forge](https://pangeo-forge.readthedocs.io/en/latest/index.html) is a combination of two things, with the ultimate goal of uploading datasets into the cloud in an analysis-ready, cloud-optimized (ARCO) format:

1. Pangeo Forge Recipes - an open source Python package, which allows you to create and run extraction, transformation, and loading pipelines (“recipes”) and run them from your own computer
2. Pangeo Forge Cloud - a cloud-based automation framework which runs these recipes in the cloud from code stored in GitHub

Pangeo Forge is inspired directly by Conda Forge, a community-led collection of recipes for building conda packages (see the [Python Tools page](https://acdguide.github.io/BigData/tools/python1.html#python) for more info on conda). Pangeo Forge seeks to play the same role for datasets.

#### When to use Pangeo Forge

Pangeo Forge is useful if you have access to some data, and would like to work with the data on the cloud. It is optimized for multidimensional array data (e.g. NetCDF, GRIB, Zarr) that can be opened with Xarray. To upload a dataset to the cloud via Pangeo Forge, a user should submit a Pull Request to the Pangeo Forge [staged-recipes](https://github.com/pangeo-forge/staged-recipes) GitHub repository, following the [introductory guide in their documentation](https://pangeo-forge.readthedocs.io/en/latest/introduction_tutorial/index.html).

