# CSIRO facilities

CSIRO researchers have access to internal HPC facilities. The current generation are known as "Petrichor" (peak HPC) and Bracewell (GPU-enabled cluster). There is also a Bowen research cloud (storage and compute), as well as the Digital Workbench (internal cloud-based infrastructure particularly targetting Machine Learning applications), and Cloud Right programme for accessing public cloud (AWS, GCP, Azure). 

When collaborating with researchers from other institutions, it is more likely that CSIRO staff will work on NCI or similar shared systems, however for internal work and data delivery CSIRO systems are often used.

Of note on Petrichor/Bracewell, 
- when accessing data, `/datastore` is the tape archive (similar to NCI's MDSS), it operates as a filesystem and was the home area on the previous Ruby system. Data in this area may be slow to access and `dm` commands should be used - see inernal documentation for futher details. 
- Disk-based storage is provided via the `/scratch` filesystems (of which there are currently two), which is regularly purged and cannot be used for longterm storage, 
- `/datasets` contains mounts of Bowen cloud storage to the HPC - this is disk-based storage which is faster than datastore but slower than scratch, so not ideal for running models but good for persistent storage of model output datasets.
- `/home` is shared across CSIRO HPC systems but cloud machines typically have their own /home area.

## When would I use this sytem?

When working only with other CSIRO researchers and/or when publishing data to CSIRO's Data Access Portal (DAP) which is >10TB.

## How do I get access?

Access is available to CSIRO staff via the [Scientific Computing platform](https://sc.it.csiro.au/).
