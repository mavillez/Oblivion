Environment Modules
===================

There are several conflicting software packages installed in the Oblivion supercomputer. In order for the user to use the adequate software there is a need to set the paths for the binaries, libraries, manuals, and includes. Either the user sets these paths for each package or the system provides an easy way to set those paths. The latter is the preferable and makes use of environment modules. 

1. Toolchains
-------------

1.1 Definitions
~~~~~~~~~~~~~~~

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

1.2 Toolchains and Sub-toolchains installed in OBLIVION
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

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

Core refers to the basic core modules that have to be loaded in order to have access to next levels of software compiled against a specific compiler (e.g., ``GCC 9.3.0, 11.2.0, 11.3.0; intel-compilers 2021.4.0, 2021.6.0 although using module version 2022.1.0``) and a MPI API (OpenMPI 4.0.3, 4.1.1, 4.1.4, 4.1.5; MPICH 3.4.2; Intel MPI 2021.4.0, 2022.1.0 - using intel-compilers 2021.6.0)). It also includes modules of software that i) are initially compiled with the system/machine compiler (e.g., binutils, gettext, M4, ncurses, pkgconf, zlib) or ii) are not being built but instead are directly installed into the stack (e.g., Anaconda, ANSYS_CFD).

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

  --------------------- /mnt/beegfs/apps/cn01470x/modules/all/Compiler/GCC/9.3.0 ----------------------
    OpenBLAS/0.3.9    OpenMPI/4.0.3

  ------------------- /mnt/beegfs/apps/cn01470x/modules/all/Compiler/GCCcore/9.3.0 --------------------
   Autoconf/2.69                    SIONlib/1.7.6-tools        hwloc/2.2.0
   Automake/1.16.1                  SQLite/3.31.1              intltool/0.51.0
   Autotools/20180311               Szip/2.1.1                 libevent/2.1.11
   Bison/3.5.3                      Tcl/8.6.10                 libfabric/1.11.0
   CMake/3.16.4                     UCX/1.8.0                  libffi/3.3
   CubeLib/4.4.4                    UDUNITS/2.2.26             libiconv/1.16
   CubeWriter/4.4.3                 UnZip/6.0                  libpciaccess/0.16
   DB/18.1.32                       X11/20200222               libpng/1.6.37
   Doxygen/1.8.17                   XZ/5.2.5                   libreadline/8.0
   GMP/6.2.0                        binutils/2.34       (L)    libtool/2.4.6
   M4/1.4.18                        bzip2/1.0.8                libunwind/1.3.1
   Meson/0.55.1-Python-3.8.2        cURL/7.69.1                libxml2/2.9.10
   Ninja/1.10.0                     expat/2.2.9                makeinfo/6.7-minimal
   OPARI2/2.0.5                     flex/2.6.4          (D)    ncurses/6.2          (D)
   OTF2/2.2                         fontconfig/2.13.92         numactl/2.0.13
   PAPI/6.0.0                       freetype/2.10.1            pkg-config/0.29.2
   PDT/3.25.1                       gettext/0.20.1             util-linux/2.35
   PMIx/3.1.5                       git/2.23.0-nodocs          xorg-macros/1.19.2
   Perl/5.30.2-minimal              gperf/3.1                  zlib/1.2.11          (L)
   Perl/5.30.2               (D)    groff/1.22.4
   Python/3.8.2                     help2man/1.47.12

  ---------------------------- /mnt/beegfs/apps/cn01470x/modules/all/Core -----------------------------
   ANSYS_CFD/2021R1                 OpenSSL/1.1                iimpi/2021b
   ANSYS_CFD/2022R2         (D)     ant/1.10.11-Java-11        iimpi/2022a              (D)
   Anaconda3/2022.05                ant/1.10.12-Java-11 (D)    imkl/2021.4.0
   Bison/3.8.2              (D)     binutils/2.34              imkl/2022.1.0            (D)
   FastQC/0.11.9-Java-11            binutils/2.37              intel-compilers/2021.4.0
   GCC/9.3.0                (L)     binutils/2.38       (D)    intel-compilers/2022.1.0 (D)
   GCC/11.2.0                       flex/2.6.4                 intel/2021b
   GCC/11.3.0               (D)     foss/2020a                 intel/2022a              (D)
   GCCcore/9.3.0            (L)     foss/2021b                 iompi/2021b
   GCCcore/11.2.0                   foss/2022a          (D)    ncurses/6.1
   GCCcore/11.3.0           (D)     gettext/0.20.1             ncurses/6.2
   GPAW-setups/0.9.20000            gettext/0.21        (D)    pkgconf/1.8.0
   Java/11.0.16             (11)    gompi/2020a                pplacer/1.1.alpha19
   Julia/1.8.5-linux-x86_64         gompi/2021b                zlib/1.2.11
   M4/1.4.19                (D)     gompi/2022a         (D)    zlib/1.2.12              (D)

  Where:
   L:        Module is loaded
   D:        Default Module

