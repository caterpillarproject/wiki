---
title: Initial Conditions
keywords: "ics, initial conditions, parameter file, music"
last_updated: November 3, 2015
summary: "Overview of the initial conditions used for the Caterpillar suite."
---

## Overview

We use the multi-scale cosmological initial conditions creator <span style="font-family:Courier">MUSIC</span>. <span style="font-family:Courier">MUSIC</span> is a computer program to generate nested grid initial conditions for high-resolution "zoom" cosmological simulations. A detailed description of the algorithms can be found in Hahn & Abel (2011). You can download the user's guide [here](https://bitbucket.org/ohahn/music/downloads/MUSIC_Users_Guide.pdf) or obtain a copy of the code [here](https://people.phys.ethz.ch/~hahn/MUSIC/). Any questions should be directed to [Brendan Griffen](brendan.f.griffen@gmail.com) and then if that fails, [Oliver Hahn](hahn@phys.ethz.ch).


## Parameters

These are the parameter files which were used to generate the initial coditions for the _Caterpillar_ project. We adopt the raw Planck (2013) cosmology and do not use 2nd order lagrangian perturbation theory. The [user guide](https://bitbucket.org/ohahn/music/downloads/MUSIC_Users_Guide.pdf) contains all the information required to understand the following parameter files. Within each simulation directory, there is a file named `OUTPUTmusic` which gives the log of the construction of the initial conditions.

### Parent Simulation

First we constructed a parent simulation of sufficient size to include thousands of Milky Way sized systems but also of sufficient resolution to construct good lagrangian volumes. We decided on ~10,000 particles per Milky Way-sized host and a 100 Mpc/h volume. Our `level_max` is 10 below, which means our parent simulation's effective resolution is (2<sup>10</sup>)<sup>3</sup> = (1024)<sup>3</sup> (i.e. a particle mass of 8.72 x 10<sup>7</sup> \\(M_\odot/h\\).

If you have access to the MIT cluster (`bigbang.mit.edu`), you can access the parent simulation data here: `/bigbang/data/AnnaGroup/caterpillar/parent/gL100X10/`. Within this folder you'll find the following file (within `ics/`): 

```bash
# parentics.conf
[setup]
boxlength               = 100
zstart                  = 127
levelmin                = 10
levelmin_TF             = 10
levelmax                = 10
padding                 = 8
overlap                 = 4
ref_center              = 0.5, 0.5, 0.5
ref_extent              = 0.2, 0.2, 0.2
align_top               = yes
baryons                 = no
use_2LPT                = no
use_LLA                 = no
periodic_TF             = yes

[cosmology]
Omega_m                 = 0.3175      
Omega_L                 = 0.6825       
Omega_b                 = 0.049     
H0                      = 67.11         
sigma_8                 = 0.8344        
nspec                   = 0.9624        
transfer                = eisenstein

[random]
seed[10]                = 34567

[output]
format                  = gadget2
filename                = ./ics
gadget_num_files        = 128

[poisson]
fft_fine                = yes
accuracy                = 1e-5
pre_smooth              = 3
post_smooth             = 3
smoother                = gs
laplace_order           = 6
grad_order              = 6
```

### Zoom Simulations

Each simulation folder has a configuration file for MUSIC (e.g. `H1079897_EX_Z127_P7_LN7_LX14_O4_NV4.conf`) which was used to construct the initial conditions.

There are a few things to note about the zoom-in paramter files when compared to the parent volume parameter file. First is we now specify a `region_point_file` which defines the x,y,z (normalized to the box width) of the particles to be resampled. We have added the parameter `hipadding` which our own modification to allow for expanded regions. 1.05, for example, represents an expanded ellipsoid (by 5%). See Section 2.3 of [Griffen et al. (2015)](http://adsabs.harvard.edu/cgi-bin/bib_query?arXiv:1509.01255) for a more detailed description of these geometries and their impact on contamination.

We also draw the reader's attention to the seed values. Note that we do not set the seed values for any level lower than the parent volume (10) which makes <span style="font-family:Courier">MUSIC</span> smooth out any levels lower than 10. The seed values at 11 are simply the halo numbers and then each level higher scales by a factor of 2 of this original number. This was required because the ICs were generated through our automatic pipeline and each simulation needs unique values to seed the random noise field. The `levelmin_TF` is also set to be the same as the parent volume (10). The padding and overlap parameters are the same for all simulations.


```bash
# H1079897_EX_Z127_P7_LN7_LX14_O4_NV4.conf
[setup]
boxlength            = 100
zstart               = 127
levelmin             = 7
levelmin_TF          = 10
levelmax             = 14
padding              = 7
overlap              = 4
region               = ellipsoid
hipadding            = 1.05
region_point_file    = /n/home01/bgriffen/data/caterpillar/ics/lagr/H1079897NRVIR4
align_top            = no
baryons              = no
use_2LPT             = no
use_2LLA             = no
periodic_TF          = yes

[cosmology]
Omega_m              = 0.3175
Omega_L              = 0.6825
Omega_b              = 0.049
H0                   = 67.11
sigma_8              = 0.8344
nspec                = 0.9624
transfer             = eisenstein

[random]
seed[10]              = 34567
seed[11]              = 1079897
seed[12]              = 2159794
seed[13]              = 3239691
seed[14]              = 4319588

[output]
format               = gadget2_double
filename             = ./ics
gadget_num_files     = 8
gadget_spreadcoarse  = yes

[poisson]
fft_fine             = yes
accuracy             = 1e-05
pre_smooth           = 3
post_smooth          = 3
smoother             = gs
laplace_order        = 6
grad_order           = 6
```