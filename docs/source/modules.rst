Environment Modules
===================


1. Introduction
---------------

There are several conflicting software packages installed in the Oblivion supercomputer. In order for the user to use the adequate software there is a need to set the paths for the binaries, libraries, manuals, and includes. Either the user sets these paths for each package or the system provides an easy way to set those paths. The latter is the preferable and makes use of environment modules. 


2. Working with Modules
-----------------------

The user sets the software environment by loading the modules associated to the packages he/she needs to use. This is easily done by using ``module load`` or ``module add``. Software dependences are set in the same way. OBLIVION uses a hierarchical module naming scheme (hmns) in which modules availability follows the software hierarchy Core/Compiler/MPI.

Core refers to the basic core modules that have to be loaded in order to have access to next levels of software compiled against a specific compiler and a MPI API.

After logging into the machine the user should execute the command ``module av`` (av for available) obtaining the list of core modules:

.. code-block:: console

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


The list shows toolchains gompi/2021a, foss/2021a, intel/2021a, iimpi/2021a, iompi/2021a. One of the toolchains has to be loaded using, say for foss/2021a

.. code-block:: console

  module load foss/2021a

To learn the loaded modules use

.. code-block:: console

  module list

obtaining

.. code-block:: console

  Currently Loaded Modules:
  1) GCCcore/10.3.0    8) libpciaccess/0.16  15) OpenMPI/4.1.1
  2) zlib/1.2.11       9) hwloc/2.4.1        16) OpenBLAS/0.3.15
  3) binutils/2.36.1  10) OpenSSL/1.1        17) FlexiBLAS/3.0.4
  4) GCC/10.3.0       11) libevent/2.1.12    18) FFTW/3.3.9
  5) numactl/2.0.14   12) UCX/1.10.0         19) ScaLAPACK/2.1.0-fb
  6) XZ/5.2.5         13) libfabric/1.12.1   20) foss/2021a
  7) libxml2/2.9.10   14) PMIx/3.2.3

Now, loading foss/2021a gives access to other modules that are only now available. Use module av and obtain

.. code-block:: console

  ----------- /opt/software/4.5.4/hmns/modules/all/MPI/GCC/10.3.0/OpenMPI/4.1.1 -----------
   ABINIT/9.4.2            PSolver/1.8.3               futile/1.8.3
   ASE/3.22.0              ParaView/5.9.1-mpi          h5py/3.2.1
   CGAL/4.14.3             PnetCDF/1.12.2              imkl/2021.2.0
   CP2K/8.2                PyTorch/1.10.0              libvdwxc/0.4.0
   ELPA/2021.05.001        QuantumESPRESSO/6.8         matplotlib/3.4.2
   FFTW/3.3.9       (L)    R/4.1.2                     ncview/2.1.8
   GDAL/3.3.0              SCOTCH/6.1.0                netCDF-C++4/4.3.1
   GPAW/21.6.0             ScaLAPACK/2.1.0-fb   (L)    netCDF-Fortran/4.5.3
   GROMACS/2021.3          Scalasca/2.6                netCDF/4.8.0
   GROMACS/2021.5   (D)    SciPy-bundle/2021.05        networkx/2.5.1
   HDF5/1.10.7             Score-P/7.0                 pycocotools/2.0.4
   HDF5/1.12.1      (D)    TensorFlow/2.6.0            scikit-learn/0.24.2
   ORCA/5.0.2              VTK/9.0.1                   spglib-python/1.16.1
   OpenFOAM/v2106          Valgrind/3.17.0             tensorboard/2.8.0
   PLUMED/2.7.2            Wannier90/3.1.0             torchvision/0.11.1

 --------------- /opt/software/4.5.4/hmns/modules/all/Compiler/GCC/10.3.0 ----------------
   Boost/1.76.0           GSL/2.7                         OpenMPI/4.1.1  (L)
   FlexiBLAS/3.0.4 (L)    Libint/2.6.0-lmax-6-cp2k        libxc/5.1.5
   GEOS/3.9.1             OpenBLAS/0.3.15          (L)    libxsmm/1.16.2

 ------------- /opt/software/4.5.4/hmns/modules/all/Compiler/GCCcore/10.3.0 --------------
   Autoconf/2.71                       Yasm/1.3.0
   Automake/1.16.3                     Zip/3.0
   Autotools/20210128                  binutils/2.36.1            (L,D)
   Bazel/3.7.2                         bzip2/1.0.8
   Bison/3.7.6                         cURL/7.76.0
   Brotli/1.0.9                        cairo/1.16.0
   CMake/3.20.1                        cppy/1.1.0
   CubeGUI/4.6                         double-conversion/3.1.5
   CubeLib/4.6                         expat/2.2.9
   CubeWriter/4.6                      expecttest/0.1.3
         ⋮                                      ⋮
 ----------------------- /opt/software/4.5.4/hmns/modules/all/Core -----------------------
   ANSYS_CFD/192                 M4/1.4.19       (D)    iimpi/2021a
   ANSYS_CFD/2021R1      (D)     OpenSSL/1.1     (L)    intel-compilers/2021.2.0
   Bison/3.8.2           (D)     binutils/2.36.1        intel/2021a
   GCC/10.3.0            (L)     flex/2.6.4             iompi/2021a
   GCCcore/10.3.0        (L)     foss/2021a      (L)    ncurses/6.2
   GPAW-setups/0.9.20000         gettext/0.21           pkg-config/0.29.2
   Java/11.0.2           (11)    gompi/2021a            zlib/1.2.11

 ------------------------- /usr/share/lmod/lmod/modulefiles/Core -------------------------
   lmod    settarg

  Where:
   L:        Module is loaded
   Aliases:  Aliases exist: foo/1.2.3 (1.2) means that "module load foo/1.2" will load foo/1.2.3
   D:        Default Module

