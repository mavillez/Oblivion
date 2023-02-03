Environment Modules
===================

There are several conflicting software packages installed in the Oblivion supercomputer. In order for the user to use the adequate software there is a need to set the paths for the binaries, libraries, manuals, and includes. Either the user sets these paths for each package or the system provides an easy way to set those paths. The latter is the preferable and makes use of environment modules. 


1. The Basics - Core Modules and Toolchains
-------------------------------------------

The user sets the software environment by loading the modules associated to the packages he/she needs to use. This is easily done by using ``module load`` or ``module add``. Software dependences are set in the same way. OBLIVION uses a hierarchical module naming scheme (hmns) in which modules availability follows the software hierarchy Core/Compiler/MPI.

Core refers to the basic core modules that have to be loaded in order to have access to next levels of software compiled against a specific compiler and a MPI API.

After logging into the machine the user should execute the command ``module av`` (av for available) obtaining the list of core modules:

.. code-block:: julia

   ---------------------------- /mnt/beegfs/stack/mn02470/modules/all/Core ----------------------------
      ANSYS_CFD/2021R1              OpenSSL/1.1                iimpi/2021b
      ANSYS_CFD/2022R2      (D)     ant/1.10.11-Java-11        imkl/2021.4.0
      Bison/3.8.2                   binutils/2.34              intel-compilers/2021.4.0
      GCC/9.3.0                     binutils/2.37       (D)    intel/2021b
      GCC/11.2.0            (D)     flex/2.6.4                 ncurses/6.1
      GCCcore/9.3.0                 foss/2021b                 ncurses/6.2              (D)
      GCCcore/11.2.0        (D)     gettext/0.20.1             pkgconf/1.8.0
      GPAW-setups/0.9.20000         gettext/0.21        (D)    zlib/1.2.11
      Java/11.0.16          (11)    gompi/2020a
      M4/1.4.19                     gompi/2021b         (D)
                                               
   Where:
      Aliases:  Aliases exist: foo/1.2.3 (1.2) means that "module load foo/1.2" will load foo/1.2.3         
      D:        Default Module

   If the avail list is too long consider trying:

   "module --default avail" or "ml -d av" to just list the default modules.                                 
   "module overview" or "ml ov" to display the number of modules for each name.                             

   Use "module spider" to find all possible modules and extensions.                                         
   Use "module keyword key1 key2 ..." to search for all possible modules matching any of the "keys".        


The list shows toolchains 

- foss: 2021b;
- intel: 2021b.
 
and sub-toolchains 

- gompi: 2020a, 2021b; 
- intel-compilers: 2021.4.0;
- iimpi: 2021b.

Toolchain foss includes the following software:

- C, C++ and Fortran compilers: GCC;
- MPI implementation: OpenMPI;
- BLAS and LAPACK implementation: OpenBLAS;
- Parallel, distributed LAPACK implementation: ScaLAPACK;
- Fourier transforms: FFTW.

Toolchain intel includes the following software:

- C, C++ and Fortran compilers (icc/icpc/ifort);
- MPI implementation (Intel MPI);
- BLAS, LAPACK and fourier transforms: Intel MKL.


2. Loading Modules
------------------

2.1 GCC Based Modules
~~~~~~~~~~~~~~~~~~~~~

Let us assume that the user wants to use software compiled with GCC-9.3.0. Hence, he loads the corresponding modules

.. code-block:: julia

  module load GCC/9.3.0

To learn the loaded modules use

.. code-block:: julia

  module list

obtaining

.. code-block:: julia

   Currently Loaded Modules:
      1) GCCcore/9.3.0   2) zlib/1.2.11   3) binutils/2.34   4) GCC/9.3.0

Loading the module GCC/9.3.0 gives access to other modules that only now became available. To see those modules use "module av" obtaining

