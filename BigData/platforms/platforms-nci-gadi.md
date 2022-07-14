(Gadi)=
## NCI Gadi HPC

The NCI's peak system "Gadi" is a large high performance computer ideal for numerical modelling and computationally intensive jobs. This HPC supports the ACCESS climate model that contributes to CMIP submissions as well as development by ACCESS staff at ACCESS-NRI, BoM, CSIRO and university collaborators. NCI's Gadi system is also used to run regional models such as WRF and CCAM, as well as many field-specific models (CABLE, WaveWatchIII, AWRA to name a few). 

The HPC is accessed via the command line and does not have a graphical user interface. Tasks are described in "job scripts" which are submitted to a batch queuing system to ensure fair use of the resources. No internet access is available from Gadi compute nodes, so all downloads/repository cloning needs to be done on login nodes.

NCI provides:
- a `/home` directory for each user where code can be stored (but typically not data), 
- a `/scratch` area for each project which is a highly performant filesystem appropriate for IO intensive jobs, 
- a `/g/data` global filesystem space for most projects for longer term storage of data such as for sharing and publication,
- the `MDSS` mass datastore facility for data archiving/backup.
- help via help@nci.org.au as well as specialised climate support from CLEX, BoM and CSIRO support staff via cws-help@nci.org.au.

### When would I use this system? 
- Gadi is intended for large scale modelling and data processing jobs. If you are running parallel numerical models at scale then you would likely work exclusively on Gadi or a similar system. It is also useful for "high throughput computing" like AI/ML workflows, and "high performance data" parallel data processing using Pangeo tools like python's `dask` library - though usually you would start exploratory data work on the NCI's OOD (see below) and only move to Gadi when greater scalability is required.
- NCI is an ideal platform to use when collaborating with peers from other institutions as it is available to all Australian researchers. 
- If you are wanting to use large reference climate datasets as input for your research, chances are the data you want may already be available at NCI, which saves you time downloading and managing the datasets, and it is more efficient for everyone to use these centralised collections.
- If you need to work interactively using code in your Gadi `/home` directory instead of the [OOD](https://acdguide.github.io/BigData/platforms/platforms-nci-ood.html), there are [helper scripts](https://github.com/coecms/nci_scripts) such as `gadi_jupyter` provided by the CLEX CMS team.

### How do I get access? 
Sign up for an account at [my.nci.org.au](https://my.nci.org.au/mancini/). You need to join an existing computational project - talk to your supervisors or colleagues for suggested project codes. As well as joining at least one "compute" project, once access is granted you will also need to use the same portal to request access to "data" projects for the data you need to work with, for example, `oi10` for replicated CMIP6 data, or `hh5` for the CLEX-run community conda environments.

To log on, in a terminal type 

`ssh <username>@gadi.nci.org.au`.

For further information, see the [Gadi user guide](https://opus.nci.org.au/display/Help/Gadi+User+Guide).
