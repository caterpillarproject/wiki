---
title: Semi-analytic Models
tags: [sams,models,baryons,stars,stellar]
last_updated: November 3, 2015
summary: "Here is a brief summary of the SAMs which are available on the Caterpillar project."
---

## L-Galaxies

Work has been done to develop [L-Galaxies](http://galformod.mpa-garching.mpg.de/public/LGalaxies) to work with the Caterpillar data. Details of the SAM can be found [here](http://galformod.mpa-garching.mpg.de/public/LGalaxies/Henriques2014a.pdf). The major components can be found [here](http://galformod.mpa-garching.mpg.de/public/LGalaxies/LGalaxies_slides.pdf).

### Accessing L-Galaxies trees

Code has been developed to convert consistent-trees merger trees into a format which L-Galaxies can read. These convereted trees can be found at the following locations for each of the halos.

```bash
/bigbang/data/AnnaGroup/caterpillar/halos/{type}_mass_halos/H{haloid}/H{haloid}_EB_Z127_P7_LN7_LX14_O4_NV4/sam/LGalaxies/treedata/
```
An example script to submit to the cluster to run a model:

```bash
#!/bin/bash
#SBATCH -o LGalaxies.o%j
#SBATCH -e LGalaxies.e%j
#SBATCH -J LGalaxiesLX11
#SBATCH -p AMD64
#SBATCH -n 64
mpirun -np 64 ./L-Galaxies ./input/input_Caterpillar_PLANCK_PLANCK.par 1>OUTPUT 2>ERROR
```

See the cluster information [here](http://www.caterpillarproject.org/wiki/doc_mit_cluster.html) for resource details. 

### Accessing L-Galaxies output

TBD.

The project is still very much in development so please contact Brendan Griffen if you would like to be involved.

