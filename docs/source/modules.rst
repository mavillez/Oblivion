Environment Modules
===================

There are several conflicting software packages installed in the Oblivion supercomputer. In order for the user to use the adequate software there is a need to set the paths for the binaries, libraries, manuals, and includes. Either the user sets these paths for each package or the system provides an easy way to set those paths. The latter is the preferable and makes use of environment modules. 

1. Toolchains
-------------

Toolchain is a pack of compiler(s)and libraries bundled together to provide a specific functionality, say, running applications using a MPI distribution compiled against GCC (toolchain gompi) or Intel compilers (toolchain iimpi) or use linear algebra libraries with a MPI API compiled with GCC (toolchain foss).

**Toolchain foss includes the following software:**

- C, C++ and Fortran compilers: GCC
- MPI implementation: OpenMPI
- OpenBLAS and LAPACK implementation: FlexiBLAS
- Parallel, distributed LAPACK implementation: ScaLAPACK
- Fourier transforms: FFTW

**Toolchain intel includes the following software:**

- C, C++ and Fortran compilers (icc/icpc/ifort)
- GCC as a base for the Intel compilers
- MPI implementation (Intel MPI)
- BLAS, LAPACK and FFTW: Intel MKL

**Sub-toolchains:** 

- gompi (GCC + OpenMPI)
- iompi (Intel compilers + OpenMPI)
- iimpi (Intel compilers + Intel MPI (MPICH))
- imkl (Intel Math Kernel Library) 


**In OBLIVION are installed the following toolchains and sub-toolchains:** 

Toolchains:

- foss: 2020a, 2021b, 2022a;
- intel: 2021b, 2022a.
 
Sub-toolchains:

- gompi: 2020a, 2021b, 2022a;
- iimpi: 2021b; 2022a;
- iompi: 2021b;
- intel-compilers: 2021.4.0, 2021.6.0;
- imkl: 2021.4.0, 2021.6.0.


2. Core Modules
---------------

The user sets the software environment by loading the modules associated to the needed packages. This is easily done by using ``module load`` or ``module add``. Software dependences are set in the same way. OBLIVION uses a hierarchical module naming scheme (hmns) in which modules availability follows the software hierarchy Core/Compiler/MPI.

Core refers to the basic core modules that have to be loaded in order to have access to next levels of software compiled against a specific compiler (e.g., ``GCC/9.3.0, GCC/11.2.0, GCC/11.3.0, intel-compilers/2021.4.0, intel-compilers/2022.1.0``) and a MPI API (OpenMPI, Intel MPI). It also includes modules of software that i) are initially compiled with the system/machine compiler (e.g., binutils, gettext, M4, ncurses, pkgconf, zlib) or ii) are not being built but instead are directly installed into the stack (e.g., Anaconda, ANSYS_CFD).

After logging into the machine the user should execute the command ``module av`` (av for available) obtaining the list of core modules (including the toolchains and sub-toolchains):

.. code-block:: julia

  ----------------------------------- /mnt/beegfs/apps/cn01470x/modules/all/Core ------------------------------------
   ANSYS_CFD/2021R1                 OpenSSL/1.1                iimpi/2021b
   ANSYS_CFD/2022R2         (D)     ant/1.10.11-Java-11        iimpi/2022a              (D)
   Anaconda3/2022.05                ant/1.10.12-Java-11 (D)    imkl/2021.4.0
   Bison/3.8.2                      binutils/2.34              imkl/2022.1.0            (D)
   FastQC/0.11.9-Java-11            binutils/2.37              intel-compilers/2021.4.0
   GCC/9.3.0                        binutils/2.38       (D)    intel-compilers/2022.1.0 (D)
   GCC/11.2.0                       flex/2.6.4                 intel/2021b
   GCC/11.3.0               (D)     foss/2020a                 intel/2022a              (D)
   GCCcore/9.3.0                    foss/2021b                 iompi/2021b
   GCCcore/11.2.0                   foss/2022a          (D)    ncurses/6.1
   GCCcore/11.3.0           (D)     gettext/0.20.1             ncurses/6.2              (D)
   GPAW-setups/0.9.20000            gettext/0.21        (D)    pkgconf/1.8.0
   Java/11.0.16             (11)    gompi/2020a                pplacer/1.1.alpha19
   Julia/1.8.5-linux-x86_64         gompi/2021b                zlib/1.2.11
   M4/1.4.19                        gompi/2022a         (D)    zlib/1.2.12              (D)

  Where:
   Aliases:  Aliases exist: foo/1.2.3 (1.2) means that "module load foo/1.2" will load foo/1.2.3
   D:        Default Module

    
