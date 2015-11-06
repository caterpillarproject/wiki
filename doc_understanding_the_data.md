---
title: Understanding The Data
tags: [data,output,particles,catalogues,directory,directories,folders,paths]
last_updated: November 3, 2015
summary: "It is important to understand the layout of Caterpillar data &mdash; particularly if your project involves a lot of minipulation of the particle output."
---


## Directory Structure

The master halo directory is here on `bigbang.mit.edu`: 

`/bigbang/data/AnnaGroup/caterpillar/halos/`

Every halo has its own directory For example, `H1387186` is located here: 

`/bigbang/data/AnnaGroup/caterpillar/halos/H1387186/`

All production halos follow the format:

`/bigbang/data/AnnaGroup/caterpillar/halos/H{parent_simulation_id}/`

Here is the initial list of Caterpillar halos found in Griffen et al. (2015) with their associated ID taken from the Rockstar catalogue of halos for the parent simulation.

<center> 

 Name | PID | Name | PID | Name | PID | Name | PID
:---: | ---   | :---:  | --- | :---:  | --- | :---:  | ---
Cat-1 | 1631506 | Cat-7  | 94687 | Cat-13 | 1725272 | Cat-19 | 1292085
Cat-2 | 264569  | Cat-8  | 1130025 | Cat-14 | 1195448 | Cat-20 | 95289
Cat-3 | 1725139 | Cat-9  | 1387186 | Cat-15 | 1599988 | Cat-21 | 1232164
Cat-4 | 44764   | Cat-10 | 581180 | Cat-16 | 796175 | Cat-22 | 1422331
Cat-5 | 5320    | Cat-11 | 1725372 | Cat-17 | 388476 | Cat-23 | 196589
Cat-6 | 581141  | Cat-12 | 1354437 | Cat-18 | 1079897 | Cat-24 | 1268839

</center>

Within each of the produdtion runs you will find the following folders.

```bash
H1387186/
 -> H1387186_EB_Z127_P7_LN7_LX12_O4_NV4/
 -> H1387186_EB_Z127_P7_LN7_LX14_O4_NV4/
 -> H1387186_EB_Z127_P7_LN7_LX11_O4_NV4/  
 -> H1387186_EB_Z127_P7_LN7_LX13_O4_NV4/
 -> H1387186_EB_Z127_P7_LN7_LX14_O4_NV4/
 -> contamination_suite/
  -> H1195448_BA_Z127_P7_LN7_LX11_O4_NV4  
  -> H1195448_EA_Z127_P7_LN7_LX11_O4_NV4
  -> H1195448_BA_Z127_P7_LN7_LX11_O4_NV5  
  -> H1195448_EA_Z127_P7_LN7_LX11_O4_NV5
  -> H1195448_BB_Z127_P7_LN7_LX11_O4_NV4  
  -> H1195448_EB_Z127_P7_LN7_LX11_O4_NV4
  -> H1195448_BC_Z127_P7_LN7_LX11_O4_NV4  
  -> H1195448_EC_Z127_P7_LN7_LX11_O4_NV4
  -> H1195448_BD_Z127_P7_LN7_LX11_O4_NV4  
  -> H1195448_EX_Z127_P7_LN7_LX11_O4_NV4
  -> H1195448_CA_Z127_P7_LN7_LX11_O4_NV4  
  -> H1195448_EX_Z127_P7_LN7_LX11_O4_NV5
  -> H1195448_CA_Z127_P7_LN7_LX11_O4_NV5
```

The naming convention is as follows: 

```bash
H1387186_EB_Z127_P7_LN7_LX12_O4_NV4
  H1387186    # halo rockstar id from parent simulation
  EB          # initial conditions type, 'B' for box" etc.
  Z127        # starting redshift (z = 127)
  P7          # padding parameters (2^7)^3
  LN7         # level_min used in MUSIC (2^7)^3
  LX12        # level_max used in MUSIC (2^12)^3
  O4          # overlap parameter (2^4)^3
  NV4         # number of times the virial radius enclosed defining lagrangian volume
```

Most of the terms remain the same for the entire Caterpillar suite. The only ones that do change are the `LX` value and the `B` value which signifies the resolution and lagrangian geometry accordingly. `NV` also represents the number of times the virial radius we enclosed within the parent volume (usually four, though sometimes 5).

