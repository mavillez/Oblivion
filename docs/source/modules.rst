Environment Modules
===================

There are several conflicting software packages installed in the OBLIVION supercomputer. In order for the user to use the adequate software there is a need to set the paths for the binaries, libraries, manuals, and includes. Either the user sets these paths for each package or the system provides an easy way to set those paths. The latter is the preferable and makes use of environment modules. 

1. Toolchains
-------------

1.1 Definitions
~~~~~~~~~~~~~~~

Toolchain is a pack of compiler(s) and libraries bundled together to provide a specific functionality, say, running applications using a MPI distribution compiled against GCC (toolchain gompi) or Intel compilers (toolchain iimpi) or use linear algebra libraries with a MPI API compiled with GCC (toolchain foss).

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

1.2 Toolchains and Sub-toolchains installed in OBLIVION
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Toolchains:

- foss: 2023a, 2024a, 2025a;
- intel: 2023a, 2024a, 2025a.
 
Sub-toolchains:

- gompi: 2023a, 2024a, 2025a.
- iimpi: 2023a, 2024a, 2025a.
- iompi: 2023a, 2024a, 2025a.
- intel-compilers: 2023.1.0, 2024.2.0, 2025.1.1.
- imkl: 2023.1.0, 2023.2.0, 2024.2.0, 2025.1.0.


2. Core Modules
---------------

The user sets the software environment by loading the modules associated to the needed packages. This is easily done by using ``module load`` or ``module add``. Software dependences are set in the same way. OBLIVION uses a hierarchical module naming scheme (HMNS) in which modules availability follows the software hierarchy Core/Compiler/MPI.

Core refers to the basic core modules that have to be loaded in order to have access to next levels of software compiled against a specific compiler (e.g., ``GCC 12.3.0, 13.3.0, or 14.2.0; intel-compilers 2023.1.0, 2024.2.0 or 2025.1.1``) and a MPI API (e.g., OpenMPI 4.1.5, 5.0.3, 5.0.7; MPICH 4.2.1, 4.2.2; Intel MPI 2021.9.0, 2021.13.0, or 2021.15.0 - the latter were compiled with intel-compilers 2023.1.0, 2024.2.0, 2025.1.1, respectively). It also includes modules of software that i) are initially compiled with the system/machine compiler (e.g., binutils, gettext, M4, ncurses, pkgconf, zlib), and ii) are not being built but instead are directly installed into the stack (e.g., Anaconda, ANSYS_CFD).

After logging into the machine the user should execute the command ``module --nx av`` (av for available software, --nx to prevent the display of the extensions list) obtaining the list of core modules (including the toolchains and sub-toolchains):

.. code-block:: julia

   -------------------------------- /mnt/beegfs/apps/modules/all/Core ---------------------------------
     Anaconda3/2025.06-1                  OpenSSL/3           (D)    iimkl/2025a              (D)
     Autoconf/2.71                        Perl/5.38.0                iimpi/2023a
     Bison/3.8.2                          ant/1.10.12-Java-17        iimpi/2024a
     FastQC/0.11.9-Java-11                ant/1.10.14-Java-11 (D)    iimpi/2025a              (D)
     GCC/12.3.0                           binutils/2.40              imkl/2023.1.0
     GCC/13.3.0                           binutils/2.42       (D)    imkl/2023.2.0
     GCC/14.2.0                 (D)       ecBuild/3.8.0              imkl/2024.2.0
     GCCcore/12.3.0                       ffnvcodec/12.0.16.0        imkl/2025.1.0            (D)
     GCCcore/13.3.0                       ffnvcodec/12.1.14.0        intel-compilers/2023.1.0
     GCCcore/14.2.0             (D)       ffnvcodec/12.2.72.0 (D)    intel-compilers/2024.2.0
     GPAW-setups/24.1.0                   flex/2.6.4                 intel-compilers/2025.1.1 (D)
     GPAW-setups/24.11.0        (D)       foss/2023a                 intel/2023a
     IJulia/1.29.0-Julia-1.11.6           foss/2024a                 intel/2024a
     Java/11.0.27               (11)      foss/2025a          (D)    intel/2025a              (D)
     Java/17.0.15               (D:17)    gettext/0.21.1             iompi/2023a
     Julia/1.11.3-linux-x86_64            gettext/0.22               iompi/2024a
     Julia/1.11.6-linux-x86_64  (D)       gettext/0.22.5      (D)    iompi/2025a              (D)
     M4/1.4.18                            gfbf/2023a                 ncurses/6.3
     M4/1.4.19                  (D)       gfbf/2024a                 ncurses/6.4
     Mamba/23.11.0-0                      gfbf/2025a          (D)    ncurses/6.5              (D)
     Miniconda3/24.7.1-0                  gompi/2023a                pkgconf/1.8.0
     Miniconda3/25.5.1-1        (D)       gompi/2024a                zlib/1.2.13
     Miniforge3/24.11.3-0                 gompi/2025a         (D)    zlib/1.3.1               (D)
     Miniforge3/25.3.0-1        (D)       iimkl/2023a
     OpenSSL/1.1                          iimkl/2024a

    Where:
     Aliases:  Aliases exist: foo/1.2.3 (1.2) means that "module load foo/1.2" will load foo/1.2.3
     D:        Default Module

    