The list displays the toolchains (foss and intel) and the sub-toolchains (GCC, gompi, iompi, intel-compilers, iimpi, and imkl) availables to the users.


3. Loading Modules
------------------

3.1 GCC Based Modules
~~~~~~~~~~~~~~~~~~~~~

Let us assume that the user wants to use software compiled with GCC-9.3.0 (this GCC version is installed because it is needed for Fortran software using "include 'mpif90.h'"; GCC versions 10.0.0+ are very strick on MPI variable declarations and the compulation of the said Fortran software is not free of problems) he loads the corresponding modules

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

  --------------------------- /mnt/beegfs/stack/cn01470/modules/all/Compiler/GCC/9.3.0 ---------------------------
    OpenMPI/4.0.3

  ------------------------- /mnt/beegfs/stack/cn01470/modules/all/Compiler/GCCcore/9.3.0 -------------------------
    Autoconf/2.69         Perl/5.30.2-minimal        groff/1.22.4           libxml2/2.9.10                      
    Automake/1.16.1       Perl/5.30.2         (D)    help2man/1.47.12       makeinfo/6.7-minimal                
    Autotools/20180311    Szip/2.1.1                 hwloc/2.2.0            ncurses/6.2          (D)            
    Bison/3.5.3           UCX/1.8.0                  libevent/2.1.11        numactl/2.0.13                      
    CMake/3.16.4          XZ/5.2.5                   libfabric/1.11.0       pkg-config/0.29.2                   
    DB/18.1.32            binutils/2.34       (L)    libjpeg-turbo/2.0.4    xorg-macros/1.19.2                  
    HDF/4.2.15            bzip2/1.0.8                libpciaccess/0.16      zlib/1.2.11          (L)            
    M4/1.4.18             cURL/7.69.1                libreadline/8.0                                            
    NASM/2.14.02          expat/2.2.9                libtirpc/1.2.6                                             
    PMIx/3.1.5            flex/2.6.4          (D)    libtool/2.4.6                                              

  ----------------------------------- /mnt/beegfs/stack/cn01470/modules/all/Core ------------------------------------
    ANSYS_CFD/2021R1                  M4/1.4.19                    iimpi/2021b                                     
    ANSYS_CFD/2022R2         (D)      OpenSSL/1.1         (L)      iimpi/2022a              (D)                    
    Anaconda3/2022.05                 ant/1.10.11-Java-11          imkl/2021.4.0                                   
    Bison/3.8.2                       binutils/2.34                imkl/2022.1.0            (L,D)                  
    FastQC/0.11.9-Java-11             binutils/2.37                intel-compilers/2021.4.0                        
    GCC/9.3.0                         binutils/2.38                intel-compilers/2022.1.0 (L,D)                  
    GCC/11.2.0                        flex/2.6.4                   intel/2021b                                     
    GCC/11.3.0               (L,D)    foss/2021b                   intel/2022a              (L,D)                  
    GCCcore/9.3.0                     foss/2022a          (D)      ncurses/6.1                                     
    GCCcore/11.2.0                    gettext/0.20.1               ncurses/6.2                                     
    GCCcore/11.3.0           (L,D)    gettext/0.21                 pkgconf/1.8.0                                   
    GPAW-setups/0.9.20000             gompi/2020a                  zlib/1.2.11                                     
    Java/11.0.16             (11)     gompi/2021b                  zlib/1.2.12                                     
    Julia/1.8.2-linux-x86_64          gompi/2022a         (L,D)                                                    



