Software Compilation Using MPI
==============================

1. Work Enviroment
------------------

To set the work environment the user must load the needed modules. To do that start by using ``module av`` to see the available modules:

.. code-block:: julia

  ---------------------------------- /mnt/beegfs/stack/cn01470/modules/all/Core -----------------------
    ANSYS_CFD/2021R1              Julia/1.8.2-linux-x86_64        gompi/2021b                                   
    ANSYS_CFD/2022R2      (D)     M4/1.4.19                       gompi/2022a              (D)                  
    Anaconda3/2022.05             OpenSSL/1.1                     iimpi/2021b                                   
    Bison/3.8.2                   ant/1.10.11-Java-11             imkl/2021.4.0                                 
    FastQC/0.11.9-Java-11         binutils/2.34                   intel-compilers/2021.4.0                      
    GCC/9.3.0                     binutils/2.37                   intel/2021b                                   
    GCC/11.2.0                    binutils/2.38            (D)    ncurses/6.1                                   
    GCC/11.3.0            (D)     flex/2.6.4                      ncurses/6.2              (D)                  
    GCCcore/9.3.0                 foss/2021b                      pkgconf/1.8.0                                 
    GCCcore/11.2.0                foss/2022a               (D)    zlib/1.2.11                                   
    GCCcore/11.3.0        (D)     gettext/0.20.1                  zlib/1.2.12              (D)                  
    GPAW-setups/0.9.20000         gettext/0.21             (D)                                                  
    Java/11.0.16          (11)    gompi/2020a

   Where:
      Aliases:  Aliases exist: foo/1.2.3 (1.2) means that "module load foo/1.2" will load foo/1.2.3         
      D:        Default Module

In OBLIVION the modules are set in a hierarchical naming structure (Core/Compiler/MPI) and thus the user must start by loading the core modules or toolchains, e.g., load OpenMPI compiled with GCC (``module load GCC/11.2.0`` + ``module load OpenMPI/4.1.1``), load OpenMPI compiled with Intel compilers or load Intel MPI (``module load iimpi/2021b`` or ``module load intel/2021b``).


2. Intel MPI
------------

2.1 Loading intel and iimpi toolchains
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Load Intel MPI sub-toolchain by executing ``module load iimpi/2021b`` and check the loaded modules using ``module list``

.. code-block:: julia

  Currently Loaded Modules:
    1) GCCcore/11.2.0   3) binutils/2.37              5) numactl/2.0.14   7) impi/2021.4.0
    2) zlib/1.2.11      4) intel-compilers/2021.4.0   6) UCX/1.11.2       8) iimpi/2021b


The intel/2021b toolchain includes, among others, the Intel MPI and MKL distributions. Using ``module load intel/2021b`` followed with ``module list`` one obtains

.. code-block:: julia
  
  Currently Loaded Modules:
    1) GCCcore/11.2.0   4) intel-compilers/2021.4.0   7) impi/2021.4.0       10) intel/2021b
    2) zlib/1.2.11      5) numactl/2.0.14             8) imkl/2021.4.0
    3) binutils/2.37    6) UCX/1.11.2                 9) imkl-FFTW/2021.4.0

In both cases the available modules are the same (check with ``module av``):