The list displays the toolchains (foss and intel) and the sub-toolchains (GCC, gompi, iompi, intel-compilers, iimpi, and imkl) availables to the users. It also displays software that do not belong to these categories and is available in the system, e.g., ANACONDA, JAVA, Julia, etc..


3. Loading Modules
------------------

3.1 GCC Based Modules
~~~~~~~~~~~~~~~~~~~~~

Let us assume that the user wants to use software compiled with GCC-13.3.0 he must load the corresponding modules

.. code-block:: julia

  module load GCC/13.3.0

To learn the loaded modules use

.. code-block:: julia

  module list

obtaining

.. code-block:: julia

   Currently Loaded Modules:
      1) GCCcore/13.3.0   2) zlib/1.3.1   3) binutils/2.42   4) GCC/13.3.0

Loading the module GCC/13.3.0 gives access to other modules that only now became available. To see those modules use "module --nx av" obtaining

.. code-block:: julia

  --------------------------------- /mnt/beegfs/apps/modules/all/Compiler/GCC/13.3.0 ---------------------------------
     ASE/3.23.0                       GEOS/3.12.2                    Simple-DFTD3/1.2.1    mrcfile/1.5.4
     ASE/3.24.0                       GSL/2.8                        TOML-Fortran/0.4.2    mstore/0.3.0
     ASE/3.25.0                (D)    HTSlib/1.21                    bokeh/3.6.0           multiprocess/0.70.17
     Arrow/17.0.0                     MAFFT/7.526-with-extensions    btllib/1.7.5          networkx/3.4.2
     BBMap/39.19                      MPICH/4.2.2                    dask/2024.9.1         pybind11/2.12.0
     BCFtools/1.21                    OpenBLAS/0.3.27                flook/0.8.4           scikit-learn/1.5.2
     BEDTools/2.31.1                  OpenMPI/5.0.3                  imageio/2.36.1        spglib-python/2.5.0
     BLIS/1.0                         Osi/0.108.11                   json-fortran/9.0.3    statsmodels/0.14.4
     BamTools/2.5.2                   PyTensor/2.30.3                kim-api/2.4.1         sympy/1.13.3
     Boost.Python-NumPy/1.85.0        Pysam/0.22.1                   libPSML/2.1.0         tensorboard/2.18.0
     Boost.Python/1.85.0              R/4.4.2                        libcint/6.1.2         test-drive/0.5.0
     Boost/1.85.0                     SAMtools/1.21                  libfdf/0.5.1          wrapt/1.16.0
     CoinUtils/2.11.12                SOCI/4.0.3                     libxc/6.2.2           xarray/2024.11.0
     DIAMOND/2.1.11                   SPAdes/4.1.0                   lpsolve/5.5.2.11      xmlf90/1.6.3
     Exonerate/2.4.0                  SciPy-bundle/2024.05           matplotlib/3.9.2
     FFTW/3.3.10                      Seaborn/0.13.2                 mctc-lib/0.3.1
     FlexiBLAS/3.4.4                  Shapely/2.0.6                  ml_dtypes/0.5.0

  ------------------------------- /mnt/beegfs/apps/modules/all/Compiler/GCCcore/13.3.0 -------------------------------
     ANTLR/2.7.7                         Python-bundle-PyPI/2024.06           libepoxy/1.5.10
     ATK/2.38.0                          Python/3.12.3                        libevent/2.1.12
     Abseil/20240722.0                   Qhull/2020.2                         libfabric/1.21.0
     Autoconf/2.72                (D)    Qt5/5.15.16                          libffi/3.4.5
     Automake/1.16.5                     Qt6/6.7.2                            libgd/2.3.3
     Autotools/20231222                  RE2/2024-07-02                       libgeotiff/1.7.3
     BWA/0.7.18                          RapidJSON/1.1.0-20240815             libgit2/1.8.1
     Bazel/6.5.0-Java-11                 Redis/7.4.1                          libglvnd/1.7.0
     BeautifulSoup/4.12.3                Rust/1.78.0                          libheif/1.19.5
     Bison/3.8.2                  (D)    SDL2/2.30.6                          libiconv/1.17
     Blosc/1.21.6                        SIONlib/1.7.7-tools                  libidn2/2.3.8
     Blosc2/2.17.0                       SIP/6.10.0                           libjpeg-turbo/3.0.1
     Brotli/1.1.0                        SQLAlchemy/2.0.36                    libogg/1.3.5
     Brunsli/0.1                         SQLite/3.45.3                        libopus/1.5.2
     CFITSIO/4.4.1                       SWIG/4.2.1                           libpciaccess/0.18.1
     CGAL/5.6.1                          Szip/2.1.1                           libpng/1.6.43
     CMake/3.29.3                        Tcl/8.6.14                           libreadline/8.2
     ...

  ---------------------------------------- /mnt/beegfs/apps/modules/all/Core -----------------------------------------
     Anaconda3/2025.06-1                  OpenSSL/3           (D)    iimkl/2025a              (D)
     Autoconf/2.71                        Perl/5.38.0                iimpi/2023a
     Bison/3.8.2                          ant/1.10.12-Java-17        iimpi/2024a
     FastQC/0.11.9-Java-11                ant/1.10.14-Java-11 (D)    iimpi/2025a              (D)
     GCC/12.3.0                           binutils/2.40              imkl/2023.1.0
     GCC/13.3.0                 (L)       binutils/2.42              imkl/2023.2.0
     GCC/14.2.0                 (D)       ecBuild/3.8.0              imkl/2024.2.0
     GCCcore/12.3.0                       ffnvcodec/12.0.16.0        imkl/2025.1.0            (D)
     GCCcore/13.3.0             (L)       ffnvcodec/12.1.14.0        intel-compilers/2023.1.0
     GCCcore/14.2.0             (D)       ffnvcodec/12.2.72.0 (D)    intel-compilers/2024.2.0
     GPAW-setups/24.1.0                   flex/2.6.4                 intel-compilers/2025.1.1 (D)
     GPAW-setups/24.11.0        (D)       foss/2023a                 intel/2023a
     IJulia/1.29.0-Julia-1.11.6           foss/2024a                 intel/2024a
     Java/11.0.27               (11)      foss/2025a          (D)    intel/2025a              (D)
     Java/17.0.15               (D:17)    gettext/0.21.1             iompi/2023a
     ...

  Where:
   L:        Module is loaded
   D:        Default Module