.. code-block:: julia

   ------------------------ /mnt/beegfs/stack/mn02470/modules/all/Compiler/GCC/9.3.0 -------------------
      OpenMPI/4.0.3

   ---------------------- /mnt/beegfs/stack/mn02470/modules/all/Compiler/GCCcore/9.3.0 -----------------
      Autoconf/2.69          Perl/5.30.2      (D)    hwloc/2.2.0             ncurses/6.2        (D)         
      Automake/1.16.1        UCX/1.8.0               libevent/2.1.11         numactl/2.0.13                 
      Autotools/20180311     XZ/5.2.5                libfabric/1.11.0        pkg-config/0.29.2              
      Bison/3.5.3            binutils/2.34    (L)    libpciaccess/0.16       xorg-macros/1.19.2             
      DB/18.1.32             expat/2.2.9             libreadline/8.0         zlib/1.2.11        (L,D)
      M4/1.4.18              flex/2.6.4       (D)    libtool/2.4.6                                          
      PMIx/3.1.5             groff/1.22.4            libxml2/2.9.10                                         
      Perl/5.30.2-minimal    help2man/1.47.12        makeinfo/6.7-minimal                                   

   ---------------------------- /mnt/beegfs/stack/mn02470/modules/all/Core -----------------------------
      ANSYS_CFD/2021R1              OpenSSL/1.1                iimpi/2021b
      ANSYS_CFD/2022R2      (D)     ant/1.10.11-Java-11        imkl/2021.4.0
      Bison/3.8.2                   binutils/2.34              intel-compilers/2021.4.0
      GCC/9.3.0                     binutils/2.37       (D)    intel/2021b
      GCC/11.2.0            (D)     flex/2.6.4                 ncurses/6.1
      GCCcore/9.3.0                 foss/2021b                 ncurses/6.2              (D)
      GCCcore/11.2.0        (D)     gettext/0.20.1             pkgconf/1.8.0
      GPAW-setups/0.9.20000         gettext/0.21        (D)    zlib/1.2.11
      Java/11.0.16          (11)    gompi/2020a
      M4/1.4.19                     gompi/2021b         (D)                                          

   Where:
     L:        Module is loaded
     D:        Default Module

Here one can see (from bottom to top) the core modules, general software compiled with GCC-9.3.0, and MPI API compiled with GCC-9.3.0 following the scheme core/compiler/MPI referred above.

The user can now load OpenMPI-4.0.3 using ``module load OpenMPI/4.0.3`` and check the loaded modules using ``module list`` obtaining

.. code-block:: julia

   Currently Loaded Modules:
      1) GCCcore/9.3.0   5) numactl/2.0.13      9) hwloc/2.2.0       13) PMIx/3.1.5
      2) zlib/1.2.11     6) XZ/5.2.5           10) libevent/2.1.11   14) OpenMPI/4.0.3
      3) binutils/2.34   7) libxml2/2.9.10     11) UCX/1.8.0
      4) GCC/9.3.0       8) libpciaccess/0.16  12) libfabric/1.11.0

Now, not only OpenMPI is loaded, but also UCX, PMIx, etc., are loaded. UCX stands for Unified Communication X and is "an optimized production communication framework for modern, high-bandwidth and low-latency networks" (see https://github.com/openucx/ucx) meaning for infiniband. PMIx stands for Process Management Interface - Exascale and enables the interaction of MPI applications with Resource Managers like SLURM (see https://pmix.github.io)

Let us now use an enviromment based on GCC-11.2.0. Hence, load the module GCC/11.2.0 (use ``module load GCC/11.2.0``) and immediately you see

.. code-block:: julia

   Inactive Modules:
      1) OpenMPI/4.0.3     3) UCX/1.8.0       5) libevent/2.1.11      7) numactl/2.0.13               
      2) PMIx/3.1.5        4) hwloc/2.2.0     6) libfabric/1.11.0                                     

   Due to MODULEPATH changes, the following have been reloaded:                                      
      1) XZ/5.2.5     2) libpciaccess/0.16     3) libxml2/2.9.10     4) zlib/1.2.11                   

   The following have been reloaded with a version change:                                           
      1) GCC/9.3.0 => GCC/11.2.0             3) binutils/2.34 => binutils/2.37                        
      2) GCCcore/9.3.0 => GCCcore/11.2.0