Here one can see (from bottom to top) general software compiled with GCC-9.3.0, and MPI API compiled with GCC-9.3.0 following the scheme core/compiler/MPI referred above.

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

No longer have access to OpenMPI-4.0.3 and associated frameworks. Let's check what is available now (use ``module av``)

.. code-block:: julia

   -------------------------- /mnt/beegfs/stack/cn01470/modules/all/Compiler/GCC/11.2.0 ---------------------------
     BEDTools/2.30.0    FlexiBLAS/3.0.4    LAPACK/3.10.1      SAMtools/1.16.1    pybedtools/0.8.2                
     BLIS/0.8.1         Flye/2.9.1         OpenBLAS/0.3.18    STAR/2.7.9a                                        
     BamTools/2.5.2     GEOS/3.9.1         OpenMPI/4.1.1      libxc/5.1.6                                        
     Boost/1.77.0       GSL/2.7            Pysam/0.17.0       libxsmm/1.17                                       

   ------------------------ /mnt/beegfs/stack/cn01470/modules/all/Compiler/GCCcore/11.2.0 -------------------------
     ANTLR/2.7.7-Java-11                 Perl/5.34.0                    libGLU/9.0.2                             
     ATK/2.36.0                          Pillow/8.3.2                   libarchive/3.5.1                         
     Autoconf/2.71                       PyYAML/5.4.1                   libcerf/1.17                             
     Automake/1.16.4                     Python/2.7.18-bare             libdap/3.20.8                            
     Autotools/20210726                  Python/3.9.6-bare              libdrm/2.4.107                           
     Bazel/4.2.2                         Python/3.9.6            (D)    libepoxy/1.5.8                           
     Bison/3.7.6                         Qhull/2020.2                   libevent/2.1.12                          
     Brotli/1.0.9                        Qt5/5.15.2                     libfabric/1.13.2                         
     CMake/3.21.1                        RE2/2022-02-01                 libffi/3.4.2                             
     CMake/3.22.1                 (D)    RapidJSON/1.1.0                libgd/2.3.3                              
     DB/18.1.40                          Rust/1.54.0                    libgeotiff/1.7.0                         
     DBus/1.13.18                        SQLite/3.36                    libgit2/1.1.1                            
     Doxygen/1.9.1                       Szip/2.1.1                     libglvnd/1.3.3                           
     Eigen/3.3.9                         Tcl/8.6.11                     libiconv/1.16                            
     Eigen/3.4.0                  (D)    Tk/8.6.11                      libjpeg-turbo/2.0.6                      
     FFmpeg/4.3.2                        Tkinter/3.9.6                  libogg/1.3.5                             
     FLAC/1.3.3                          Togl/2.0                       libpciaccess/0.16          (L)
     ...

   ----------------------------------- /mnt/beegfs/stack/cn01470/modules/all/Core ------------------------------------
     ANSYS_CFD/2021R1                  M4/1.4.19                    iimpi/2021b                                     
     ANSYS_CFD/2022R2         (D)      OpenSSL/1.1         (L)      iimpi/2022a              (D)                    
     Anaconda3/2022.05                 ant/1.10.11-Java-11          imkl/2021.4.0                                   
     Bison/3.8.2                       binutils/2.34                imkl/2022.1.0            (L,D)                  
     FastQC/0.11.9-Java-11             binutils/2.37                intel-compilers/2021.4.0                        
     GCC/9.3.0                         binutils/2.38                intel-compilers/2022.1.0 (L,D)                  
     GCC/11.2.0                        flex/2.6.4                   intel/2021b                                     
     GCC/11.3.0               (L,D)    foss/2021b                   intel/2022a              (L,D)                  
     GCCcore/9.3.0                     foss/2022a          (D)      ncurses/6.1                                     
     GCCcore/11.2.0                    gettext/0.20.1               ncurses/6.2                                     
     GCCcore/11.3.0           (L,D)    gettext/0.21                 pkgconf/1.8.0                                   
     GPAW-setups/0.9.20000             gompi/2020a                  zlib/1.2.11                                     
     Java/11.0.16             (11)     gompi/2021b                  zlib/1.2.12                                     
     Julia/1.8.2-linux-x86_64          gompi/2022a         (L,D)                                                    

      
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

   ---------------------- /mnt/beegfs/stack/cn01470/modules/all/MPI/GCC/11.2.0/OpenMPI/4.1.1 ----------------------
     ABINIT/9.6.2                       MultiQC/1.12                              Valgrind/3.18.1
     ASE/3.22.1                         NCO/5.0.3                                 Wannier90/3.1.0
     AmberTools/22.3                    ORCA/5.0.3                                XCrySDen/1.6.2
     Arrow/6.0.0                        OSU-Micro-Benchmarks/5.7.1                arpack-ng/3.8.0
     ArviZ/0.11.4                       OpenCV/4.5.5-contrib                      arrow-R/6.0.0.2-R-4.1.2
     Bambi/0.7.1                        OpenFOAM/v2112                            ecCodes/2.24.2
     Biopython/1.79                     PLUMED/2.8.0                              futile/1.8.3
     CGAL/4.14.3                        PSolver/1.8.3                             h5py/3.6.0
     CP2K/8.2                           ParMETIS/4.0.3                            imkl-FFTW/2021.4.0
     Dalton/2020.0                      ParaView/5.9.1-mpi                        libGridXC/0.9.6
     ELPA/2021.05.001                   PnetCDF/1.12.3                            libvdwxc/0.4.0
     ESMF/8.2.0                         PyMC3/3.11.1                              matplotlib/3.4.3
     FFTW/3.3.10                 (L)    QuantumESPRESSO/7.0                       ncview/2.1.8
     FMS/2022.02                        R-bundle-Bioconductor/3.14-R-4.1.2        netCDF-C++4/4.3.1
     GDAL/3.3.2                         R/4.1.2                                   netCDF-Fortran/4.5.3
     GPAW/22.8.0                        SCOTCH/6.1.2                              netCDF/4.8.1
     GROMACS/2021.5-PLUMED-2.8.0        SPOTPY/1.5.14                             netcdf4-python/1.5.7
     GROMACS/2021.5              (D)    ScaFaCoS/1.0.1                            networkx/2.6.3
     HDF/4.2.15                  (D)    ScaLAPACK/2.1.0-fb                 (L)    numba/0.54.1
     HDF5/1.12.1                        SciPy-bundle/2021.10                      scikit-bio/0.5.7
     HPL/2.3                            Siesta/4.1.5                              scikit-learn/1.0.2
     Hypre/2.24.0                       SimPEG/0.18.1                             snakemake/6.10.0
     IMB/2021.3                         SuiteSparse/5.10.1-METIS-5.1.0            spglib-python/1.16.3
     LAMMPS/23Jun2022-kokkos            SuperLU/5.3.0                             statsmodels/0.13.1
     Libint/2.6.0-lmax-6-cp2k           TELEMAC-MASCARET/8p3r1                    worker/1.6.12
     MDAnalysis/2.0.0                   TensorFlow/2.8.4                          xarray/0.20.1
     MDTraj/1.9.7                       Theano/1.1.2-PyMC
     MUMPS/5.4.1-metis                  VTK/9.1.0

   -------------------------- /mnt/beegfs/stack/cn01470/modules/all/Compiler/GCC/11.2.0 ---------------------------
     BEDTools/2.30.0    FlexiBLAS/3.0.4    LAPACK/3.10.1          SAMtools/1.16.1    pybedtools/0.8.2
     BLIS/0.8.1         Flye/2.9.1         OpenBLAS/0.3.18        STAR/2.7.9a
     BamTools/2.5.2     GEOS/3.9.1         OpenMPI/4.1.1   (L)    libxc/5.1.6
     Boost/1.77.0       GSL/2.7            Pysam/0.17.0           libxsmm/1.17

   ------------------------ /mnt/beegfs/stack/cn01470/modules/all/Compiler/GCCcore/11.2.0 -------------------------
     ANTLR/2.7.7-Java-11                 Perl/5.34.0                    libGLU/9.0.2
     ATK/2.36.0                          Pillow/8.3.2                   libarchive/3.5.1
     Autoconf/2.71                       PyYAML/5.4.1                   libcerf/1.17
     Automake/1.16.4                     Python/2.7.18-bare             libdap/3.20.8
     Autotools/20210726                  Python/3.9.6-bare              libdrm/2.4.107
     Bazel/4.2.2                         Python/3.9.6            (D)    libepoxy/1.5.8
     Bison/3.7.6                         Qhull/2020.2                   libevent/2.1.12            (L)
     Brotli/1.0.9                        Qt5/5.15.2                     libfabric/1.13.2           (L)
     ...

