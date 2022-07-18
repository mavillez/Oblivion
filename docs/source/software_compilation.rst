Software Compilation
====================

Before compiling a software several things must be carried out by the user:

#. Set the work environment by loading modules
#. Identify the software paths (compiler, libraries, includes, etc)
#. Adjust the Makefile
#. Compile the software

1. Work Enviroment
------------------

To set the work environment the user must load the needed modules. To do that start by using `module av` to see the provided modules. 
In OBLIVION the modules are set in a hierarchical structure and thus the user must start by loading the core modules or toolchains.

Say that the user wants to compile his/her software with Intel MPI. First executes `module av` obtaining

.. code-block:: julia

   ----------------------- /opt/software/4.5.4/hmns/modules/all/Core -----------------------
     ANSYS_CFD/192                 M4/1.4.19          iimpi/2021a
     ANSYS_CFD/2021R1      (D)     OpenSSL/1.1        intel-compilers/2021.2.0
     Bison/3.8.2                   binutils/2.36.1    intel/2021a
     GCC/10.3.0                    flex/2.6.4         iompi/2021a
     GCCcore/10.3.0                foss/2021a         ncurses/6.2
     GPAW-setups/0.9.20000         gettext/0.21       pkg-config/0.29.2
     Java/11.0.2           (11)    gompi/2021a        zlib/1.2.11
    ------------------------- /usr/share/lmod/lmod/modulefiles/Core -------------------------
    lmod    settarg

    Where:
     Aliases:  Aliases exist: foo/1.2.3 (1.2) means that "module load foo/1.2" will load foo/1.2.3
     D:        Default Module

  Use "module spider" to find all possible modules and extensions.
  Use "module keyword key1 key2 ..." to search for all possible modules matching any of the "keys".

Load Intel MPI toolchain by executing

.. code-block:: julia

  module load iimpi/2021a
  
Check the loaded modules using `module list` and obtaining

.. code-block:: julia

  Currently Loaded Modules:
  1) GCCcore/10.3.0   3) binutils/2.36.1            5) numactl/2.0.14   7) impi/2021.2.0
  2) zlib/1.2.11      4) intel-compilers/2021.2.0   6) UCX/1.10.0       8) iimpi/2021a

2. MPIIFORT Vs. MPIF90
----------------------

When installing Intel MPI the user gets two flavours: MPI compiled with GCC and MPI compiled with Intel Compilers. First lets get the binaries location

.. code-block:: julia

  $ which mpif90

  /opt/software/4.5.4/software/impi/2021.2.0-intel-compilers-2021.2.0/mpi/2021.2.0/bin/mpiifort
  
Now lets see the content of the bin folder:

.. code-block:: julia

  $ ls -1 /opt/software/4.5.4/software/impi/2021.2.0-intel-compilers-2021.2.0/mpi/2021.2.0/bin
  
  ⋮  
  mpif77
  mpif90
  mpifc
  mpigcc
  mpigxx
  mpiicc
  mpiicpc
  mpiifort
  ⋮ 

`mpif77`, `mpif90`, `mpigcc`, and `mpigxx` are the executables for MPI compiled against GCC.

`mpiicc`, mpiicpc`, and `mpiifort` are the executables for MPI compiled against Intel Compilers.

To check this just type

.. code-block:: julia

  $ less /opt/software/4.5.4/software/impi/2021.2.0-intel-compilers-2021.2.0/mpi/2021.2.0/bin/mpif90

obtaining

.. code-block:: julia

  # Default settings for compiler, flags, and libraries
  # Determined by a combination of environment variables and tests within
  # configure (e.g., determining whehter -lsocket is needed)
  FC="gfortran"

and

.. code-block:: julia

  $ less /opt/software/4.5.4/software/impi/2021.2.0-intel-compilers-2021.2.0/mpi/2021.2.0/bin/mpiifort

obtaining

.. code-block:: julia

  # Default settings for compiler, flags, and libraries
  # Determined by a combination of environment variables and tests within
  # configure (e.g., determining whehter -lsocket is needed)
  FC="ifort"

You can also run

.. code-block:: julia

  $ mpif90 --version
  GNU Fortran (GCC) 10.3.0
  Copyright (C) 2020 Free Software Foundation, Inc.
  This is free software; see the source for copying conditions.  There is NO
  warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

or

.. code-block:: julia

  $ mpiifort --version
  ifort (IFORT) 2021.2.0 20210228
  Copyright (C) 1985-2021 Intel Corporation.  All rights reserved.


Lets find the PATHs for binary, libraries and include. So, first lets determine the path of the binaries, say, mpif90 (mpiifort):

.. code-block:: julia

  $ which mpiifort
  
  /opt/software/4.5.4/software/impi/2021.2.0-intel-compilers-2021.2.0/mpi/2021.2.0/bin/mpiifort
  
Now look for the paths:

.. code-block:: julia

  $ ls /opt/software/4.5.4/software/impi/2021.2.0-intel-compilers-2021.2.0/mpi/2021.2.0/
  
  bin  etc  include  lib  libfabric  man

So, here we show a few of the folders and lets set the paths to be used in the Makefile

.. code-block:: julia

  MPI_BIN=/opt/software/4.5.4/software/impi/2021.2.0-intel-compilers-2021.2.0/mpi/2021.2.0/bin
  MPI_LIB=/opt/software/4.5.4/software/impi/2021.2.0-intel-compilers-2021.2.0/mpi/2021.2.0/lib
  MPI_INC=/opt/software/4.5.4/software/impi/2021.2.0-intel-compilers-2021.2.0/mpi/2021.2.0/bin
  
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
  SOFTWARE=/opt/software/4.5.4/software
  MPI_VER=impi/2021.2.0-intel-compilers-2021.2.0/mpi/2021.2.0
  MPI_BIN=${SOFTWARE}/${MPI_VER}/bin
  MPI_LIB=${SOFTWARE}/${MPI_VER}/lib
  MPI_INC=${SOFTWARE}/${MPI_VER}/include
  F90 = mpiifort
  CC  = mpiicc
  endif
  
  ifeq ($(SYSTYPE),"oblivion_impi_gcc")
  SOFTWARE=/opt/software/4.5.4/software
  MPI_VER=impi/2021.2.0-intel-compilers-2021.2.0/mpi/2021.2.0
  MPI_BIN=${SOFTWARE}/${MPI_VER}/bin
  MPI_LIB=${SOFTWARE}/${MPI_VER}/lib
  MPI_INC=${SOFTWARE}/${MPI_VER}/include
  F90 = mpif90
  CC  = mpicc
  endif
    
Note that three setups are referred in SYSTYPE and oblivion_impi_intel was the chosen one. Now, in the Makefile there are also the OPTS, OBJS, etc....






