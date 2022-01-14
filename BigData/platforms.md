# Computing Platforms

In Australia, there is a vast quantity of climate and related datasets hosted at the [National Computational Infrastructure](https://nci.org.au/) (NCI) High Performance Computing (HPC) facility located at the Australian National University in Canberra. As such, much of our documentation assumes use of this system, but this page covers a few options and notes about various systems people might work on.

The datasets hosted on NCI may be centrally managed, e.g. by NCI (CMIP, ERA5) or the community (precipitation data, other reanalyses). An interface to help researchers identify what climate data is available and where is under development.

## NCI Gadi HPC

The NCI's peak system "Gadi" is a large high performance computer ideal for numerical modelling and computationally intensive jobs. This HPC supports the ACCESS climate model that contributes to CMIP submissions as well as development by ACCESS staff at BoM, CSIRO and university collaborators. NCI's Gadi system is also used to run regional models such as WRF and CCAM, as well as many field-specific models (CABLE, WaveWatchIII, AWRA to name a few). 

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

### How do I get access? 
Sign up for an account at [my.nci.org.au](https://my.nci.org.au/mancini/). You need to join an existing computational project - talk to your supervisors or colleagues for suggested project codes. As well as joining at least one "compute" project, once access is granted you will also need to use the same portal to request access to "data" projects for the data you need to work with, for example, `oi10` for replicated CMIP6 data, or `hh5` for the CLEX-run community conda environments.

To log on, in a terminal type 

`ssh <username>@gadi.nci.org.au`.

For further information, see the [Gadi user guide](https://opus.nci.org.au/display/Help/Gadi+User+Guide).

## NCI OOD virtual desktop

The NCI also support a virtual desktop infrastructure (VDI) called "Open, On-Demand" (OOD) hosted on their internal cloud infrastructure, and which provides an interface to the HPC infrastructure. The current generation of the VDI is web-based, offering a virtual desktop service mimicking the older 'Strudel' interface, and a Jupyter Notebook server. Documentation can be [found here](https://opus.nci.org.au/display/OOD/Open+OnDemand+%28OOD%29+Service).

The OOD VDI service is accessed via a cloud launcher page which creates an image with access to the requested compute resources and looks like this:
![OOD launcher](images/OOD-launcher.PNG)

Launching the VDI desktop via the button gives access to a familiar linux graphical interface with a terminal, browser and various applications.
![OOD interface](images/OOD-interface.PNG)

### When would I use the OOD VDI?
The "OOD" or "VDI" is ideal for exploratory and interactive work such as code development, data exploration and visualisation, and tasks requiring internet access (e.g. working with data from external sources via OPeNDAP or S3).

As with Gadi, the OOD VDI has access to all the climate data stored at NCI*, so it is an ideal place to work when large scale input data is required (as this minimises effort and risk associated with creating copies of input data). 

*Note that the VDI shares *only* the `/g/data` filesystems with Gadi, data on other Gadi filesystems is not visible on the VDI and the two systems do not share a common `/home` directory.

### How do I get access?

Same as Gadi (see above).

## Pangeo

Pangeo is a community built aroudn analysis of large scale Earth systems data. Pangeo recommend use of python tools like jupyter, xarray and dask. Pangeo supports collections of selected datasets publicly in commercial cloud. The pangeo documentation also hosts not only examples, but jupyter 'binders' in which users can spin up their own notebooks to interact with data in the cloud.
For more information, see ??? join the Pangeo Oceania community ??

### When would I use Pangeo?

Pangeo as a paradigm, all the time. The pangeo cloud infrastructure though??

### How do I get access?

## Pawsey HPC

There are a number of academic HPC systems in Australia outside of those hosted by specific institutions. [Pawsey](https://pawsey.org.au/) in WA hosts a peak HPC machine, local cloud, and data storage. Pawsey is a facility managed by CSIRO. Access to large scale compute quotas is available through the National Computaitonal Merit Allocation Scheme (NCMAS) as with Gadi, as well as a separate Pawsey allocation process which can be accessed more frequently. Currently only limited climate work is being done on the Pawsey peak system but this is likely to increase in the future.

### When would I use this system?

When you need large scale HPC facilities and do not have a dependency on shared reference data, e.g. when running a large scale numeric model and forcing data is either relatively small in size or available online.

### How do I get access?

Director's share applications for limited resources to test the system can be made at any time, and Merit Allocations are available via a competitive process twice a year. See the [support documentation](https://support.pawsey.org.au/documentation/display/US/Getting+Access) for further information.

## CSIRO facilities

CSIRO researchers have access to internal HPC facilities. The current generation are known as "Petrichor" (peak HPC) and Bracewell (GPU-enabled cluster). There is also a Bowen research cloud (storage and compute), as well as the Digital Workbench (internal cloud-based infrastructure particularly targetting Machine Learning applications), and Cloud Right programme for accessing public cloud (AWS, GCP, Azure). 

When collaborating with researchers from other institutions, it is more likely that CSIRO staff will work on NCI or similar shared systems, however for internal work and data delivery CSIRO systems are often used.

Of note on Petrichor/Bracewell, 
- when accessing data, `/datastore` is the tape archive (similar to NCI's MDSS), it operates as a filesystem and was the home area on the previous Ruby system. Data in this area may be slow to access and `dm` commands should be used - see inernal documentation for futher details. 
- Disk-based storage is provided via the `/scratch` filesystems (of which there are currently two), which is regularly purged and cannot be used for longterm storage, 
- `/datasets` contains mounts of Bowen cloud storage to the HPC - this is disk-based storage which is faster than datastore but slower than scratch, so not ideal for running models but good for persistent storage of model output datasets.
- `/home` is shared across CSIRO HPC systems but cloud machines typically have their own /home area.

### When would I use this sytem?

When working only with other CSIRO researchers and/or when publishing data to CSIRO's Data Access Portal (DAP) which is >10TB.

### How do I get access?

Access is available to CSIRO staff via the [Scientific Computing platform](https://sc.it.csiro.au/).

## BoM facilities

The Australian Bureau of Meteorology run an internal HPC "Australis" for operational forecasting, however research and collaboration is mostly carried out on the NCI system.

## Institutional shared resources

Many universities support internal HPCs on a much smaller scale than NCI and Pawsey, but that are often beneficial at a research group level. UTas (TPAC's Kunanyi), UniMelb (Spartan), and UNSW (Katana) all have systems that researchers may be able to access for tasks that do not require the shared data on NCI but are too big for the researcher's laptop. The main drawback of these options is that they limit collaboration with peers at other institutions, as well as support available as you will have to rely on internal support staff, unlike when using NCI where mutiple help channels are available.

### When would I use this sytem?

When working only with other researchers from your local institution and when you are not dependent on large-scale reference data at NCI and do not require assistance from community support staff.

### How do I get access?

Enquire with your local IT support staff.

## Personal computer

For some researchers, the computational power of their own laptop may be sufficient for much of their work. Online access to data via cloud resources or online data access protocols (e.g. OPeNDAP) means that in some cases, direct access to HPC facilities may not be required. See the [Tools](https://acdguide.github.io/BigData/different_tools.html) page and other parts of this book for advice on working with large scale data using tools like Anaconda and Jupyter notebooks.

### When would I use this approach?

When working alone without need to collaborate directly on the filesystem (i.e. collaboration only via Github etc.) and not dependent on large datasets hosted at NCI (other than subsets which can be accessed via OPeNDAP).