Here one can see (from bottom to top) the list of core modules indicating those loaded with **(L)**, followed by general software compiled with GCC-9.3.0, and MPI API compiled with GCC-9.3.0 - all following the hierarchical scheme core/compiler/MPI referred above.

The user can now load OpenMPI-4.0.3 using ``module load OpenMPI/4.0.3`` and check the loaded modules using ``module list`` obtaining

.. code-block:: julia

   Currently Loaded Modules:
      1) GCCcore/9.3.0   5) numactl/2.0.13      9) hwloc/2.2.0       13) PMIx/3.1.5
      2) zlib/1.2.11     6) XZ/5.2.5           10) libevent/2.1.11   14) OpenMPI/4.0.3
      3) binutils/2.34   7) libxml2/2.9.10     11) UCX/1.8.0
      4) GCC/9.3.0       8) libpciaccess/0.16  12) libfabric/1.11.0

Now, not only OpenMPI is loaded, but also UCX, PMIx, etc., are loaded. UCX stands for Unified Communication X and is "an optimized production communication framework for modern, high-bandwidth and low-latency networks" (see https://github.com/openucx/ucx) meaning for infiniband. PMIx stands for Process Management Interface - Exascale and enables the interaction of MPI applications with Resource Managers like SLURM (see https://pmix.github.io)

Let us now use an enviromment based on GCC-11.3.0. Hence, load the module GCC/11.3.0 (use ``module load GCC/11.3.0``) and immediately it is seen

.. code-block:: julia

  Inactive Modules:
    1) OpenMPI/4.0.3     3) UCX/1.8.0       5) libevent/2.1.11      7) libxml2/2.9.10
    2) PMIx/3.1.5        4) hwloc/2.2.0     6) libfabric/1.11.0     8) numactl/2.0.13

  Due to MODULEPATH changes, the following have been reloaded:
    1) XZ/5.2.5     2) libpciaccess/0.16

  The following have been reloaded with a version change:
    1) GCC/9.3.0 => GCC/11.3.0             3) binutils/2.34 => binutils/2.38
    2) GCCcore/9.3.0 => GCCcore/11.3.0     4) zlib/1.2.11 => zlib/1.2.12


So, what happen? Basically the system is smart enough to understand that the dependences and core files in the previous environment are incompatible to GCC/11.3.0 and replaces or deactivates modules. Check the loaded modules with ``module list``

.. code-block:: julia

  Currently Loaded Modules:
    1) GCCcore/11.3.0   3) binutils/2.38   5) XZ/5.2.5
    2) zlib/1.2.12      4) GCC/11.3.0      6) libpciaccess/0.16

  Inactive Modules:
    1) numactl/2.0.13   3) hwloc/2.2.0       5) UCX/1.8.0          7) PMIx/3.1.5
    2) libxml2/2.9.10   4) libevent/2.1.11   6) libfabric/1.11.0   8) OpenMPI/4.0.3

No longer have access to OpenMPI-4.0.3 and associated frameworks. Let's check what is available now (use ``module av``)

