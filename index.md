---
title: Introduction
tags: 
  - "getting-started"
type: first_page
homepage: true
published: true
---

## Recent News
[APJ Jan 14, 2016] The majority of halos have now been upgraded to 320 snaps. 
Some halos had hiccups in the upgrade process. These are H95289 and H1725372 (Cat-20 and Cat-11, respectively). They are currently being fixed and should be online soon. Standard postprocessing plugins have not yet been run.
Status of all Caterpillar halos can be found at this link (updated once per day): https://www.dropbox.com/s/t35s3k7l15257uq/status.png

[APJ] *IMPORTANT!* We are currently re-running rockstar on several LX14 halos to increase their time resolution up to 320 snaps (instead of 256). Due to our slow rockstar, I this will probably take about 2-3 weeks per halo on one AMD node (at best 7 run at a time). While this happens, our standard haloutils.py code (e.g. load\_rscat, load\_partblock ) will not correctly access data from several halo IDs. For the bad halos, Brendan has kept their old rockstar catalogs in halos\_bound\_coarse which you can read manually with RSDataReader and MTCatalogue. The old coarse snapshots are kept on the "grinder" storage server, and if you need them you should ask Brendan.

Here's the list of halo IDs that are good/not good to use.
Only use (14 halos): `447649, 95289, 1354437, 5320, 581141, 1130025, 1725139, 581180, 1232164, 1387186, 1292085, 94687, 1599988, 388476`

You can get a list of good-to-use hpaths with:
```python
hpaths = [haloutils.get_hpath_lx(hid,14) for hid in [447649, 95289, 1354437, 5320, 581141, 1130025, 1725139, 581180, \
 								    1232164, 1387186, 1292085, 94687, 1599988, 388476]]
```

## Overview 

This site provides documentation, toolkits, and other notes for the Caterpillar project. There's a lot of information about how to do a variety of things here, and it's not all unique to the Caterpillar project. But by and large, understanding how to access the Caterpillar data properly will both speed up your workflow and make assisting you much easier should a problem arise.

## What You Can Find Here

Some of the more prominent features of this wiki include, but are not limited to, the following:

* general information about the simulation suite and parameters
* introduction into the data structure and architecture
* hundreds of little functions and tools to access the data
* examples of how to manipulate, particle data, halo and merger tree catalogues
* compute cluster information for running analysis on either your machine or ours
* documentation about postprocessed datasets such as semi-analytic models
* important information about strengths and limitations of the dataset which are not fully published
* rules of thumb to keep in mind (resolution limits, contamination etc.)

## PDF Download Of Documentation

If you would like to download this wiki as a PDF, you can do so here. The PDF is mostly the same  as the online help, except that some elements (such as video or special layouts) don't translate the the print medium, so they're excluded.

<a target="_blank" class="noCrossRef" href="doc_{{site.audience}}_pdf.pdf"><button type="button" class="btn btn-default" aria-label="Left Align"><span class="glyphicon glyphicon-download-alt" aria-hidden="true"></span> PDF Download</button></a>

The PDF contains a timestamp in the header indicating when it was last generated. If you download a PDF, keep in mind that it may go out of date quickly. Always compare your PDF timestamp against the online help timestamp (which you can find in the footer).

## Questions?

If you have questions, contact Brendan Griffen at <a href="mailto:brendan.f.griffen@gmail.com">brendan.f.griffen@gmail.com</a>. His regular site is [brendangriffen.com](http://www.brendangriffen.com). He is eager to make this wiki content as clear as possible, so please let him know if there are areas of confusion that need clarifying.
