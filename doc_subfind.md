---
title:  SUBFIND
tags: [content-types]
keywords: groups, api, structure
last_updated: November 2, 2015
summary: "SUBFIND is our alternative halo finder which we use for comparisons to Rockstar. It is the traditionally SUBFIND found in runs such as Aquarius."
---

## <span style="font-family:Courier">SUBFIND</span>

For a nice summary of how <span style="font-family:Courier">SUBFIND</span> works, please see [this talk](http://popia.ft.uam.es/public/HaloesGoingMAD/PRESENTATIONS/Subfind-FIannuzzi.pdf) by Volker Springel, the code author.

To access the <span style="font-family:Courier">SUBFIND</span> catalogues and associated smoothing lengths for the particles, head to any _Caterpillar_ halo directory on `bigbang.mit.edu`. There you will find the following:

```bash
H1387186_EB_Z127_P7_LN7_LX14_O4_NV4
 -> outputs/      # gadget raw snapshot output (particle data)
  -> groups_319/  # the subfind catalogues are also stored (mostly for the last snapshot)
  -> hsmldir_319/ # the smoothing lengths for the corresponding particle data
 -> analysis/     # post-processed output files (halo profiles, mass functions, minihalos etc.)
```

See Data Access to load the <span style="font-family:Courier">SUBFIND</span> catalogues.