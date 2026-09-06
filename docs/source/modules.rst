Environment Modules
===================

There are several conflicting software packages installed in the OBLIVION supercomputer. In order for the user to use the adequate software there is a need to set the paths for the binaries, libraries, manuals, and includes. Either the user sets these paths for each package or the system provides an easy way to set those paths. The latter is the preferable and makes use of environment modules. 

1. Toolchains
-------------

1.1 Definitions
~~~~~~~~~~~~~~~

Toolchain is a pack of compiler(s) and libraries bundled together to provide a specific functionality, say, running applications using a MPI distribution compiled against GCC (toolchain gompi) or Intel compilers (toolchain iimpi) or use linear algebra libraries with a MPI API compiled with GCC (toolchain foss).

**Toolchain foss includes the following software:**

- GCC compilers (C, C++, Fortran)
- OpenMPI
- OpenBLAS and FlexiBLAS
- ScaLAPACK
- FFTW

**Toolchain intel includes the following software:**

- Intel compilers (C, C++ and Fortran: icx/icpx/ifx)
- MPI implementation (Intel MPI)
- BLAS, LAPACK and FFTW: Intel MKL

**Toolchain lfoss includes the following software:**

- LLVM compilers
- OpenMPI
- AOCL-BLAS, OpenBLAS and FlexiBLAS
- ScaLAPACK
- FFTW

**Sub-toolchains:** 

- gompi (GCC + OpenMPI)
- lompi (LLVM + OpenMPI)
- iompi (Intel compilers + OpenMPI)
- iimpi (Intel compilers + Intel MPI (MPICH))
- imkl (Intel Math Kernel Library) 

1.2 Toolchains and Sub-toolchains installed in OBLIVION
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Toolchains:

- foss: 2025b, 2026.1
- intel: 2025b, 2026.1
 
Sub-toolchains:

- gompi: 2025b, 2026.1
- iimpi: 2025b, 2026.1
- iompi: 2025b
- intel-compilers: 2025.2.0, 2025.3.3
- imkl: 2025.2.0, 2025.3.1 


2. Core Modules
---------------

The user sets the software environment by loading the modules associated to the needed packages. This is easily done by using ``module load`` or ``module add``. Software dependences are set in the same way. OBLIVION uses a hierarchical module naming scheme (HMNS) in which modules availability follows the software hierarchy Core/Compiler/MPI.

Core refers to the basic core modules that have to be loaded in order to have access to next levels of software compiled against a specific compiler (e.g., ``GCC 14.3.0, 15.2.0; intel-compilers 2025.2.0, 2025.3.3``) and a MPI API (e.g., OpenMPI 5.0.8, 5.0.10; MPICH 4.3.2, 5.0.1). 

After logging into the machine the user should execute the command ``module av`` (av for available software) obtaining the list of core modules (including the toolchains and sub-toolchains):

.. code-block:: julia

  ----------------------------------- /mnt/beegfs/appsx/modules/all/Core -----------------------------------
    GCC/14.3.0            foss/2026.1  (D)    iimpi/2025b                     intel-compilers/2025.3.3 (D)
    GCC/15.2.0     (D)    gfbf/2025b          iimpi/2026.1             (D)    intel/2025b
    GCCcore/14.3.0        gfbf/2026.1  (D)    imkl/2025.2.0                   intel/2026.1             (D)
    GCCcore/15.2.0 (D)    gompi/2025b         imkl/2025.3.1            (D)    site/langs
    foss/2025b            gompi/2026.1 (D)    intel-compilers/2025.2.0

  Where:
     Aliases:  Aliases exist: foo/1.2.3 (1.2) means that "module load foo/1.2" will load foo/1.2.3
     D:        Default Module


The list displays the toolchains (foss and intel) and the sub-toolchains (GCC, gompi, iompi, intel-compilers, iimpi, and imkl) availables to the users. It also displays site/langs that includes the modules of packages not built but downloaded into the stack (e.g., Anaconda, Julia, Perl, Miniconda, etc...)

In order to see the contents of site/langs run the command ``module load site/langs && module av`` obtaining

.. code-block:: julia
  
  ---------------------------------- /mnt/beegfs/appsx/modules/all/langs -----------------------------------
    Anaconda3/2025.12-1        Java/21.0.8  (21)    Julia/1.12.7        (D)    Miniforge3/25.3.0-3
    Anaconda3/2026.07-1 (D)    Julia/1.12.6         Miniconda3/25.7.0-2        Perl/5.38.0


Core also includes modules of software that are initially compiled with the system/machine compiler (e.g., binutils, gettext, M4, ncurses, pkgconf, zlib) but are not shown to the user - hidden modules - that can be seen by using the command ``module --show-hidden av``:

.. code-block:: julia

  --------------------------------------- /mnt/beegfs/appsx/modules/all/Core -------------------------------
    Bison/3.8.2    (H)    Pandoc/3.6.2        (H)    gettext/0.25  (H)    intel-compilers/2025.2.0
    GCC/14.3.0            ant/1.10.15-Java-21 (H)    gfbf/2025b           intel-compilers/2025.3.3 (D)
    GCC/15.2.0     (D)    binutils/2.40       (H)    gfbf/2026.1   (D)    intel/2025b
    GCCcore/14.3.0        binutils/2.44       (H)    gompi/2025b          intel/2026.1             (D)
    GCCcore/15.2.0 (D)    binutils/2.45       (H)    gompi/2026.1  (D)    ncurses/6.5              (H)
    M4/1.4.19      (H)    ffnvcodec/13.0.19.0 (H)    iimpi/2025b          pkgconf/1.8.0            (H)
    M4/1.4.20      (H)    flex/2.6.4          (H)    iimpi/2026.1  (D)    site/langs
    OSPRay/2.12.0  (H)    foss/2025b                 imkl/2025.2.0        zlib/1.2.13              (H)
    OpenSSL/3      (H)    foss/2026.1         (D)    imkl/2025.3.1 (D)    zlib/1.3.1               (H)

  Where:
   D:  Default Module
   H:  Hidden Module


3. Loading Modules
------------------

3.1 GCC Based Modules
~~~~~~~~~~~~~~~~~~~~~

Let us assume that the user wants to use software compiled with GCC-13.3.0 he must load the corresponding modules

.. code-block:: julia

  module load GCC/14.3.0

To learn the loaded modules use

.. code-block:: julia

  module list

obtaining

.. code-block:: julia

  Currently Loaded Modules:
    1) GCCcore/14.3.0   2) zlib/1.3.1   3) binutils/2.44   4) GCC/14.3.0