The user got access to all the software that was compiled against OpenMPI-4.1.1 (top row), which in turn was compiled with GCC compiler (second row of modules). The third row displays the core modules associated to GCC/11.2.0.

3.2 Foss Toolchain
~~~~~~~~~~~~~~~~~~

Accessing the software modules made available by loading GCC/11.2.0 and OpenMPI/4.1.1 can be done by just loading foss/2021b with the penalty of loading extra modules like BLIS, FFTW, FlexiBLAS, OpenBLAS, ScaLAPACK. So, let's check it. Start with ``module purge`` followed by ``module load foss/2021b`` and ``module list`` obtaining

.. code-block:: julia

   Currently Loaded Modules:
     1) GCCcore/11.2.0   6) XZ/5.2.5           11) libevent/2.1.12   16) OpenBLAS/0.3.18
     2) zlib/1.2.11      7) libxml2/2.9.10     12) UCX/1.11.2        17) FlexiBLAS/3.0.4
     3) binutils/2.37    8) libpciaccess/0.16  13) libfabric/1.13.2  18) FFTW/3.3.10
     4) GCC/11.2.0       9) hwloc/2.5.0        14) PMIx/4.1.0        19) ScaLAPACK/2.1.0-fb
     5) numactl/2.0.14  10) OpenSSL/1.1        15) OpenMPI/4.1.1     20) foss/2021b

