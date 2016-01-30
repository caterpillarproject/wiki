---
title: Datasets
tags: [postprocessed,datasets]
last_updated: November 6, 2015
summary: "Here you can find a range of available datasets which have been postprocessed from the Caterpillar simulation suite."
---

## Subhalo Catalogue Information

### Data Description 
In each of the halo `analysis/' subdirectories we have extant and destroyed catalogue information for the subhalos of the main host.

We have all halo information at three different times:

* max \\(M_{vir}\\): the peak mass the halo reaches along its main branch
* infall: the halo just as it crosses the virial radius of the main host halo.
* peak \\(V_{max}\\): the halo at the maximum of \\(V_{max}\\) along its main branch (i.e. the highest \\(V_{max}\\) its ever had).

In the catalogues these variables are prepended with 'max_', 'infall_' and 'peak_'. The full list of variables accessible in these catalogues are as follows: 

<center>

| Variable | 
|  :----: |
| Unnamed  |
| sub_rank  |
| rsid  |
| backsnap  |
| depth  |
| max_mass_rsid  |
| max_mass_snap  |
| max_mass_vmax  |
| max_mass  |
| max_mass_posx  |
| max_mass_posy  |
| max_mass_posz  |
| max_mass_pecvx  |
| max_mass_pecvy  |
| max_mass_pecvz  |
| max_mass_virialratio  |
| max_mass_hostid_MT  |
| max_mass_rvir  |
| max_mass_spinbullock  |
| max_mass_rs  |
| max_mass_scale_of_last_MM  |
| max_mass_Jx  |
| max_mass_Jy  |
| max_mass_Jz  |
| max_mass_xoff  |
| peak_rsid  |
| peak_snap  |
| peak_vmax  |
| peak_mvir  |
| peak_posx  |
| peak_posy  |
| peak_posz  |
| peak_pecvx  |
| peak_pecvy  |
| peak_pecvz  |
| peak_virialratio  |
| peak_hostid_MT  |
| peak_rvir  |
| peak_spinbullock  |
| peak_rs  |
| peak_scale_of_last_MM  |
| peak_Jx  |
| peak_Jy  |
| peak_Jz  |
| peak_xoff  |
| infall_rsid  |
| infall_snap  |
| infall_vmax  |
| infall_mvir  |
| infall_posx  |
| infall_posy  |
| infall_posz  |
| infall_pecvx  |
| infall_pecvy  |
| infall_pecvz  |
| infall_virialratio  |
| infall_hostid_MT  |
| infall_rvir  |
| infall_spinbullock  |
| infall_rs  |
| infall_scale_of_last_MM  |
| infall_Jx  |
| infall_Jy  |
| infall_Jz  |
| infall_xoff  |
| peak_mgrav  |
| infall_mgrav  |
| peak_hostid_RS  |
| infall_hostid_RS  |
| peak_rvmax  |
| infall_rvmax  |
| peak_corevelx  |
| peak_corevely  |
| peak_corevelz  |
| infall_corevelx  |
| infall_corevely  |
| infall_corevelz  |
| nstars  |
| start_pos  |

</center>

### Data Access 

Below is an example of how to access this data. Be sure to have obtained access to the Caterpillar modules from the git repository and that you are using `MTanalysis3' to load the destroyed catalogues.

```python
import MTanalysis3 as mta
import numpy as np
import haloutils as htils

hpath = htils.get_paper_paths_lx(14)[0]

AD = mta.AllDestroyedData()
AE = mta.AllExtantData()

dataE = AE.read(hpath) # all the extant (surviving) halo information along their history
dataD = AD.read(hpath) # all the destroyed halo information along their history

# we also have carried out 5% most bound tagging of the particles at infall

idsE = mta.load_idsE(hpath) # all tagged particles in extant list
idsD = mta.load_idsD(hpath) # all tagged particles in destroyed list

stars_a = mta.getStars(dataE,idsE,row=10) # particle ids tagged in particular halo
stars_b = mta.getStars(dataD,idsD,row=25) # particle ids tagged in particular halo

mper_a = mta.mass_per_particle(hpath,dataE,row=10) # stellar mass per particle of particular halo
mper_b = mta.mass_per_particle(hpath,dataD,row=25) # stellar mass per particle of particular halo
```

## Other Available Datasets

To be added soon.

This will include:

* minihalo catalogues: properties of minihalos, i.e. halos which first go above \\(T_{vir}\sim2000\\)K etc.
* first galaxy catalogues: properties of first galaxies, i.e. halos which first go above \\(T_{vir}\sim10,000\\)K etc.

... more soon.