Loading the module GCC/14.3.0 gives access to other modules that only now became available. To see those modules use ``module av`` obtaining

.. code-block:: julia

  -------------------------------- /mnt/beegfs/appsx/modules/all/Compiler/GCC/14.3.0 --------------------------
     AOCL-BLAS/5.1            Delly/2.0.0       (D)    OpenMPI/5.0.8           imageio/2.37.0
     ASE/3.26.0               Dyninst/13.0.0           Osi/0.108.11            kim-api/2.4.1
     ASE/3.29.0        (D)    FFTW/3.3.10              Pysam/0.23.3            libxc/7.0.0
     Arrow/22.0.0             FlexiBLAS/3.4.5          R/4.5.2                 lpsolve/5.5.2.14
     BCFtools/1.22            GEOS/3.13.1              SAMtools/1.22.1         matplotlib/3.10.5
     BLIS/2.0                 GKlib-METIS/5.1.1        SOCI/4.1.2              mrcfile/1.5.4
     BamTools/2.5.3           GSL/2.8                  SciPy-bundle/2025.07    networkx/3.5
     Biopython/1.86           HTSlib/1.22.1            Seaborn/0.13.2          pybind11/3.0.0
     Boost/1.88.0             Kokkos/5.0.2             Shapely/2.1.1           scikit-learn/1.7.1
     CoinUtils/2.11.12        MGARD/1.6.0              SuiteSparse/7.11.0      spglib-python/2.6.0
     DFT-D3/3.2.0             MPICH/4.3.2              bokeh/3.7.3             statsmodels/0.14.6
     Delly/1.7.3              OpenBLAS/0.3.30          dask/2025.9.1

 ------------------------------ /mnt/beegfs/appsx/modules/all/Compiler/GCCcore/14.3.0 -------------------------
    ATK/2.38.0                          PostgreSQL/17.5                     libarchive/3.8.1
    Abseil/20250512.1                   PyYAML/6.0.2                        libcerf/3.0
    Autoconf/2.72                       Python-bundle-PyPI/2025.07          libclc/20.1.8
    Automake/1.18                       Python/3.13.5                       libde265/1.0.16
    Autotools/20250527                  Qhull/2020.2                        libdeflate/1.24
    Bison/3.8.2                         Qt6/6.9.3                           libdrm/2.4.125
    Blosc/1.21.6                        RE2/2025-07-22                      libepoxy/1.5.10
    Blosc2/2.19.0                       RapidJSON/1.1.0-20250205            libevent/2.1.12
    Brotli/1.1.0                        Redis/8.2.2                         libfabric/2.1.0
    Brunsli/0.1                         Rust/1.88.0                         libffi/3.5.1
    CFITSIO/4.6.2                       SDL2/2.32.10                        libgd/2.3.3
    CGAL/6.0.1                          SIONlib/1.7.7-tools                 libgeotiff/1.7.4
     ...
 ----------------------------------- /mnt/beegfs/appsx/modules/all/Core -----------------------------------
    GCC/14.3.0     (L)    foss/2026.1  (D)    iimpi/2025b                     intel-compilers/2025.3.3 (D)
    GCC/15.2.0     (D)    gfbf/2025b          iimpi/2026.1             (D)    intel/2025b
    GCCcore/14.3.0 (L)    gfbf/2026.1  (D)    imkl/2025.2.0                   intel/2026.1             (D)
    GCCcore/15.2.0 (D)    gompi/2025b         imkl/2025.3.1            (D)    site/langs
    foss/2025b            gompi/2026.1 (D)    intel-compilers/2025.2.0


Here one can see (from bottom to top) the foundation layer (Core) modules (compilers, toolchains, base configurations) indicating those currently loaded with (L). This is followed by a minimalist layer (GCCcore) built using only the GCC compiler; it intentionally separates software from specific math optimizations (like BLAS) and parallel communication libraries (MPI) to maximize reusability across different toolchains. Finally, the full compiler layer (GCC) contains software leveraging the full capabilities of GCC, including high-level scientific libraries, math-intensive Python bundles, and the core parallel communication engines (OpenMPI and MPICH) compiled for this specific GCC version.

To have access to software compiled with OpenMPI-5.0.8, the user needs to use ``module load OpenMPI/5.0.8``. The list of packages loaded is now given by ``module list``:

.. code-block:: julia

   Currently Loaded Modules:
     1) GCCcore/14.3.0   6) XZ/5.8.1                 11) libevent/2.1.12  16) UCC/1.4.4
     2) zlib/1.3.1       7) libxml2/2.14.3           12) UCX/1.19.0       17) OpenMPI/5.0.8
     3) binutils/2.44    8) libpciaccess/0.18.1      13) libfabric/2.1.0
     4) GCC/14.3.0       9) hwloc/2.12.1             14) PMIx/5.0.8
     5) numactl/2.0.19  10) OpenSSL/3           (H)  15) PRRTE/3.0.11

  Where:
   H:  Hidden Module