.. code-block:: julia

  ------------------------- /mnt/beegfs/apps/cn01470x/modules/all/Compiler/GCC/11.3.0 -------------------------
    BEDTools/2.30.0    Flye/2.9.1                 GTK4/4.7.0             Pysam/0.19.1
    BLIS/0.9.0         GEOS/3.10.3                LAPACK/3.10.1          SAMtools/1.16.1
    BamTools/2.5.2     GSL/2.7                    MPICH/3.4.2            STAR/2.7.9a
    Boost/1.79.0       GST-plugins-bad/1.20.2     OpenBLAS/0.3.20        libxc/5.2.3
    FFTW/3.3.10        GST-plugins-base/1.20.2    OpenMPI/4.1.4          libxsmm/1.17
    FlexiBLAS/3.2.0    GStreamer/1.20.2           OpenMPI/4.1.5   (D)    pybedtools/0.9.0

  ----------------------- /mnt/beegfs/apps/cn01470x/modules/all/Compiler/GCCcore/11.3.0 -----------------------
    ANTLR/2.7.7-Java-11                 PROJ/9.0.0                       intltool/0.51.0
    ATK/2.38.0                          Pango/1.50.7                     jbigkit/2.1
    Autoconf/2.71                       Perl/5.34.1-minimal              kim-api/2.3.0
    Automake/1.16.5                     Perl/5.34.1             (D)      libGLU/9.0.2
    Autotools/20220317                  Pillow/9.1.1                     libaec/1.0.6
    Bazel/4.2.2                         PyCairo/1.21.0                   libarchive/3.6.1
    Bazel/5.1.1                  (D)    PyGObject/3.42.1                 libcerf/2.1
    BeautifulSoup/4.10.0                PyYAML/6.0                       libdap/3.20.11
    Bison/3.8.2                  (D)    Python/2.7.18-bare               libdeflate/1.10
    Brotli/1.0.9                        Python/3.10.4-bare               libdrm/2.4.110
    CMake/3.23.1                        Python/3.10.4           (D)      libepoxy/1.5.10
    CMake/3.24.3                 (D)    Qhull/2020.2                     libevent/2.1.12
    CubeLib/4.8                         Qt5/5.15.5                       libfabric/1.15.1
    CubeWriter/4.8                      RE2/2022-06-01                   libffi/3.4.2
    DB/18.1.40                          RapidJSON/1.1.0                  libgd/2.3.3
    DBus/1.14.0                         Rust/1.60.0                      libgeotiff/1.7.1
    Doxygen/1.9.4                       SIONlib/1.7.7-tools              libgit2/1.4.3
    Eigen/3.4.0                         SQLite/3.38.3                    libglvnd/1.4.0
    FFmpeg/4.4.2                        Szip/2.1.1                       libiconv/1.17
    FLAC/1.3.4                          Tcl/8.6.12                       libjpeg-turbo/2.1.3
    ...
    PAPI/7.0.0                          groff/1.22.4                     xorg-macros/1.19.3
    PCRE/8.45                           gzip/1.12                        xxd/8.2.4220
    PCRE2/10.40                         help2man/1.49.2                  zlib/1.2.12             (L,D)
    PDT/3.25.1                          hwloc/2.7.1                      zstd/1.5.2
    PMIx/4.1.2                          hypothesis/6.46.7

  -------------------------------- /mnt/beegfs/apps/cn01470x/modules/all/Core ---------------------------------
    ANSYS_CFD/2021R1                  OpenSSL/1.1                iimpi/2021b
    ANSYS_CFD/2022R2         (D)      ant/1.10.11-Java-11        iimpi/2022a              (D)
    Anaconda3/2022.05                 ant/1.10.12-Java-11 (D)    imkl/2021.4.0
    Bison/3.8.2                       binutils/2.34              imkl/2022.1.0            (D)
    FastQC/0.11.9-Java-11             binutils/2.37              intel-compilers/2021.4.0
    GCC/9.3.0                         binutils/2.38              intel-compilers/2022.1.0 (D)
    GCC/11.2.0                        flex/2.6.4                 intel/2021b
    GCC/11.3.0               (L,D)    foss/2020a                 intel/2022a              (D)
    GCCcore/9.3.0                     foss/2021b                 iompi/2021b
    GCCcore/11.2.0                    foss/2022a          (D)    ncurses/6.1
    GCCcore/11.3.0           (L,D)    gettext/0.20.1             ncurses/6.2
    GPAW-setups/0.9.20000             gettext/0.21               pkgconf/1.8.0
    Java/11.0.16             (11)     gompi/2020a                pplacer/1.1.alpha19
    Julia/1.8.5-linux-x86_64          gompi/2021b                zlib/1.2.11
    M4/1.4.19                         gompi/2022a         (D)    zlib/1.2.12

    Where:
      L:        Module is loaded
      D:        Default Module

