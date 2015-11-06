---
title: Zoom Simulations
audience: astronomers
tags: [zoom simulations,resimulation,ics,gadget,contamination,geometry,lagangian]
last_updated: August 12, 2015
keywords: zoom, simulations, 
summary: "Here we provide a brief summary of what was presented in Griffen et al. (2015). We also present some extra details pertaining to the contamination study."
---

The majority of the information about the zoom-in runs can be found in [Griffen et al. (2015)](http://adsabs.harvard.edu/cgi-bin/bib_query?arXiv:1509.01255). Here we simply outline some details which were left out of the publication for the sake of brevity.

## Overview

<center>

~*Aquarius* <br> Level | <span style="font-family:Courier">MUSIC</span> <br> *levelmax* | Effective <br> Resolution | \\(m_{dm}\\) <br> 10^4 *h*^-1 \\(M_\odot\\) | \\(m_{dm}\\) <br> 10^4 \\(M_\odot\\) | \\(\epsilon_{dm}\\) <br> pc 
  :---: | :---: | :---: | :---: | :---: | :---: 
1 | 15 | 32768^3 | 0.25 | 0.37 | 36
**2** | **14** | **16384^3** | **2** | **3** | **76**
3 | 13 | 8096^3 | 16 | 24 | 152
4 | 12 | 4096^3 | 128 | 190 | 228
5 | 11 | 2048^3 | 1025 | 1527 | 452

<img title="LX11 simulation" src="images/Cat1_LX11.jpg" style="max-width: 200px;">
<img title="LX12 simulation" src="images/Cat1_LX12.jpg" style="max-width: 200px;">
<img title="LX13 simulation" src="images/Cat1_LX13.jpg" style="max-width: 200px;">
<img title="LX14 simulation" src="images/Cat1_LX14.jpg" style="max-width: 200px;">

</center>

Each panel represents one single realization of the Cat-1 halo at different resolutions. The far left is an LX11 run and the far right is an LX14 run.

{{site.data.alerts.note}} The <span style="font-family:Courier">LX15</span> run has currently only been run for one of the halos and has been temporarily paused at z = 1. This will be finished with a few others once the main suite has been completed.{{site.data.alerts.end}}

{{site.data.alerts.warning}} Timesteps are spaced logarithmically in expansion factor to z = 6, then linearly spaced in expansion factor down to z = 0. Always be aware of this as it could be strength and a weakness of your study.{{site.data.alerts.end}}

We have complete (modified) <span style="font-family:Courier">rockstar</span> halo catalogues (together with <span style="font-family:Courier">consistent-trees</span> merger trees) and z = 0 <span style="font-family:Courier">subfind</span> catalogues.

## Contamination Study

A number of contamination studies have been carried out. This involves changing the Lagrangian geometry in some way to keep the contamination (distance to the nearest particle type 2 as far as possible) low whilst conserving CPU hours. Our selected test geometries were as follows

<center>

|  Geometry           | Detail  |
:---: | ---
BA | Original MUSIC bounding box (e.g. the exact bounding box of lagr volume).
BB | 1.2 bounding box extent
BC | 1.4 bounding box extent
BD | 1.6 bounding box extent
CA | Convex Hull Volume
EA | Original MUSIC ellipsoid (e.g. the exact bounding box of lagr volume).
EB | 1.1 padding
EC | 1.2 padding
EX | 1.05 padding

</center>

We did this for both 4 and 5 times the virial radius at z = 0 (marked by the letter 4 or 5 at the end of the abbreviated geometry). Making a total of ~18 test halos per _Caterpillar_ halo. Our requirement was that there was no contamation (particle type 2) within 1 Mpc of the host at the LX11 level.

We also looked at how the geometry of the lagrangian volume affected the contamination radius. As outlined in Griffen et al. (2015), we did not find any correlation with geometry and overall level contamination. Every simulation requires its own tailored geometry to achieve our contamination requirements.

<center>

<img title="Cat-1 contamination information" src="images/paper_contamination.jpg" style="max-width: 400px;">

</center>

The size of the lagrangian volumes were also another challenge to overcome. If a halo had LX11 ICs which were larger than 300mb, we found that we could not run these at LX14 on national facilities. The size and distance became our two biggest obstacles when running the _Caterpillar_ suite.

<center>
![Contamination](https://raw.githubusercontent.com/caterpillarproject/wiki/master/media/paper_contam.png "Contamination Plot")
</center>

Our rockstar catalogues only use the high-resolution particles. This means that there will be halos in the outskirts of the simulation which are contaminated. These are shown clearly below. Be sure not to just take all halos within the rockstar catalogues as some of them will be contaminated (underestimated masses, wrong profiles etc.). As a safety, one should only take halos which are within the contamination distance. This changes as a function of redshift so make sure you update your cut for each snapshot. The plots below are for z = 0.

<center>

<img title="Cat-1 contamination information" src="images/Cat-1_contamsub.png" style="max-width: 400px;">
<img title="Cat-2 contamination information" src="images/Cat-2_contamsub.png" style="max-width: 400px;">
<img title="Cat-3 contamination information" src="images/Cat-3_contamsub.png" style="max-width: 400px;">
<img title="Cat-4 contamination information" src="images/Cat-4_contamsub.png" style="max-width: 400px;">
<img title="Cat-5 contamination information" src="images/Cat-5_contamsub.png" style="max-width: 400px;">
<img title="Cat-6 contamination information" src="images/Cat-6_contamsub.png" style="max-width: 400px;">

</center>