Here one can see (from bottom to top) the list of sytem Core modules indicating those loaded with **(L)**, followed by the general software compiled with GCC-13.3.0 becoming the core software (GCCcore) against which all the packages are compiled, and, the software compiled using the GCCcore software, e.g., MPICH and OpenMPI-5.0.3. Note that GCC-13.3.0 is the base compilers collection for 2024a toolchains.

To have access to software compiled with OpenMPI-5.0.3, the user needs to use ``module load OpenMPI/5.0.3``. The list of packages loaded is now given by ``module list``:

.. code-block:: julia

   Currently Loaded Modules:
      1) GCCcore/13.3.0   6) XZ/5.4.5             11) libevent/2.1.12   16) UCC/1.3.0
      2) zlib/1.3.1       7) libxml2/2.12.7       12) UCX/1.16.0        17) OpenMPI/5.0.3
      3) binutils/2.42    8) libpciaccess/0.18.1  13) libfabric/1.21.0
      4) GCC/13.3.0       9) hwloc/2.10.0         14) PMIx/5.0.2
      5) numactl/2.0.18  10) OpenSSL/3            15) PRRTE/3.0.5

Now, not only OpenMPI is loaded, but also UCX, PMIx, etc., are loaded. UCX stands for Unified Communication X and is "an optimized production communication framework for modern, high-bandwidth and low-latency networks" (see https://github.com/openucx/ucx) meaning for infiniband. PMIx stands for Process Management Interface - Exascale and enables the interaction of MPI applications with Resource Managers like SLURM (see https://pmix.github.io)

Let us now change the enviromment to one using GCC-14.2.0. Hence, load the module GCC/14.2.0 (use ``module load GCC/14.2.0``) and immediately it is seen

.. code-block:: julia

   Inactive Modules:
     1) OpenMPI/5.0.3     3) PRRTE/3.0.5     5) hwloc/2.10.0         7) libxml2/2.12.7
     2) PMIx/5.0.2        4) XZ/5.4.5        6) libfabric/1.21.0

   Due to MODULEPATH changes, the following have been reloaded:
     1) UCC/1.3.0     2) binutils/2.42     3) libevent/2.1.12     4) libpciaccess/0.18.1     5) zlib/1.3.1

   The following have been reloaded with a version change:
     1) GCC/13.3.0 => GCC/14.2.0             3) UCX/1.16.0 => UCX/1.18.0
     2) GCCcore/13.3.0 => GCCcore/14.2.0     4) numactl/2.0.18 => numactl/2.0.19

