---
title: Setting Up Your Work Environment
tags: [modules,analysis,code,python,data]
last_updated: July 12, 2016
summary: "Once you have an account on the MKI compute cluster you can use these tools to get your started on accessing the Caterpillar data."
published: true
---

* Read the [cluster information](https://github.com/caterpillarproject/wiki/wiki/Cluster-Information).

* Keep in mind: `bigbang.mit.edu` (our group box) is where you want to do your analysis, access data etc. and `antares.mit.edu` is the login node for the cluster for the whole Institute. Remember: analysis = bigbang, jobs = antares! See Compute Cluster Information in the sidebar for more detail on these two machines and how the data is actually distributed.

* Get our modules from the [Caterpillar Project github page](https://github.com/caterpillarproject). These include the following two critical libraries:

```bash
git@github.com:caterpillarproject/modules.git
git@github.com:caterpillarproject/analysis.git
```

You can obtain them in the following way (ensure you are in the directory you want them to be placed):

```bash
> git clone git@github.com:caterpillarproject/modules.git
```

(sorry we dont have any support for IDL or Matlab at this time - please share any you develop!)

* Add the following to your `.cshrc` file.

```bash
setenv PYTHONPATH /path/to/modules:$PYTHONPATH
```

* Install the [Anaconda](https://store.continuum.io/cshop/anaconda/) python distribution and be sure to run `conda update` periodically. You can also

* You will also need `asciitable`, `h5py` and `pandas` to make full use of our module package. You can install some of these packages with Anaconda by `> conda install pandas`, for example. If that fails, simply do ` > pip install pandas` etc. to install each of these. When you open `ipython`, you should be able to import each of these - please make sure you can before importing the modules! 

You're now ready to roll!
