Software Compilation Using MPI
==============================

In order to compile a software several steps must be carried out by the user:

#. Set the work environment by loading modules
#. Determine the software paths (compiler, libraries, includes, etc)
#. Adjust the Makefile
#. Compile the software

1. Work Enviroment
------------------

To set the work environment the user must load the needed modules. To do that start by using `module av` to see the provided modules. 
In OBLIVION the modules are set in a hierarchical structure and thus the user must start by loading the core modules or toolchains.

Say that the user wants to compile his/her software with Intel MPI. First executes `module av` obtaining

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

Load Intel MPI toolchain by executing

.. code-block:: julia

  module load iimpi/2021b
  
Check the loaded modules using ``module list``

.. code-block:: julia

  Currently Loaded Modules:
     1) GCCcore/11.2.0   3) binutils/2.37              5) numactl/2.0.14   7) impi/2021.4.0
     2) zlib/1.2.11      4) intel-compilers/2021.4.0   6) UCX/1.11.2       8) iimpi/2021b


2. MPIIFORT Vs. MPIF90
----------------------

When installing Intel MPI the user gets two flavours: MPI compiled with GCC and MPI compiled with Intel Compilers. First lets get the binaries location

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
