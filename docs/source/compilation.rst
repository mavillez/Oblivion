Software Compilation Using MPI
==============================

1. Work Enviroment
------------------

To set the work environment the user must load the needed modules. To do that start by using ``module --nx av`` to see the available modules:

.. code-block:: julia

 --------------------------------- /mnt/beegfs/apps/modules/all/Core ----------------------------------
   Anaconda3/2025.06-1                  Miniforge3/25.3.0-1 (D)    gompi/2024a
   Autoconf/2.71                        OSPRay/2.12.0              gompi/2025a
   Bison/3.8.2                          OpenSSL/1.1                gompi/2025b              (D)
   FastQC/0.11.9-Java-11                OpenSSL/3           (D)    iimkl/2023a
   GCC/12.3.0                           Pandoc/3.6.2               iimkl/2024a
   GCC/13.3.0                           Perl/5.38.0                iimkl/2025a              (D)
   GCC/14.2.0                           ant/1.10.12-Java-17        iimpi/2023a
   GCC/14.3.0                 (D)       ant/1.10.14-Java-11 (D)    iimpi/2024a
   GCCcore/12.3.0                       binutils/2.40              iimpi/2025a              (D)
   GCCcore/13.3.0                       binutils/2.42              imkl/2023.1.0
   GCCcore/14.2.0                       binutils/2.44       (D)    imkl/2023.2.0
   GCCcore/14.3.0             (D)       ecBuild/3.8.0              imkl/2024.2.0
   GPAW-setups/24.1.0                   ffnvcodec/12.0.16.0        imkl/2025.1.0            (D)
   GPAW-setups/24.11.0        (D)       ffnvcodec/12.1.14.0        intel-compilers/2023.1.0
   IJulia/1.29.0-Julia-1.11.6           ffnvcodec/12.2.72.0        intel-compilers/2024.2.0
   Java/11.0.27               (11)      ffnvcodec/13.0.19.0 (D)    intel-compilers/2025.1.1 (D)
   Java/17.0.15               (17)      flex/2.6.4                 intel/2023a
   Java/21.0.8                (D:21)    foss/2023a                 intel/2024a
   Julia/1.11.3-linux-x86_64            foss/2024a                 intel/2025a              (D)
   Julia/1.11.6-linux-x86_64            foss/2025a                 iompi/2023a
   Julia/1.11.7                         foss/2025b          (D)    iompi/2024a
   Julia/1.12.2                         gettext/0.21.1             iompi/2025a              (D)
   Julia/1.12.6               (D)       gettext/0.22               ncurses/6.3
   M4/1.4.18                            gettext/0.22.5             ncurses/6.4
   M4/1.4.19                            gettext/0.25        (D)    ncurses/6.5              (D)
   M4/1.4.20                  (D)       gfbf/2023a                 pkgconf/1.8.0
   Mamba/23.11.0-0                      gfbf/2024a                 zlib/1.2.13
   Miniconda3/24.7.1-0                  gfbf/2025a                 zlib/1.3.1               (D)
   Miniconda3/25.5.1-1        (D)       gfbf/2025b          (D)
   Miniforge3/24.11.3-0                 gompi/2023a

   Where:
      Aliases:  Aliases exist: foo/1.2.3 (1.2) means that "module load foo/1.2" will load foo/1.2.3         
      D:        Default Module


In OBLIVION the modules are set in a hierarchical naming structure (Core/Compiler/MPI) and thus the user must start by loading the core modules or toolchains, e.g., load OpenMPI compiled with GCC (``module load GCC/11.2.0`` + ``module load OpenMPI/4.1.1``), load OpenMPI compiled with Intel compilers or load Intel MPI (``module load iimpi/2021b`` or ``module load intel/2021b``).


2. Intel MPI
------------

2.1 Loading intel and iimpi toolchains
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Load Intel MPI sub-toolchain by executing ``module load iimpi/2025a`` and check the loaded modules using ``module list``

.. code-block:: julia

  Currently Loaded Modules:
    1) GCCcore/14.2.0   3) binutils/2.42              5) numactl/2.0.19   7) impi/2021.15.0
    2) zlib/1.3.1       4) intel-compilers/2025.1.1   6) UCX/1.18.0       8) iimpi/2025a


The intel/2025a toolchain includes, among others, the Intel MPI and MKL distributions. Using ``module load intel/2025a`` followed with ``module list`` one obtains

