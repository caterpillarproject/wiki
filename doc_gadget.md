---
title: Gadget
tags: [analysis,data]
last_updated: November 3, 2015
summary: "Here you can find information relating to our core running code; Gadget."
---

## Parameters

For the simulation suite we use <span style="font-family:Courier">P-Gadget3</span> (first 24 halos) and <span style="font-family:Courier">Gadget4</span> (remainder). The parent directory of each folder contains `.gadget3` or `.gadget4` to indicate which halo was run with which code. We use the following compile flags for our runs. `PMGRID` changes depending on the resolution of the run. There is a great amount of documentation on each of these parameters on Phil Hopkin's [GIZMO website](http://www.tapir.caltech.edu/~phopkins/Site/GIZMO_files/gizmo_documentation.html).

```Text
PERIODIC  
UNEQUALSOFTENINGS  
PMGRID=512  
GRIDBOOST=2  
PLACEHIGHRESREGION=2  
ENLARGEREGION=1.2  
MULTIPLEDOMAINS=16  
PEANOHILBERT  
WALLCLOCK  
MYSORT  
DOUBLEPRECISION  
OUTPUT_IN_DOUBLEPRECISION  
INPUT_IN_DOUBLEPRECISION  
ORDER_SNAPSHOTS_BY_ID  
NO_ISEND_IRECV_IN_DOMAIN  
FIX_PATHSCALE_MPI_STATUS_IGNORE_BUG  
COMPUTE_POTENTIAL_ENERGY  
OUTPUTPOTENTIAL  
RECOMPUTE_POTENTIAL_ON_OUTPUT  
HAVE_HDF5  
DEBUG  
```

Parameter files for each of the runs can be found as `param.txt` in the relevant simulation directory. For those users who have access to `antares.mit.edu` (MKI's compute cluster), Brendan Griffen's home directory contains the master files (configuration files, parameter files and expansion factor lists for output) for the entire suite.

```bash
/home/bgriffen/exec/gadget3
/home/bgriffen/exec/gadget4
```

## Data Blocks In The Gadget Files

See Data Access Examples to see how to load each of these blocks. One such way is:

```python
import haloutils as htils
hpath = htils.get_paper_paths_lx(14)[0]
zoomid = htils.load_zoomid(hpath)
blockname = "POS " # NOTE THE EXTRA SPACE!
pos = htils.load_partblock(hpath,zoomid,blockname,partids=[listofids]) # units Mpc/h
```

{{site.data.alerts.tip}} Make sure you include the right number of spaces (4) otherwise you will get an error when trying to read the blocks.{{site.data.alerts.end}}

Other blocks you have access to are listed below:

<center>

Block Name | Description | Units | Notes
:----: | ---- | -----  | ------
`ID`  | ID number (N) | -  | --  
`POS` | position (Nx3) | comoving Mpc/h  | They are all around ~50 Mpc/h as they are in the center of the 100 Mpc/h box for the resimulations.  
`VEL` | velocity (Nx3) | spatial km\\(\sqrt(a)\\)/s  | Spatial velocity. The peculiar velocity is obtained by multiplying this value by \\(\sqrt(a)\\).
`MASS` | particle mass (N) | Msol/h | Use `header.massarr[1]*1e10` as it is faster than reading in the entire block.  
`POT` | potential energy | ---  | ----  

</center>

{{site.data.alerts.warning}} Remember to use the scale factor for the snapshot you are working on to convert the comoving quantities to physical. For z = 0, it doesn't matter.{{site.data.alerts.end}}