So, what happen? Basically the system is smart enough to understand that the dependences and core files in the previous environment are incompatible to GCC/11.2.0 and replaces or deactivates modules. Check the loaded modules with ``module list``

.. code-block:: julia

   Currently Loaded Modules:
      1) GCCcore/11.2.0   3) GCC/11.2.0    5) XZ/5.2.5         7) libpciaccess/0.16
      2) binutils/2.37    4) zlib/1.2.11   6) libxml2/2.9.10

   Inactive Modules:
      1) numactl/2.0.13   3) libevent/2.1.11   5) libfabric/1.11.0   7) OpenMPI/4.0.3
      2) hwloc/2.2.0      4) UCX/1.8.0         6) PMIx/3.1.5

No longer have access to OpenMPI-4.0.3 and assocated frameworks. Let's check what is available now (use ``mnodule av``)

.. code-block:: julia

   --------------------------- /mnt/beegfs/stack/mn02470/modules/all/Compiler/GCC/11.2.0 ---------------
      BLIS/0.8.1      FlexiBLAS/3.0.4 (L)    GSL/2.7          OpenBLAS/0.3.18 (L)    libxc/5.1.6                    
      Boost/1.77.0    GEOS/3.9.1             LAPACK/3.10.1    OpenMPI/4.1.1   (L)    libxsmm/1.17        

   ------------------------- /mnt/beegfs/stack/mn02470/modules/all/Compiler/GCCcore/11.2.0 -------------
      ANTLR/2.7.7-Java-11                 Pillow/8.3.2                     intltool/0.51.0
      ATK/2.36.0                          PyYAML/5.4.1                     jbigkit/2.1
      Autoconf/2.71                       Python/2.7.18-bare               kim-api/2.3.0
      Automake/1.16.4                     Python/3.9.6-bare                libGLU/9.0.2
      Autotools/20210726                  Python/3.9.6            (D)      libarchive/3.5.1
      Bazel/4.2.2                         Qhull/2020.2                     libcerf/1.17
      Bison/3.7.6                         Qt5/5.15.2                       libdap/3.20.8
      Brotli/1.0.9                        Rust/1.54.0                      libdrm/2.4.107
      CMake/3.21.1                        SQLite/3.36                      libepoxy/1.5.8
      CMake/3.22.1                 (D)    Szip/2.1.1                       libevent/2.1.12
      DB/18.1.40                          Tcl/8.6.11                       libfabric/1.13.2
      DBus/1.13.18                        Tk/8.6.11                        libffi/3.4.2
      Doxygen/1.9.1                       Tkinter/3.9.6                    libgd/2.3.3
      Eigen/3.3.9                         Togl/2.0                         libgeotiff/1.7.0
      ...

   ---------------------------- /mnt/beegfs/stack/mn02470/modules/all/Core -----------------------------
      ANSYS_CFD/2021R1              OpenSSL/1.1                iimpi/2021b
      ANSYS_CFD/2022R2      (D)     ant/1.10.11-Java-11        imkl/2021.4.0
      Bison/3.8.2                   binutils/2.34              intel-compilers/2021.4.0
      GCC/9.3.0                     binutils/2.37       (D)    intel/2021b
      GCC/11.2.0            (D)     flex/2.6.4                 ncurses/6.1
      GCCcore/9.3.0                 foss/2021b                 ncurses/6.2              (D)
      GCCcore/11.2.0        (D)     gettext/0.20.1             pkgconf/1.8.0
      GPAW-setups/0.9.20000         gettext/0.21        (D)    zlib/1.2.11
      Java/11.0.16          (11)    gompi/2020a
      M4/1.4.19                     gompi/2021b         (D)
      
   Where:
      L:        Module is loaded
      D:        Default Module