.. code-block:: julia
  
  Currently Loaded Modules:
    1) GCCcore/14.2.0   4) intel-compilers/2025.1.1   7) impi/2021.15.0      10) intel/2025a
    2) zlib/1.3.1       5) numactl/2.0.19             8) imkl/2025.1.0
    3) binutils/2.42    6) UCX/1.18.0                 9) imkl-FFTW/2025.1.0


In both cases the available modules are the same (check with ``module --nx av``):

.. code-block:: julia

------------------- /mnt/beegfs/apps/modules/all/MPI/intel/2025.1.1/impi/2021.15.0 -------------------
   HDF5/1.14.6    OSU-Micro-Benchmarks/7.5    Wannier90/3.1.0     imkl-FFTW/2025.1.0
   HPL/2.3        Score-P/9.2                 buildenv/default

------------------------ /mnt/beegfs/apps/modules/all/Compiler/intel/2025.1.1 ------------------------
   OpenMPI/5.0.7    impi/2021.15.0 (L)


2.2 MPIIFX Vs. MPIF90
~~~~~~~~~~~~~~~~~~~~~

With Intel MPI the user gets two flavours: MPI compiled with GCC and MPI compiled with Intel compilers. First lets get the binaries location

.. code-block:: julia

  $ which mpif90

  /mnt/beegfs/apps/software/impi/2021.15.0-intel-compilers-2025.1.1/mpi/2021.15/bin/mpif90
  
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
   mpiifx
   ⋮ 

``mpif77``, ``mpif90``, ``mpigcc``, and ``mpigxx`` are the executables for MPI compiled against GCC.

``mpiicc``, ``mpiicpc``, and ``mpiifort`` are the executables for MPI compiled against Intel Compilers.

To check this just type

.. code-block:: julia

  $ less /mnt/beegfs/apps/software/impi/2021.15.0-intel-compilers-2025.1.1/mpi/2021.15/bin/mpif90

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

  $ less /mnt/beegfs/apps/software/impi/2021.15.0-intel-compilers-2025.1.1/mpi/2021.15/bin/mpiifx

obtaining

.. code-block:: julia

  ⋮ 
  # Default settings for compiler, flags, and libraries
  # Determined by a combination of environment variables and tests within
  # configure (e.g., determining whehter -lsocket is needed)
  COMPILER="ifort"
  ⋮ 

You can also run

.. code-block:: julia

  $ mpif90 --version
  GNU Fortran (GCC) 14.2.0
  Copyright (C) 2024 Free Software Foundation, Inc.
  This is free software; see the source for copying conditions.  There is NO
  warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

or

.. code-block:: julia

  $ mpiifx --version
  ifx (IFX) 2025.1.1 20250418
  Copyright (C) 1985-2025 Intel Corporation. All rights reserved.


Lets find the PATHs for binary, libraries and include. So, first lets determine the path of the binaries, say, mpif90 (mpiifort):

.. code-block:: julia

  $ which mpiifx
  
  /mnt/beegfs/apps/software/impi/2021.15.0-intel-compilers-2025.1.1/mpi/2021.15/bin/mpiifx
  
Now look for the paths:

.. code-block:: julia

  $ ls /mnt/beegfs/apps/software/impi/2021.15.0-intel-compilers-2025.1.1/mpi/2021.15/
  
   bin  env  etc  include  lib  libfabric  man  opt  share

So, here we show some of the listed folders. Lets set the paths to be used in the Makefile

.. code-block:: julia

  MPI_BIN=/mnt/beegfs/apps/software/impi/2021.15.0-intel-compilers-2025.1.1/mpi/2021.15/bin
  MPI_LIB=/mnt/beegfs/apps/software/impi/2021.15.0-intel-compilers-2025.1.1/mpi/2021.15/lib
  MPI_INC=/mnt/beegfs/apps/software/impi/2021.15.0-intel-compilers-2025.1.1/mpi/2021.15/include
  
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
  MPI_VER=/mnt/beegfs/apps/software/impi/2021.15.0-intel-compilers-2025.1.1/mpi/2021.15/
  MPI_BIN=${SOFTWARE}/${MPI_VER}/bin
  MPI_LIB=${SOFTWARE}/${MPI_VER}/lib
  MPI_INC=${SOFTWARE}/${MPI_VER}/include
  F90 = mpiifort
  CC  = mpiicc
  endif
  
  ifeq ($(SYSTYPE),"oblivion_impi_gcc")
  SOFTWARE=/mnt/beegfs/stack/cn01470/software
  MPI_VER=/mnt/beegfs/apps/software/impi/2021.15.0-intel-compilers-2025.1.1/mpi/2021.15/
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