The available modules are (use ``module av``)

.. code-block:: julia

   ---------------------- /mnt/beegfs/stack/cn01470/modules/all/MPI/GCC/11.2.0/OpenMPI/4.1.1 ----------------------
     ABINIT/9.6.2                       MultiQC/1.12                              Valgrind/3.18.1
     ASE/3.22.1                         NCO/5.0.3                                 Wannier90/3.1.0
     AmberTools/22.3                    ORCA/5.0.3                                XCrySDen/1.6.2
     Arrow/6.0.0                        OSU-Micro-Benchmarks/5.7.1                arpack-ng/3.8.0
     ArviZ/0.11.4                       OpenCV/4.5.5-contrib                      arrow-R/6.0.0.2-R-4.1.2
     Bambi/0.7.1                        OpenFOAM/v2112                            ecCodes/2.24.2
     Biopython/1.79                     PLUMED/2.8.0                              futile/1.8.3
     CGAL/4.14.3                        PSolver/1.8.3                             h5py/3.6.0
     CP2K/8.2                           ParMETIS/4.0.3                            imkl-FFTW/2021.4.0
     Dalton/2020.0                      ParaView/5.9.1-mpi                        libGridXC/0.9.6
     ...
      
It is the same obtained previously by loading GCC/11.2.0 and OpenMPI/4.1.1.

Changing to foss/2022a leads to (after using ``module load foss/2022a``)

