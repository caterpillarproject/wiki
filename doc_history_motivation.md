---
title: What Is The Caterpillar Project?
tags: [history,overview,motivation,science]
last_updated: November 3, 2015
summary: "To get started with the Caterpillar project, first make sure you have a firm understanding of the strengths and weaknesses of the data."
---

## A Brief History

The Caterpillar project was the brainchild of Prof. Anna Frebel and Lars Hernquist at a meeting at the CfA in 2013. Phillip Zukin was then a PhD student under Edmund Bertschinger who was temporarily employed to carry out the first parent volume simulations that would become the Caterpillar project. His work continued until Brendan Griffen arrived as the new postdoc to take charge of the project in its entirety. Graduate students Greg Dooley and Alex Ji are also working part time on the project. At a conference at UC Irvine in early 2014 between Anna Frebel, Brian O'Shea, Brendan Griffen and Facundo Gomez, it was agreed that we would combined datasets and future efforts (Brian and Facundo had a similar sized project for studying stellar halos at the time). In September of 2015, the first Caterpillar paper was put [on arxiv](http://arxiv.org/abs/1509.01255v1).

## Motivation

The Caterpillar project is a large simulation suite of dark matter halos of mass similar in size to that of the Milky Way. By virtue of the extremely large number of halos, temporal and spatial resolution of the simulations, it is the largest simulation suite of its kind in the world. We plan to extend this initial release sample to 70 halos in total. Many of these 70 halos have already completed but are currently unpublished. All halos are accessible to the community by contacting the collaboration.

The goal of the Caterpillar Project is to use these relatively inexpensive dark matter simulations to:

* statistically probe the merger history of Milky Way-like galaxies,
* understand the origin and evolution of the satellite systems of Milky Way-like galaxies and
* determine to what extent the Milky Way is unusual relative to its similar sized cousins.

## Summary of Method:

{{site.data.alerts.tip}} Much more detail of the Caterpillar project can be found in Griffen et al. (2015).{{site.data.alerts.end}}

The _Caterpillar_ suite was run using <span style="font-family:Courier">P-Gadget3</span> and <span style="font-family:Courier">Gadget4</span>, tree-based _n_-body codes based on <span style="font-family:Courier">Gadget2</span> (Springel et al. 2005). For the underlying cosmological model we adopt the \\(\Lambda\\)CDM parameter set characterised by a Planck cosmology given by, \\(\Omega_m=0.32\\), \\(\Omega_\Lambda=0.68\\), \\(\Omega_b=0.05\\), \\(n_s=0.96\\), \\(\sigma_8=0.83\\) and Hubble constant, H = 100 \\(h\ km\ s^{-1}\ Mpc^{-1}\\) = 67.11 \\(km\ s^{-1}\ Mpc^{-1}\\) (Planck et al. 2014). All initial conditions were constructed using music (Hahn & Able 2011).  We identify dark matter halos via <span style="font-family:Courier">Rockstar</span> (Behroozi et al. 2013) and construct merger trees using <span style="font-family:Courier">consistent-trees</span> (Behroozi et al. 2012). <span style="font-family:Courier">Rockstar</span> assigns virial masses to halos, \\(M_{vir}\\), using the evolution of the virial relation from Bryan & Norman (1998). for our particular cosmology. At _z_ = 0, this definition corresponds to an over-density of 97x the critical density of the Universe. We have modified rockstar to output _all_ particles belonging to each halo so we can reconstruct any halo property in post-processing if required. We have also improved the code to include iterative unbinding. In our work, we restrict our definition of virial mass to include only those particles which are bound to the halo.