So, what happen? Basically the system is smart enough to understand that the dependences and core files in the previous environment are incompatible with GCC/14.2.0 and replaces or deactivates modules. Check the loaded modules with ``module list``

.. code-block:: julia

   Currently Loaded Modules:
     1) GCCcore/14.2.0   3) GCC/14.2.0   5) libpciaccess/0.18.1   7) libevent/2.1.12   9) UCX/1.18.0
     2) binutils/2.42    4) zlib/1.3.1   6) OpenSSL/3             8) numactl/2.0.19   10) UCC/1.3.0

   Inactive Modules:
     1) XZ/5.4.5         3) hwloc/2.10.0       5) PMIx/5.0.2    7) OpenMPI/5.0.3
     2) libxml2/2.12.7   4) libfabric/1.21.0   6) PRRTE/3.0.5

No longer have access to OpenMPI-5.0.3 and associated frameworks. Let's check what is available (use ``module --nx av``)

.. code-block:: julia

   ------------------------------- /mnt/beegfs/apps/modules/all/Compiler/GCC/14.2.0 --------------------------------
      AOCL-BLAS/5.0                    FlexiBLAS/3.4.5         Seaborn/0.13.2       networkx/3.5
      ASE/3.25.0                       GEOS/3.13.1             Shapely/2.1.1        pybind11/2.13.6
      ASE/3.26.0                (D)    GSL/2.8                 bokeh/3.7.3          scikit-learn/1.7.0
      BLIS/1.1                         OpenBLAS/0.3.29         dask/2025.5.1        spglib-python/2.6.0
      Boost.Python-NumPy/1.88.0        OpenMPI/5.0.7           libxc/7.0.0
      Boost/1.88.0                     SAMtools/1.22.1         matplotlib/3.10.3
      FFTW/3.3.10                      SciPy-bundle/2025.06    mrcfile/1.5.4

   ----------------------------- /mnt/beegfs/apps/modules/all/Compiler/GCCcore/14.2.0 ------------------------------
      Abseil/20250512.1                   PyZMQ/27.0.0                         libdeflate/1.24
      Autoconf/2.72                (D)    Python-bundle-PyPI/2025.04           libevent/2.1.12          (L)
      Automake/1.17                       Python/3.13.1                        libfabric/2.0.0
      Autotools/20240712                  Qhull/2020.2                         libffi/3.4.5
      BeautifulSoup/4.13.4                Redis/8.0.2                          libgeotiff/1.7.4
      Bison/3.8.2                  (D)    Rust/1.85.1                          libheif/1.19.8
      Blosc/1.21.6                        SIONlib/1.7.7-tools                  libiconv/1.18
      Blosc2/2.19.0                       SQLite/3.47.2                        libidn2/2.3.7
      Brotli/1.1.0                        SWIG/4.3.1                           libjpeg-turbo/3.1.0
      Brunsli/0.1                         Szip/2.1.1                           libpciaccess/0.18.1      (L)
      CFITSIO/4.6.2                       Tcl/8.6.16                           libpng/1.6.48
      CMake/3.31.3                        Tk/8.6.16                            libpsl/0.21.5
      Catch2/2.13.10                      Tkinter/3.13.1                       libreadline/8.2
      Catch2/3.8.1                 (D)    UCC/1.3.0                   (L)      libsodium/1.0.20
      Check/0.15.2                        UCX/1.18.0                  (L)      libtirpc/1.3.6
      ...
      Perl-bundle-CPAN/5.40.0             json-c/0.18                          xorg-macros/1.20.2
      Perl/5.40.0                  (D)    jupyter-server/2.16.0                xxd/9.1.1457
      Pillow/11.3.0                       libaec/1.1.4                         zlib/1.3.1               (L,D)
      Pint/0.24.4                         libarchive/3.7.7                     zstd/1.5.6
      PyYAML/6.0.2                        libde265/1.0.16

   --------------------------------------- /mnt/beegfs/apps/modules/all/Core ---------------------------------------
      Anaconda3/2025.06-1                  OpenSSL/3           (L,D)    iimkl/2025a              (D)
      Autoconf/2.71                        Perl/5.38.0                  iimpi/2023a
      Bison/3.8.2                          ant/1.10.12-Java-17          iimpi/2024a
      FastQC/0.11.9-Java-11                ant/1.10.14-Java-11 (D)      iimpi/2025a              (D)
      GCC/12.3.0                           binutils/2.40                imkl/2023.1.0
      GCC/13.3.0                           binutils/2.42                imkl/2023.2.0
      GCC/14.2.0                 (L,D)     ecBuild/3.8.0                imkl/2024.2.0
      GCCcore/12.3.0                       ffnvcodec/12.0.16.0          imkl/2025.1.0            (D)
      ...

    Where:
      L:        Module is loaded
      D:        Default Module