Again, besides the core modules, there is a huge list of packages compiled with GCC-11.3.0 including OpenMPI-4.1.4 and 4.1.5, OpenBLAS, LAPACK, etc.. Load OpenMPI/4.1.4 (``module load OpenMPI/4.1.4``) obtaining

.. code-block:: julia

   Activating Modules:
     1) OpenMPI/4.1.4     3) UCX/1.12.1      5) libevent/2.1.12      7) libxml2/2.9.13
     2) PMIx/4.1.2        4) hwloc/2.7.1     6) libfabric/1.15.1     8) numactl/2.0.14

list the load modules (``module list``)

.. code-block:: julia

   Currently Loaded Modules:
     1) GCCcore/11.3.0   5) XZ/5.2.5            9) hwloc/2.7.1      13) libfabric/1.15.1
     2) zlib/1.2.12      6) libpciaccess/0.16  10) OpenSSL/1.1      14) PMIx/4.1.2
     3) binutils/2.38    7) numactl/2.0.14     11) libevent/2.1.12  15) UCC/1.0.0
     4) GCC/11.3.0       8) libxml2/2.9.13     12) UCX/1.12.1       16) OpenMPI/4.1.4

and see what is available (``module av``)

.. code-block:: julia

  -------------------- /mnt/beegfs/apps/cn01470x/modules/all/MPI/GCC/11.3.0/OpenMPI/4.1.4 ---------------------
    ABINIT/9.6.2                       MUMPS/5.5.1-metis                     Valgrind/3.20.0
    ASE/3.22.1                         MultiQC/1.12                          Wannier90/3.1.0
    AmberTools/22.3                    NCO/5.1.0                             XCrySDen/1.6.2
    Arrow/8.0.0                        ORCA/5.0.3                            arpack-ng/3.8.0
    ArviZ/0.12.1                       OSU-Micro-Benchmarks/5.9              arrow-R/8.0.0-R-4.2.1
    Bambi/0.10.0                       OpenCV/4.6.0-contrib                  astropy/5.1.1
    Biopython/1.79                     OpenFOAM/v2206                        buildenv/default
    CGAL/4.14.3                        PLUMED/2.8.1                          ecCodes/2.27.0
    CP2K/8.2                           PSolver/1.8.3                         futile/1.8.3
    CheMPS2/1.8.12                     ParMETIS/4.0.3                        h5py/3.7.0
    Dalton/2020.0                      ParaView/5.10.1-mpi                   imkl-FFTW/2022.1.0
    ELPA/2021.11.001                   PnetCDF/1.12.3                        libGridXC/0.9.6
    ESMF/8.3.0                         PyMC3/3.11.1                          libvdwxc/0.4.0
    FFTW.MPI/3.3.10                    PyTorch/1.12.1                        matplotlib/3.5.2
    FMS/2022.02                        QuantumESPRESSO/7.1                   ncview/2.1.8
    GDAL/3.5.0                         R-bundle-Bioconductor/3.15-R-4.2.1    netCDF-C++4/4.3.1
    GPAW/22.8.0                        R/4.2.1                               netCDF-Fortran/4.6.0
    GROMACS/2021.5-PLUMED-2.8.1        SCOTCH/7.0.1                          netCDF/4.9.0
    GROMACS/2021.5              (D)    SUNDIALS/6.3.0                        netcdf4-python/1.6.1
    HDF/4.2.15                  (D)    ScaFaCoS/1.0.1                        networkx/2.8.4
    HDF5/1.12.2                        ScaLAPACK/2.2.0-fb                    numba/0.56.4
    HPL/2.3                            SciPy-bundle/2022.05                  scikit-bio/0.5.7
    Hypre/2.25.0                       Score-P/8.0                           scikit-learn/1.1.2
    IMB/2021.3                         Siesta/4.1.5                          snakemake/7.22.0
    KaHIP/3.14                         SimPEG/0.18.1                         spglib-python/2.0.0
    LAMMPS/23Jun2022-kokkos            SuiteSparse/5.13.0-METIS-5.1.0        statsmodels/0.13.1
    LMfit/1.0.3                        SuperLU/5.3.0                         worker/1.6.13
    Libint/2.6.0-lmax-6-cp2k           TensorFlow/2.8.4                      xarray/2022.6.0
    MDAnalysis/2.2.0                   Theano/1.1.2-PyMC                     xarray/2022.9.0       (D)
    MDTraj/1.9.7                       VTK/9.2.2

  ------------------------- /mnt/beegfs/apps/cn01470x/modules/all/Compiler/GCC/11.3.0 -------------------------
    BEDTools/2.30.0    Flye/2.9.1                 GTK4/4.7.0             Pysam/0.19.1
    BLIS/0.9.0         GEOS/3.10.3                LAPACK/3.10.1          SAMtools/1.16.1
    BamTools/2.5.2     GSL/2.7                    MPICH/3.4.2            STAR/2.7.9a
    Boost/1.79.0       GST-plugins-bad/1.20.2     OpenBLAS/0.3.20        libxc/5.2.3
    FFTW/3.3.10        GST-plugins-base/1.20.2    OpenMPI/4.1.4   (L)    libxsmm/1.17
    FlexiBLAS/3.2.0    GStreamer/1.20.2           OpenMPI/4.1.5   (D)    pybedtools/0.9.0

  ----------------------- /mnt/beegfs/apps/cn01470x/modules/all/Compiler/GCCcore/11.3.0 -----------------------
    ANTLR/2.7.7-Java-11                 PROJ/9.0.0                       intltool/0.51.0
    ATK/2.38.0                          Pango/1.50.7                     jbigkit/2.1
    Autoconf/2.71                       Perl/5.34.1-minimal              kim-api/2.3.0
    Automake/1.16.5                     Perl/5.34.1             (D)      libGLU/9.0.2
    Autotools/20220317                  Pillow/9.1.1                     libaec/1.0.6
    ...