.. code-block:: julia

  -------------- /mnt/beegfs/stack/cn01470/modules/all/MPI/intel/2021.4.0/impi/2021.4.0 ---------------
    ABINIT/9.6.2                MUMPS/5.4.1-metis                 ecCodes/2.24.2
    ASE/3.22.1                  NCO/5.0.3                         futile/1.8.3
    AmberTools/21               NWChem/7.0.2                      h5py/3.6.0
    ArviZ/0.11.4                OSU-Micro-Benchmarks/5.8          imkl-FFTW/2021.4.0   (L)
    Bambi/0.7.1                 OpenMolcas/22.10                  libGridXC/0.9.6
    Biopython/1.79              PLUMED/2.8.0                      libvdwxc/0.4.0
    CGAL/4.14.3                 PSolver/1.8.3                     libxsmm/1.17
    CP2K/8.2                    ParMETIS/4.0.3                    matplotlib/3.4.3
    ELPA/2021.05.001            PnetCDF/1.12.3                    mkl-service/2.3.0
    ESMF/8.2.0                  PyMC3/3.11.1                      ncview/2.1.8
    FDS/6.7.7                   QuantumESPRESSO/7.0               netCDF-C++4/4.3.1
    FFTW/3.3.10                 SCOTCH/6.1.2                      netCDF-Fortran/4.5.3
    FMS/2022.02                 SPOTPY/1.5.14                     netCDF/4.8.1
    GDAL/3.3.2                  ScaFaCoS/1.0.1                    netcdf4-python/1.5.7
    GEOS/3.9.1                  SciPy-bundle/2021.10              networkx/2.6.3
    GPAW/22.8.0                 Siesta/4.1.5                      numba/0.54.1
    GlobalArrays/5.8.1          SimPEG/0.18.1                     scikit-bio/0.5.7
    HDF5/1.12.1                 SuiteSparse/5.10.1-METIS-5.1.0    scikit-learn/1.0.1
    HPL/2.3                     SuperLU/5.3.0                     spglib-python/1.16.3
    Hypre/2.24.0                Theano/1.1.2-PyMC                 statsmodels/0.13.1
    IMB/2021.3                  VTK/9.1.0                         worker/1.6.13
    Libint/2.6.0-lmax-6-cp2k    Valgrind/3.18.1                   xarray/0.20.1
    MDAnalysis/2.0.0            Wannier90/3.1.0
    MDTraj/1.9.7                XCrySDen/1.6.2

  ------------------- /mnt/beegfs/stack/cn01470/modules/all/Compiler/intel/2021.4.0 -------------------
    BLIS/0.9.0      DFT-D3/3.2.0    GSL/2.7          NLopt/2.7.0   (D)    libxc/5.1.6
    Boost/1.77.0    Flye/2.9        LAPACK/3.10.1    impi/2021.4.0 (L)    xmlf90/1.5.4


2.2 MPIIFORT Vs. MPIF90
~~~~~~~~~~~~~~~~~~~~~~~

With Intel MPI the user gets two flavours: MPI compiled with GCC and MPI compiled with Intel compilers. First lets get the binaries location

.. code-block:: julia

  $ which mpif90

  /mnt/beegfs/stack/cn01470/software/impi/2021.4.0-intel-compilers-2021.4.0/mpi/2021.4.0/bin/mpif90
  
Now lets see the content of the bin folder:

.. code-block:: julia

   $ ls -1 /mnt/beegfs/stack/cn01470/software/impi/2021.4.0-intel-compilers-2021.4.0/mpi/2021.4.0/bin/
  
   ⋮  
   mpicc
   mpicxx
   mpiexec
   mpiexec.hydra
   mpif77
   mpif90
   mpifc
   mpigcc
   mpigxx
   mpiicc
   mpiicpc
   mpiifort
   ⋮ 

``mpif77``, ``mpif90``, ``mpigcc``, and ``mpigxx`` are the executables for MPI compiled against GCC.

``mpiicc``, ``mpiicpc``, and ``mpiifort`` are the executables for MPI compiled against Intel Compilers.

To check this just type

.. code-block:: julia

  $ less /mnt/beegfs/stack/cn01470/software/impi/2021.4.0-intel-compilers-2021.4.0/mpi/2021.4.0/bin/mpif90

obtaining

.. code-block:: julia

  ⋮ 
  # Default settings for compiler, flags, and libraries
  # Determined by a combination of environment variables and tests within
  # configure (e.g., determining whehter -lsocket is needed)
  FC="gfortran"
  ⋮
  
and

.. code-block:: julia

  $ less /mnt/beegfs/stack/cn01470/software/impi/2021.4.0-intel-compilers-2021.4.0/mpi/2021.4.0/bin/mpiifort

