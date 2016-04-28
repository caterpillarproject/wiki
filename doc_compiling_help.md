---
title: Compiling Help
tags: [mit,bigbang,odyssey,stampede,arepo,gadget]
last_updated: November 3, 2015
summary: "Here are a variety of environment settings to get you going on clusters. Hopefully this saves you a boat load of time."
---

## Antares

### P-Gadget3

```bash
ifeq ($(SYSTYPE),"antares")
CC   =  mpicc
OPT  += -DMPICH_IGNORE_CXX_SEEK
OPTIMIZE   = -std=c99 -g -O2 #-xW -ipo -Wall -Wno-unknown-pragmas
GSL_INCL = -I/bigbang/data/bgriffen/lib/gsl-1.9/include
GSL_LIBS = -L/bigbang/data/bgriffen/lib/gsl-1.9/lib -Wl,-R/bigbang/data/bgriffen/lib/gsl-1.9/lib -lgsl -lgslcblas
FFTW_INCL= -I/bigbang/data/bgriffen/lib/fftw2/include
FFTW_LIBS= -L/bigbang/data/bgriffen/lib/fftw2/lib
MPICHLIB = -L/opt/mpich2/gnu/lib
HDF5INCL = -I/bigbang/data/bgriffen/lib/hdf5-1.8.13/include -DH5_USE_16_API
HDF5LIB  = -L/bigbang/data/bgriffen/lib/hdf5-1.8.13/lib -lhdf5 -lz
endif
```

### Gadget4

The Makefile will simply contain:

```bash
ifeq ($(SYSTYPE),"antares")
include Makefile.comp.antares
include Makefile.path.antares
endif
```

Makefile.comp.antares:

```bash
CC   =  mpicxx  -xc++ -std=c++11  # sets the C-compiler
CPP  =  mpicxx  -std=c++11 # sets the C++-compiler
ifeq (EXPLICIT_VECTORIZATION,$(findstring EXPLICIT_VECTORIZATION,$(CONFIGVARS)))
CFLAGS_VECTOR += -mavx   # enables generation of AVX instructions (used through vectorclass)
CPV  =  $(CPP)
else
CFLAGS_VECTOR =
CPV  =  $(CC)
endif
```

Makefile.path.antares will contain:

```bash
GSL_INCL   = -I/bigbang/data/bgriffen/lib/gsl-1.9/include
GSL_LIBS   = -L/bigbang/data/bgriffen/lib/gsl-1.9/lib -Wl,-R/bigbang/data/bgriffen/lib/gsl-1.9/lib -lgsl -lgslcblas
FFTW_INCL  = -I/bigbang/data/bgriffen/lib/fftw-3.3.4/include
FFTW_LIBS  = -L/bigbang/data/bgriffen/lib/fftw-3.3.4/lib
MPICHLIB   = -L/opt/mpich2/gnu/lib
HDF5_INCL  = -I/bigbang/data/bgriffen/lib/hdf5-1.8.10/include
HDF5_LIBS  = -L/bigbang/data/bgriffen/lib/hdf5-1.8.10/lib -lhdf5 -Xlinker -R -Xlinker /bigbang/data/bgriffen/lib/hdf5-1.8.10/lib
HWLOC_LIBS = -L/bigbang/data/bgriffen/lib/hwloc-1.8.1/lib -lhwloc
```
### Arepo

```bash
ifeq ($(SYSTYPE),"antares")
CC   =  mpicc -g
OPTIMIZE =  -O1 -g -Wall -m64
GSL_INCL = -I/home/bgriffen/lib/gsl-1.9/include
GSL_LIBS = -L/home/bgriffen/lib/gsl-1.9/lib
FFTW_INCL= -I/home/bgriffen/lib/fftw-2.1.5/include
FFTW_LIBS= -L/home/bgriffen/lib/fftw-2.1.5/lib
MPICHLIB = -L/opt/mpich2/gnu/lib
HDF5INCL = -I/bigbang/data/bgriffen/lib/hdf5-1.8.10/include
HDF5LIB  = -L/bigbang/data/bgriffen/lib/hdf5-1.8.10/lib -lhdf5 -lz -Wl,-R/bigbang/data/bgriffen/lib/hdf5-1.8.10/lib
endif
```