Again, besides the core modules, there is a huge list of packages compiled with GCC-14.2.0 including OpenMPI-5.0.7, OpenBLAS, LAPACK, etc.. Load OpenMPI/5.0.7 (``module load OpenMPI/5.0.7``) obtaining

.. code-block:: julia

   Activating Modules:
      1) OpenMPI/5.0.7     3) PRRTE/3.0.8     5) hwloc/2.11.2        7) libxml2/2.13.4
      2) PMIx/5.0.6        4) XZ/5.6.3        6) libfabric/2.0.0

and list the loaded modules (``module list``)

.. code-block:: julia

   Currently Loaded Modules:
      1) GCCcore/14.2.0        6) OpenSSL/3        11) XZ/5.6.3         16) PRRTE/3.0.8
      2) binutils/2.42         7) libevent/2.1.12  12) libxml2/2.13.4   17) OpenMPI/5.0.7
      3) GCC/14.2.0            8) numactl/2.0.19   13) hwloc/2.11.2
      4) zlib/1.3.1            9) UCX/1.18.0       14) libfabric/2.0.0
      5) libpciaccess/0.18.1  10) UCC/1.3.0        15) PMIx/5.0.6

and see what is available (``module --nx av``)

.. code-block:: julia

   ------------------ /mnt/beegfs/apps/modules/all/MPI/GCC/14.2.0/OpenMPI/5.0.7 -------------------
      Armadillo/14.6.0              OSU-Micro-Benchmarks/7.5              Wannier90/3.1.0
      Biopython/1.85                OpenMM/8.3.0                          arpack-ng/3.9.1
      CDO/2.5.2                     PETSc/3.23.5                          buildenv/default
      Cartopy/0.24.1                PLUMED/2.9.4                          dtcmp/1.1.5
      DIRAC/25.0                    ParMETIS/4.0.3                        ecCodes/2.43.0
      ELPA/2025.01.002              PnetCDF/1.14.0                        h5py/3.14.0
      FFTW.MPI/3.3.10               PyStan/3.10.0                         ipyparallel/9.0.1
      Fiona/1.10.1                  PyTables/3.10.2                       libcircle/0.3
      GDAL/3.11.1                   RDKit/2025.03.4                       libvdwxc/0.5.0
      GPAW/25.7.0-ASE-3.26.0        Ray-project/2.47.1                    lwgrp/1.0.6
      GPAW/25.7.0            (D)    SCOTCH/7.0.8                          mpi4py/4.1.0
      GROMACS/2025.2                ScaLAPACK/2.2.2-fb                    mpifileutils/0.12
      HDF5/1.14.6                   Scalasca/2.6.2                        netCDF-Fortran/4.6.2
      HPCG/3.1                      Score-P/9.0                           netCDF/4.9.3
      HPL/2.3                       Score-P/9.2                    (D)    netcdf4-python/1.7.2
      Hypre/2.33.0                  SuiteSparse/7.10.3-METIS-5.1.0        nglview/3.1.4
      MDAnalysis/2.9.0              SuiteSparse/7.10.3             (D)    zarr/3.0.10
      MDTraj/1.11.0                 SuperLU_DIST/9.1.0
      MUMPS/5.8.1-metis             Valgrind/3.25.0

   ----------------------- /mnt/beegfs/apps/modules/all/Compiler/GCC/14.2.0 -----------------------
      AOCL-BLAS/5.0                    GSL/2.8                     libxc/7.0.0
      ASE/3.25.0                       OpenBLAS/0.3.29             matplotlib/3.10.3
      ASE/3.26.0                (D)    OpenMPI/5.0.7        (L)    mrcfile/1.5.4
      BLIS/1.1                         SAMtools/1.22.1             networkx/3.5
      Boost.Python-NumPy/1.88.0        SciPy-bundle/2025.06        pybind11/2.13.6
      Boost/1.88.0                     Seaborn/0.13.2              scikit-learn/1.7.0
      FFTW/3.3.10                      Shapely/2.1.1               spglib-python/2.6.0
      FlexiBLAS/3.4.5                  bokeh/3.7.3
      GEOS/3.13.1                      dask/2025.5.1