Use "module spider" to find all possible modules and extensions.
Use "module keyword key1 key2 ..." to search for all possible modules matching any of the "keys".


The top row displays the modules for software compiled against OpenMPI, which in turn was compiled with GCC compiler (second row of modules). The third row displays the modules of software compiled with GCC/10.3.0. Finally, the fourth row displays the core modules already seen before.

Now the user only needs to load the modules of interest. For example, if a user wants to use ``TensorFlow/2.6.0`` he/she executes the following command:

.. code-block:: console

  module load TensorFlow/2.6.0

or if the user wants to use ``GROMACS/2021.5`` then just execute

.. code-block:: console

  module load GROMACS/2021.5

In the latter case the loaded modules, given by ``module list``, are

.. code-block:: console

  Currently Loaded Modules:
  1) GCCcore/10.3.0     12) UCX/1.10.0          23) libreadline/8.1
  2) zlib/1.2.11        13) libfabric/1.12.1    24) Tcl/8.6.11
  3) binutils/2.36.1    14) PMIx/3.2.3          25) SQLite/3.35.4
  4) GCC/10.3.0         15) OpenMPI/4.1.1       26) GMP/6.2.1
  5) numactl/2.0.14     16) OpenBLAS/0.3.15     27) libffi/3.3
  6) XZ/5.2.5           17) FlexiBLAS/3.0.4     28) Python/3.9.5
  7) libxml2/2.9.10     18) FFTW/3.3.9          29) pybind11/2.6.2
  8) libpciaccess/0.16  19) ScaLAPACK/2.1.0-fb  30) SciPy-bundle/2021.05
  9) hwloc/2.4.1        20) foss/2021a          31) networkx/2.5.1
 10) OpenSSL/1.1        21) bzip2/1.0.8         32) GROMACS/2021.5
 11) libevent/2.1.12    22) ncurses/6.2


3. Purging Modules
------------------

The user can purge the loaded modules by executing 

.. code-block:: console
  
  module purge
  
Often a user uses different environments for his/her processes. Hence, he/she needs to load and purge the loaded modules several times. An easy way to proceed is to save those module environments into a file, say <module_environment>, by using 

.. code-block:: console

  module save <module_environment>. 
  
Later, the environment can be reloaded using the command 

.. code-block:: console

  module restore <module_environment>.

 
4. List of Commonly Used commands
---------------------------------

.. list-table::

  * - **Command**	
    - **Function**
  * - module avail	
    - Displays the list of available modules in the machine
  * - module list	
    - Displays the modules that are currently loaded
  * - module add [module_name]	
    - Loads the module [module_name]
  * - module unload [module_name]	
    - Unloads the module [module_name]
  * - module purge	
    - Clears all modules in your environment
  * - module save [name_of_file]	
    - Saves a module environment in the file [name_file] for later use
  * - module restore [name_of_file]	
    - Loads a module environment saved in file [name_file]
  * - module savelist	
    - Displays the list of saved modules environment