obtaining

.. code-block:: julia

  ⋮ 
  # Default settings for compiler, flags, and libraries
  # Determined by a combination of environment variables and tests within
  # configure (e.g., determining whehter -lsocket is needed)
  FC="ifort"
  ⋮ 

You can also run

.. code-block:: julia

  $ mpif90 --version
  GNU Fortran (GCC) 11.2.0
  Copyright (C) 2021 Free Software Foundation, Inc.
  This is free software; see the source for copying conditions.  There is NO
  warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

or

.. code-block:: julia

  $ mpiifort --version
  ifort (IFORT) 2021.4.0 20210910
  Copyright (C) 1985-2021 Intel Corporation.  All rights reserved.


Lets find the PATHs for binary, libraries and include. So, first lets determine the path of the binaries, say, mpif90 (mpiifort):

.. code-block:: julia

  $ which mpiifort
  
  /mnt/beegfs/stack/cn01470/software/impi/2021.4.0-intel-compilers-2021.4.0/mpi/2021.4.0/bin/mpiifort
  
Now look for the paths:

.. code-block:: julia

  $ ls /mnt/beegfs/stack/cn01470/software/impi/2021.4.0-intel-compilers-2021.4.0/mpi/2021.4.0/
  
   benchmarks  bin  include  lib  man

So, here we show some of the listed folders. Lets set the paths to be used in the Makefile

.. code-block:: julia

  MPI_BIN=/mnt/beegfs/stack/cn01470/software/impi/2021.4.0-intel-compilers-2021.4.0/mpi/2021.4.0/bin
  MPI_LIB=/mnt/beegfs/stack/cn01470/software/impi/2021.4.0-intel-compilers-2021.4.0/mpi/2021.4.0/lib
  MPI_INC=/mnt/beegfs/stack/cn01470/software/impi/2021.4.0-intel-compilers-2021.4.0/mpi/2021.4.0/include
  
3. Makefile
-----------

If you are using your software in different machines your Makefile must be tailored for each of them. Here is the procedure to be used.

a) First set the machine you are using through the SYSTYPE variable
b) Then set the PATHs for that machine

Here is an example for two setups in OBLIVION. In the header of the Makefile add the following lines

.. code-block:: julia

  SYSTYPE="oblivion_impi_intel"
  #SYSTYPE="oblivion_impi_gcc"
  #SYSTYPE="marenostrum_impi"

  ifeq ($(SYSTYPE),"oblivion_impi_intel")
  SOFTWARE=/mnt/beegfs/stack/cn01470/software
  MPI_VER=/impi/2021.4.0-intel-compilers-2021.4.0/mpi/2021.4.0
  MPI_BIN=${SOFTWARE}/${MPI_VER}/bin
  MPI_LIB=${SOFTWARE}/${MPI_VER}/lib
  MPI_INC=${SOFTWARE}/${MPI_VER}/include
  F90 = mpiifort
  CC  = mpiicc
  endif
  
  ifeq ($(SYSTYPE),"oblivion_impi_gcc")
  SOFTWARE=/mnt/beegfs/stack/cn01470/software
  MPI_VER=/impi/2021.4.0-intel-compilers-2021.4.0/mpi/2021.4.0
  MPI_BIN=${SOFTWARE}/${MPI_VER}/bin
  MPI_LIB=${SOFTWARE}/${MPI_VER}/lib
  MPI_INC=${SOFTWARE}/${MPI_VER}/include
  F90 = mpif90
  CC  = mpicc
  endif
    
Note that three setups are referred in SYSTYPE and oblivion_impi_intel was the chosen one. Now, in the Makefile there are also the OPTS, OBJS, etc....

4. Software Compilation
-----------------------

After adjusting the Makefile execute the following commands:

.. code-block:: julia
   
   make
   
in case your makefile is named ``Makefile`` or

.. code-block:: julia
   
   make -f <Makefile_name>

for a makefile with a different name.