Again, besides the core modules, there is a huge list of packages compiled with GCC-11.2.0 including OpenMPI-4.1.1, OpenBLAS, LAPACK, etc.. Load OpenMPI/4.1.1 (``module load OpenMPI/4.1.1``) obtaining

.. code-block:: julia

   Activating Modules:
      1) OpenMPI/4.1.1     3) UCX/1.11.2      5) libevent/2.1.12      7) numactl/2.0.14
      2) PMIx/4.1.0        4) hwloc/2.5.0     6) libfabric/1.13.2

list the load modules (``module list``)

.. code-block:: julia

   Currently Loaded Modules:
      1) GCCcore/11.2.0   5) XZ/5.2.5            9) hwloc/2.5.0      13) libfabric/1.13.2
      2) binutils/2.37    6) libxml2/2.9.10     10) OpenSSL/1.1      14) PMIx/4.1.0
      3) GCC/11.2.0       7) libpciaccess/0.16  11) libevent/2.1.12  15) OpenMPI/4.1.1
      4) zlib/1.2.11      8) numactl/2.0.14     12) UCX/1.11.2

and see what is available (``module av``)

.. code-block:: julia

   ----------------------- /mnt/beegfs/stack/mn02470/modules/all/MPI/GCC/11.2.0/OpenMPI/4.1.1 -----------------------
      ABINIT/9.6.2                       MDAnalysis/2.0.0                      Theano/1.1.2-PyMC                    
      ASE/3.22.1                         MDTraj/1.9.7                          VTK/9.1.0                            
      AmberTools/22.3                    MUMPS/5.4.1-metis                     Valgrind/3.18.1                      
      ArviZ/0.11.4                       ORCA/5.0.3                            Wannier90/3.1.0                      
      Bambi/0.7.1                        OSU-Micro-Benchmarks/5.7.1            XCrySDen/1.6.2                       
      Biopython/1.79                     OpenCV/4.5.5-contrib                  arpack-ng/3.8.0                      
      CGAL/4.14.3                        OpenFOAM/v2112                        h5py/3.6.0                           
      Dalton/2020.0                      PLUMED/2.8.0                          libGridXC/0.9.6                      
      ELPA/2021.05.001                   ParMETIS/4.0.3                        libvdwxc/0.4.0                       
      ESMF/8.2.0                         ParaView/5.9.1-mpi                    matplotlib/3.4.3                     
      FFTW/3.3.10                 (L)    PnetCDF/1.12.3                        ncview/2.1.8                         
      FMS/2022.02                        PyMC3/3.11.1                          netCDF-C++4/4.3.1                    
      GDAL/3.3.2                         QuantumESPRESSO/7.0                   netCDF-Fortran/4.5.3                 
      GPAW/22.8.0                        SCOTCH/6.1.2                          netCDF/4.8.1                         
      GROMACS/2021.5-PLUMED-2.8.0        ScaFaCoS/1.0.1                        netcdf4-python/1.5.7                 
      GROMACS/2021.5              (D)    ScaLAPACK/2.1.0-fb             (L)    networkx/2.6.3                       
      HDF/4.2.15                  (D)    SciPy-bundle/2021.10                  numba/0.54.1                         
      HDF5/1.12.1                        Siesta/4.1.5                          scikit-bio/0.5.7                     
      HPL/2.3                            SuiteSparse/5.10.1-METIS-5.1.0        scikit-learn/1.0.2                   
      IMB/2021.3                         SuperLU/5.3.0                         spglib-python/1.16.3                 
      LAMMPS/23Jun2022-kokkos            TELEMAC-MASCARET/8p3r1                statsmodels/0.13.1                   
      Libint/2.6.0-lmax-6-cp2k           TensorFlow/2.8.4                      xarray/0.20.1                        
                                                                                                                 
  --------------------------- /mnt/beegfs/stack/mn02470/modules/all/Compiler/GCC/11.2.0 ----------------------------
      BLIS/0.8.1      FlexiBLAS/3.0.4 (L)    GSL/2.7          OpenBLAS/0.3.18 (L)    libxc/5.1.6                    
      Boost/1.77.0    GEOS/3.9.1             LAPACK/3.10.1    OpenMPI/4.1.1   (L)    libxsmm/1.17                   
                                                                                                                 
  ------------------------- /mnt/beegfs/stack/mn02470/modules/all/Compiler/GCCcore/11.2.0 --------------------------
      ANTLR/2.7.7-Java-11                 Pillow/8.3.2                     intltool/0.51.0                          
      ATK/2.36.0                          PyYAML/5.4.1                     jbigkit/2.1                              
      Autoconf/2.71                       Python/2.7.18-bare               kim-api/2.3.0                            
      Automake/1.16.4                     Python/3.9.6-bare                libGLU/9.0.2                             
      Autotools/20210726                  Python/3.9.6            (D)      libarchive/3.5.1                         
      Bazel/4.2.2                         Qhull/2020.2                     libcerf/1.17                             
      Bison/3.7.6                         Qt5/5.15.2                       libdap/3.20.8                            
      Brotli/1.0.9                        Rust/1.54.0                      libdrm/2.4.107                
      ...

