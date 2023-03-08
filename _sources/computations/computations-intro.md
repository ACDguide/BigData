# Large-scale data analysis

## Overview
This section includes examples of common types of computations performed on large, climate datasets. We focus on the theory of how these computations are handled on chunked datasets when using tools like `Xarray` and `Dask`. We then provide examples of the specific functions that are often used to carry out each computation and, where possible, demonstrations of these tools in use.

## How is analysis on large-scale data different from that on smaller datasets?
Large datasets are often too big to load into RAM on the computer or server that you use to do your analysis. However, we often don't need to do a computation on the entire dataset. The [section on large-scale climate data](https://acdguide.github.io/BigData/data/data-netcdf.html) discusses how large datasets are typically saved in "chunks". Analysis on large-scale datasets also makes use of these chunks in order to apply computations only on the portion of the dataset needed for that particular calculation.

An important concept for doing analysis on large datasets is the idea of "lazy computation", previously mentioned in the [data structure section](https://acdguide.github.io/BigData/data/data-structure.html). This is what the software package `Xarray` uses in conjunction with tools like `Dask`. When you read a dataset in `Xarray`, it will just read in the metadata (e.g. the variables names, the dimensions, the units, the size of each dimension, and any other metadata that the data creators provided). As you write code to do a computation, the actual calculation isn't carried out until you specify it. These are typically in the form of `.compute()`, `.load()`, etc. Only then is the computation performed by pulling in the specific chunks needed to complete the calculation. These concepts are incredibly powerful, and allow for quick analysis of big datasets!