The user got access to  a new level the software hierarchy. Hence, having access to all the software that was compiled against OpenMPI-5.0.7 (top row), which in turn was compiled with GCC-14.2.0 (as displayed in the second row of modules - from top to bottom).


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
~~~~~~~~~~~~~~~~~~~~~~~~-----------

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

Similar procedure to what has been outlined above applies for software using the Intel compilers, MKL, and MPI. At the entering level if the user executes ``module av`` obtains 

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
    D:        Default Module
      
After loading intel/2021b or iimpi/2021b (``module load intel/2021b`` or ``module load iimpi/2021b``) ``module list`` shows

.. code-block:: julia

   Currently Loaded Modules:
      1) GCCcore/11.2.0   3) binutils/2.37              5) numactl/2.0.14   7) impi/2021.4.0   9) imkl-FFTW/2021.4.0
      2) zlib/1.2.11      4) intel-compilers/2021.4.0   6) UCX/1.11.2       8) imkl/2021.4.0  10) intel/2021b

and ``module av`` displays

.. code-block:: julia

  -------------------- /mnt/beegfs/apps/cn01470x/modules/all/MPI/intel/2021.4.0/impi/2021.4.0 ---------------------
    ABINIT/9.6.2          HMMER/3.3.2                 SPOTPY/1.5.14                         libvdwxc/0.4.0
    ASE/3.22.1            HPL/2.3                     ScaFaCoS/1.0.1                        libxsmm/1.17
    AmberTools/21         Hypre/2.24.0                SciPy-bundle/2021.10                  loompy/3.0.7
    ArviZ/0.11.4          IMB/2021.3                  Siesta/4.1.5                          matplotlib/3.4.3
    BDBag/1.6.3           Libint/2.6.0-lmax-6-cp2k    SimPEG/0.18.1                         mkl-service/2.3.0
    Bambi/0.7.1           MDAnalysis/2.0.0            SuiteSparse/5.10.1-METIS-5.1.0        ncview/2.1.8
    Biopython/1.79        MDTraj/1.9.7                SuperLU/5.3.0                         netCDF-C++4/4.3.1
    CGAL/4.14.3           MUMPS/5.4.1-metis           Theano/1.1.2-PyMC                     netCDF-Fortran/4.5.3
    CP2K/8.2              NCO/5.0.3                   VTK/9.1.0                             netCDF/4.8.1
    ELPA/2021.05.001      NWChem/7.0.2                Valgrind/3.18.1                       netcdf4-python/1.5.7
    ESMF/8.2.0            OSU-Micro-Benchmarks/5.8    Wannier90/3.1.0                       networkx/2.6.3
    FDS/6.7.7             OpenMolcas/22.10            XCrySDen/1.6.2                        numba/0.54.1
    FFTW/3.3.10           PLUMED/2.8.0                YAXT/0.9.2                            scikit-bio/0.5.7
    FMS/2022.02           PSolver/1.8.3               astropy/5.1.1                         scikit-learn/1.0.1
    GDAL/3.3.2            ParMETIS/4.0.3              buildenv/default                      spglib-python/1.16.3
    GEOS/3.9.1            PnetCDF/1.12.3              ecCodes/2.24.2                        statsmodels/0.13.1
    GPAW/22.8.0           PyMC3/3.11.1                futile/1.8.3                          worker/1.6.13
    GTDB-Tk/2.0.0         QuantumESPRESSO/7.0         h5py/3.6.0                            xarray/0.20.1
    GlobalArrays/5.8.1    SCOTCH/6.1.2                imkl-FFTW/2021.4.0             (L)
    HDF5/1.12.1           SISSO/3.1-20220324          libGridXC/0.9.6

  ------------------------- /mnt/beegfs/apps/cn01470x/modules/all/Compiler/intel/2021.4.0 -------------------------
    BLIS/0.9.0      FastANI/1.33    LAPACK/3.10.1        OpenMPI/4.1.1        xmlf90/1.5.4
    Boost/1.77.0    Flye/2.9        Mash/2.3             impi/2021.4.0 (L)
    DFT-D3/3.2.0    GSL/2.7         NLopt/2.7.0   (D)    libxc/5.1.6

  ------------------------- /mnt/beegfs/apps/cn01470x/modules/all/Compiler/GCCcore/11.2.0 -------------------------
    ANTLR/2.7.7-Java-11                 PCRE2/10.37                    hypothesis/6.14.6
    ATK/2.36.0                          PDT/3.25.1                     intltool/0.51.0
    Autoconf/2.71                       PMIx/4.1.0                     jbigkit/2.1
    Automake/1.16.4                     PROJ/8.1.0                     kim-api/2.3.0
    Autotools/20210726                  Pango/1.48.8                   libGLU/9.0.2
    Bazel/4.2.2                         Perl/5.34.0-minimal            libarchive/3.5.1
    Bison/3.7.6                         Perl/5.34.0             (D)    libcerf/1.17
    Brotli/1.0.9                        Pillow/8.3.2                   libdap/3.20.8
    CMake/3.21.1                        PyYAML/5.4.1                   libdrm/2.4.107
    CMake/3.22.1                 (D)    Python/2.7.18-bare             libepoxy/1.5.8
    CapnProto/0.9.1                     Python/3.9.6-bare              libevent/2.1.12
    CubeLib/4.8                         Python/3.9.6            (D)    libfabric/1.13.2
    CubeWriter/4.8                      Qhull/2020.2                   libffi/3.4.2
    ...

