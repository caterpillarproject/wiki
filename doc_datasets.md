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

These are the quantites at maximum mass:  
  
<center>

|  Variable | 
|  :----: | 
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
</center>

These are the quantities At \\(V_{peak}\\):

<center>

|  Variable |
|  :----: |
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
| peak_mgrav  |
| peak_corevelx  |
| peak_corevely  |
| peak_corevelz  |
| peak_hostid_RS  |
| peak_rvmax  |  

</center> 

These are the quantities at infall:

<center>

| Variable  |
|  :----: |
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
| infall_mgrav  |
| infall_hostid_RS  |
| infall_rvmax  |
| infall_corevelx  |
| infall_corevely  |
| infall_corevelz  |  

</center>

Here are some extra variables:

<center>

| Variable | Description | 
|  :----: | :----: |
| Unnamed  |  | 
| sub_rank  |  uniquely labels all halos, unless they merged with another subhalo in the catalog. In such a case the sub_rank is the negative of the one it ultimately merged with | 
| rsid  |   | 
| backsnap  | the last time a halo is in the merger tree before merging | 
| depth  | number of mergers of the satellite until a z=0 halo |
| nstars  |   | 
| start_pos  |    | 

</center>
Note: extant halos all have a 'backsnap' of 319, and the destroyed are all < 319

The extant data and destroyed data sets have the same columns and  can thus be combined. The extant data now recursively trace halos that fell into the host, then merged with an extant sub which they did not before. Also, the data includes systems that exist but don't have a proper infall time (formed within the virial radius, or don't have a good merger tree that far back). All the infall properties are set to -1 in such cases. There is a 'depth' column which tells you the number of mergers of the satellite until a z=0 halo. e.g. a subhalo mergers with a subhalo which merges with an extant halo has a depth of 2.  

The 'sub_rank' column uniquely labels all halos, unless they merged with another subhalo in the catalog. In such a case the sub_rank is the negative of the one it ultimately merged with. So you can quickly find all subhalos that ever merged with a particular extant, or depth=1 destroyed subhalo.


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