## Odyssey

### P-Gadget3

Set the correct systype:

```bash
SYSTYPE="odyssey"
```

```bash
ifeq ($(SYSTYPE),"odyssey")
CC   =  mpicc
ifeq (SOFTDOUBLEDOUBLE,$(findstring SOFTDOUBLEDOUBLE,$(CONFIGVARS)))
CC   =  mpiCC
endif
OPT      +=  -DMPICH_IGNORE_CXX_SEEK
OPTIMIZE =   -O1 -g -Wall -m64 #-O3 -g -Wall -m64
GSL_INCL =
GSL_LIBS =
FFTW_INCL=
FFTW_LIBS=
MPICHLIB =
HDF5INCL =  -DH5_USE_16_API
HDF5LIB  =  -lhdf5
endif
```

### Gadget4

Set your SYSTYPE:

```bash
SYSTYPE="odyssey"
```

Makefile.path.odyssey:

```bash
GSL_INCL =
GSL_LIBS =
FFTW_INCL= -I/n/home03/cpopa/fftw3/include
FFTW_LIBS= -L/n/home03/cpopa/fftw3/lib
MPICHLIB =
HDF5_INCL =
HDF5_LIBS  =  -lhdf5 -lz
HWLOC_INCL=
HWLOC_LIB = -lhwloc
```

Makefile.comp.odyssey:

```bash
CC   =  mpiicc -std=c99    # sets the C-compiler
OPT  += -DMPICH_IGNORE_CXX_SEEK
CPP  =  mpiicpc  -std=c++11  # sets the C++-compiler
OPTIMIZE =   -O2 -Wno-unknown-pragmas
CPV  =  $(CC)
LINKER = mpiicpc
ifeq (NUM_THREADS,$(findstring NUM_THREADS,$(CONFIGVARS)))
OPTIMIZE +=  -fopenmp
OPT  += -DIMPOSE_PINNING -DSOCKETS=4 -DMAX_CORES=16
else
OPTIMIZE +=  -Wno-unknown-pragmas
endif
```

### Arepo

Set the correct systype:

```bash
SYSTYPE="odyssey-intel"
```

You need to use the new modules on Odyssey:

```bash
module purge
source new-modules.sh
module load legacy/0.0.1-fasrc01
module load intel/15.0.0-fasrc01
module load intel-mpi/5.0.1.035-fasrc01
module load centos6/hdf5-1.8.11_gcc-4.4.7
module load  gmp/5.1.3-fasrc01
module load  hpc/fftw-2.1.5_impi-4.1.0.024_intel-13.0.079
```

Within the Makefile, place:

```bash
ifeq ($(SYSTYPE),"odyssey-intel")
CC       =  mpiicc
OPT      += -DMPICH_IGNORE_CXX_SEEK
OPTIMIZE =  -std=c99 -O2 -parallel -ipo -funroll-loops
GSL_INCL =
GSL_LIBS =
FFTW_INCL= -I/n/home03/cpopa/fftw3/include
FFTW_LIBS= -L/n/home03/cpopa/fftw3/lib
MPICHLIB =
HDF5INCL = -DH5_USE_16_API
HDF5LIB  = -lhdf5 -lz
ifeq (MONOTONE_CONDUCTION,$(findstring MONOTONE_CONDUCTION,$(CONFIGVARS)))
HYPRE_INCL = -I/n/home13/kannan/hypre-2.10.0b/src/hypre/include
HYPRE_LIB = -L/n/home13/kannan/hypre-2.10.0b/src/hypre/lib -lHYPRE
endif
ifeq (TGCHEM,$(findstring TGCHEM,$(CONFIGVARS)))
CVODE_INCL =  -I/n/home01/tgreif/libs/cvode/include
CVODE_LIB  =  -L/n/home01/tgreif/libs/cvode/lib -lsundials_cvode -lsundials_nvecserial
endif
ifeq (NUM_THREADS,$(findstring NUM_THREADS,$(CONFIGVARS)))
OPTIMIZE += -fopenmp
OPT      += -DIMPOSE_PINNING -DSOCKETS=4 -DMAX_CORES=16
else
OPTIMIZE += -diag-disable 3180
endif
endif
```

Hopefully you find these helpful. Please leave a comment below if you have any questions.