Not only OpenMPI is loaded, but also UCX, PMIx, etc., are loaded. UCX stands for Unified Communication X and is "an optimized production communication framework for modern, high-bandwidth and low-latency networks" (see https://github.com/openucx/ucx) meaning for infiniband. PMIx stands for "Process Management Interface - Exascale" and enables the interaction of MPI applications with Resource Managers like SLURM (see https://pmix.github.io).

We loaded ``OpenMPI/5.0.8`` and checked the list of loaded modules, but loading OpenMPI/5.0.8 module opens a new avenue to access all the packages compiled against OpenMPI/5.0.8 that can be seen by exe cuting the command ``module av``:

.. code-block:: julia

  --------------------- /mnt/beegfs/appsx/modules/all/MPI/GCC/14.3.0/OpenMPI/5.0.8 -------------------
    ABINIT/10.4.7               Kraken2/2.17.1                        Valgrind/3.25.1
    ADIOS2/2.10.2               MDAnalysis/2.10.0                     Wannier90/3.1.0
    ASAP3/3.13.11               MDTraj/1.11.0                         Zoltan/3.901
    AUGUSTUS/3.5.0              MUMPS/5.8.1-metis                     arpack-ng/3.9.1
    Armadillo/15.0.1            ORCA/6.1.0-avx2                       biom-format/2.1.17
    Armadillo/15.2.6     (D)    ORCA/6.1.0                            buildenv/default
    BLAST+/2.17.0               ORCA/6.1.1-avx2                       cnvpytor/1.3.1
    Boost.MPI/1.88.0            ORCA/6.1.1                     (D)    dtcmp/1.1.5
    CASTEP/25.12                OSU-Micro-Benchmarks/7.5.1            ecCodes/2.43.0
    CASTEP/26.11         (D)    OpenFOAM/v2506                        h5py/3.14.0
    CDO/2.5.3                   OpenFOAM/v2512                 (D)    iodata/1.0.0a8
    Cabana/0.7.0                Optuna/4.6.0                          libcircle/0.3
    Cartopy/0.25.0              PLUMED/2.9.4                          libosmium/2.22.0
    Cbc/2.10.12                 ParMETIS/4.0.3                        libvdwxc/0.5.0
    Cgl/0.60.9                  ParaView-Catalyst/2.0.0               lwgrp/1.0.6
    Clp/1.17.10                 ParaView/6.0.1                        maeparser/1.3.3
    CoordgenLibs/3.0.2          PnetCDF/1.14.1                        mpi4py/4.1.0
    DIRAC/25.0                  PuLP/3.3.0                            mpifileutils/0.12
    DIRAC/26.0           (D)    PyHMMER/0.12.0                        ncbi-vdb/3.4.1
    Dalton/2026-parallel        PyTables/3.10.2                       ncview/2.1.11
    ESPResSo/4.2.2              PyWavelets/1.9.0                      netCDF-C++4/4.3.1
    FFTW.MPI/3.3.10             SCOTCH/7.0.10                         netCDF-Fortran/4.6.2
    Fiona/1.10.1                SUNDIALS/7.6.0                        netCDF/4.9.3
    GDAL/3.11.3                 ScaFaCoS/1.0.4                        netcdf4-python/1.7.2
    HDF5/1.14.6                 ScaLAPACK/2.2.2-fb                    numba/0.62.0
    HMMER/3.4                   Scalasca/2.6.2                        osmium-tool/1.18.0
    HPCToolkit/2025.0.1         Score-P/9.4                           pyFFTW/0.15.1
    HPL/2.3                     SuiteSparse/7.11.0-METIS-5.1.0        s3fs/2025.10.0
    HeFFTe/2.4.1                SuperLU_DIST/9.1.0                    scikit-bio/0.7.2
    HighFive/3.3.0              VASP/6.5.0                            scikit-image/0.25.0
    Hypre/2.33.0                VASP/6.5.1                     (D)    snakemake/9.22.0
    KaHIP/3.19                  VTK/9.5.2

 -------------------------- /mnt/beegfs/appsx/modules/all/Compiler/GCC/14.3.0 -----------------------
   AOCL-BLAS/5.1            Delly/2.0.0       (D)    OpenMPI/5.0.8        (L)    imageio/2.37.0
   ASE/3.26.0               Dyninst/13.0.0           Osi/0.108.11                kim-api/2.4.1
   ASE/3.29.0        (D)    FFTW/3.3.10              Pysam/0.23.3                libxc/7.0.0
   Arrow/22.0.0             FlexiBLAS/3.4.5          R/4.5.2                     lpsolve/5.5.2.14
   BCFtools/1.22            GEOS/3.13.1              SAMtools/1.22.1             matplotlib/3.10.5
   BLIS/2.0                 GKlib-METIS/5.1.1        SOCI/4.1.2                  mrcfile/1.5.4
   BamTools/2.5.3           GSL/2.8                  SciPy-bundle/2025.07        networkx/3.5
   Biopython/1.86           HTSlib/1.22.1            Seaborn/0.13.2              pybind11/3.0.0
   Boost/1.88.0             Kokkos/5.0.2             Shapely/2.1.1               scikit-learn/1.7.1
   CoinUtils/2.11.12        MGARD/1.6.0              SuiteSparse/7.11.0   (D)    spglib-python/2.6.0
   DFT-D3/3.2.0             MPICH/4.3.2              bokeh/3.7.3                 statsmodels/0.14.6
   Delly/1.7.3              OpenBLAS/0.3.30          dask/2025.9.1
   ...


This list shows the new level in the hierarchy: the parallel software compiled with OpenMPI/5.0.8, followed by the compiler layer (GCC) discussed previously. A large number of packages were compiled and optimized for OBLIVION CPUs.


Let us now change the enviromment to one using GCC-15.2.0. Hence, load the module GCC/15.2.0 (use ``module load GCC/15.2.0``) and immediately it is seen

.. code-block:: julia

  Inactive Modules:
    1) OpenMPI/5.0.8     3) PRRTE/3.0.11     5) UCX/1.19.0     7) hwloc/2.12.1        9) libpciaccess/0.18.1
    2) PMIx/5.0.8        4) UCC/1.4.4        6) XZ/5.8.1       8) libfabric/2.1.0    10) libxml2/2.14.3

  Due to MODULEPATH changes, the following have been reloaded:
    1) libevent/2.1.12     2) numactl/2.0.19

  The following have been reloaded with a version change:
    1) GCC/14.3.0 => GCC/15.2.0             3) binutils/2.44 => binutils/2.45
    2) GCCcore/14.3.0 => GCCcore/15.2.0     4) zlib/1.3.1 => zlib/2.3.2


So, what happen? Basically the system is smart enough to understand that the dependences and core files in the previous environment are incompatible with GCC/15.2.0 and replaces or deactivates modules. Check the loaded modules with ``module list``

.. code-block:: julia

  Currently Loaded Modules:
    1) GCCcore/15.2.0   3) binutils/2.45   5) numactl/2.0.19       7) libevent/2.1.12
    2) zlib/2.3.2       4) GCC/15.2.0      6) OpenSSL/3      (H)

  Where:
    H:  Hidden Module

  Inactive Modules:
    1) XZ/5.8.1         3) libpciaccess/0.18.1   5) UCX/1.19.0        7) PMIx/5.0.8     9)  UCC/1.4.4
    2) libxml2/2.14.3   4) hwloc/2.12.1          6) libfabric/2.1.0   8) PRRTE/3.0.11   10) OpenMPI/5.0.8


No longer have access to OpenMPI/5.0.8 and associated frameworks. Let's check what is available (use ``module --nx av``)