Now the user got access to all the software that was compiled against OpenMPI-4.1.1. The top row displays the modules for software compiled against OpenMPI, which in turn was compiled with GCC compiler (second row of modules). The third row displays the core modules associated to GCC/11.2.0.

All this work can be executed with just a single command by loading foss/2021b. So, let's check it. Start with a ``module purge`` followed with ``module av`` getting

.. code-block:: julia

   ---------------------------- /mnt/beegfs/stack/mn02470/modules/all/Core -----------------------------
      ANSYS_CFD/2021R1              OpenSSL/1.1                iimpi/2021b
      ANSYS_CFD/2022R2      (D)     ant/1.10.11-Java-11        imkl/2021.4.0
      Bison/3.8.2                   binutils/2.34              intel-compilers/2021.4.0
      GCC/9.3.0                     binutils/2.37       (D)    intel/2021b
      GCC/11.2.0            (D)     flex/2.6.4                 ncurses/6.1
      GCCcore/9.3.0                 foss/2021b                 ncurses/6.2              (D)
      GCCcore/11.2.0        (D)     gettext/0.20.1             pkgconf/1.8.0
      GPAW-setups/0.9.20000         gettext/0.21        (D)    zlib/1.2.11
      Java/11.0.16          (11)    gompi/2020a
      M4/1.4.19                     gompi/2021b         (D)                    

Load foss/2021b (``module load foss/2021b``) and check what is available (``module av``) getting

.. code-block:: julia

   ----------------------- /mnt/beegfs/stack/mn02470/modules/all/MPI/GCC/11.2.0/OpenMPI/4.1.1 ----------
      ABINIT/9.6.2                       MDAnalysis/2.0.0                      Theano/1.1.2-PyMC                    
      ASE/3.22.1                         MDTraj/1.9.7                          VTK/9.1.0                            
      AmberTools/22.3                    MUMPS/5.4.1-metis                     Valgrind/3.18.1                      
      ArviZ/0.11.4                       ORCA/5.0.3                            Wannier90/3.1.0                      
      Bambi/0.7.1                        OSU-Micro-Benchmarks/5.7.1            XCrySDen/1.6.2                       
      Biopython/1.79                     OpenCV/4.5.5-contrib                  arpack-ng/3.8.0                      
      CGAL/4.14.3                        OpenFOAM/v2112                        h5py/3.6.0
      Dalton/2020.0                      PLUMED/2.8.0                          libGridXC/0.9.6                      
      ELPA/2021.05.001                   ParMETIS/4.0.3                        libvdwxc/0.4.0
      ...
      
It is the same obtained previously by loading GCC/11.2.0 and OpenMPI/4.1.1.


