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

- foss: 2025b, 2026.1;
- intel: 2025b, 2026.1.
 
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
...

The list displays the toolchains (foss and intel) and the sub-toolchains (GCC, gompi, iompi, intel-compilers, iimpi, and imkl) availables to the users. It also displays site/langs that includes the modules of packages not built but downloaded into the stack (e.g., Anaconda, Julia, Perl, Miniconda, etc...)

In order to see the contents of site/langs run the command ``module load site/langs && module av`` obtaining

.. code-block:: julia
  ---------------------------------- /mnt/beegfs/appsx/modules/all/langs -----------------------------------
    Anaconda3/2025.12-1        Java/21.0.8  (21)    Julia/1.12.7        (D)    Miniforge3/25.3.0-3
    Anaconda3/2026.07-1 (D)    Julia/1.12.6         Miniconda3/25.7.0-2        Perl/5.38.0
...

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
...


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
  -------------------------------- /mnt/beegfs/appsx/modules/all/Compiler/GCC/14.3.0 --------------------------------
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

 ------------------------------ /mnt/beegfs/appsx/modules/all/Compiler/GCCcore/14.3.0 ------------------------------
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
...

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
...

Not only OpenMPI is loaded, but also UCX, PMIx, etc., are loaded. UCX stands for Unified Communication X and is "an optimized production communication framework for modern, high-bandwidth and low-latency networks" (see https://github.com/openucx/ucx) meaning for infiniband. PMIx stands for "Process Management Interface - Exascale" and enables the interaction of MPI applications with Resource Managers like SLURM (see https://pmix.github.io).

We loaded ``OpenMPI/5.0.8`` and checked the list of loaded modules, but loading OpenMPI/5.0.8 module opens a new avenue to access all the packages compiled against OpenMPI/5.0.8 that can be seen by exe cuting the command ``module av``:

.. code-block:: julia

  --------------------- /mnt/beegfs/appsx/modules/all/MPI/GCC/14.3.0/OpenMPI/5.0.8 ----------------------
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

 -------------------------- /mnt/beegfs/appsx/modules/all/Compiler/GCC/14.3.0 --------------------------
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
```

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
...

No longer have access to OpenMPI/5.0.8 and associated frameworks. Let's check what is available (use ``module --nx av``)

.. code-block:: julia

  ------------------------------- /mnt/beegfs/appsx/modules/all/Compiler/GCC/15.2.0 -------------------------------
     AOCL-BLAS/5.2     Delly/2.0.0     (D)    Kokkos/5.1.1       SciPy-bundle/2026.05    matplotlib/3.10.9
     ASE/3.28.0        FFTW/3.3.10            MPICH/5.0.1        Seaborn/0.13.2          mrcfile/1.5.4
     Arrow/24.0.0      FlexiBLAS/3.5.0        OpenBLAS/0.3.32    Shapely/2.1.2           networkx/3.6.1
     BLIS/2.0          GEOS/3.14.1            OpenMPI/5.0.10     SuiteSparse/7.12.2      scikit-learn/1.8.0
     Biopython/1.87    GSL/2.8                Pysam/0.24.0       bokeh/3.10.0            spglib-python/2.7.0
     Delly/1.7.3       HTSlib/1.23.1          R/4.6.1            dask/2026.7.1

  ----------------------------- /mnt/beegfs/appsx/modules/all/Compiler/GCCcore/15.2.0 -----------------------------
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
   
 -------------------------------------- /mnt/beegfs/appsx/modules/all/Core ---------------------------------------
   GCC/14.3.0              foss/2025b         gompi/2026.1  (D)    intel-compilers/2025.2.0
   GCC/15.2.0     (L,D)    foss/2026.1 (D)    iimpi/2025b          intel-compilers/2025.3.3 (D)
   GCCcore/14.3.0          gfbf/2025b         iimpi/2026.1  (D)    intel/2025b
   GCCcore/15.2.0 (L,D)    gfbf/2026.1 (D)    imkl/2025.2.0        intel/2026.1             (D)
   binutils/2.45           gompi/2025b        imkl/2025.3.1 (D)    site/langs

  Where:
   L:  Module is loaded
   D:  Default Module
...

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

  --------------------------- /mnt/beegfs/appsx/modules/all/MPI/GCC/15.2.0/OpenMPI/5.0.10 ---------------------------
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
...

The user got access to  a new level the software hierarchy. Hence, having access to all the software that was compiled against OpenMPI-5.0.10, which in turn was compiled with GCC-15.2.0.


3.2 Foss/2025a Toolchain
~~~~~~~~~~~~~~~~~~~~~~~~

Accessing the software modules made available by loading GCC/14.2.0 and OpenMPI/5.0.3 can be done by just loading foss/2025a with the penalty of loading extra modules like BLIS, FFTW, FlexiBLAS, OpenBLAS, ScaLAPACK. let's check it. Start with ``module purge`` followed by ``module load foss/2025a`` and ``module list`` obtaining

.. code-block:: julia

   Currently Loaded Modules:
      1) GCCcore/14.2.0   7) libxml2/2.13.4       13) libfabric/2.0.0  19) FlexiBLAS/3.4.5
      2) zlib/1.3.1       8) libpciaccess/0.18.1  14) PMIx/5.0.6       20) FFTW/3.3.10
      3) binutils/2.42    9) hwloc/2.11.2         15) PRRTE/3.0.8      21) FFTW.MPI/3.3.10
      4) GCC/14.2.0      10) OpenSSL/3            16) UCC/1.3.0        22) ScaLAPACK/2.2.2-fb
      5) numactl/2.0.19  11) libevent/2.1.12      17) OpenMPI/5.0.7    23) foss/2025a
      6) XZ/5.6.3        12) UCX/1.18.0           18) OpenBLAS/0.3.29

The available modules are (use ``module --nx av``)

.. code-block:: julia

   -------------------------- /mnt/beegfs/apps/modules/all/MPI/GCC/14.2.0/OpenMPI/5.0.7 ---------------------------
      Armadillo/14.6.0              OSU-Micro-Benchmarks/7.5              Wannier90/3.1.0
      Biopython/1.85                OpenMM/8.3.0                          arpack-ng/3.9.1
      CDO/2.5.2                     PETSc/3.23.5                          buildenv/default
      Cartopy/0.24.1                PLUMED/2.9.4                          dtcmp/1.1.5
      DIRAC/25.0                    ParMETIS/4.0.3                        ecCodes/2.43.0
      ELPA/2025.01.002              PnetCDF/1.14.0                        h5py/3.14.0
      FFTW.MPI/3.3.10        (L)    PyStan/3.10.0                         ipyparallel/9.0.1
      Fiona/1.10.1                  PyTables/3.10.2                       libcircle/0.3
      ...
   ------------------------------- /mnt/beegfs/apps/modules/all/Compiler/GCC/14.2.0 -------------------------------
      AOCL-BLAS/5.0                    FlexiBLAS/3.4.5      (L)    Seaborn/0.13.2       networkx/3.5
      ASE/3.25.0                       GEOS/3.13.1                 Shapely/2.1.1        pybind11/2.13.6
      ASE/3.26.0                (D)    GSL/2.8                     bokeh/3.7.3          scikit-learn/1.7.0
      BLIS/1.1                         OpenBLAS/0.3.29      (L)    dask/2025.5.1        spglib-python/2.6.0
      Boost.Python-NumPy/1.88.0        OpenMPI/5.0.7        (L)    libxc/7.0.0
      Boost/1.88.0                     SAMtools/1.22.1             matplotlib/3.10.3
      FFTW/3.3.10               (L)    SciPy-bundle/2025.06        mrcfile/1.5.4
      ...
      
It is the same obtained previously by loading GCC/14.2.0 and OpenMPI/5.0.7.


3.2 Foss/2024a and 2023a Toolchains
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The foss/2024a and 2023a toolchains have the same software but with different versions being the latest versions (as off September 2025) in foss/2024a and foss/2025a. The latter toolchain includes only a subgroup of the software in foss/2024a and foss/2023a toolchains. 

Let us explore the foss/2024a toolchain.

Start with ``module purge`` followed with ``module load foss/2024a`` and ``module --nx av`` obtaining

.. code-block:: julia

   ------------------------------- /mnt/beegfs/apps/modules/all/MPI/GCC/13.3.0/OpenMPI/5.0.3 -------------------------------
      ABySS/2.3.10                           MDTraj/1.10.3                               YAXT/0.11.3
      AUGUSTUS/3.5.0                         MMseqs2/17-b804f                            YaHS/1.2.2
      Armadillo/14.0.3                       MUMPS/5.7.2-metis                           arpack-ng/3.9.1
      ArviZ/0.21.0                           MultiQC/1.28                                arrow-R/17.0.0.1-R-4.4.2
      BLAST+/2.16.0                          NAMD/3.0-mpi                                bayesian-optimization/2.0.3
      Biopython/1.84                         NCO/5.2.9                                   bcbio-gff/0.7.1
      Boost.MPI/1.85.0                       ORCA/6.0.1-avx2                             biom-format/2.1.16
      CDO/2.4.4                              ORCA/6.0.1                           (D)    buildenv/default
      CMSeq/1.0.4                            OSU-Micro-Benchmarks/7.4                    clisops/0.15.0
      Cartopy/0.24.1                         OpenFOAM/v2406                              cnvpytor/1.3.1
      Cbc/2.10.12                            OpenMM/8.3.0                                cp2k-input-tools/0.9.1
      Cgl/0.60.8                             Optuna/4.1.0                                dtcmp/1.1.5
      Clp/1.17.10                            PETSc/3.23.5                                ecCodes/2.38.3
      CodingQuarry/2.0                       PLUMED/2.9.3                                geopandas/1.0.1
      Critic2/1.2                            ParMETIS/4.0.3                              h5glance/0.9.0
      CrystFEL/0.11.1                        ParaView/5.13.2                             h5netcdf/1.6.1
      DIRAC/24.0                             PnetCDF/1.14.0                              h5py/3.12.1
      ELPA/2024.05.001                       PuLP/2.8.0                                  kallisto/0.51.1
      EMD/0.8.1                              PyMC/5.22.0                                 koopmans-kcp/0.2.0
      ESMF/8.7.0                             PyStan/3.10.0                               koopmans-qe-utils/0.1.0
      ESPResSo/4.2.2                         PyTables/3.10.2                             koopmans/1.1.0
      ETE/3.1.3                              PyVista/0.44.2                              libGridXC/2.0.2
      Extrae/4.2.5                           PyWavelets/1.8.0                            libcircle/0.3
      FFTW.MPI/3.3.10                 (L)    QuantumESPRESSO/7.4-minimal                 libvdwxc/0.4.0
      Fiona/1.10.1                           QuantumESPRESSO/7.4                  (D)    lwgrp/1.0.6
      GDAL/3.10.0                            R-bundle-Bioconductor/3.20-R-4.4.2          maeparser/1.3.1
      GOATOOLS/1.4.12                        R-bundle-CRAN/2024.11                       modin/0.32.0
      GPAW/24.6.0                            RDKit/2025.03.3                             mpi4py/4.0.1
      GPAW/25.1.0-ASE-3.24.0                 RStudio-Server/2024.12.0+467-R-4.4.2        mpifileutils/0.11.1
      GPAW/25.1.0-ASE-3.25.0          (D)    Ray-project/2.37.0                          ncbi-vdb/3.2.0
      HDF5/1.14.5                            Rmath/4.4.2                                 ncview/2.1.11
      HISAT2/2.2.1                           SCOTCH/7.0.6                                netCDF-C++4/4.3.1
      HMMER/3.4                              SRA-Toolkit/3.2.0                           netCDF-Fortran/4.6.1
      HPCG/3.1                               SUNDIALS/7.0.0                              netCDF/4.9.2
      HPL/2.3                                ScaFaCoS/1.0.4                              netcdf4-python/1.7.1.post2
      HPX/1.10.0                             ScaLAPACK/2.2.0-fb                   (L)    nf-core/2.14.1
      HeFFTe/2.4.1                           Score-P/8.4                                 nglview/3.1.4
      Hybpiper/2.3.2                         Siesta/5.4.0                                numba/0.60.0
      Hypre/2.32.0                           SuiteSparse/7.8.2-METIS-5.1.0               pathos/0.3.3
      IMB/2021.6                             SuiteSparse/7.10.1                   (D)    s3fs/2024.9.0
      Infernal/1.1.5                         SuperLU_DIST/9.1.0                          scikit-image/0.25.0
      JAGS/4.3.2                             TINKER/25.3                                 sisl/0.15.2
      KaHIP/3.16                             VASP/6.5.0                                  snakemake/8.27.0
      Kraken2/2.1.4                          VASP/6.5.1                           (D)    tRNAscan-SE/2.0.12
      KrakenTools/1.2                        VTK/9.3.1                                   xclim/0.55.1
      LAMMPS/29Aug2024_update2-kokkos        Valgrind/3.24.0                             zarr/2.18.4
      MDAnalysis/2.9.0                       Wannier90/3.1.0
      MDI/1.4.26                             XCrySDen/1.6.2

  ----------------------------------- /mnt/beegfs/apps/modules/all/Compiler/GCC/13.3.0 ------------------------------------
     ASE/3.23.0                       GEOS/3.12.2                        Simple-DFTD3/1.2.1    mrcfile/1.5.4
     ASE/3.24.0                       GSL/2.8                            TOML-Fortran/0.4.2    mstore/0.3.0
     ASE/3.25.0                (D)    HTSlib/1.21                        bokeh/3.6.0           multiprocess/0.70.17
     Arrow/17.0.0                     MAFFT/7.526-with-extensions        btllib/1.7.5          networkx/3.4.2
     BBMap/39.19                      MPICH/4.2.2                        dask/2024.9.1         pybind11/2.12.0
     BCFtools/1.21                    OpenBLAS/0.3.27             (L)    flook/0.8.4           scikit-learn/1.5.2
     BEDTools/2.31.1                  OpenMPI/5.0.3               (L)    imageio/2.36.1        spglib-python/2.5.0
     BLIS/1.0                         Osi/0.108.11                       json-fortran/9.0.3    statsmodels/0.14.4
     BamTools/2.5.2                   PyTensor/2.30.3                    kim-api/2.4.1         sympy/1.13.3
     Boost.Python-NumPy/1.85.0        Pysam/0.22.1                       libPSML/2.1.0         tensorboard/2.18.0
     Boost.Python/1.85.0              R/4.4.2                            libcint/6.1.2         test-drive/0.5.0
     Boost/1.85.0                     SAMtools/1.21                      libfdf/0.5.1          wrapt/1.16.0
     CoinUtils/2.11.12                SOCI/4.0.3                         libxc/6.2.2           xarray/2024.11.0
     DIAMOND/2.1.11                   SPAdes/4.1.0                       lpsolve/5.5.2.11      xmlf90/1.6.3
     Exonerate/2.4.0                  SciPy-bundle/2024.05               matplotlib/3.9.2
     FFTW/3.3.10               (L)    Seaborn/0.13.2                     mctc-lib/0.3.1
     FlexiBLAS/3.4.4           (L)    Shapely/2.0.6                      ml_dtypes/0.5.0

 --------------------------------- /mnt/beegfs/apps/modules/all/Compiler/GCCcore/13.3.0 ----------------------------------
    ANTLR/2.7.7                         Python-bundle-PyPI/2024.06           libepoxy/1.5.10
    ATK/2.38.0                          Python/3.12.3                        libevent/2.1.12              (L)
    Abseil/20240722.0                   Qhull/2020.2                         libfabric/1.21.0             (L)
    Autoconf/2.72                (D)    Qt5/5.15.16                          libffi/3.4.5
    Automake/1.16.5                     Qt6/6.7.2                            libgd/2.3.3
    Autotools/20231222                  RE2/2024-07-02                       libgeotiff/1.7.3
    BWA/0.7.18                          RapidJSON/1.1.0-20240815             libgit2/1.8.1
   ...
   
Most of the software, as can be seen in the top and middle rows, are similar to that available in foss/2023a and foss/2025a, but was compiled with a different version of GCC (13.3.0) and OpenMPI (5.0.3). Thel ist of packages includes well known and established software used by the quantum chemistry and material sciences communities, e.g., DIRAC, ELPA, Orca, Scafacos, VASP, QuantumEspresso, etc., and computational fluid dynamics (CFD), e.g., OpenFoam).

As an exercise lets load DIRAC using the command ``module load DIRAC/24.0`` or just ``module load DIRAC`` followed with ``module list`` obtaining

.. code-block:: julia

   Currently Loaded Modules:
     1) GCCcore/13.3.0       11) libevent/2.1.12   21) FFTW.MPI/3.3.10     31) Szip/2.1.1
     2) zlib/1.3.1           12) UCX/1.16.0        22) ScaLAPACK/2.2.0-fb  32) HDF5/1.14.5
     3) binutils/2.42        13) libfabric/1.21.0  23) foss/2024a          33) cffi/1.16.0
     4) GCC/13.3.0           14) PMIx/5.0.2        24) bzip2/1.0.8         34) cryptography/42.0.8
     5) numactl/2.0.18       15) PRRTE/3.0.5       25) ncurses/6.5         35) virtualenv/20.26.2
     6) XZ/5.4.5             16) UCC/1.3.0         26) libreadline/8.2     36) Python-bundle-PyPI/2024.06
     7) libxml2/2.12.7       17) OpenMPI/5.0.3     27) Tcl/8.6.14          37) SciPy-bundle/2024.05
     8) libpciaccess/0.18.1  18) OpenBLAS/0.3.27   28) SQLite/3.45.3       38) mpi4py/4.0.1
     9) hwloc/2.10.0         19) FlexiBLAS/3.4.4   29) libffi/3.4.5        39) h5py/3.12.1
    10) OpenSSL/3            20) FFTW/3.3.10       30) Python/3.12.3       40) DIRAC/24.0

So, we see dependences listed plus DIRAC/24.0. Lets check which DIRAC versions are installed in the software stack: ``module spider DIRAC``

.. code-block:: julia

   ---------------------------------------------------------------------------------------------------------------------
     DIRAC:
   ---------------------------------------------------------------------------------------------------------------------
     Description:
      DIRAC: Program for Atomic and Molecular Direct Iterative Relativistic All-electron Calculations

     Versions:
        DIRAC/23.0
        DIRAC/24.0
        DIRAC/25.0
        DIRAC/26.0

   ---------------------------------------------------------------------------------------------------------------------
   For detailed information about a specific "DIRAC" package (including how to load the modules) use the module's full 
   name. For example:

     $ module spider DIRAC/25.0

Let's follow the suggestion using ``module spider DIRAC/25.0`` obtaining

.. code-block:: julia

   ---------------------------------------------------------------------------------------------------------------------
      DIRAC: DIRAC/25.0
   ---------------------------------------------------------------------------------------------------------------------
      Description:
        DIRAC: Program for Atomic and Molecular Direct Iterative Relativistic All-electron Calculations

      You will need to load all module(s) on any one of the lines below before the "DIRAC/25.0" module is available to load

      GCC/14.2.0  OpenMPI/5.0.7
 
    Help:
      Description
      ===========
      DIRAC: Program for Atomic and Molecular Direct Iterative Relativistic All-electron Calculations
      
      
      More information
      ================
       - Homepage: http://www.diracprogram.org

So, now one only has to follow the suggestion ``module load GCC/14.2.0 OpenMPI/5.0.7 DIRAC/25.0``.

3.3 Intel-Compilers Based Modules
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Similar procedure to what has been outlined above applies for software using the Intel compilers, MKL, and MPI. At the entering level if the user executes ``module --nx av`` obtains 

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
  
      
After loading intel/2025a or iimpi/2025a (``module load intel/2025a`` or ``module load iimpi/2025a``) ``module list`` shows

.. code-block:: julia

   Currently Loaded Modules:
     1) GCCcore/14.2.0   4) intel-compilers/2025.1.1   7) impi/2021.15.0      10) intel/2025a
     2) zlib/1.3.1       5) numactl/2.0.19             8) imkl/2025.1.0
     3) binutils/2.42    6) UCX/1.18.0                 9) imkl-FFTW/2025.1.0

and ``module --nx av`` displays

.. code-block:: julia

  ------------------- /mnt/beegfs/apps/modules/all/MPI/intel/2025.1.1/impi/2021.15.0 -------------------
    HDF5/1.14.6    OSU-Micro-Benchmarks/7.5    Wannier90/3.1.0     imkl-FFTW/2025.1.0 (L)
    HPL/2.3        Score-P/9.2                 buildenv/default

  ------------------------ /mnt/beegfs/apps/modules/all/Compiler/intel/2025.1.1 ------------------------
    OpenMPI/5.0.7    impi/2021.15.0 (L)

  ------------------------ /mnt/beegfs/apps/modules/all/Compiler/GCCcore/14.2.0 ------------------------
    Abseil/20250512.1                   ecBuild/3.11.0              (D)
    Autoconf/2.72                (D)    elfutils/0.193
    Automake/1.17                       expat/2.6.4
    Autotools/20240712                  flex/2.6.4                  (D)
    BeautifulSoup/4.13.4                flit/3.10.1
    Bison/3.8.2                  (D)    fontconfig/2.16.2
    Blosc/1.21.6                        fonttools/4.58.4
    Blosc2/2.19.0                       freetype/2.13.3
    Brotli/1.1.0                        gettext/0.24
    ...

On the top row the software compiled against Intel MPI (which is MPICH compiled with Intel compilers) is displayed followed by the software compiled with Intel C, C++ and Fortran compilers. On the bottom row the software compiled with GCC/14.2.0 as a backend is displayed.

The user can change to GCC based modules, e.g., to the foss/2025a toochain, by issuing ``module load foss/2025a`` obtaining with `module list`

.. code-block:: julia

  Currently Loaded Modules:
    1) GCCcore/14.2.0             8) imkl/2025.1.0        15) hwloc/2.11.2     22) OpenMPI/5.0.7
    2) zlib/1.3.1                 9) imkl-FFTW/2025.1.0   16) OpenSSL/3        23) OpenBLAS/0.3.29
    3) binutils/2.42             10) intel/2025a          17) libevent/2.1.12  24) FlexiBLAS/3.4.5
    4) intel-compilers/2025.1.1  11) GCC/14.2.0           18) libfabric/2.0.0  25) FFTW/3.3.10
    5) numactl/2.0.19            12) XZ/5.6.3             19) PMIx/5.0.6       26) FFTW.MPI/3.3.10
    6) UCX/1.18.0                13) libxml2/2.13.4       20) PRRTE/3.0.8      27) ScaLAPACK/2.2.2-fb
    7) impi/2021.15.0            14) libpciaccess/0.18.1  21) UCC/1.3.0        28) foss/2025a


4. Loading a Particular Software
--------------------------------

4.1 scipy, mpi4py, numpy, numexpr, pandas
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

These packages, as well as others, are included in the module scipy-bundle. Therefore, the user needs to follow the following procedure to use these packages: (i) decide which toolchain to use (foss or intel). 

First check which scipy versions are available using `module spider scipy` obtaining

.. code-block:: julia

  --------------------------------------------------------------------------------------------------
    scipy:
  --------------------------------------------------------------------------------------------------
     Versions:
        scipy/1.11.1 (E)
        scipy/1.13.1 (E)
        scipy/1.16.0 (E)
        scipy/1.16.1 (E)
     Other possible modules matches:
        SciPy-bundle
   ...
   For example:

     $ module spider scipy/1.16.1
  --------------------------------------------------------------------------------------------------

So, lets try `module spider scipy/1.16.1`

. code-block:: julia

  --------------------------------------------------------------------------------------------------
    scipy: scipy/1.16.1 (E)
  --------------------------------------------------------------------------------------------------
    This extension is provided by the following modules. To access the extension you must load one of 
    the following modules. Note that any module names in parentheses show the module location in the softw
    are hierarchy.

       SciPy-bundle/2025.07 (GCC/14.3.0)

Now we know that for scipy/1.16.1 we need to load SciPy-bundle/2025.07 that depdens on GCC/14.3.0. Hence, either load GCC/14.3.0 or the toochain that includes it, e.g., gompi/2025b or foss/2025b.

First clear the modules from your environment using `module purge`, then 

.. code-block:: julia

  module load foss/2025b  SciPy-bundle/2025.07

Now check the modules that were loaded using `module list`

.. code-block:: julia

  Currently Loaded Modules:
   1) GCCcore/14.3.0       13) libfabric/2.1.0     25) ncurses/6.5
   2) zlib/1.3.1           14) PMIx/5.0.8          26) libreadline/8.2
   3) binutils/2.44        15) PRRTE/3.0.11        27) libtommath/1.3.0
   4) GCC/14.3.0           16) UCC/1.4.4           28) Tcl/9.0.1
   5) numactl/2.0.19       17) OpenMPI/5.0.8       29) SQLite/3.50.1
   6) XZ/5.8.1             18) OpenBLAS/0.3.30     30) libffi/3.5.1
   7) libxml2/2.14.3       19) FlexiBLAS/3.4.5     31) Python/3.13.5
   8) libpciaccess/0.18.1  20) FFTW/3.3.10         32) cffi/1.17.1
   9) hwloc/2.12.1         21) FFTW.MPI/3.3.10     33) cryptography/45.0.5
  10) OpenSSL/3            22) ScaLAPACK/2.2.2-fb  34) virtualenv/20.32.0
  11) libevent/2.1.12      23) foss/2025b          35) Python-bundle-PyPI/2025.07
  12) UCX/1.19.0           24) bzip2/1.0.8         36) SciPy-bundle/2025.07

Now the user can use, for example, mpi4py or numpy in their submission scripts.



4.3 GROMACS
~~~~~~~~~~~

In OBLIVION there are several versions of GROMACS compiled with/without PLUMED. First determine the GROMACS versions that are available using `module spider gromacs`

.. code-block:: julia

  ...
  Versions:
        GROMACS/2023.3-PLUMED-2.9.0
        GROMACS/2023.4
        GROMACS/2024.4
        GROMACS/2025.2
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

For the full list of installed modules see the :ref:`installed software section <Installed Software>`.