.. code-block:: julia

  ------------------------------- /mnt/beegfs/appsx/modules/all/Compiler/GCC/15.2.0 ---------------------------
     AOCL-BLAS/5.2     Delly/2.0.0     (D)    Kokkos/5.1.1       SciPy-bundle/2026.05    matplotlib/3.10.9
     ASE/3.28.0        FFTW/3.3.10            MPICH/5.0.1        Seaborn/0.13.2          mrcfile/1.5.4
     Arrow/24.0.0      FlexiBLAS/3.5.0        OpenBLAS/0.3.32    Shapely/2.1.2           networkx/3.6.1
     BLIS/2.0          GEOS/3.14.1            OpenMPI/5.0.10     SuiteSparse/7.12.2      scikit-learn/1.8.0
     Biopython/1.87    GSL/2.8                Pysam/0.24.0       bokeh/3.10.0            spglib-python/2.7.0
     Delly/1.7.3       HTSlib/1.23.1          R/4.6.1            dask/2026.7.1

  ----------------------------- /mnt/beegfs/appsx/modules/all/Compiler/GCCcore/15.2.0 -------------------------
    Abseil/20260107.1                   PyYAML/6.0.3                        libde265/1.1.0
    Autoconf/2.72                       Pygments/2.20.0                     libdeflate/1.25
    Automake/1.18.1                     Python-bundle-PyPI/2026.04          libdrm/2.4.133
    Autotools/20250626                  Python/3.14.2                       libevent/2.1.12          (L)
    BeautifulSoup/4.14.3                Qhull/2020.2                        libfabric/2.5.0
    Bison/3.8.2                         RE2/2025-11-05                      libffi/3.5.2
    Blosc/1.21.6                        RapidJSON/1.1.0-20250205            libgeotiff/1.7.4
    Blosc2/3.1.2                        Rust/1.94.1                         libgit2/1.9.4
    Boost/1.90.0                        SIONlib/1.7.7-tools                 libheif/1.22.2
    Brotli/1.2.0                        SOCI/4.1.4                          libiconv/1.18
    Brunsli/0.1                         SQLite/3.51.1                       libidn2/2.3.8
    CFITSIO/4.6.4                       SWIG/4.4.1                          libjpeg-turbo/3.1.4.1
    CMake/3.31.11                       Szip/2.1.1                          libogg/1.3.6
    CMake/4.2.1                  (D)    Tcl/9.0.3                           libopus/1.6.1
    Catch2/2.13.10                      Tk/9.0.3                            libpciaccess/0.19
    Check/0.15.2                        Tkinter/3.14.2                      libpng/1.6.56
    CubeLib/4.9.1                       UCC/1.7.0                           libpsl/0.21.5
    ...
   
 -------------------------------------- /mnt/beegfs/appsx/modules/all/Core -----------------------------------
   GCC/14.3.0              foss/2025b         gompi/2026.1  (D)    intel-compilers/2025.2.0
   GCC/15.2.0     (L,D)    foss/2026.1 (D)    iimpi/2025b          intel-compilers/2025.3.3 (D)
   GCCcore/14.3.0          gfbf/2025b         iimpi/2026.1  (D)    intel/2025b
   GCCcore/15.2.0 (L,D)    gfbf/2026.1 (D)    imkl/2025.2.0        intel/2026.1             (D)
   binutils/2.45           gompi/2025b        imkl/2025.3.1 (D)    site/langs

  Where:
   L:  Module is loaded
   D:  Default Module


Again, besides the core modules, there is a large list of packages compiled with GCC-15.2.0 including OpenMPI-5.0.10, OpenBLAS, LAPACK, etc.. Load OpenMPI/5.0.10 (``module load OpenMPI/5.0.10``) obtaining

.. code-block:: julia

  Activating Modules:
  1) OpenMPI/5.0.10     3) PRRTE/4.1.0     5) UCX/1.20.0       7) libfabric/2.5.0       9) libxml2/2.15.1
  2) PMIx/6.1.0         4) UCC/1.7.0       6) hwloc/2.13.0     8) libpciaccess/0.19

and list the loaded modules (``module list``)

.. code-block:: julia

  Currently Loaded Modules:
    1) GCCcore/15.2.0   6) libxml2/2.15.1         11) libevent/2.1.12  16) UCC/1.7.0
    2) binutils/2.45    7) libpciaccess/0.19      12) UCX/1.20.0       17) OpenMPI/5.0.10
    3) GCC/15.2.0       8) ncurses/6.6            13) libfabric/2.5.0
    4) numactl/2.0.19   9) hwloc/2.13.0           14) PMIx/6.1.0
    5) zlib/2.3.2      10) OpenSSL/3         (H)  15) PRRTE/4.1.0

  Where:
    H:  Hidden Module

  Inactive Modules:
    1) XZ/5.8.1

and see what is available (``module --nx av``)

.. code-block:: julia

  --------------------------- /mnt/beegfs/appsx/modules/all/MPI/GCC/15.2.0/OpenMPI/5.0.10 ----------------------
    Armadillo/15.2.7        MDAnalysis/2.10.0                       SuperLU_DIST/9.2.1
    Boost.MPI/1.90.0        MDTraj/1.11.1                           arpack-ng/3.9.1
    CASTEP/26.11            MUMPS/5.9.1-metis                       buildenv/default
    Cartopy/0.25.0          OSU-Micro-Benchmarks/7.5.2              cnvpytor/1.3.1
    CoordgenLibs/3.0.2      OpenMM/8.5.2                            h5py/3.16.0
    Dalton/2026-parallel    PETSc/3.25.0                            iodata/1.0.1
    Extrae/5.0.6            PLUMED/2.10.0                           maeparser/1.3.3
    FFTW.MPI/3.3.10         ParMETIS/4.0.3                          mpi4py/4.1.2
    Fiona/1.10.1            PnetCDF/1.14.1                          ncbi-vdb/3.4.1
    GDAL/3.13.0             PyTables/3.11.1                         netCDF-C++4/4.3.1
    HDF5/2.1.1              R-bundle-CRAN/2026.07                   netCDF-Fortran/4.6.3
    HPCToolkit/2026.0.1     RStudio-Server/2026.06.0+242-R-4.6.1    netCDF/4.10.0
    HPL/2.3                 SCOTCH/7.0.11                           netcdf4-python/1.7.4
    HeFFTe/2.4.1            ScaLAPACK/2.2.2-fb                      numba/0.65.1
    Hypre/3.1.0             Score-P/10.1
    ...

The user got access to  a new level the software hierarchy. Hence, having access to all the software that was compiled against OpenMPI-5.0.10, which in turn was compiled with GCC-15.2.0.


3.2 Foss/2025b Toolchain
~~~~~~~~~~~~~~~~~~~~~~~~

Accessing the software modules made available by loading GCC/14.3.0 and OpenMPI/5.0.8 can be done by just loading foss/2025b with the penalty of loading extra modules like BLIS, FFTW, FlexiBLAS, OpenBLAS, ScaLAPACK. let's check it. Start with ``module purge`` followed by ``module load foss/2025b`` and ``module list`` obtaining

