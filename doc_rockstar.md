---
title: Rockstar
tags: [friends of friends, halo finder, Rockstar,rockstar,algorithm,catalogue]
last_updated: November 2, 2015 
summary: "The Caterpillar project primarily uses Rockstar for its halo finding. Here you can find an overview of the code and the properties of the halos you might want to access."
---

## Overview

Rockstar identifies dark matter halos, substructure, and tidal features in phase space.The approach is based on adaptive hierarchical refinement of friends-of-friends groups in six dimensions, which allows for robust (grid-independent, shape-independent, and noise-resilient) tracking of substructure; as such, it is named Rockstar (Robust Overdensity Calculation using K-Space Topologically Adaptive Refinement). The "consistent trees" algorithm for generating merger trees and halo catalogs explicitly ensures consistency of halo properties (mass, position, velocity, radius) across timesteps. The algorithm has demonstrated the ability to increase both the completeness (through inserting otherwise missing halos) and purity (through removing spurious objects) of both merger trees and halo catalogs. In addition, the method is able to robustly measure the self-consistency of halo finders; it is the first to directly measure the uncertainties in halo positions, halo velocities, and the halo mass function for a given halo finder based on actual cosmological simulations.

The code's paper can be found [here](http://arxiv.org/abs/1110.4372). Documentation on getting it running and a more exhaustive documentation of the catalogue's content at its [Bitbucket Repository](https://bitbucket.org/gfcstanford/rockstar).

## Rockstar and Iterative Unbinding

<span style="font-family:Courier">Rockstar</span> is able to find any overdensity in 6D phase space including both halos and streams. To distinguish gravitationally bound halos from other phase space structures, <span style="font-family:Courier">Rockstar</span> performs a single-pass energy calculation to determine which particles are gravitationally bound to the halo. Over-densities where at least 50% of
the mass is gravitationally bound are considered halos, with the exact fraction a tuneable parameter `unbound_threshold` of the algorithm Behroozi et al. (2013).

This definition is generally very effective at identifying halos and subhalos -- but it fails in two important situations. First, if a subhalo is experiencing significant tidal stripping, the 50% cutoff can remove a subhalo from the catalog that should actually exist. We have found that changing the cutoff can recover the missing subhalos, but the best value of the cutoff is not easily determined. Second, <span style="font-family:Courier">Rockstar</span> is occasionally _too_ effective at finding substructure in our high resolution simulations. In particular, it often finds velocity substructures in the cores of our halos that are clearly spurious based on their mass accretion histories and density profiles. Importantly, these two issues do not just affect low mass subhalos, but they can also add or remove halos with \\(V_{max}\\) > 25 km/s.

Both of these problems can be alleviated by applying an iterative unbinding procedure. We have implemented such an iterative unbinding procedure within <span style="font-family:Courier">Rockstar</span>. At each iteration, we remove particles whose kinetic energy exceeds the potential energy from other particles in that iteration. The potential is computed with the <span style="font-family:Courier">Rockstar</span> Barnes-Hut method (see Appendix B of Behroozi et al. (2013). We iterate the unbinding until we obtain a self-bound set of particles. Halos are only considered resolved if they contain at least 20 self-bound particles. All halo properties are then computed as usual, but with the self-bound particles instead of the one-pass bound particles. The iterative unbinding recovers the missing subhalos and removes most but not all of the spurious subhalos. Across 13 of our _Caterpillar_ halos, we recover 52 halos with subhalo masses above 10<sup>8</sup> \\(M_\odot\\) which would have otherwise been lost using the conventional <span style="font-family:Courier">Rockstar</span>. See [Griffen et al. (2015)](http://adsabs.harvard.edu/cgi-bin/bib_query?arXiv:1509.01255) for further details.

## Rockstar Properties

<span style="font-family:Courier">Rockstar</span> outputs a large number of halo properties.

<center>

variable | units |   definition | notes
 :---: | --- | --- | --- 
`id` | | the ID number of the halo | 
`posX` | Mpc/h  |  comoving x-position | 
`posY` | Mpc/h  |  comoving y-position | 
`posZ` | Mpc/h  |  comoving z-position | 
`pecVX` |   km/s | Peculiar x-dir velocity | 
`pecVY` |   km/s | Peculiar y-dir velocity | 
`pecVZ` |   km/s | Peculiar z-dir velocity | 
`corevelX` | km/s | x-dir core velocity | 
`corevelY` | km/s | y-dir core velocity | 
`corevelZ` | km/s | z-dir core velocity | 
`bulkvelX` | comoving km/s | x-dir bulk velocity | 
`bulkvelY` | comoving km/s | y-dir bulk velocity | 
`bulkvelZ` | comoving km/s | z-dir bulk velocity | 
`mvir` |  Msol/h | virial mass set by [Bryan & Norman (1998)](http://cdsads.u-strasbg.fr/abs/1998ApJ...495...80B) prescription | this should only be used for the massive host halos - it is best to just use `mgrav`  
`mgrav` |  Msol/h | virial mass set by [Bryan & Norman (1998)](http://cdsads.u-strasbg.fr/abs/1998ApJ...495...80B) prescription | when using the catalogues, only use this quantity as it is the gravitationally bound mass of the halo - especially for subhalos  
`rvir` |  kpc/h | virial radius set by [Bryan & Norman (1998)](http://cdsads.u-strasbg.fr/abs/1998ApJ...495...80B) prescription | 
`child_r` | |  | 
`vmax_r` | km/s |  | 
`mgrav` |   Msol/h  | gravitational mass | 
`vmax` |  km/s  | Maximum circular velocity | 
`rvmax` |  kpc/h | radius at maximum circular velocity | 
`rs` | kpc/h |  NFW scale radius | 
`rs_klypin` | kpc/h | Klypin defined scale radius | better for smaller halos
`vrms` |  km/s  | rms velocity | 
`Jx` | | x-component angular momentum | 
`Jy` | | y-component angular momentum | 
`Jz` | | z-component angular momentum | 
`Epot` | | Potential energy of the halo | 
`spin` | | Peebles defined spin parameter | This should only be used for halos with a large number of particles (~500+)
`spin_bullock` | | Bullock's modified spin parameter | See above.
`xoff` | | Position offset parameter abs(center of mass position - halo position) | 
`voff` | | Velocity offset parameter abs(center of mass velocity - halo velocity) | 
`b_to_a` | | Halo ellipticity b/a | 
`c_to_a` | | Halo ellipticity c/a | 
`b_to_a2` | |  Halo ellipticity b/a (different prescription) | 
`c_to_a2` | |  Halo ellipticity c/a (different prescription) | 
`A[x]` | | x-axis for ellipticity a | 
`A[y]` | | y-xis for ellipticity a | 
`A[z]` | | z-xis for ellipticity a | 
`A2[x]` | |  x-axis for ellipticity a (different prescription) | 
`A2[y]` | |  y-xis for ellipticity a (different prescription) | 
`A2[z]` | |  z-xis for ellipticity a (different prescription) | 
`T/abs(U)` | |  Virial ratio (kinetic/abs(potential)) | 
`min_pos_err` | |  Minimum error in position | 
`min_vel_err` | |  Minimum error in bulk velocity | 
`min_bulkvel_err` | | Minimum bulk velocity error.  | 
`total_npart` | |  The total number of particles associated with this halo (subs included) | 
`npart` | |  The number of particles associated with a halo (does not include subhalos) | 

</center>

## Rockstar Full Particles Binary

Summary of changes for full particle binary output. 8/19/2014 (Alex Ji)

Alex added a <span style="font-family:Courier">Rockstar</span> option to be able to output in binary all of the particle ids belonging to each halo. This means there is no need to recursively get particles from subhalos in `RSDataReader`. It also means particles from subhalo fof groups that were deemed unbound or too small, and hence not previously written out, are now correctly written out to the halo they should belong to.

Usage: Compiling When compiling <span style="font-family:Courier">Rockstar</span>, use the command % make full_particle_binary This automatically compiles with HDF5 also.

### Config File 

In the <span style="font-family:Courier">Rockstar</span> config file, specify `FULL_PARTICLE_BINARY = num processors` to write out full binary files This should be set to the same as `NUM_WRITERS`. It could be less, but I don't see any reason for that.
Also specify `DELETE_BINARY_OUTPUT_AFTER_FINISHED = 1` This deletes the standard .bin files. The new files contain all of the same info as the old .bin files, so they are just a waste of space.

### Reading Data
Use `RSDataReader.py` version 7. There is now a new parameter called `total_npart` which specifies the total number of particles belonging to a halo, including substructure. 

* `get_particles_from_halo()` will now return all particles from a halo. 
* `get_all_particles_from_halo()` returns exactly the same thing in version=7. T

he first “npart” particles from any given halo are exactly the same particles you would have gotten in the original <span style="font-family:Courier">Rockstar</span> output. The extra particles after that are the ones we were after.

* the files now end with the extension `.fullbin` instead of `.bin`

They are ~10% larger due to the extra particles.

### Changes to Rockstar code

Files Modified: `config.template.h Makefile halo.h properties.c meta_io.c io_internal.c io_internal.h`
In config file, will need to specify `FULL_PARTICLE_BINARY = num processors` to write out full binary files This config option is now added to config.template.h and by default set to 0.
When compiling, specify the option: `$ make full_particle_binary` defines the flag `FULL_PARTICLE_BINARY_FLAG` This will compile a version such that the halo struct has an extra parameter total_num_p, found in `halo.h`. It will also compile with hdf5 since this is the case we most often want. `total_num_p` is assigned in `properties.c` in the function `calc_additional_halo_props()` This corresponds to the total number of particles belonging to the halo, including particles assigned to fof groups that were not printed out (for being unbound, too small, etc.) The extra parameter for each halo needs to be read in with `RSDataReader.py` version 7. it is named 'total_npart' for now.
In the file `meta_io.c`, in the function output_halos() Added another if statement that calls `output_full_particles_binary()` to print data as we want it. Function called only if `FULL_PARTICLE_BINARY > 0` as specified in config file.
Wrote `output_full_particles_binary()` to `io_internal.c`. It is just like `output_binary()` except it uses a recursive function, `print_child_particles_binary()` to print all of the particles, not just `num_p` of them. Declared `output_full_particles_binary()` in `io_internal.h`

`print_child_particles_binary()` in `io_internal.c` is my function which is a modification of `print_child_particles()` to recursively trace child halos and print their particles ids to binary, not ascii. `print_child_particles_binary()` did not need to be declared in any header file. Just written in file before function that calls it.
Output files will now be named `halos_x.y.fullbin` x is the snap number y is the chunk number. Ranges from 0 to FULL_PARTICLES_BINARY-1

In the halo output, `p_start` corresponds to the starting position in the particle array p of the processor it is on where particles are belonging to that halo. We write this out as `numstart` in `RSDataReader`. It was not used before, and now shouldn't ever be used.

At the moment, `cat.total_particles` will not be truly accurate. It will refer to the sum of npart of every halo. It would be easy to change this to refer to the sum of `total_npart` for each halo in `RSDataReader`. I don't see a good use for `cat.total_particles` either way though. The binary header information will specify num_particles as the sum of `npart`, or `num_p`. Changing this is a bad idea, as it will break other parts of how <span style="font-family:Courier">Rockstar</span> is run. To specify a different particle count, such as number of unique particles written to file, or sum of `total_num_p`, would require an extra parameter to the `struct binary_output_header`, or to change `num_particles` just in the output. I don't see a good use for this right now.

Another note: Files are not written out in order to keep halo ids written out in order. Ex: halos.0.bin might write halos 0 – 250 `halos.1.bin` would write out halos 1001-1250 and `halos.2.bin` would write out halos 251-500. This means the unsorted data[i] array in `RSDataReader` will not always have the property that `data[i]` is the halo of `id = i`.

## Running Rockstar/P-Gadget3

This will be a brief tutorial on running <span style="font-family:Courier">Rockstar</span>/<span style="font-family:Courier">P-Gadget3</span> using code developed by the MIT group (Brendan, Alex, Greg). 

So you have a Gadget simulation output and you want to run <span style="font-family:Courier">Rockstar</span> on it.

First you'll need a working version of <span style="font-family:Courier">Rockstar</span> which works with the read modules that we will be using later. As of April 2014, the current version of <span style="font-family:Courier">Rockstar</span> (RC2+ with hdf5 support) works with our modules. Be sure to also get `consistent-trees-0.9.9.2`.

1. Go to your <span style="font-family:Courier">Rockstar</span>-hdf5 directory and run "make with_hdf5". If it breaks, you need to point to the hdf5 libraries on your system.

```bash
HDF5_FLAGS = -DH5_USE_16_API -lhdf5 -DENABLE_HDF5 -I/bigbang/data/bgriffen/lib/hdf5-1.8.10/include -L/bigbang/data/bgriffen/lib/hdf5-1.8.10/lib -lhdf5 -lz
```

2. Go to your consistent-trees directory and run "make all".

3. Make a folder inside your <span style="font-family:Courier">Rockstar</span>-hdf5/ directory called "cfgs" where you will put your cfg files.

The parent simulation configuration file (parent.cfg) for the MIT halos looks like:

```bash
PARALLEL_IO = 1
PARALLEL_IO_SERVER_INTERFACE = "ib0"
FORK_READERS_FROM_WRITERS = 1
FORK_PROCESSORS_PER_MACHINE = 16
NUM_WRITERS = 128
INBASE = /bigbang/data/AnnaGroup/caterpillar/parent/gL100X10/outputs
FILENAME = snapdir_<snap>/snap_<snap>.<block>.hdf5 
NUM_BLOCKS = 256                       
FILE_FORMAT = "AREPO"            
FULL_PARTICLE_CHUNKS = 1             
OUTBASE=/bigbang/data/AnnaGroup/caterpillar/parent/gL100X10/Rockstar
AREPO_LENGTH_CONVERSION=1
AREPO_MASS_CONVERSION=1e+10
SNAPSHOT_NAMES=/bigbang/data/AnnaGroup/caterpillar/parent/gL100X10/Rockstar/snapparent.dat
FORCE_RES=0.002
OUTPUT_FORMAT = "BOTH"
```

You'll have to modify this to suite your needs. This is suited for infiniband using 16 cores per machine and `NUM_WRITERS = Nnodes*Ncores` being used. `NUM_BLOCKS` is also for the number of input files for a given snapshot which you specified for Gadget. You can leave it as `AREPO` as this is just default for using the HDF5 snapshots. You just need to make sure you set the `LENGTH` and `MASS` conversions correctly. The ones above are native for `P-Gadget3`. Be also sure to get `FORCE_RES` or force softening correct. Here we use 1/40 times the mean inter-particle separation or (1/40)*boxwidth/(np)^(1/3) which in our case is 100/2^10/40 ~ 0.002. If you have the space, I would suggest leaving OUTPUT_FORMAT to be BOTH. You can clean up the files you don't want later. Snapshot names is just a file listing the snapshot names `000,001,002 ...` up to the number of snaps, 127 in this case.

4. You will then have to submit the <span style="font-family:Courier">Rockstar</span> run to the cluster. For a cluster like Stampede or other SLURM build systems you can adapt the following:

```bash
#!/bin/bash
#SBATCH -o parentrock.o%j
#SBATCH -e parentrock.e%j
#SBATCH -t 48:00:00
#SBATCH -p normal
#SBATCH --mail-user=bgriffen@mit.edu
#SBATCH -J parentrock
#SBATCH --mail-type=ALL
#SBATCH -n 512

module load hdf5

rsdir=/home1/02670/bgriffen/Rockstar
exe=/home1/02670/bgriffen/Rockstar/rockstar

cd $rsdir
outdir=/scratch/02670/bgriffen/gL100X10/Rockstar

$exe -c $rsdir/cfgs/parent.cfg &
#$exe -c $outdir/restart.cfg & #only use for restarting the run, comment line above.
cd $outdir
perl -e 'sleep 1 while (!(-e "auto-rockstar.cfg"))'
srun -n 512 $exe -c auto-rockstar.cfg
```

If you are running on PBS, please email [Brendan Griffen](brendan.f.griffen@gmail.com) for a PBS version which works just as well.


