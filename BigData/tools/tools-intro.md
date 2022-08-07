# Available tools and their best use

There are many languages and packages available to analyse and handle data. Each has its own strengths and weaknesses, but this section of the book aims to help identify which languages/tools are best suited to specific tasks.

To identify the best tool/s for a task the following factors should be considered:

- the data: most languages and packages provide the same or overlapping functionalities, so often the data format, size and the way it is structured determines which tool is best.
- the kind of analysis: some operations can be performed with several different tools, but for specific tasks you might need a specific tool, and the performance might vary significantly for different operations.
- user experience: the previous experience matters, acquiring new skills is good but can take time, it can also be safer to stick with the same tool as you get to know its limitations better. The community experience is also important, i.e. it can be useful to have others around you that use the same software and can provide code and help. 

Included are:
* []{tools-python1}
* []{tools-rcran}
* {ref}`matlab`
* {ref}`nco`
* {ref}`cdo`

Other languages and tools exist which can work with netCDF data (e.g., C, FORTRAN, ArcGIS, QGIS, paraview, panoply, Ferret, as well as the deprecated NCL), but on this page we focus on tools commonly used for *analysis* of large scale climate data (typically netCDF).

# Analysis environments

It is good practice, where possible, to use existing/provided analysis environments in order to avoid generating large numbers of duplicate files. Before installing [conda](https://docs.conda.io/en/latest/), for example, it's a good idea to check whether a shared conda installation and environment that serves your needs doesn't already exist. Some examples of managed analysis environments include:
- the [CLEX-CMS](http://climate-cms.wikis.unsw.edu.au/Conda) team maintain a conda Python environment on NCI that includes a wide variety of climate- and weather-related libraries. They also provide instructions on how to create your own custom conda environment using their conda installation.
- the NCI team manage an open project, [dk92](https://opus.nci.org.au/pages/viewpage.action?pageId=134742126), that provides a module for data analysis that integrates Python, R and Julia platforms together with hundreds of pre-built packages.
- Petrichor users (CSIRO employees only) can `module load miniconda3` to use a conda installation managed by IM&T. Custom conda environments and packages can be installed to a preferred location using the [`.condarc` configuration file](https://docs.conda.io/projects/conda/en/latest/user-guide/configuration/use-condarc.html#specify-env-directories)