.. code-block:: julia

  Currently Loaded Modules:
    1) GCCcore/14.3.0   6) XZ/5.8.1                 11) libevent/2.1.12  16) UCC/1.4.4        21) FFTW.MPI/3.3.10
    2) zlib/1.3.1       7) libxml2/2.14.3           12) UCX/1.19.0       17) OpenMPI/5.0.8    22) ScaLAPACK/2.2.2-fb
    3) binutils/2.44    8) libpciaccess/0.18.1      13) libfabric/2.1.0  18) OpenBLAS/0.3.30  23) foss/2025b
    4) GCC/14.3.0       9) hwloc/2.12.1             14) PMIx/5.0.8       19) FlexiBLAS/3.4.5
    5) numactl/2.0.19  10) OpenSSL/3           (H)  15) PRRTE/3.0.11     20) FFTW/3.3.10

  Where:
    H:  Hidden Module


The available modules are (use ``module --nx av``)

.. code-block:: julia

  --------------------------- /mnt/beegfs/appsx/modules/all/MPI/GCC/14.3.0/OpenMPI/5.0.8 ------------------------
    ABINIT/10.4.7               Kraken2/2.17.1                        Valgrind/3.25.1
    ADIOS2/2.10.2               MDAnalysis/2.10.0                     Wannier90/3.1.0
    ASAP3/3.13.11               MDTraj/1.11.0                         Zoltan/3.901
    AUGUSTUS/3.5.0              MUMPS/5.8.1-metis                     arpack-ng/3.9.1
    Armadillo/15.0.1            ORCA/6.1.0-avx2                       biom-format/2.1.17
    Armadillo/15.2.6     (D)    ORCA/6.1.0                            buildenv/default
    BLAST+/2.17.0               ORCA/6.1.1-avx2                       cnvpytor/1.3.1
    Boost.MPI/1.88.0            ORCA/6.1.1                     (D)    dtcmp/1.1.5
    CASTEP/25.12                OSU-Micro-Benchmarks/7.5.1            ecCodes/2.43.0
    CASTEP/26.11         (D)    OpenFOAM/v2506                        h5py/3.14.0
    CDO/2.5.3                   OpenFOAM/v2512                 (D)    iodata/1.0.0a8
    Cabana/0.7.0                Optuna/4.6.0                          libcircle/0.3
    Cartopy/0.25.0              PLUMED/2.9.4                          libosmium/2.22.0
    Cbc/2.10.12                 ParMETIS/4.0.3                        libvdwxc/0.5.0
    Cgl/0.60.9                  ParaView-Catalyst/2.0.0               lwgrp/1.0.6
    Clp/1.17.10                 ParaView/6.0.1                        maeparser/1.3.3
    ...
  
 -------------------------------- /mnt/beegfs/appsx/modules/all/Compiler/GCC/14.3.0 ----------------------------
   AOCL-BLAS/5.1            Delly/2.0.0       (D)    OpenMPI/5.0.8        (L)    imageio/2.37.0
   ASE/3.26.0               Dyninst/13.0.0           Osi/0.108.11                kim-api/2.4.1
   ASE/3.29.0        (D)    FFTW/3.3.10       (L)    Pysam/0.23.3                libxc/7.0.0
   Arrow/22.0.0             FlexiBLAS/3.4.5   (L)    R/4.5.2                     lpsolve/5.5.2.14
   BCFtools/1.22            GEOS/3.13.1              SAMtools/1.22.1             matplotlib/3.10.5
   BLIS/2.0                 GKlib-METIS/5.1.1        SOCI/4.1.2                  mrcfile/1.5.4
   BamTools/2.5.3           GSL/2.8                  SciPy-bundle/2025.07        networkx/3.5
   Biopython/1.86           HTSlib/1.22.1            Seaborn/0.13.2              pybind11/3.0.0
   Boost/1.88.0             Kokkos/5.0.2             Shapely/2.1.1               scikit-learn/1.7.1
   CoinUtils/2.11.12        MGARD/1.6.0              SuiteSparse/7.11.0   (D)    spglib-python/2.6.0
   DFT-D3/3.2.0             MPICH/4.3.2              bokeh/3.7.3                 statsmodels/0.14.6
   Delly/1.7.3              OpenBLAS/0.3.30   (L)    dask/2025.9.1
   ...

      
It is the same obtained previously by loading GCC/14.3.0 and OpenMPI/5.0.8. The list of packages includes well known and established software used by the quantum chemistry and material sciences communities, e.g., DIRAC, ELPA, Orca, Scafacos, VASP, QuantumEspresso, etc., and computational fluid dynamics (CFD), e.g., OpenFoam).

As an exercise lets load DIRAC using the command ``module load DIRAC/25.0`` followed with ``module list`` obtaining

.. code-block:: julia

   Currently Loaded Modules:
  1) GCCcore/14.3.0       10) OpenSSL/3       (H)  19) FlexiBLAS/3.4.5     28) Tcl/9.0.1
  2) zlib/1.3.1           11) libevent/2.1.12      20) FFTW/3.3.10         29) SQLite/3.50.1
  3) binutils/2.44        12) UCX/1.19.0           21) FFTW.MPI/3.3.10     30) libffi/3.5.1
  4) GCC/14.3.0           13) libfabric/2.1.0      22) ScaLAPACK/2.2.2-fb  31) Python/3.13.5
  5) numactl/2.0.19       14) PMIx/5.0.8           23) foss/2025b          32) libaec/1.1.4
  6) XZ/5.8.1             15) PRRTE/3.0.11         24) bzip2/1.0.8         33) Perl/5.40.2
  7) libxml2/2.14.3       16) UCC/1.4.4            25) ncurses/6.5         34) HDF5/1.14.6
  8) libpciaccess/0.18.1  17) OpenMPI/5.0.8        26) libreadline/8.2     35) DIRAC/25.0
  9) hwloc/2.12.1         18) OpenBLAS/0.3.30      27) libtommath/1.3.0

  Where:
   H:  Hidden Module


So, we see dependences listed plus DIRAC/25.0. Lets check which DIRAC versions are installed in the software stack: ``module spider DIRAC``

.. code-block:: julia

   ---------------------------------------------------------------------------------------------------------------
     DIRAC:
   ---------------------------------------------------------------------------------------------------------------
     Description:
      DIRAC: Program for Atomic and Molecular Direct Iterative Relativistic All-electron Calculations

     Versions:
        DIRAC/25.0
        DIRAC/26.0

   ---------------------------------------------------------------------------------------------------------------
   For detailed information about a specific "DIRAC" package (including how to load the modules) use the module's
   name. For example:

     $ module spider DIRAC/26.0