.. code-block:: julia

   Due to MODULEPATH changes, the following have been reloaded:                                           
     1) FFTW/3.3.10     2) XZ/5.2.5     3) libevent/2.1.12     4) libpciaccess/0.16     5) numactl/2.0.14 

   The following have been reloaded with a version change:                                                
     1) FlexiBLAS/3.0.4 => FlexiBLAS/3.2.0           8) UCX/1.11.2 => UCX/1.12.1                          
     2) GCC/11.2.0 => GCC/11.3.0                     9) binutils/2.37 => binutils/2.38                    
     3) GCCcore/11.2.0 => GCCcore/11.3.0            10) foss/2021b => foss/2022a                          
     4) OpenBLAS/0.3.18 => OpenBLAS/0.3.20          11) hwloc/2.5.0 => hwloc/2.7.1                        
     5) OpenMPI/4.1.1 => OpenMPI/4.1.4              12) libfabric/1.13.2 => libfabric/1.15.1              
     6) PMIx/4.1.0 => PMIx/4.1.2                    13) libxml2/2.9.10 => libxml2/2.9.13                  
     7) ScaLAPACK/2.1.0-fb => ScaLAPACK/2.2.0-fb    14) zlib/1.2.11 => zlib/1.2.12             

So, among others, GCC/11.2.0 and OpenMPI/4.1.1 were replaced by GCC/11.3.0 and OpenMPI/4.1.4, respectively. The loaded and available modules are

.. code-block:: julia

   Currently Loaded Modules:
     1) OpenSSL/1.1      7) hwloc/2.7.1       13) OpenBLAS/0.3.20     19) XZ/5.2.5                        
     2) GCCcore/11.3.0   8) UCX/1.12.1        14) FlexiBLAS/3.2.0     20) libpciaccess/0.16               
     3) zlib/1.2.12      9) libfabric/1.15.1  15) FFTW.MPI/3.3.10     21) libevent/2.1.12                 
     4) binutils/2.38   10) PMIx/4.1.2        16) ScaLAPACK/2.2.0-fb  22) FFTW/3.3.10                     
     5) GCC/11.3.0      11) UCC/1.0.0         17) foss/2022a                                              
     6) libxml2/2.9.13  12) OpenMPI/4.1.4     18) numactl/2.0.14                    

and

.. code-block:: julia

   ----------------------- /mnt/beegfs/stack/cn01470/modules/all/MPI/GCC/11.3.0/OpenMPI/4.1.4 ------------------------
     FFTW.MPI/3.3.10 (L)    ScaLAPACK/2.2.0-fb (L)

   ---------------------------- /mnt/beegfs/stack/cn01470/modules/all/Compiler/GCC/11.3.0 ----------------------------
     BLIS/0.9.0    FFTW/3.3.10 (L)    FlexiBLAS/3.2.0 (L)    OpenBLAS/0.3.20 (L)    OpenMPI/4.1.4 (L)
  
   -------------------------- /mnt/beegfs/stack/cn01470/modules/all/Compiler/GCCcore/11.3.0 --------------------------
     Autoconf/2.71             SQLite/3.38.3          flex/2.6.4        (D)    libtool/2.4.7
     Automake/1.16.5           Tcl/8.6.12             groff/1.22.4             libxml2/2.9.13     (L)
     Autotools/20220317        UCC/1.0.0     (L)      help2man/1.49.2          ncurses/6.3        (D)
     Bison/3.8.2        (D)    UCX/1.12.1    (L)      hwloc/2.7.1       (L)    numactl/2.0.14     (L)
     CMake/3.23.1              UnZip/6.0              libarchive/3.6.1         pkgconf/1.8.0      (D)
     DB/18.1.40                XZ/5.2.5      (L)      libevent/2.1.12   (L)    xorg-macros/1.19.3
     M4/1.4.19          (D)    binutils/2.38 (L,D)    libfabric/1.15.1  (L)    zlib/1.2.12        (L,D)
     PMIx/4.1.2         (L)    bzip2/1.0.8            libffi/3.4.2             Perl/5.34.1               
     cURL/7.83.0               libpciaccess/0.16 (L)  Python/3.10.4-bare       expat/2.4.8            
     libreadline/8.1.2

Not many modules as in foss/2021b.


