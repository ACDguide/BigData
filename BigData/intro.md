# Overview

COMMENT this should become 
 better overview of what's in the book, how to use it if:
-  you're a newbie
- you're looking for specific advice
- you're familiar with climate science analysis moving from a different system/language etc.  


This Jupyter Book is a compilation of resources and best practices for climate scientists in Australia who work with big/challenging datasets. This book is intended for both dataset creators and dataset users. While the focus is on Australian climate scientists, many of the resources listed here are generally applicable.

:::{note}
This Jupyter Book is currently under development, so expect frequent updates.
:::

This Jupyter Book was created as part of a working group formed at the Australian Meteorological and Oceanographic Society's Annual Conference 2021. Please see the goals below and governance (to be added soon) for more information about the working group.

This is meant to be a collaborative resource, and we welcome contributions! Instructions on how to contribute can be found in the book's [github repository](https://github.com/ACDguide/BigData).


# Working Group Goals

## Ultimate Goal:
**Create a best practices framework for climate-related scientists in Australia working with big/challenging datasets.**

## Definition of big/challenging datasets: 
“Big/challenging/very large data”: when the size and complexity of the dataset is such that “traditional data-processing application software” is inadequate to handle the analysis or management of the dataset ([Wikipedia page for “Big data”](https://en.wikipedia.org/wiki/Big_data); [Pangeo FAQ](https://pangeo.io/faq.html))

## Steps to achieve our goal:
- Consolidate a list of software, tools, learning/training modules, relevant documentation, and other resources that currently exist for dealing with, specifically for storing, accessing, and analyzing, big data.
- Identify weak points/missing gaps in existing resources
- Expand documentation, create learning modules, etc. to fill the identified gaps in currently available resources
- Communicate with other working groups (e.g. on data management guidelines) on issues related to improving data organization


### Topics to include in the above steps:
- Methods of data storage (e.g. compression techniques)
- Methods of accessing data and metadata quickly (e.g. `intake` example from Scott)
- Interpreting data format, variables, metadata (e.g. grids, chunking)
- Carrying out computations on large datasets (e.g. using `xarray` and `dask`)
- Identifying which languages/tools are best suited to specific tasks