Let's follow the suggestion using ``module spider DIRAC/26.0`` obtaining

.. code-block:: julia

   ---------------------------------------------------------------------------------------------------------------
      DIRAC: DIRAC/26.0
   ---------------------------------------------------------------------------------------------------------------
      Description:
        DIRAC: Program for Atomic and Molecular Direct Iterative Relativistic All-electron Calculations

      You will need to load all module(s) on any one of the lines below before the "DIRAC/26.0" module is available 
        to load

      GCC/14.3.0  OpenMPI/5.0.8
 
    Help:
      Description
      ===========
      DIRAC: Program for Atomic and Molecular Direct Iterative Relativistic All-electron Calculations
      
      
      More information
      ================
       - Homepage: http://www.diracprogram.org

So, just follow the suggestion ``module load GCC/14.3.0 OpenMPI/5.0.8 DIRAC/26.0``.


3.2 Intel-Compilers Based Modules
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Similar procedure to what has been outlined above applies for software using the Intel compilers, MKL, and MPI. At the entering level if the user executes ``module av`` obtains 

.. code-block:: julia

  ----------------------------------- /mnt/beegfs/appsx/modules/all/Core -----------------------------------
    GCC/14.3.0            foss/2026.1  (D)    iimpi/2025b                     intel-compilers/2025.3.3 (D)
    GCC/15.2.0     (D)    gfbf/2025b          iimpi/2026.1             (D)    intel/2025b
    GCCcore/14.3.0        gfbf/2026.1  (D)    imkl/2025.2.0                   intel/2026.1             (D)
    GCCcore/15.2.0 (D)    gompi/2025b         imkl/2025.3.1            (D)    site/langs
    foss/2025b            gompi/2026.1 (D)    intel-compilers/2025.2.0

   Where:
     Aliases:  Aliases exist: foo/1.2.3 (1.2) means that "module load foo/1.2" will load foo/1.2.3
     D:        Default Module


After loading intel/2025b or iimpi/2025b (``module load intel/2025b`` or ``module load iimpi/2025a``) ``module list`` shows

.. code-block:: julia

   Currently Loaded Modules:
    1) GCCcore/14.3.0   4) intel-compilers/2025.2.0   7) impi/2021.16.1      10) intel/2025b
    2) zlib/1.3.1       5) numactl/2.0.19             8) imkl/2025.2.0
    3) binutils/2.44    6) UCX/1.19.0                 9) imkl-FFTW/2025.2.0


and ``module av`` displays

.. code-block:: julia

  ------------------- /mnt/beegfs/appsx/modules/all/MPI/intel/2025.2.0/impi/2021.16.1 -------------------
    HDF5/1.14.6    OSU-Micro-Benchmarks/7.5.1    Wannier90/3.1.0     imkl-FFTW/2025.2.0 (L)
    HPL/2.3        Score-P/9.4                   buildenv/default

  ------------------------ /mnt/beegfs/appsx/modules/all/Compiler/intel/2025.2.0 ------------------------
    impi/2021.16.1 (L)

  ------------------------ /mnt/beegfs/appsx/modules/all/Compiler/GCCcore/14.3.0 ------------------------
    ATK/2.38.0                          PostgreSQL/17.5                     libarchive/3.8.1
    Abseil/20250512.1                   PyYAML/6.0.2                        libcerf/3.0
    Autoconf/2.72                       Python-bundle-PyPI/2025.07          libclc/20.1.8
    Automake/1.18                       Python/3.13.5                       libde265/1.0.16
    Autotools/20250527                  Qhull/2020.2                        libdeflate/1.24
    Bison/3.8.2                         Qt6/6.9.3                           libdrm/2.4.125
    Blosc/1.21.6                        RE2/2025-07-22                      libepoxy/1.5.10
    Blosc2/2.19.0                       RapidJSON/1.1.0-20250205            libevent/2.1.12
    Brotli/1.1.0                        Redis/8.2.2                         libfabric/2.1.0
    Brunsli/0.1                         Rust/1.88.0                         libffi/3.5.1
    CFITSIO/4.6.2                       SDL2/2.32.10                        libgd/2.3.3
    CGAL/6.0.1                          SIONlib/1.7.7-tools                 libgeotiff/1.7.4
    CMake/3.31.8                        SQLAlchemy/2.0.41                   libgit2/1.9.1
    CMake/4.0.3                  (D)    SQLite/3.50.1                       libglvnd/1.7.0
    Catch2/2.13.10                      SVT-AV1/3.1.2                       libheif/1.20.2
    ...

On the top row the software compiled against Intel MPI (which is MPICH compiled with Intel compilers) is displayed followed by the software compiled with Intel C, C++ and Fortran compilers. On the bottom row the software compiled with GCC/14.3.0 as a backend is displayed.

The user can change to GCC based modules, e.g., to the foss/2025b toochain, by issuing ``module load foss/2025b`` obtaining with `module list`

.. code-block:: julia

  Currently Loaded Modules:
    1) GCCcore/14.3.0             8) imkl/2025.2.0        15) hwloc/2.12.1         22) OpenMPI/5.0.8
    2) zlib/1.3.1                 9) imkl-FFTW/2025.2.0   16) OpenSSL/3       (H)  23) OpenBLAS/0.3.30
    3) binutils/2.44             10) intel/2025b          17) libevent/2.1.12      24) FlexiBLAS/3.4.5
    4) intel-compilers/2025.2.0  11) GCC/14.3.0           18) libfabric/2.1.0      25) FFTW/3.3.10
    5) numactl/2.0.19            12) XZ/5.8.1             19) PMIx/5.0.8           26) FFTW.MPI/3.3.10
    6) UCX/1.19.0                13) libxml2/2.14.3       20) PRRTE/3.0.11         27) ScaLAPACK/2.2.2-fb
    7) impi/2021.16.1            14) libpciaccess/0.18.1  21) UCC/1.4.4            28) foss/2025b

  Where:
   H:  Hidden Module


4. Loading a Particular Software Modules
----------------------------------------

4.1 Julia
~~~~~~~~~

Julia is a high-level, high-performance dynamic programming language designed for
numerical and scientific computing, combining the productivity of Python with the
performance of compiled languages like C and Fortran. On OBLIVION, Julia is not
part of the default module view; it is available through the language ecosystems
tree. To access it, first load the languages meta-module and then the desired
Julia version:

.. code-block:: julia

    module load site/langs
    module load Julia/1.12.7

or in just a single line command ``module load site/langs Julia/1.12.7``.

Lets see what is loaded using ``module list``:

.. code-block:: julia
  
  Currently Loaded Modules:
    1) site/langs   2) Julia/1.12.7