3.3 Intel-Compilers Based Modules
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Similar procedure to what has been outlined above applies for software using the Intel compilers, MKL, and MPI. At the entering level if the user executes ``module av`` obtains 

.. code-block:: julia

   ----------------------------------- /mnt/beegfs/stack/cn01470/modules/all/Core ------------------------------------
     ANSYS_CFD/2021R1                  M4/1.4.19                    iimpi/2021b                                     
     ANSYS_CFD/2022R2         (D)      OpenSSL/1.1         (L)      iimpi/2022a              (D)                    
     Anaconda3/2022.05                 ant/1.10.11-Java-11          imkl/2021.4.0                                   
     Bison/3.8.2                       binutils/2.34                imkl/2022.1.0            (L,D)                  
     FastQC/0.11.9-Java-11             binutils/2.37                intel-compilers/2021.4.0                        
     GCC/9.3.0                         binutils/2.38                intel-compilers/2022.1.0 (L,D)                  
     GCC/11.2.0                        flex/2.6.4                   intel/2021b                                     
     GCC/11.3.0               (L,D)    foss/2021b                   intel/2022a              (L,D)                  
     GCCcore/9.3.0                     foss/2022a          (D)      ncurses/6.1                                     
     GCCcore/11.2.0                    gettext/0.20.1               ncurses/6.2                                     
     GCCcore/11.3.0           (L,D)    gettext/0.21                 pkgconf/1.8.0                                   
     GPAW-setups/0.9.20000             gompi/2020a                  zlib/1.2.11                                     
     Java/11.0.16             (11)     gompi/2021b                  zlib/1.2.12                                     
     Julia/1.8.2-linux-x86_64          gompi/2022a         (L,D)                                                    

      
After loading intel/2021b or iimpi/2021b (``module load intel/2021b`` or ``module load iimpi/2021b``) ``module list`` shows

.. code-block:: julia

   Currently Loaded Modules:
      1) GCCcore/11.2.0   3) binutils/2.37              5) numactl/2.0.14   7) impi/2021.4.0   9) imkl-FFTW/2021.4.0
      2) zlib/1.2.11      4) intel-compilers/2021.4.0   6) UCX/1.11.2       8) imkl/2021.4.0  10) intel/2021b

and ``module av`` displays

.. code-block:: julia

   --------------------- /mnt/beegfs/stack/cn01470/modules/all/MPI/intel/2021.4.0/impi/2021.4.0 ----------------------
     ABINIT/9.6.2          HPL/2.3                     SPOTPY/1.5.14                         libxsmm/1.17
     ASE/3.22.1            Hypre/2.24.0                ScaFaCoS/1.0.1                        matplotlib/3.4.3
     AmberTools/21         IMB/2021.3                  SciPy-bundle/2021.10                  mkl-service/2.3.0
     ArviZ/0.11.4          Libint/2.6.0-lmax-6-cp2k    Siesta/4.1.5                          ncview/2.1.8
     Bambi/0.7.1           MDAnalysis/2.0.0            SimPEG/0.18.1                         netCDF-C++4/4.3.1
     Biopython/1.79        MDTraj/1.9.7                SuiteSparse/5.10.1-METIS-5.1.0        netCDF-Fortran/4.5.3
     CGAL/4.14.3           MUMPS/5.4.1-metis           SuperLU/5.3.0                         netCDF/4.8.1
     CP2K/8.2              NCO/5.0.3                   Theano/1.1.2-PyMC                     netcdf4-python/1.5.7
     ELPA/2021.05.001      NWChem/7.0.2                VTK/9.1.0                             networkx/2.6.3
     ESMF/8.2.0            OSU-Micro-Benchmarks/5.8    Valgrind/3.18.1                       numba/0.54.1
     FDS/6.7.7             OpenMolcas/22.10            Wannier90/3.1.0                       scikit-bio/0.5.7
     FFTW/3.3.10           PLUMED/2.8.0                XCrySDen/1.6.2                        scikit-learn/1.0.1
     FMS/2022.02           PSolver/1.8.3               ecCodes/2.24.2                        spglib-python/1.16.3
     GDAL/3.3.2            ParMETIS/4.0.3              futile/1.8.3                          statsmodels/0.13.1
     GEOS/3.9.1            PnetCDF/1.12.3              h5py/3.6.0                            worker/1.6.13
     GPAW/22.8.0           PyMC3/3.11.1                imkl-FFTW/2021.4.0             (L)    xarray/0.20.1
     GlobalArrays/5.8.1    QuantumESPRESSO/7.0         libGridXC/0.9.6
     HDF5/1.12.1           SCOTCH/6.1.2                libvdwxc/0.4.0

   -------------------------- /mnt/beegfs/stack/cn01470/modules/all/Compiler/intel/2021.4.0 --------------------------
     BLIS/0.9.0      DFT-D3/3.2.0    GSL/2.7          NLopt/2.7.0   (D)    libxc/5.1.6
     Boost/1.77.0    Flye/2.9        LAPACK/3.10.1    impi/2021.4.0 (L)    xmlf90/1.5.4

   -------------------------- /mnt/beegfs/stack/cn01470/modules/all/Compiler/GCCcore/11.2.0 --------------------------
     ANTLR/2.7.7-Java-11                 Perl/5.34.0                    libGLU/9.0.2
     ATK/2.36.0                          Pillow/8.3.2                   libarchive/3.5.1
     Autoconf/2.71                       PyYAML/5.4.1                   libcerf/1.17
     Automake/1.16.4                     Python/2.7.18-bare             libdap/3.20.8
     Autotools/20210726                  Python/3.9.6-bare              libdrm/2.4.107
     Bazel/4.2.2                         Python/3.9.6            (D)    libepoxy/1.5.8
     ...