On the top rwo the software compiled against Intel MPI (which is MPICH compiled against the Intel compilers) is displayed followed by the software compiled with Intel C, C++ and Fortran compilers. On the bottom row the software compiled with GCC/11.2.0 as a backend is displayed.

The user can change to GCC based modules, e.g., to the foss/2022a toochain, by issuing ``module load foss/2022a`` obtaining

.. code-block:: julia

   Lmod is automatically replacing "intel-compilers/2021.4.0" with "GCC/11.3.0".
   
   Inactive Modules:
     1) imkl-FFTW/2021.4.0     2) impi/2021.4.0

   Due to MODULEPATH changes, the following have been reloaded:
     1) numactl/2.0.14

   The following have been reloaded with a version change:
    1) GCCcore/11.2.0 => GCCcore/11.3.0     3) binutils/2.37 => binutils/2.38
    2) UCX/1.11.2 => UCX/1.12.1             4) zlib/1.2.11 => zlib/1.2.12


and ``module list`` gives

.. code-block:: julia

  Currently Loaded Modules:
    1) imkl/2021.4.0    6) GCC/11.3.0         11) OpenSSL/1.1       16) UCC/1.0.0        21) FFTW.MPI/3.3.10
    2) intel/2021b      7) XZ/5.2.5           12) libevent/2.1.12   17) OpenMPI/4.1.4    22) ScaLAPACK/2.2.0-fb
    3) GCCcore/11.3.0   8) libxml2/2.9.13     13) UCX/1.12.1        18) OpenBLAS/0.3.20  23) foss/2022a
    4) zlib/1.2.12      9) libpciaccess/0.16  14) libfabric/1.15.1  19) FlexiBLAS/3.2.0  24) numactl/2.0.14
    5) binutils/2.38   10) hwloc/2.7.1        15) PMIx/4.1.2        20) FFTW/3.3.10

  Inactive Modules:
    1) impi/2021.4.0   2) imkl-FFTW/2021.4.0


4. Loading a Particular Software
--------------------------------

4.1 scipy, mpi4py, numpy, numexpr, pandas
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

