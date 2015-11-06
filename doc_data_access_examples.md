---
title: Data Access Examples
tags: [examples,python,code,access]
last_updated: August 12, 2015
summary: "Here are some examples to get you started working with the Caterpillar data."
---

## Overview

Here you'll find all sorts of information about analysing the Caterpillar halos. If you still aren't sure what to do after reading the documentation please send an email to [Brendan Griffen](mailto:brendan.f.griffen@gmail.com). Make sure you have setup your work environment correctly and obtained the necessary libraries.

## Examples (under construction)

One example combining these two modules with some general inspection would look something like as follows:

```python
import brendanlib.grifflib as glib
import haloutils as htils

# load the first 24 halos
# (just change 14 to 11 for the lower resolution halos)

hpaths = htils.get_paper_paths_lx(14)

# loop through all high-resolution halos

for hpath in hpaths:

    # strip down the path to just the halo id

    parentid = htils.get_parent_hid(hpath)

    # get the central host id of the zoom-in halo
    # parentid is named from the parent simulation 
    # the zooms have different ids

    zoomid = htils.load_zoomid(hpath)

    # Return the pandas data frame with all the halos, 
    # return mvir for example

    lastsnap = htils.get_lastsnap(hpath)
    halos = htils.load_rscat(hpath,lastsnap,verbose=True)
    mvir_host = halos.ix[zoomid]['mvir'] # units of Msol/h

    # get one specific parameter of the host at
    # z = 0 (quick version of above)

    mvir_host = htils.get_quant_zoom(hpath,'mvir') # units of Msol/h
   
    # return all halo virial masses

    all_mvir = halos['mvir'] # units of Msol/h

    # get the merger tree of the host and all its subs

    cat = htils.load_mtc(hpath,haloids=[zoomid])

    # read in every halo's merger tree

    all_trees = htils.load_mtc(hpath,indexbyrsid=True)
    tree = all_trees[zoomid] 

    # if you feed more than one id to the above 
    # it would be cat[0],cat[1] etc.

    # you can now access the main branch > try tree. then tab complete to see other functions
    mainbranch = tree.getMainBranch() 

    # we also have a short hand version:
    # mainbranch = htils.get_mainbranch(hpath) 
    # which is very fast and skips the reading of the entire progenitor tree if you don't need dit
    # see further down this page for more options
    # output the main branch virial mass

    print mainbranch['mvir'] 

```

### Functions Available Once You've Loaded A Rockstar Catalogue

To load a rockstar catalogue for a given snapshot, you simple do:

```python
rscat = htils.load_rscat(hpath,snapshot,verbose=True)
```

Once you do this you will have the ability to do the following kinds of tasks (`rscat.` then tab complete):

```python
 def __init__(self, dir, snap_num, version=2, sort_by='mvir', base='halos_', digits=2, AllParticles=False):
   def get_particles_from_halo(self, haloID):
       # @param haloID: id number of halo. Not its row position in matrix
       # @return: a list of particle IDs in the Halo
   def get_subhalos_from_halo(self,haloID):
       #Retrieve subhalos only one level deep.
       #Does not get sub-sub halos, etc.
   def get_subhalos_from_halos(self,haloIDs):
       #Returns an array of pandas data frames of subhalos. one data frame
       #for each host halo. returns only first level of subhalos.
   def get_subhalos_from_halos_flat(self,haloIDs):
       #Returns a flattened pandas data frame of all subhalos within
       #the hosts given by haloIDs. Returns only first level of subhalos.
   def get_hosts(self):
       # Get host halo frame only
   def get_subs(self):
       # Get subhalo frame only
   def get_all_subs_recurse(self,haloID):
       # Retrieve all subhalos: sub and sub-sub, etc. 
       # just need mask of all subhalos, then return data frame subset
   def get_all_subhalos_from_halo(self,haloID):
       # Retrieve all subhalos: sub and sub-sub, etc.
       # return pandas data frame of subhalos
   def get_all_sub_particles_from_halo(self,haloID):
       #returns int array of particle IDs belonging to all substructure
       #within host of haloID
   def get_all_particles_from_halo(self,haloID):
       #returns int array of all particles belonging to haloID
   def get_all_num_particles_from_halo(self,haloID):
       # Get the actual number of particles 'total_npart' from halo as opposed to 'npart'.
       # mainly for versions less than 7
   def get_block_from_halo(self, snapshot_dir, haloID, blockname, allparticles=True):
       # quick load a block (hdf5 block) of particles belong to halo.
       # e.g. you want particle positions for haloid = 10 (use blockname="pos")
       # this works fastest on snapshots ordered by id and requires import readsnapHDF5_greg 
   def H(self):
       #returns hubble parameter for rockstar run
   def get_most_gravbound_particles_from_halo(self,snapshot_dir, haloID):
       # Gets most bound particles just based on potential energy for specific halo ID
   def get_most_bound_particles_from_halo(self, snapshot_dir, haloID):
       # Gets most bound particles for halo based on pot. energy and kin. energy
       # if potential block does not exist, it is calculate assuming a spherical halo
   def getversion(self):
       # returns the version of rockstar the run was done within
       # this will include versions made by Alex Ji, Greg Dooley & Brendan Griffen
```