On the top section the software compiled against Intel MPI (which is MPICH compiled against the Intel compilers) is displayed followed by the software compiled with Intel C, C++ and Fortran compilers. On the bottom is the software compiled with GCC/11.2.0 as a backend.

The user can change to GCC based modules, e.g., to the foss/2021b toochain, by issuing ``module load foss/2021b`` obtaining

.. code-block:: julia

   Lmod is automatically replacing "intel-compilers/2021.4.0" with "GCC/11.2.0".
   
   Inactive Modules:
     1) impi/2021.4.0

   Due to MODULEPATH changes, the following have been reloaded:                                                      
     1) imkl-FFTW/2021.4.0


and ``module list`` gives

.. code-block:: julia

   Currently Loaded Modules:
     1) GCCcore/11.2.0   6) imkl/2021.4.0   11) libpciaccess/0.16  16) PMIx/4.1.0       21) ScaLAPACK/2.1.0-fb       
     2) zlib/1.2.11      7) intel/2021b     12) hwloc/2.5.0        17) OpenMPI/4.1.1    22) foss/2021b               
     3) binutils/2.37    8) GCC/11.2.0      13) OpenSSL/1.1        18) OpenBLAS/0.3.18  23) imkl-FFTW/2021.4.0       
     4) numactl/2.0.14   9) XZ/5.2.5        14) libevent/2.1.12    19) FlexiBLAS/3.0.4                               
     5) UCX/1.11.2      10) libxml2/2.9.10  15) libfabric/1.13.2   20) FFTW/3.3.10            

   Inactive Modules:
      1) impi/2021.4.0


4. Loading a Particular Software
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


5. Operations With Modules
--------------------------

5.1 Purging Modules
~~~~~~~~~~~~~~~~~~~

The user can purge the loaded modules by executing 

.. code-block:: julia
  
  module purge
  
  
5.2 Save and Restore Modules
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Often a user uses different environments for his/her processes. Hence, he/she needs to load and purge the loaded modules several times. An easy way to proceed is to save those module environments into a file, say <module_environment>, by using 

.. code-block:: julia

  module save <module_environment>. 
  
Later, the environment can be reloaded using the command 

.. code-block:: julia

  module restore <module_environment>


5.3 Module Details
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

 
6. List of Commonly Used Commands
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


7. Available Modules
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