2.2 Intel-Compilers Based Modules
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Similar procedure to what has been outlined above applies for modules using the Intel compilers, MKL, and MPI. At the entering level if the user executes ``module av`` obtains 

.. code-block:: julia

    ---------------------------- /mnt/beegfs/stack/mn02470/modules/all/Core -----------------------------
      ANSYS_CFD/2021R1              OpenSSL/1.1                iimpi/2021b
      ANSYS_CFD/2022R2      (D)     ant/1.10.11-Java-11        imkl/2021.4.0
      Bison/3.8.2                   binutils/2.34              intel-compilers/2021.4.0
      GCC/9.3.0                     binutils/2.37       (D)    intel/2021b
      GCC/11.2.0            (D)     flex/2.6.4                 ncurses/6.1
      GCCcore/9.3.0                 foss/2021b                 ncurses/6.2              (D)
      GCCcore/11.2.0        (D)     gettext/0.20.1             pkgconf/1.8.0
      GPAW-setups/0.9.20000         gettext/0.21        (D)    zlib/1.2.11
      Java/11.0.16          (11)    gompi/2020a
      M4/1.4.19                     gompi/2021b         (D)
      
After loading intel/2021b or iimpi/2021b (``module load intel/2021b`` or ``module load iimpi/2021b``) ``module list`` shows

.. code-block:: julia

   Currently Loaded Modules:
      1) GCCcore/11.2.0   3) binutils/2.37              5) numactl/2.0.14   7) impi/2021.4.0   9) imkl-FFTW/2021.4.0
      2) zlib/1.2.11      4) intel-compilers/2021.4.0   6) UCX/1.11.2       8) imkl/2021.4.0  10) intel/2021b

and ``module av`` displays

.. code-block:: julia

   --------------------- /mnt/beegfs/stack/mn02470/modules/all/MPI/intel/2021.4.0/impi/2021.4.0 ----------------------
      ABINIT/9.6.2          HDF5/1.12.1                 SCOTCH/6.1.2                mkl-service/2.3.0
      ASE/3.22.1            HPL/2.3                     SPOTPY/1.5.14               ncview/2.1.8
      AmberTools/21         Hypre/2.24.0                SciPy-bundle/2021.10        netCDF-C++4/4.3.1
      ArviZ/0.11.4          IMB/2021.3                  Siesta/4.1.5                netCDF-Fortran/4.5.3
      Bambi/0.7.1           Libint/2.6.0-lmax-6-cp2k    SimPEG/0.18.1               netCDF/4.8.1
      Biopython/1.79        MDAnalysis/2.0.0            SuperLU/5.3.0               netcdf4-python/1.5.7
      CGAL/4.14.3           MDTraj/1.9.7                Theano/1.1.2-PyMC           networkx/2.6.3
      CP2K/8.2              MUMPS/5.4.1-metis           Valgrind/3.18.1             numba/0.54.1
      ELPA/2021.05.001      NCO/5.0.3                   Wannier90/3.1.0             scikit-learn/1.0.1
      ESMF/8.2.0            NWChem/7.0.2                futile/1.8.3                spglib-python/1.16.3
      FDS/6.7.7             OSU-Micro-Benchmarks/5.8    h5py/3.6.0                  statsmodels/0.13.1
      FFTW/3.3.10           PLUMED/2.8.0                imkl-FFTW/2021.4.0   (L)    worker/1.6.13
      GDAL/3.3.2            PSolver/1.8.3               libGridXC/0.9.6             xarray/0.20.1
      GEOS/3.9.1            ParMETIS/4.0.3              libvdwxc/0.4.0
      GPAW/22.8.0           PyMC3/3.11.1                libxsmm/1.17
      GlobalArrays/5.8.1    QuantumESPRESSO/7.0         matplotlib/3.4.3

   -------------------------- /mnt/beegfs/stack/mn02470/modules/all/Compiler/intel/2021.4.0 --------------------------
      Boost/1.77.0    Flye/2.9    GSL/2.7    impi/2021.4.0 (L)    libxc/5.1.6    xmlf90/1.5.4

   -------------------------- /mnt/beegfs/stack/mn02470/modules/all/Compiler/GCCcore/11.2.0 --------------------------
      ANTLR/2.7.7-Java-11                 Pillow/8.3.2                     intltool/0.51.0
      ATK/2.36.0                          PyYAML/5.4.1                     jbigkit/2.1
      Autoconf/2.71                       Python/2.7.18-bare               kim-api/2.3.0
      Automake/1.16.4                     Python/3.9.6-bare                libGLU/9.0.2
      Autotools/20210726                  Python/3.9.6            (D)      libarchive/3.5.1
      Bazel/4.2.2                         Qhull/2020.2                     libcerf/1.17
      Bison/3.7.6                         Qt5/5.15.2                       libdap/3.20.8
      Brotli/1.0.9                        Rust/1.54.0                      libdrm/2.4.107
      ...

