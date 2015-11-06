---
title: MIT Compute Cluster
tags: [cluster,computing,hardware,slurm,mit,amd]
last_updated: November 3, 2015
summary: "Information relating to the compute cluster at MKI."
---

The MKI cluster (with respect to Caterpillar) at MIT has one login node (`antares.mit.edu`) which connects to a three primary blocks of slave nodes and one analysis node (`bigbang.mit.edu`). 

## Compute Nodes (for farming jobs)

### RegNodes
Block 1:
* 22 nodes (176 cores)
* 8 core [2 Ghz]
* 16GB memory
* 1 Gbit interconnect
 
Block 2:
* 9 nodes (72 cores)
* 8 cores, 2.5 Ghz
* 16GB memory
* 1 Gbit interconnect

This partition should be used for _serial jobs_. All nodes can be accessed via using the partition `RegNodes`.

### HyperNodes
* 12 nodes (144 cores)
* 12 cores, 2.3 Ghz (24 with hyper-threading)
* 24GB memory
* 10 Gbit interconnect

This partition should only be used for _MPI enabled_ codes as it has faster interconnect.

### AMD64
* 8 nodes, 512 cores
* 64 cores, 2.2 Ghz
* 256GB memory
* 10 Gbit interconnect

This partition should _only be used for extremely expensive runs which require 100GB++ memory and for codes that are MPI enabled_. You can also used openMP multi-threaded jobs which require large amounts of memory (1 job per node). At this stage, there are no time-limits or fixed memory limits for each of the nodes.

## Analysis Node (for plotting, running scripts etc.)

The Frebel group as MIT also has access to an analysis node which is connected to the Caterpillar data called `bigbang`. It can be accessed via ssh `username@bigbang.mit.edu`. See Data Access on this site to get started with your analysis once you understand the storage layout below.

### Storage File Systems/Servers

The Frebel group has access to ~120GB RAID storage and 300TB+ of ZFS tape storage. The ZFS tape storage consists of two 130TB servers (`grinder` and `cruncher`). These are significantly slower to do I/O from _the first time_. They each have solid state drives on them acting as a large cache which makes reading data comparable to the RAID drives if you are repeatedly accessing the same snapshot on the tape, for example. The directory structure is as follows:

```bash
login node: ssh username@bigbang.mit.edu

# ZFS TAPE STORAGE SERVERS (not as fast as the RAID  slow to load)
 -> cruncher/
  -> caterpillar/
   -> parent/
   -> halos/
    -> H1079897/

 -> grinder/  # ZFS tape drive with 
  -> caterpillar/
   -> halos/
    -> H1387186
    ...
  -> aquarius/
   -> Aq-A/
   -> Aq-B/
   -> Aq-C/
   -> Aq-D/
   -> Aq-E/
   -> Aq-F/

# RAID STORAGE
 -> data/AnnaGroup/     # RAID drive with all runs lower than LX14
  -> caterpillar/
   -> halos/            # master folder with all of the caterpillar data within
    -> H1079897/        # halos are named according to their folders
    ...
   -> ics/
    -> lagr/            # lagrangian volume files
   -> parent/           # parent simulation runs
    -> gL100X10/        # actual parent simulation (X10 refers to Level_max=10 in MUSIC)
```

### Current Storage Status

We are quite limited with storage space and so we ask you to request more storage if you need it. You will then be allocated a directory on `bigbang/data/{username}/`. The current break down of the storage capability is as follows:

```bash
bigbang% df -h
Filesystem            Size  Used Avail Use% Mounted on
/dev/mapper/vg_bigbang-lv_home
                       57G  1.9G   52G   4% /home
/dev/mapper/vg_bigdata-lv_bigdata
                      121T   93T   28T  78% /bigbang/data
/cruncher/data
                      127T  114T   14T  90% /cruncher/data
/grinder/data
                      127T   74T   54T  59% /grinder/data
/spacebase/data
                       36T   33M   36T   1% /spacebase/data
```

# Where is the data _actually_ stored?

Due to our storage limitations we have opted for the following strategy

* you should only ever get your hpaths from haloutils via `hpaths = haloutils.get_paper_paths_lx(14)`, for example. Do not get your halo paths from ever going into `caterpillar/halos/middle_mass_halos/*`.
* all simulation runs lower than LX14 (i.e. LX11,12,13) have been stored on the RAID drives.
* all LX14 runs are _symbolically linked_ on the RAID drives but might not actually exist physically on the drives. 
  * all halo catalogues are on the RAID drives so access for these is very fast
  * all particle data _except_ the last snapshot are actually stored on the ZFS tape drives (though it will appear in outputs/ as a symlink)
  * only the last snapshot of the particle data is actually stored on the RAID drives
  * all postprocessed catalogues in `analysis/` of each halo directory are on the RAID drives
* all of the aquarius data is on the ZFS tape storage as this is not often used
* the parent simulation (`gL100X10`) is stored in the same way as the LX14 runs: only the last snapshot of the particle data is stored on the RAID drives, everything else is on the ZFS tape drives.