Here is an example of some of these in action:

```python
# load required modules

import haloutils as htils 
import numpy as np

# select the caterpillar halo of interest

want_cat_sim = 5

# select the last snapshot (z = 0)
snapshot = htils.get_lastsnap(hpath)

# load rockstar id of the host halo

zoomid = htils.load_zoomid(hpath)

# get the directory path for this halo

hpaths = htils.get_paper_paths_lx(14)
hpath = hpaths[want_cat_sim-1]

#Load Halos

halos = htils.load_rscat(hpath,snapshot)

#Select host halos

hosts = halos.get_hosts()

#Select subhalos

subs = halos.get_subs()

# Get positions of subs and hosts

print np.array(hosts.posX),np.array(hosts.posY),np.array(hosts.posZ) 
print np.array(subs.posX),np.array(subs.posY),np.array(subs.posZ) 

#Get particle ids from halo of interest (here it is the host)

print halos.get_particles_from_halo(zoomid) 

#Get virial radius of a specific halo id

print halos.ix[zoomid]['rvir'] # units of kpc/h

```



### Merger Tree Manipulation

Similarly the merger tree catalogues (once loaded) have a number of its own functions.

```python
def getMainBranch(self, row=0):
   """
   @param row: row of the halo you want the main branch for. Defaults to row 0
   Uses getSubTree, then finds the smallest dfid that has no progenitors
   @return: all halos that are in the main branch of the halo specified by row (in a np structured array)
   """
def getMMP(self, row):
   """
   @param row: row number (int) of halo considered
   @return: row number of the most massive parent, or None if no parent
   """
def getNonMMPprogenitors(self,row):
   """
   return row index of all progenitors that are not the most massive
   These are all the subhalos that were destroyed in the interval
   """
```

These can be used in the following example:

```python
# load required modules

import haloutils as htils 
import numpy as np

# select the caterpillar halo of interest

want_cat_sim = 5

# select the last snapshot (z = 0)
snapshot = htils.get_lastsnap(hpath)

# load rockstar id of the host halo

zoomid = htils.load_zoomid(hpath)

# Load every tree (VERY slow)

trees = htils.load_mtc(hpath)

# Just look at the first tree
# tree = cat[0]
# You can access all trees via: cat[0], cat[1] etc.

# If you want to load the tree for a particular ID from the rockstar catalogue

trees = htils.load_mtc(haloids=[zoomid]) # in this case the host (quite slow) 
tree = cat[0] # tree will contain all progenitors, including subhalos

# Just say you want to index the merger tree by the z = 0 root rockstar id 
# (i.e. the base of the tree). This is quite powerful because you might select
# halos of interest in the rockstar catalogue then want to know, just for those
# what their merger tree is (e.g. say you just want dwarf systems of a particular size)

trees = htils.load_mtc(haloids=[zoomid],indexbyrsid=True) 
tree = trees[zoomid]

# You can just loop through rockstar ids (if you gave it more than one id above)
# and get out the accretion histories for a small sample of trees quite quickly

# to get the main branch

main_branch = tree.getMainBranch()

# print mass evolution
for mass,scale in zip(main_branch['mvir'],main_branch['scale']):
   print "%3.2f: %3.2e" % (scale,mass)

# output
1.00: 2.86e+14 
0.99: 2.89e+14 
0.98: 2.90e+14 
0.97: 2.90e+14 
0.95: 2.88e+14 
0.94: 2.86e+14 
0.93: 2.83e+14 
0.92: 2.80e+14 
0.91: 2.76e+14 
0.90: 2.73e+14 
0.89: 2.64e+14 
0.88: 2.47e+14 ... 
```

## Accessing Particle Data

If you want the Gadget header:

```python

# get a random halo
hpaths = htils.get_paper_paths_lx(14)
hpath = hpaths[0]

header = htils.get_halo_header(hpath)

header.(tab complet)
header.boxsize        header.massarr        header.omegaL
header.cooling        header.metals         header.redshift
header.double         header.nall           header.sfr
header.feedback       header.nall_highword  header.stellar_age
header.filenum        header.npart          header.time
header.hubble         header.omega0 
```

Be sure to divide the relevant quantities (pos, rvir etc.) by `header.hubble`. See the Gadget section in the sidebar for more information on the header and block types available.

If you wanted to get the postions of all the particles for a specific halo (or any block). 

```python
pos htils.load_partblock(hpath,zoomid,"POS ") # units Mpc/h
# "VEL ", "ID  " also work (notice the space)

print pos*1000. # kpc/h

# output
[ 32085.45117188  57312.79296875  44314.35546875]
[ 27002.18554688  10062.73242188   9899.70019531]
[ 26711.08789062   9560.22460938  10165.18847656]
[ 49757.3515625   21470.00195312   6461.90917969]
...
```

If you wanted just the ids for a selection of particle ids:

```python
pos = htils.load_partblock(hpath,zoomid,"POS ",partids=[listofids]) # units Mpc/h
```