All halos directories will have three key sub-directories: `halos_bound/`, `outputs/` and `analysis/`. In each of these directors you'll find the following folders:

```bash
H1387186_EB_Z127_P7_LN7_LX14_O4_NV4
 -> halos_bound/  # rockstar and merger tree catalogues
  -> halos_0/     # each folder contains the catalogue for each snapshot
  -> halos_1/
  ...
  -> halos_319/
  -> outputs/
  -> trees/
    -> forests.list
    -> locations.dat
    -> tree_0_0_0.dat
    -> tree_0_0_1.dat
    ...
    -> tree_1_1_1.dat
    -> tree.bin
    -> treeindex.csv
 -> outputs/      # gadget raw snapshot output (particle data)
  -> snapdir_000/ # each folder contains the particle data for each snapshot
  -> snapdir_001/
  ...
  -> snapdir_319/
  -> groups_319/  # the subfind catalogues are also stored (mostly for the last snapshot)
  -> hsmldir_319/ # the smoothing lengths for the corresponding particle data
 -> analysis/     # post-processed output files (halo profiles, mass functions, minihalos etc.)
```

You will also notice some other folders, such as `halos/` which was the original catalogues produced prior to our implementation of iterative binding. This data can be used to explore the benefits of using full iterative unbinding.

## Ancillary Information

At the top level of the halos directory (`/bigbang/data/AnnaGroup/caterpillar/halos/`), you will find `parent_zoom_index.txt`. This file contains the following kind of table

| parentid | ictype | LX | NV | zoomid |     min2 |      x |      y |      z |      mvir |  rvir | badflag | badsubf | allsnaps |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|    95289 |     BB | 11 |  4 |    728 | 1.127053 | 48.957 | 47.206 | 52.622 | 7.686e+11 | 242.0 |       0 |       0 |        0 |
|    95289 |     BB | 12 |  4 |   4911 | 1.020584 | 48.953 | 47.193 | 52.625 | 7.557e+11 | 240.7 |       0 |       0 |        1 |
|    95289 |     BB | 13 |  4 |  32895 | 0.924030 | 48.966 | 47.195 | 52.626 | 7.607e+11 | 241.2 |       0 |       0 |        1 |
|    95289 |     BB | 14 |  4 | 216324 | 0.881211 | 48.962 | 47.205 | 52.625 | 7.026e+11 | 234.9 |       1 |       1 |        1 |
|  1195448 |     EC | 11 |  4 |    282 | 1.688313 | 52.961 | 55.513 | 47.329 | 7.439e+11 | 239.4 |       0 |       0 |        1 |
|  1195448 |     EC | 12 |  4 |   1042 | 1.544539 | 52.951 | 55.513 | 47.323 | 7.572e+11 | 240.8 |       0 |       0 |        1 |
|  1195448 |     EC | 13 |  4 |   6861 | 1.499513 | 52.944 | 55.526 | 47.308 | 7.542e+11 | 240.5 |       0 |       0 |        1 |
|  ... |      |  |   |    |  |  |  |  |  |  |        |        |         |

This above contains _Caterpillar as it stands today_. The columns are as follows:

<center> 

 Column | Description
:---------: | -------------
PARENTID | the halo id number of the simulation from the parent run 
ICTYPE | the geometry of the initial conditions 
LX | level_max used in MUSIC 
NV | number of times we enclose the virial radius 
MIN2 | distance to the closest contamination particle (parttype=2)
X | x-position in the box 
Y | y-position in the box 
Z | z-position in the box
MVIR | halos mass (in units of Msol/h)
RVIR | halo virial radius (in units of kpc/h)
BADFLAG | a binary flag (1 or 0) if there was a mismatch in parameters between the parent and zoom run
BADSUBF | a binary flag (1 or 0) if the SUBFIND halo properties do not match the rockstar halo properties
ALLSNAPS | a binary flag (1 or 0) if the all the snapshots exist

</center> 

Although it is not recommended, you can load up any of these quantities by importing Brendan Griffen's modules:

```python
import brendanlib.grifflib as glib
import haloutils as htils

catnum = 1
lx = 14

hpath = htils.get_hpath(catnum,lx)
xpos = htils.get_quant_zoom(hpath,'x')
min2 = htils.get_quant_zoom(hpath,'min2')
```