The user got access to  a new level the software hierarchy. Hence, having access to all the software that was compiled against OpenMPI-4.1.4 (top row), which in turn was compiled with GCC-11.3.0 (as displayed in the second row of modules - from top to bottom). Finally, the third row displays the core modules associated to GCC/11.3.0.


3.2 Foss/2022a Toolchain
~~~~~~~~~~~~~~~~~~~~~~~~

Accessing the software modules made available by loading GCC/11.3.0 and OpenMPI/4.1.4 can be done by just loading foss/2022a with the penalty of loading extra modules like BLIS, FFTW, FlexiBLAS, OpenBLAS, ScaLAPACK. let's check it. Start with ``module purge`` followed by ``module load foss/2022a`` and ``module list`` obtaining

.. code-block:: julia

   Currently Loaded Modules:
     1) GCCcore/11.3.0   7) libxml2/2.9.13     13) libfabric/1.15.1  19) FFTW/3.3.10
     2) zlib/1.2.12      8) libpciaccess/0.16  14) PMIx/4.1.2        20) FFTW.MPI/3.3.10
     3) binutils/2.38    9) hwloc/2.7.1        15) UCC/1.0.0         21) ScaLAPACK/2.2.0-fb
     4) GCC/11.3.0      10) OpenSSL/1.1        16) OpenMPI/4.1.4     22) foss/2022a
     5) numactl/2.0.14  11) libevent/2.1.12    17) OpenBLAS/0.3.20
     6) XZ/5.2.5        12) UCX/1.12.1         18) FlexiBLAS/3.2.0

The available modules are (use ``module av``)

.. code-block:: julia

   -------------------- /mnt/beegfs/apps/cn01470x/modules/all/MPI/GCC/11.3.0/OpenMPI/4.1.4 ---------------------
     ABINIT/9.6.2                       MUMPS/5.5.1-metis                     Valgrind/3.20.0
     ASE/3.22.1                         MultiQC/1.12                          Wannier90/3.1.0
     AmberTools/22.3                    NCO/5.1.0                             XCrySDen/1.6.2
     Arrow/8.0.0                        ORCA/5.0.3                            arpack-ng/3.8.0
     ArviZ/0.12.1                       OSU-Micro-Benchmarks/5.9              arrow-R/8.0.0-R-4.2.1
     Bambi/0.10.0                       OpenCV/4.6.0-contrib                  astropy/5.1.1
     Biopython/1.79                     OpenFOAM/v2206                        buildenv/default
     CGAL/4.14.3                        PLUMED/2.8.1                          ecCodes/2.27.0
     CP2K/8.2                           PSolver/1.8.3                         futile/1.8.3
     CheMPS2/1.8.12                     ParMETIS/4.0.3                        h5py/3.7.0
     Dalton/2020.0                      ParaView/5.10.1-mpi                   imkl-FFTW/2022.1.0
     ELPA/2021.11.001                   PnetCDF/1.12.3                        libGridXC/0.9.6
     ...
      