Two versions are currently provided (1.12.6 and 1.12.7, with the newest marked as
default). The user can learn on these versions by using ``module spider Julia`` or module spider julia``.

Note that Julia's package manager (Pkg) is available directly from the REPL or via
`julia --project` workflows; users are encouraged to install packages into their
own project environments under their home or project directories, since the
central Julia installation is read-only. 

For MPI-parallel Julia workloads, load a toolchain (e.g. `foss/2025b`) before 
loading Julia so that MPI.jl can pick up the system OpenMPI libraries. 

for instance the PAMNEI (Parallel Adaptive Mesh Refinement MHD Multispecies Non-Equilibrium Ionization; de Avillez+ 2026) code is a Julia based plasma astrophysics code that uses MPI (either in OpenMPI or MPICH flavours), HDF5, NetCDF, and VTK data formats and ADIOS2 (ADaptable I/O System 2) which is an I/O framework/middleware for HPC that includes its own format (BP). So, to run this code one needs to load the different modules in the submission scripts or in an interactive session:

.. code-block:: julia

  module purge
  module load site/langs Julia/1.12.7
  module load GCC/14.3.0
  module load OpenMPI/5.0.8
  module load HDF5/1.14.6
  module load netCDF/4.9.3
  module load ADIOS2/2.10.2

Here we start by purging the modules and then load the needed modules. Note thta to load Julia/1.12.7 we load the module site/langs.

4.2 mpi4py
~~~~~~~~~~

Lets load mpi4py to test the communication between different cores and compute nodes. First we need to find if mpi4py is available by using ``module spider mpi4py``:

.. code-block:: julia

  ---------------------------------------------------------------------------------------------------------
    mpi4py:
  ---------------------------------------------------------------------------------------------------------
     Versions:
        mpi4py/4.1.0
        mpi4py/4.1.0 (E)
        mpi4py/4.1.2
        mpi4py/4.1.2 (E)

   Names marked by a trailing (E) are extensions provided by another module.
   ...
   
   For detailed information about a specific "mpi4py" package (including how to load the modules) use the 
    module's full name.

   For example:

     $ module spider mpi4py/4.1.2

Lets check mpi4py/4.1.0 (``module spider mpi4py/4.1.0``):

.. code-block:: julia

  ---------------------------------------------------------------------------------------------------------
    mpi4py: mpi4py/4.1.0
  ---------------------------------------------------------------------------------------------------------
    Description:
      MPI for Python (mpi4py) provides bindings of the Message Passing Interface (MPI) standard for the
      Python programming language, allowing any Python program to exploit multiple processors.

    You will need to load all module(s) on any one of the lines below before the "mpi4py/4.1.0" module is 
      available to load.

      GCC/14.3.0  OpenMPI/5.0.8

    This extension is provided by the following modules. To access the extension you must load one of the 
      following modules. Note that any module names in parentheses show the module location in the software 
      hierarchy.

       mpi4py/4.1.0 (GCC/14.3.0 OpenMPI/5.0.8)

So, clearly we need to load GCC/14.3.0 and OpenMPI/5.0.8 before loading mpi4py/4.1.0: 

.. code-block:: julia

   module load GCC/14.3.0 OpenMPI/5.0.8 mpi4py/4.1.0

Lets check what is loaded (``module list``):

.. code-block:: julia

  Currently Loaded Modules:
    1) GCCcore/14.3.0   8) libpciaccess/0.18.1      15) PRRTE/3.0.11      22) Tcl/9.0.1
    2) zlib/1.3.1       9) hwloc/2.12.1             16) UCC/1.4.4         23) SQLite/3.50.1
    3) binutils/2.44   10) OpenSSL/3           (H)  17) OpenMPI/5.0.8     24) libffi/3.5.1
    4) GCC/14.3.0      11) libevent/2.1.12          18) bzip2/1.0.8       25) Python/3.13.5
    5) numactl/2.0.19  12) UCX/1.19.0               19) ncurses/6.5       26) mpi4py/4.1.0
    6) XZ/5.8.1        13) libfabric/2.1.0          20) libreadline/8.2
    7) libxml2/2.14.3  14) PMIx/5.0.8               21) libtommath/1.3.0

  Where:
    H:  Hidden Module

There it is, mpi4py/4.1.0 is loaded. Now it can be used, say in a file named hello_mpi4py.py

.. code-block:: python

  from mpi4py import MPI
  comm = MPI.COMM_WORLD
  rank = comm.Get_rank()
  print('hello world from process', rank)

which is used to test the MPI communication between cores through the submission script 

.. code-block:: julia
   
  #!/bin/bash
  #SBATCH --time=00-00:05:00
  #SBATCH --account=<ACCOUNT NAME>
  #SBATCH --job-name=hello_mpi4py
  #SBATCH --output=%x_%j.out
  #SBATCH --error=%x_%j.error
  #SBATCH --ntasks=512
  #SBATCH --cpus-per-task=1
  #SBATCH --ntasks-per-socket=18
  #SBATCH --exclusive
  #SBATCH --partition=debug

  export PMIX_MCA_psec=native

  # Load software
  module purge
  module load GCC/14.3.0 OpenMPI/5.0.8 mpi4py/4.1.0

  # Run python script
  srun python hello_mpi4py.py

Details of this script are presented below. What matters here, for the sake of the discussion, is the use of the module and how it can be included into a parallel communication test script.

The result of the above submission is given in file named ``hello_mpi4py_65368.out`` (resulting from the directive ``--output=%x_%j.out`` in the script; ``%j`` is the job number):

.. code-block:: julia
  
  hello world from process 252
  hello world from process 185
  hello world from process 209
  hello world from process 341
  hello world from process 75
  hello world from process 377
  hello world from process 467
  hello world from process 117

Et voilà, the cores replied and said "hello" to the world.


4.3 scipy, numpy, numexpr, pandas
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

These packages, as well as others, are included in the module scipy-bundle. Therefore, the user needs to follow the following procedure to use these packages: (i) decide which toolchain to use (foss or intel). 

First check which scipy versions are available using ``module spider scipy`` obtaining

.. code-block:: julia

  --------------------------------------------------------------------------------------------------
    scipy:
  --------------------------------------------------------------------------------------------------
     Versions:
        scipy/1.16.1 (E)
        scipy/1.17.1 (E)
     Other possible modules matches:
        SciPy-bundle
 
  ---------------------------------------------------------------------------------------------------------
    To find other possible module matches execute:

      $ module -r spider '.*scipy.*'

  ---------------------------------------------------------------------------------------------------------
    For detailed information about a specific "scipy" package (including how to load the modules) use the 
    module's full name.
    
    For example:

     $ module spider scipy/1.17.1
  ---------------------------------------------------------------------------------------------------------