On the top section the software compiled against Intel MPI (which is composed by MPICH compiled against the Intel compilers)
is displayed followed by the software compiled with Intel C, C++ and Fortran compilers.

The user can change to GCC based modules, e.g., to the foss/2021b toochain, by issuing ``module load foss/2021b`` obtaining

.. code-block:: julia

   Lmod is automatically replacing "intel-compilers/2021.4.0" with "GCC/11.2.0".
   
   Inactive Modules:
      1) imkl-FFTW/2021.4.0     2) impi/2021.4.0

and ``module list`` gives

.. code-block:: julia

   Currently Loaded Modules:
      1) GCCcore/11.2.0   6) imkl/2021.4.0   11) libpciaccess/0.16  16) PMIx/4.1.0       21) ScaLAPACK/2.1.0-fb
      2) zlib/1.2.11      7) intel/2021b     12) hwloc/2.5.0        17) OpenMPI/4.1.1    22) foss/2021b
      3) binutils/2.37    8) GCC/11.2.0      13) OpenSSL/1.1        18) OpenBLAS/0.3.18
      4) numactl/2.0.14   9) XZ/5.2.5        14) libevent/2.1.12    19) FlexiBLAS/3.0.4
      5) UCX/1.11.2      10) libxml2/2.9.10  15) libfabric/1.13.2   20) FFTW/3.3.10

   Inactive Modules:
      1) impi/2021.4.0   2) imkl-FFTW/2021.4.0


3. Loading a Particular Software
--------------------------------

The user only needs to load the modules of interest. For example, if a user wants to use ``TensorFlow/2.8.4`` after loading foss/2021b he/she executes the command

.. code-block:: julia

  module load TensorFlow/2.8.4

or if the user wants to use ``GROMACS/2021.5`` then just execute

.. code-block:: julia

  module load GROMACS/2021.5

In the latter case the loaded modules, given by ``module list``, are

.. code-block:: julia

   Currently Loaded Modules:
      1) GCCcore/11.2.0      9) hwloc/2.5.0       17) FlexiBLAS/3.0.4     25) SQLite/3.36
      2) zlib/1.2.11        10) OpenSSL/1.1       18) FFTW/3.3.10         26) GMP/6.2.1
      3) binutils/2.37      11) libevent/2.1.12   19) ScaLAPACK/2.1.0-fb  27) libffi/3.4.2
      4) GCC/11.2.0         12) UCX/1.11.2        20) foss/2021b          28) Python/3.9.6
      5) numactl/2.0.14     13) libfabric/1.13.2  21) bzip2/1.0.8         29) pybind11/2.7.1
      6) XZ/5.2.5           14) PMIx/4.1.0        22) ncurses/6.2         30) SciPy-bundle/2021.10
      7) libxml2/2.9.10     15) OpenMPI/4.1.1     23) libreadline/8.1     31) networkx/2.6.3
      8) libpciaccess/0.16  16) OpenBLAS/0.3.18   24) Tcl/8.6.11          32) GROMACS/2021.5


