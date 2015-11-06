---
title: Consistent-Trees
tags: [trees,catalogues,merger,halo]
last_updated: November 2, 2015
summary: "The halos from rockstar are connected together in time using consistent trees. Here you can find information about the structure of the tree and various quantities relate to the formation history of a given halo or subhalo(s)."
---

## Overview

The code's paper can be found [here](http://arxiv.org/abs/1110.4370). Documentation on getting it running and a more exhaustive documentation of the catalogue's content at its [Bitbucket Repository](https://bitbucket.org/pbehroozi/consistent-trees).

## Catalogue Parameters

The consistent-trees output it similar to that of the rockstar catalogue except now there are additional paramters which connect the halos in time.

<center>

variable | units | definition
:---: | :---: | ---
`id` | - | id of the halo in the merger tree
`pid` | - | parent id
`upid` | -  | 
`bfid` | - | breadth first order id
`dfid` | - | depth first order id
`origid` | - | original id as identified in Rockstar catalogue
`lastprog_dfid` | - | last progenitor depth first id 
`snap` | - | snapshot 
`scale` | - | scale factor
`desc_id` | - | descendant id 
`num_prog` | - | number of progenitors
`phantom` | - | whether it is a phantom halo 
`sam_mvir` | Msol/h | smoothed virial mass
`mvir` |  Msol/h | virial mass (Brian & Norman prescription) (it is actually mgrav from Rockstar) 
`rvir` | kpc | virial radius (Brian & Norman prescription) 
`rs` | kpc | scale radius
`vrms` | km/s | rms velocity
`mmp` | - | most massive progenitor
`scale_of_last_MM` | - | scale factor of last major merger
`vmax` | km/s | maximum circular velocity
`posX` | kpc | x-position
`posY` | kpc | y-position
`posZ` | kpc | z-position
`pecVX` | km/s | x-dir peculiar velocity
`pecVY` | km/s | y-dir peculiar velocity
`pecVZ` | km/s | z-dir peculiar velocity
`Jx` | | x-component angular momentum
`Jy` | | y-component angular momentum
`Jz` | | z-component angular momentum
`spin` | | spin parameter (Peebles version)
`spin_bullock` | | spin parameter (Bullock modified version)
`m200c_all` |  Msol/h | virial mass of all particles based on 200x critical density
`m200b` |   Msol/h | virial mass of all particles based on 200x background density
`xoff` | kpc | Position offset parameter abs(center of mass position - halo position)
`voff` | kpc | Velocity offset parameter abs(center of mass velocity - halo velocity)
`b_to_a(500c)` | - | Halo ellipticity b/a
`c_to_a(500c)` | - | Halo ellipticity c/a
`A[x](500c)` | | x-axis for ellipticity
`A[y](500c)` | | y-xis for ellipticity
`A[z](500c)` | | z-xis for ellipticity
`T/abs(U)` | - |  Virial ratio (kinetic/abs(potential))

</center>