So, lets try ``module spider scipy/1.17.1``

.. code-block:: julia

  --------------------------------------------------------------------------------------------------
    scipy: scipy/1.17.1 (E)
  --------------------------------------------------------------------------------------------------
    To access the extension you must load one of the following modules. Note that any module names in 
    parentheses show the module location in the software hierarchy.

       SciPy-bundle/2026.05 (GCC/15.2.0)

Now we know that for scipy/1.17.1 we need to load SciPy-bundle/2026.05 (from May 2026) that depdends on GCC/15.2.0. 

Lets check for numpy. Which package includes it? The solution is given by ``module spider numpy``:

.. code-block:: julia

  ---------------------------------------------------------------------------------------------------------
    numpy:
  ---------------------------------------------------------------------------------------------------------
     Versions:
        numpy/2.3.2 (E)
        numpy/2.4.6 (E)

    Names marked by a trailing (E) are extensions provided by another module.

  ---------------------------------------------------------------------------------------------------------
    For detailed information about a specific "numpy" package (including how to load the modules) use the 
    module's full name.
  
    For example:

     $ module spider numpy/2.4.6
  ---------------------------------------------------------------------------------------------------------

Lets check what spider tells us about numpy/2.4.6. Use ``module spider numpy/2.4.6`` obtaining

.. code-block:: julia

  ---------------------------------------------------------------------------------------------------------
    numpy: numpy/2.4.6 (E)
  ---------------------------------------------------------------------------------------------------------
    This extension is provided by the following modules. To access the extension you must load one of the 
    following modules. Note that any module names in parentheses show the module location in the software 
    hierarchy.

       SciPy-bundle/2026.05 (GCC/15.2.0)

So, now we know that to haver access to scipy and numpy we need to load either load GCC/15.2.0 or the toochain that includes it, e.g., gompi/2026.1 or foss/2026.1.

First clear the modules from your environment using ``module purge``, then ``module load foss/2026.1  SciPy-bundle/2026.05``. 

Check the modules that were loaded (``module list``):

.. code-block:: julia

  Currently Loaded Modules:
    1) GCCcore/15.2.0         13) libfabric/2.5.0     25) libreadline/8.3      37) virtualenv/21.3.0
    2) zlib/2.3.2             14) PMIx/6.1.0          26) libtommath/1.3.0     38) psutil/7.2.1
    3) binutils/2.45          15) PRRTE/4.1.0         27) Tcl/9.0.3            39) coverage/7.13.5
    4) GCC/15.2.0             16) UCC/1.7.0           28) SQLite/3.51.1        40) tibs/0.5.7
    5) numactl/2.0.19         17) OpenMPI/5.0.10      29) XZ/5.8.2             41) Pygments/2.20.0
    6) libxml2/2.15.1         18) OpenBLAS/0.3.32     30) libffi/3.5.2         42) watchfiles/1.1.1
    7) libpciaccess/0.19      19) FlexiBLAS/3.5.0     31) gzip/1.14            43) BeautifulSoup/4.14.3
    8) ncurses/6.6            20) FFTW/3.3.10         32) lz4/1.10.0           44) libyaml/0.2.5
    9) hwloc/2.13.0           21) FFTW.MPI/3.3.10     33) zstd/1.5.7           45) PyYAML/6.0.3
   10) OpenSSL/3         (H)  22) ScaLAPACK/2.2.2-fb  34) Python/3.14.2        46) rpds-py/0.30.0
   11) libevent/2.1.12        23) foss/2026.1         35) cffi/2.0.0           47) Python-bundle-PyPI/2026.04
   12) UCX/1.20.0             24) bzip2/1.0.8         36) cryptography/47.0.0  48) SciPy-bundle/2026.05

  Where:
   H:  Hidden Module

Now the user can use, for example, scipy or numpy in their submission scripts.


4.4 GROMACS
~~~~~~~~~~~

In OBLIVION there are several versions of GROMACS compiled with/without PLUMED. First determine the GROMACS versions that are available using `module spider gromacs`

.. code-block:: julia

  ...
  Versions:
        GROMACS/2025.4
        GROMACS/2026.2


Check GROMACS/2026.2 using `module spider GROMACS/2026.2`

.. code-block:: julia

   Description:
      GROMACS is a versatile package to perform molecular dynamics, i.e. simulate the Newtonian
      equations of motion for systems with hundreds to millions of particles. This is a CPU only
      build, containing both MPI and threadMPI binaries for both single and double precision. It
      also contains the gmxapi extension for the single precision MPI build. 

      You will need to load all module(s) on any one of the lines below before the "GROMACS/2026.2" module is available to load.

      GCC/14.3.0  OpenMPI/5.0.8

So, follow the instruction: `module load GCC/14.3.0  OpenMPI/5.0.8 GROMACS/2026.2` and check what was loaded with `module list`

.. code-block:: julia

  Currently Loaded Modules:
    1) GCCcore/14.3.0       14) PMIx/5.0.8          27) Tcl/9.0.1
    2) zlib/1.3.1           15) PRRTE/3.0.11        28) SQLite/3.50.1
    3) binutils/2.44        16) UCC/1.4.4           29) libffi/3.5.1
    4) GCC/14.3.0           17) OpenMPI/5.0.8       30) Python/3.13.5
    5) numactl/2.0.19       18) OpenBLAS/0.3.30     31) cffi/1.17.1
    6) XZ/5.8.1             19) FlexiBLAS/3.4.5     32) cryptography/45.0.5
    7) libxml2/2.14.3       20) FFTW/3.3.10         33) virtualenv/20.32.0
    8) libpciaccess/0.18.1  21) FFTW.MPI/3.3.10     34) Python-bundle-PyPI/2025.07
    9) hwloc/2.12.1         22) ScaLAPACK/2.2.2-fb  35) SciPy-bundle/2025.07
   10) OpenSSL/3            23) bzip2/1.0.8         36) networkx/3.5
   11) libevent/2.1.12      24) ncurses/6.5         37) mpi4py/4.1.0
   12) UCX/1.19.0           25) libreadline/8.2     38) GROMACS/2026.2
   13) libfabric/2.1.0      26) libtommath/1.3.0



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
        GROMACS/2021.5-PLUMED-2.8.1
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

      You will need to load all module(s) on any one of the lines below before the "GROMACS/2021.5" module 
      is available to load.

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

For the full list of installed modules see the :ref:`Installed Software section <Installed Software>`.