These packages, as well as others, are included in the module scipy-bundle. Therefore, the user needs to follow the following procedure to use these packages: (i) decide which toolchain to use (foss or intel). If using foss/2022a execute ``module load foss/2022a`` followed with ``module load SciPy-bundle/2022.05``. These two commands can be typed once by using

.. code-block:: julia

  module load foss/2022a SciPy-bundle/2022.05
  
``module list`` displays the loaded modules as

.. code-block:: julia

 Currently Loaded Modules:
   1) GCCcore/11.3.0      9) hwloc/2.7.1       17) OpenBLAS/0.3.20     25) libreadline/8.1.2
   2) zlib/1.2.12        10) OpenSSL/1.1       18) FlexiBLAS/3.2.0     26) Tcl/8.6.12
   3) binutils/2.38      11) libevent/2.1.12   19) FFTW/3.3.10         27) SQLite/3.38.3
   4) GCC/11.3.0         12) UCX/1.12.1        20) FFTW.MPI/3.3.10     28) GMP/6.2.1
   5) numactl/2.0.14     13) libfabric/1.15.1  21) ScaLAPACK/2.2.0-fb  29) libffi/3.4.2
   6) XZ/5.2.5           14) PMIx/4.1.2        22) foss/2022a          30) Python/3.10.4
   7) libxml2/2.9.13     15) UCC/1.0.0         23) bzip2/1.0.8         31) pybind11/2.9.2
   8) libpciaccess/0.16  16) OpenMPI/4.1.4     24) ncurses/6.3         32) SciPy-bundle/2022.05


Now the user can use, for example, mpi4py or numpy in their submission scripts.


4.2 TensorFlow
~~~~~~~~~~~~~~

Similarly to the procedure discussed previously the user loads foss/2021b + TensorFlow/2.8.4 or foss/2022a + TensorFlor/2.8.4 as

.. code-block:: julia

  module load foss/2021b TensorFlow/2.8.4

or

.. code-block:: julia

  module load foss/2022a TensorFlow/2.8.4

and ``module list`` gives (here the results for foss/2021b + TensorFlow/2.8.4) are shown)

.. code-block:: julia

 Currently Loaded Modules:
   1) GCCcore/11.2.0     14) PMIx/4.1.0          27) libffi/3.4.2             40) JsonCpp/1.9.4
   2) zlib/1.2.11        15) OpenMPI/4.1.1       28) Python/3.9.6             41) NASM/2.15.05
   3) binutils/2.37      16) OpenBLAS/0.3.18     29) pybind11/2.7.1           42) libjpeg-turbo/2.0.6
   4) GCC/11.2.0         17) FlexiBLAS/3.0.4     30) SciPy-bundle/2021.10     43) LMDB/0.9.29
   5) numactl/2.0.14     18) FFTW/3.3.10         31) Szip/2.1.1               44) nsync/1.24.0
   6) XZ/5.2.5           19) ScaLAPACK/2.1.0-fb  32) HDF5/1.12.1              45) protobuf/3.17.3
   7) libxml2/2.9.10     20) foss/2021b          33) h5py/3.6.0               46) protobuf-python/3.17.3
   8) libpciaccess/0.16  21) bzip2/1.0.8         34) cURL/7.78.0              47) flatbuffers-python/2.0
   9) hwloc/2.5.0        22) ncurses/6.2         35) dill/0.3.4               48) libpng/1.6.37
  10) OpenSSL/1.1        23) libreadline/8.1     36) double-conversion/3.1.5  49) snappy/1.1.9
  11) libevent/2.1.12    24) Tcl/8.6.11          37) flatbuffers/2.0.0        50) networkx/2.6.3
  12) UCX/1.11.2         25) SQLite/3.36         38) giflib/5.2.1             51) TensorFlow/2.8.4
  13) libfabric/1.13.2   26) GMP/6.2.1           39) ICU/69.1

Note the presence of PMIx, UCX, and libfabric in the list - these packages are loaded to make sure for multi-core, multi-nodes communications.


4.3 GROMACS
~~~~~~~~~~~

In both foss/2021b and foss/2022a there are two flavours of GROMACS. One compiled with PLUMED-2.8.0 (in foss/2021b) and 2.8.1 (in foss/2022a) (see https://www.plumed.org/doc-v2.8/user-doc/html/index.html) and the other without. To load GROMACS follow the procedures outlined above. Load the foss toolchain and then GROMACS. Here is an example using foss/2021b:

.. code-block:: julia

  module load foss/2021b GROMACS/2021.5

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