It is the same obtained previously by loading GCC/11.3.0 and OpenMPI/4.1.4.


3.2 Foss/2021b Toolchain
~~~~~~~~~~~~~~~~~~~~~~~~

The foss/2021b toolchain has the same software as the foss/2022a toolchain refereed in the previous subsection, but compiled against GCC/11.2.0 and in many cases having previous software versions. Let us explore this toolchain.

Changing to foss/2021b leads to (after using ``module load foss/2021b``)

.. code-block:: julia

   Inactive Modules:
     1) FFTW.MPI/3.3.10

   Due to MODULEPATH changes, the following have been reloaded:
     1) FFTW/3.3.10     2) UCC/1.0.0     3) XZ/5.2.5     4) libevent/2.1.12     5) libpciaccess/0.16     6) numactl/2.0.14

   The following have been reloaded with a version change:
     1) FlexiBLAS/3.2.0 => FlexiBLAS/3.0.4           8) UCX/1.12.1 => UCX/1.11.2
     2) GCC/11.3.0 => GCC/11.2.0                     9) binutils/2.38 => binutils/2.37
     3) GCCcore/11.3.0 => GCCcore/11.2.0            10) foss/2022a => foss/2021b
     4) OpenBLAS/0.3.20 => OpenBLAS/0.3.18          11) hwloc/2.7.1 => hwloc/2.5.0
     5) OpenMPI/4.1.4 => OpenMPI/4.1.1              12) libfabric/1.15.1 => libfabric/1.13.2
     6) PMIx/4.1.2 => PMIx/4.1.0                    13) libxml2/2.9.13 => libxml2/2.9.10
     7) ScaLAPACK/2.2.0-fb => ScaLAPACK/2.1.0-fb    14) zlib/1.2.12 => zlib/1.2.11
   
So, among others, GCC/11.3.0 and OpenMPI/4.1.4 were replaced by GCC/11.2.0 and OpenMPI/4.1.1, respectively. Similarly all the dependences, including the libraries managing the interconnects, where also adjusted accordingly.

The loaded and inactive modules are (``module list``)

.. code-block:: julia

   Currently Loaded Modules:
     1) OpenSSL/1.1      7) hwloc/2.5.0       13) FlexiBLAS/3.0.4     19) libevent/2.1.12
     2) GCCcore/11.2.0   8) UCX/1.11.2        14) ScaLAPACK/2.1.0-fb  20) UCC/1.0.0
     3) zlib/1.2.11      9) libfabric/1.13.2  15) foss/2021b          21) FFTW/3.3.10
     4) binutils/2.37   10) PMIx/4.1.0        16) numactl/2.0.14
     5) GCC/11.2.0      11) OpenMPI/4.1.1     17) XZ/5.2.5
     6) libxml2/2.9.10  12) OpenBLAS/0.3.18   18) libpciaccess/0.16

   Inactive Modules:
     1) FFTW.MPI/3.3.10
               
and the available modules are (``module av``)