4. Operations With Modules
--------------------------

4.1 Purging Modules
~~~~~~~~~~~~~~~~~~~

The user can purge the loaded modules by executing 

.. code-block:: julia
  
  module purge
  
  
4.2 Save and Restore Modules
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Often a user uses different environments for his/her processes. Hence, he/she needs to load and purge the loaded modules several times. An easy way to proceed is to save those module environments into a file, say <module_environment>, by using 

.. code-block:: julia

  module save <module_environment>. 
  
Later, the environment can be reloaded using the command 

.. code-block:: julia

  module restore <module_environment>


4.3 Module Details
~~~~~~~~~~~~~~~~~~

To learn further details of a module, how to load it, and dependencies use 

.. code-block:: julia

  module spider <module_name>  
  
and to find detailed information of a module use

.. code-block:: julia

  module spider <module_name/version>

Let's check the information on GROMACS by using ``module spider GROMACS`` obtaining

.. code-block:: julia

   ------------------------------------------------------------------------------------------------------
      GROMACS:
   ------------------------------------------------------------------------------------------------------
      Description:
         GROMACS is a versatile package to perform molecular dynamics, i.e. simulate the Newtonian
         equations of motion for systems with hundreds to millions of particles. This is a CPU only
         build, containing both MPI and threadMPI builds for both single and double precision. It also
         contains the gmxapi extension for the single precision MPI build next to PLUMED.

      Versions:
         GROMACS/2021.5-PLUMED-2.8.0
         GROMACS/2021.5

   ------------------------------------------------------------------------------------------------------
      For detailed information about a specific "GROMACS" package (including how to load the modules) use the 
      module's full name.
      Note that names that have a trailing (E) are extensions provided by other modules.
      For example:

         $ module spider GROMACS/2021.5
------------------------------------------------------------------------------------------------------

and obtain details on the module by using ``module spider GROMACS/2021.5``

.. code-block:: julia

   ------------------------------------------------------------------------------------------------------
      GROMACS: GROMACS/2021.5
   ------------------------------------------------------------------------------------------------------
      Description:
         GROMACS is a versatile package to perform molecular dynamics, i.e. simulate the Newtonian
         equations of motion for systems with hundreds to millions of particles. This is a CPU only
         build, containing both MPI and threadMPI builds for both single and double precision. It also
         contains the gmxapi extension for the single precision MPI build. 

      You will need to load all module(s) on any one of the lines below before the "GROMACS/2021.5" module is available to load.

         GCC/11.2.0  OpenMPI/4.1.1
         GCC/11.3.0  OpenMPI/4.1.4
 
      ...
      
      More information
      ================
       - Homepage: https://www.gromacs.org
      
      
      Included extensions
      ===================
      gmxapi-0.2.2.1

 
5. List of Commonly Used commands
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


6. Available Modules
--------------------

To list all the available modules the user can use the command ``module spider`` obtaining

.. code-block:: julia

  ---------------------------------------------------------------------------------------------------
   The following is a list of the modules and extensions currently available:
  ---------------------------------------------------------------------------------------------------
  ABINIT: ABINIT/9.6.2
    ABINIT is a package whose main program allows one to find the total energy, charge density and
    electronic structure of systems made of electrons and nuclei (molecules and periodic solids)
    within Density Functional Theory (DFT), using pseudopotentials and a planewave or wavelet
    basis. 

  ANSYS_CFD: ANSYS_CFD/2021R1, ANSYS_CFD/2022R2
    ANSYS computational fluid dynamics (CFD) simulation software allows you to predict, with
    confidence, the impact of fluid flows on your product throughout design and manufacturing as
    well as during end use. ANSYS renowned CFD analysis tools include the widely used and
    well-validated ANSYS Fluent and ANSYS CFX.
  ...

For the full list of installed modules see the :ref:`installed software section <Installed Software>`.
