---
title: XSEDE
tags: [single-sourcing]
keywords: includes, conref, dita, transclusion, transclude, inclusion, reference
last_updated: August 12, 2015
summary: "Information about the compute clusters at XSEDE."
---


## Stampede: the Caterpillar work horse

As per [XSEDE website](https://www.xsede.org/tacc-stampede).

The TACC Stampede system is a 10 PFLOPS (PF) Dell Linux Cluster based on 6,400+ Dell PowerEdge server nodes, each outfitted with 2 Intel Xeon E5 (Sandy Bridge) processors and an Intel Xeon Phi Coprocessor (MIC Architecture). The aggregate peak performance of the Xeon E5 processors is 2+PF, while the Xeon Phi processors deliver an additional aggregate peak performance of 7+PF. The system also includes a set of login nodes, large-memory nodes, graphics nodes (for both remote visualization and computation), and dual-coprocessor nodes. Additional nodes (not directly accessible to users) provide management and file system services.

One of the important design considerations for Stampede was to create a multi-use cyberinfrastructure resource, offering large memory, large data transfer, and GPU capabilities for data-intensive, accelerated or visualization computing. By augmenting some of the compute-intensive nodes within the system with very large memory and GPUs there is no need to move data for data-intensive computing, remote visualization and GPGPU computing. For those situations requiring large-data transfers from other sites, 4 high-speed data servers have been integrated into the Lustre file systems.

* Compute Nodes: The majority of the 6400 nodes are configured with two Xeon E5-2680 processors and one Intel Xeon Phi SE10P Coprocessor (on a PCIe card). These compute nodes are configured with 32GB of "host" memory with an additional 8GB of memory on the Xeon Phi coprocessor card. A smaller number of compute nodes are configured with two Xeon Phi Coprocessors -- the specific number of nodes available in each configuration at any time will be available from the batch queue summary.

* Large Memory Nodes: There are an additional 16 large-memory nodes with 32 cores/node and 1TB of memory for data-intense applications requiring disk caching to memory and large-memory methods.
Visualization Nodes: For visualization and GPGPU processing 128 compute nodes are augmented with a single NVIDIA K20 GPU on each node with 5GB of on-board GDDR5 memory.

* File Systems: The Stampede system supports a 14PB global, parallel file storage managed as three Lustre file systems. Each node contains a local 250GB disk. Also, the TACC Ranch tape archival system (60 PB capacity) is accessible from Stampede.

* Interconnect: Nodes are interconnected with Mellanox FDR InfiniBand technology in a 2-level (cores and leafs) fat-tree topology.

**Notes about running Caterpillar halos on stampede:**

You need to specify a minimum 512 cores (-N 32 -n 512) to get started. Depending on the error you should change the following parameters in order and try again.

* Config.sh: `DOUBLEPRECISION_FFTW`
* Config.sh: `MULTIPLEDOMAINS` 16 -> 32
* param.txt.: `Buffersize` 100 -> 50
* param.txt: `MaxMemSize` 3300