.. code-block:: julia

   ---------------------- /mnt/beegfs/apps/cn01470x/modules/all/MPI/GCC/11.2.0/OpenMPI/4.1.1 -----------------------
     ABINIT/9.6.2                       MUMPS/5.4.1-metis                         VTK/9.1.0
     ASE/3.22.1                         MultiQC/1.12                              Valgrind/3.18.1
     AmberTools/22.3                    NCO/5.0.3                                 Wannier90/3.1.0
     Arrow/6.0.0                        ORCA/5.0.3                                XCrySDen/1.6.2
     ArviZ/0.11.4                       OSU-Micro-Benchmarks/5.7.1                arpack-ng/3.8.0
     Bambi/0.7.1                        OpenCV/4.5.5-contrib                      arrow-R/6.0.0.2-R-4.2.0
     Biopython/1.79                     OpenFOAM/v2112                            astropy/5.0.4
     CGAL/4.14.3                        PLUMED/2.8.0                              buildenv/default
     CP2K/8.2                           PSolver/1.8.3                             ecCodes/2.24.2
     CheMPS2/1.8.11                     ParMETIS/4.0.3                            futile/1.8.3
     Dalton/2020.0                      ParaView/5.9.1-mpi                        h5py/3.6.0
     ELPA/2021.05.001                   PnetCDF/1.12.3                            imkl-FFTW/2021.4.0
     ESMF/8.2.0                         PyMC3/3.11.1                              libGridXC/0.9.6
     FFTW/3.3.10                 (L)    QuantumESPRESSO/7.0                       libvdwxc/0.4.0
     FMS/2022.02                        R-bundle-Bioconductor/3.15-R-4.2.0        matplotlib/3.4.3
     GDAL/3.3.2                         R/4.2.0                                   ncview/2.1.8
     GPAW/22.8.0                        SCOTCH/6.1.2                              netCDF-C++4/4.3.1
     GROMACS/2021.5-PLUMED-2.8.0        SPOTPY/1.5.14                             netCDF-Fortran/4.5.3
     GROMACS/2021.5              (D)    SUNDIALS/6.3.0                            netCDF/4.8.1
     HDF/4.2.15                  (D)    ScaFaCoS/1.0.1                            netcdf4-python/1.5.7
     HDF5/1.12.1                        ScaLAPACK/2.1.0-fb                 (L)    networkx/2.6.3
     HPL/2.3                            SciPy-bundle/2021.10                      numba/0.54.1
     Hypre/2.24.0                       Score-P/8.0                               scikit-bio/0.5.7
     IMB/2021.3                         Siesta/4.1.5                              scikit-learn/1.0.2
     KaHIP/3.14                         SimPEG/0.18.1                             snakemake/6.10.0
     LAMMPS/23Jun2022-kokkos            SuiteSparse/5.10.1-METIS-5.1.0            spglib-python/1.16.3
     LMfit/1.0.3                        SuperLU/5.3.0                             statsmodels/0.13.1
     Libint/2.6.0-lmax-6-cp2k           TELEMAC-MASCARET/8p3r1                    worker/1.6.12
     MDAnalysis/2.0.0                   TensorFlow/2.8.4                          xarray/0.20.1
     MDTraj/1.9.7                       Theano/1.1.2-PyMC

  --------------------------- /mnt/beegfs/apps/cn01470x/modules/all/Compiler/GCC/11.2.0 ---------------------------
    BEDTools/2.30.0    FlexiBLAS/3.0.4 (L)    LAPACK/3.10.1          OpenMPI/4.1.5   (D)    libxc/4.3.4
    BLIS/0.8.1         Flye/2.9.1             MPICH/3.4.2            Pysam/0.17.0           libxc/5.1.6      (D)
    BamTools/2.5.2     GEOS/3.9.1             OpenBLAS/0.3.18 (L)    SAMtools/1.16.1        libxsmm/1.17
    Boost/1.77.0       GSL/2.7                OpenMPI/4.1.1   (L)    STAR/2.7.9a            pybedtools/0.8.2

  ------------------------- /mnt/beegfs/apps/cn01470x/modules/all/Compiler/GCCcore/11.2.0 -------------------------
    ANTLR/2.7.7-Java-11                 PCRE2/10.37                    hypothesis/6.14.6
    ATK/2.36.0                          PDT/3.25.1                     intltool/0.51.0
    Autoconf/2.71                       PMIx/4.1.0              (L)    jbigkit/2.1
    Automake/1.16.4                     PROJ/8.1.0                     kim-api/2.3.0
    Autotools/20210726                  Pango/1.48.8                   libGLU/9.0.2
    Bazel/4.2.2                         Perl/5.34.0-minimal            libarchive/3.5.1
    Bison/3.7.6                         Perl/5.34.0             (D)    libcerf/1.17
    ...
 
Most of the software, as can be seen in the top and middle rows, are similar to that available in foss/2022a, but was compiled with a dofferent version of GCC (11.2.0) and OpenMPI (4.1.1).

So, the user just should use the toolchain that suits better his needs. **There is a catch**, though, there are software that can be available in one toolchain but not in the other, e.g., PyTorch, PyTorch-Lightning, torchvision, torchsampler, tensorboard, etc., are available in foss/2022a but not in foss/2021b.


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
