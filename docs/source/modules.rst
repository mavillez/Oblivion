Environment Modules
===================

There are several conflicting software packages installed in the Oblivion supercomputer. In order for the user to use the adequate software there is a need to set the paths for the binaries, libraries, manuals, and includes. Either the user sets these paths for each package or the system provides an easy way to set those paths. The latter is the preferable and makes use of environment modules. 


1. The Basics - Core Modules and Toolchains
-------------------------------------------

The user sets the software environment by loading the modules associated to the packages he/she needs to use. This is easily done by using ``module load`` or ``module add``. Software dependences are set in the same way. OBLIVION uses a hierarchical module naming scheme (hmns) in which modules availability follows the software hierarchy Core/Compiler/MPI.

Core refers to the basic core modules that have to be loaded in order to have access to next levels of software compiled against a specific compiler and a MPI API.

After logging into the machine the user should execute the command ``module av`` (av for available) obtaining the list of core modules:

.. code-block:: julia

   ------------------------------ /mnt/beegfs/apps/4.7.x/modules/all/Core ------------------------------
      Bison/3.8.2           (D)     ant/1.10.11-Java-11        iimpi/2021b
      GCC/9.3.0                     binutils/2.34              imkl/2021.4.0
      GCC/11.2.0                    binutils/2.37              intel-compilers/2021.4.0
      GCCcore/9.3.0                 flex/2.6.4          (D)    intel/2021b
      GCCcore/11.2.0                foss/2021b                 ncurses/6.1
      GPAW-setups/0.9.20000 (D)     gettext/0.20.1             ncurses/6.2              (D)
      Java/11.0.16          (11)    gettext/0.21        (D)    pkgconf/1.8.0            (D)
      M4/1.4.19             (D)     gompi/2020a                zlib/1.2.11
      OpenSSL/1.1           (D)     gompi/2021b

   ------------------------------ /mnt/beegfs/apps/4.7.0/modules/all/Core ------------------------------
      Bison/3.8.2                  flex/2.6.4                      intel/2022a   (D)
      GCC/11.3.0            (D)    foss/2022a               (D)    iompi/2022a
      GCCcore/11.3.0        (D)    gettext/0.20.1                  ncurses/6.1
      GPAW-setups/0.9.20000        gettext/0.21                    ncurses/6.2
      M4/1.4.19                    gompi/2022a              (D)    pkgconf/1.8.0
      OpenSSL/1.1                  iimpi/2022a              (D)    zlib/1.2.11
      binutils/2.34                imkl/2022.1.0            (D)    zlib/1.2.12   (D)
      binutils/2.38         (D)    intel-compilers/2022.1.0 (D)

   -------------------------------------- /usr/share/modulefiles ---------------------------------------
      pmi/pmix-x86_64

   ------------------------------- /usr/share/lmod/lmod/modulefiles/Core -------------------------------
      lmod    settarg

      Where:
         Aliases:  Aliases exist: foo/1.2.3 (1.2) means that "module load foo/1.2" will load foo/1.2.3
         D:        Default Module

      Use "module spider" to find all possible modules and extensions.
      Use "module keyword key1 key2 ..." to search for all possible modules matching any of the "keys".


The list shows toolchains 

- foss/2021b, 
- foss/2022a, 
- intel/2021b, 
- intel/2022a

and sub-toolchains 

- gompi/2020a, 
- gompi/2021b, 
- gompi/2022a, 
- intel-compilers/2021.4.0, 
- intel-compilers/2022.1.0, 
- iimpi/2021b, 
- iimpi/2022a,
- iompi/2022a.

Toolchain foss includes the following software:

- C, C++ and Fortran compilers: GCC
- MPI implementation: OpenMPI
- BLAS and LAPACK implementation: OpenBLAS
- Parallel, distributed LAPACK implementation: ScaLAPACK
- Fourier transforms: FFTW

Toolchain intel includes the following software:

- C, C++ and Fortran compilers (icc/icpc/ifort)
- MPI implementation (Intel MPI)
- BLAS, LAPACK and fourier transforms: Intel MKL


2. Loading Modules
------------------

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

   ----------------------- /mnt/beegfs/apps/4.7.x/modules/all/Compiler/GCC/9.3.0 ------------------------
      OpenMPI/4.0.3

   --------------------- /mnt/beegfs/apps/4.7.x/modules/all/Compiler/GCCcore/9.3.0 ----------------------
      Autoconf/2.69          Perl/5.30.2      (D)    hwloc/2.2.0             ncurses/6.2        (D)
      Automake/1.16.1        UCX/1.8.0               libevent/2.1.11         numactl/2.0.13
      Autotools/20180311     XZ/5.2.5                libfabric/1.11.0        pkg-config/0.29.2
      Bison/3.5.3            binutils/2.34    (L)    libpciaccess/0.16       xorg-macros/1.19.2
      DB/18.1.32             expat/2.2.9             libreadline/8.0         zlib/1.2.11        (L)
      M4/1.4.18              flex/2.6.4       (D)    libtool/2.4.6
      PMIx/3.1.5             groff/1.22.4            libxml2/2.9.10
      Perl/5.30.2-minimal    help2man/1.47.12        makeinfo/6.7-minimal

   ------------------------------ /mnt/beegfs/apps/4.7.x/modules/all/Core -------------------------------
      Bison/3.8.2           (D)     ant/1.10.11-Java-11        iimpi/2021b
      GCC/9.3.0             (L)     binutils/2.34              imkl/2021.4.0
      GCC/11.2.0                    binutils/2.37              intel-compilers/2021.4.0
      GCCcore/9.3.0         (L)     flex/2.6.4                 intel/2021b
      GCCcore/11.2.0                foss/2021b                 ncurses/6.1
      GPAW-setups/0.9.20000 (D)     gettext/0.20.1             ncurses/6.2
      Java/11.0.16          (11)    gettext/0.21        (D)    pkgconf/1.8.0            (D)
      M4/1.4.19             (D)     gompi/2020a                zlib/1.2.11
      OpenSSL/1.1           (D)     gompi/2021b

   L:  Module is loaded
   D:  Default module


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
      1) GCC/9.3.0 => GCC/11.2.0     2) GCCcore/9.3.0 => GCCcore/11.2.0     3) binutils/2.34 => binutils/2.37

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

   ----------------------- /mnt/beegfs/apps/4.7.x/modules/all/Compiler/GCC/11.2.0 -----------------------
      BLIS/0.8.1         GEOS/3.9.1       OpenBLAS/0.3.18    libxc/5.1.6  (D)
      Boost/1.77.0       GSL/2.7          OpenMPI/4.1.1      libxsmm/1.17
      FlexiBLAS/3.0.4    LAPACK/3.10.1    libxc/4.3.4

   --------------------- /mnt/beegfs/apps/4.7.x/modules/all/Compiler/GCCcore/11.2.0 ---------------------
      ANTLR/2.7.7-Java-11                 Pillow/8.3.2                   libGLU/9.0.2
      ATK/2.36.0                          PyYAML/5.4.1                   libarchive/3.5.1
      Autoconf/2.71                       Python/2.7.18-bare             libcerf/1.17
      Automake/1.16.4                     Python/3.9.6-bare              libdap/3.20.8
      Autotools/20210726                  Python/3.9.6            (D)    libdrm/2.4.107
      Bazel/4.2.2                         Qhull/2020.2                   libepoxy/1.5.8
      Bison/3.7.6                         Qt5/5.15.2                     libevent/2.1.12
      Brotli/1.0.9                        Rust/1.54.0                    libfabric/1.13.2
      CMake/3.21.1                        SQLite/3.36                    libffi/3.4.2
      CMake/3.22.1                 (D)    Szip/2.1.1                     libgd/2.3.3
      DB/18.1.40                          Tcl/8.6.11                     libgeotiff/1.7.0
      DBus/1.13.18                        Tk/8.6.11                      libgit2/1.1.1
      Doxygen/1.9.1                       Tkinter/3.9.6                  libglvnd/1.3.3
      Eigen/3.3.9                         Togl/2.0                       libiconv/1.16
      FFmpeg/4.3.2                        UCX/1.11.2                     libjpeg-turbo/2.0.6
      FLAC/1.3.3                          UDUNITS/2.2.28                 libogg/1.3.5
      Flask/2.0.2                         UnZip/6.0                      libpciaccess/0.16          (L)
      FriBidi/1.0.10                      Voro++/0.4.6                   libpng/1.6.37
      GLPK/5.0                            X11/20210802                   libreadline/8.1
      GLib/2.69.1                         XZ/5.2.5                (L)    libsndfile/1.0.31
      GMP/6.2.1                           Xvfb/1.20.13                   libtirpc/1.3.2
      GObject-Introspection/1.68.0        Yasm/1.3.0                     libtool/2.4.6
      GTK3/3.24.31                        Zip/3.0                        libunwind/1.5.0
      Gdk-Pixbuf/2.42.6                   archspec/0.1.3                 libvorbis/1.3.7
      Ghostscript/9.54.0                  at-spi2-atk/2.38.0             libwebp/1.2.0
      HDF/4.2.15                          at-spi2-core/2.40.3            libxml2/2.9.10             (L)
      HarfBuzz/2.8.2                      attr/2.5.1                     libyaml/0.2.5
      ICU/69.1                            binutils/2.37           (L)    lz4/1.9.3
      ImageMagick/7.1.0-4                 bwidget/1.9.15                 make/4.3
      JasPer/2.0.33                       bzip2/1.0.8                    ncurses/6.2                (D)
      JsonCpp/1.9.4                       cURL/7.78.0                    nettle/3.7.3
      LAME/3.100                          cairo/1.16.0                   nodejs/14.17.6
      LLVM/12.0.1                         cppy/1.1.0                     nsync/1.24.0
      LMDB/0.9.29                         dill/0.3.4                     numactl/2.0.14
      LibTIFF/4.3.0                       double-conversion/3.1.5        pixman/0.40.0
      LittleCMS/2.12                      expat/2.4.1                    pkg-config/0.29.2
      Lua/5.4.3                           flatbuffers-python/2.0         pkgconf/1.8.0              (D)
      M4/1.4.19                    (D)    flatbuffers/2.0.0              pkgconfig/1.5.5-python
      METIS/5.1.0                         flex/2.6.4              (D)    protobuf-python/3.17.3
      MPFR/4.1.0                          fontconfig/2.13.94             protobuf/3.17.3
      Mako/1.1.4                          freetype/2.11.0                pybind11/2.7.1
      Mesa/21.1.7                         gettext/0.21            (D)    re2c/2.2
      Meson/0.58.2                        giflib/5.2.1                   scikit-build/0.11.1
      NASM/2.15.05                        git/2.33.1-nodocs              snappy/1.1.9
      NLopt/2.7.0                         gnuplot/5.4.2                  tbb/2020.3
      NSPR/4.32                           gperf/3.1                      tqdm/4.62.3
      NSS/3.69                            graphite2/1.3.14               typing-extensions/3.10.0.2
      Ninja/1.10.2                        groff/1.22.4                   util-linux/2.37
      OpenEXR/3.1.1                       gzip/1.10                      x264/20210613
      PCRE/8.45                           help2man/1.48.3                x265/3.5
      PCRE2/10.37                         hwloc/2.5.0                    xorg-macros/1.19.3
      PMIx/4.1.0                          hypothesis/6.14.6              xxd/8.2.4220
      PROJ/8.1.0                          intltool/0.51.0                zlib/1.2.11                (L)
      Pango/1.48.8                        jbigkit/2.1                    zstd/1.5.0
      Perl/5.34.0                         kim-api/2.3.0

   ------------------------------ /mnt/beegfs/apps/4.7.x/modules/all/Core -------------------------------
      Bison/3.8.2           (D)     ant/1.10.11-Java-11    iimpi/2021b
      GCC/9.3.0                     binutils/2.34          imkl/2021.4.0
      GCC/11.2.0            (L)     binutils/2.37          intel-compilers/2021.4.0
      GCCcore/9.3.0                 flex/2.6.4             intel/2021b
      GCCcore/11.2.0        (L)     foss/2021b             ncurses/6.1
      GPAW-setups/0.9.20000 (D)     gettext/0.20.1         ncurses/6.2
      Java/11.0.16          (11)    gettext/0.21           pkgconf/1.8.0
      M4/1.4.19                     gompi/2020a            zlib/1.2.11
      OpenSSL/1.1           (D)     gompi/2021b

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

   ------------------ /mnt/beegfs/apps/4.7.x/modules/all/MPI/GCC/11.2.0/OpenMPI/4.1.1 -------------------
      ABINIT/9.6.2                       Libint/2.6.0-lmax-6-cp2k          TensorFlow/2.8.4
      ASE/3.22.1                         MDTraj/1.9.7                      Theano/1.1.2-PyMC
      AmberTools/22.3                    MUMPS/5.4.1-metis                 VTK/9.1.0
      ArviZ/0.11.4                       ORCA/5.0.3                        Valgrind/3.18.1
      Bambi/0.7.1                        OSU-Micro-Benchmarks/5.7.1        Wannier90/3.1.0
      BigDFT/1.9.1                       OpenCV/4.5.5-contrib              XCrySDen/1.6.2
      Boost.MPI/1.77.0                   OpenFOAM/v2112                    arpack-ng/3.8.0
      CGAL/4.14.3                        PLUMED/2.8.0                      h5py/3.6.0
      CP2K/8.2                           ParMETIS/4.0.3                    libGridXC/0.9.6
      Dalton/2020.0                      ParaView/5.9.1-mpi                libvdwxc/0.4.0
      ELPA/2021.05.001                   PnetCDF/1.12.3                    matplotlib/3.4.3
      ESMF/8.2.0                         PyMC3/3.11.1                      mpi4py/3.1.4-Python-3.9.6
      FFTW/3.3.10                        QuantumESPRESSO/7.0               ncview/2.1.8
      FMS/2022.02                        R/4.2.0                           netCDF-C++4/4.3.1
      GDAL/3.3.2                         SCOTCH/6.1.2                      netCDF-Fortran/4.5.3
      GPAW/22.8.0                        ScaFaCoS/1.0.1                    netCDF/4.8.1
      GROMACS/2021.5-PLUMED-2.8.0        ScaLAPACK/2.1.0-fb                netcdf4-python/1.5.7
      GROMACS/2021.5              (D)    SciPy-bundle/2021.10              networkx/2.6.3
      HDF/4.2.15                  (D)    Siesta/4.1.5                      scikit-learn/1.0.2
      HDF5/1.12.1                        SuiteSparse/5.10.1-METIS-5.1.0    spglib-python/1.16.3
      IMB/2021.3                         SuperLU/5.3.0                     statsmodels/0.13.1
      LAMMPS/23Jun2022-kokkos            TELEMAC-MASCARET/8p3r1            xarray/0.20.1

   ----------------------- /mnt/beegfs/apps/4.7.x/modules/all/Compiler/GCC/11.2.0 -----------------------
      BLIS/0.8.1         GEOS/3.9.1       OpenBLAS/0.3.18        libxc/5.1.6  (D)
      Boost/1.77.0       GSL/2.7          OpenMPI/4.1.1   (L)    libxsmm/1.17
      FlexiBLAS/3.0.4    LAPACK/3.10.1    libxc/4.3.4

   --------------------- /mnt/beegfs/apps/4.7.x/modules/all/Compiler/GCCcore/11.2.0 ---------------------
      ANTLR/2.7.7-Java-11                 Pillow/8.3.2                   libGLU/9.0.2
      ATK/2.36.0                          PyYAML/5.4.1                   libarchive/3.5.1
      Autoconf/2.71                       Python/2.7.18-bare             libcerf/1.17
      Automake/1.16.4                     Python/3.9.6-bare              libdap/3.20.8
      Autotools/20210726                  Python/3.9.6            (D)    libdrm/2.4.107
      Bazel/4.2.2                         Qhull/2020.2                   libepoxy/1.5.8
   ...

Now the user got access to all the software that was compiled against OpenMPI-4.1.1. The top row displays the modules for software compiled against OpenMPI, which in turn was compiled with GCC compiler (second row of modules). The third row displays the core modules associated to GCC/11.2.0.

We could do all this in a single command by loading foss/2021b. So, let's check it. Start with a ``module purge`` followed with ``module av`` getting

.. code-block:: julia

   -------------------------------- /mnt/beegfs/apps/4.7.x/modules/all/Core ---------------------------------
      Bison/3.8.2           (D)     ant/1.10.11-Java-11        iimpi/2021b
      GCC/9.3.0                     binutils/2.34              imkl/2021.4.0
      GCC/11.2.0                    binutils/2.37              intel-compilers/2021.4.0
      GCCcore/9.3.0                 flex/2.6.4          (D)    intel/2021b
      GCCcore/11.2.0                foss/2021b                 ncurses/6.1
      GPAW-setups/0.9.20000 (D)     gettext/0.20.1             ncurses/6.2              (D)
      Java/11.0.16          (11)    gettext/0.21        (D)    pkgconf/1.8.0            (D)
      M4/1.4.19             (D)     gompi/2020a                zlib/1.2.11
      OpenSSL/1.1           (D)     gompi/2021b


Load foss/2021b (``module load foss/2021b``) and check what is available with ``module av`` getting

.. code-block:: julia

   -------------------- /mnt/beegfs/apps/4.7.x/modules/all/MPI/GCC/11.2.0/OpenMPI/4.1.1 ---------------------
      ABINIT/9.6.2                       Libint/2.6.0-lmax-6-cp2k              TensorFlow/2.8.4
      ASE/3.22.1                         MDTraj/1.9.7                          Theano/1.1.2-PyMC
      AmberTools/22.3                    MUMPS/5.4.1-metis                     VTK/9.1.0
      ArviZ/0.11.4                       ORCA/5.0.3                            Valgrind/3.18.1
      Bambi/0.7.1                        OSU-Micro-Benchmarks/5.7.1            Wannier90/3.1.0
      BigDFT/1.9.1                       OpenCV/4.5.5-contrib                  XCrySDen/1.6.2
      Boost.MPI/1.77.0                   OpenFOAM/v2112                        arpack-ng/3.8.0
      CGAL/4.14.3                        PLUMED/2.8.0                          h5py/3.6.0
      CP2K/8.2                           ParMETIS/4.0.3                        libGridXC/0.9.6
      ...
      
the same obtained previously by loading GCC/11.2.0 and OpenMPI/4.1.1.




Now the user only needs to load the modules of interest. For example, if a user wants to use ``TensorFlow/2.6.0`` he/she executes the following command:

.. code-block:: julia

  module load TensorFlow/2.6.0

or if the user wants to use ``GROMACS/2021.5`` then just execute

.. code-block:: julia

  module load GROMACS/2021.5

In the latter case the loaded modules, given by ``module list``, are

.. code-block:: julia

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


Purging Modules
~~~~~~~~~~~~~~~

The user can purge the loaded modules by executing 

.. code-block:: julia
  
  module purge
  
Often a user uses different environments for his/her processes. Hence, he/she needs to load and purge the loaded modules several times. An easy way to proceed is to save those module environments into a file, say <module_environment>, by using 

.. code-block:: julia

  module save <module_environment>. 
  
Later, the environment can be reloaded using the command 

.. code-block:: julia

  module restore <module_environment>

Module Details
~~~~~~~~~~~~~~

To learn further details of a module, how to load it, and dependencies use 

.. code-block:: julia

  module spider <module_name>

and to find detailed information of a module use

.. code-block:: julia

  module spider <module_name/version>
  
e.g., ``intel/2021a``

.. code-block:: julia

   $ module spider intel/2021a

   -----------------------------------------------------------------------------------------------
   intel: intel/2021a
   -----------------------------------------------------------------------------------------------
    Description:
      Compiler toolchain including Intel compilers, Intel MPI and Intel Math Kernel Library (MKL).

    This module can be loaded directly: module load intel/2021a

    Help:
      
      Description
      ===========
      Compiler toolchain including Intel compilers, Intel MPI and Intel Math Kernel Library (MKL).
      
      More information
      ================
       - Homepage: https://easybuild.readthedocs.io/en/master/Common-toolchains.html#intel-toolchain

 
2. List of Commonly Used commands
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


3. Available Modules
--------------------

To list all the available modules the user can use the command ``module spider`` obtaining

.. code-block:: julia

  ABINIT: ABINIT/9.4.2, ABINIT/9.6.2
    ABINIT is a package whose main program allows one to find the total energy, charge density and electronic structure of systems made of electrons and nuclei (molecules and periodic solids) within Density
    Functional Theory (DFT), using pseudopotentials and a planewave or wavelet basis.

  ANSYS_CFD: ANSYS_CFD/192, ANSYS_CFD/2021R1
    ANSYS computational fluid dynamics (CFD) simulation software allows you to predict, with confidence, the impact of fluid flows on your product throughout design and manufacturing as well as during end
    use. ANSYS renowned CFD analysis tools include the widely used and well-validated ANSYS Fluent and ANSYS CFX.

  ASE: ASE/3.22.0
    ASE is a python package providing an open source Atomic Simulation Environment in the Python scripting language. From version 3.20.1 we also include the ase-ext package, it contains optional
    reimplementations in C of functions in ASE. ASE uses it automatically when installed.

  Autoconf: Autoconf/2.71
    Autoconf is an extensible package of M4 macros that produce shell scripts to automatically configure software source code packages. These scripts can adapt the packages to many kinds of UNIX-like systems
    without manual user intervention. Autoconf creates a configuration script for a package from a template file that lists the operating system features that the package can use, in the form of M4 macro
    calls.

  Automake: Automake/1.16.3
    Automake: GNU Standards-compliant Makefile generator

  Autotools: Autotools/20210128
    This bundle collect the standard GNU build tools: Autoconf, Automake and libtool 

  Bazel: Bazel/3.7.2
    Bazel is a build tool that builds code quickly and reliably. It is used to build the majority of Google's software.

  Bison: Bison/3.7.6, Bison/3.8.2
    Bison is a general-purpose parser generator that converts an annotated context-free grammar into a deterministic LR or generalized LR (GLR) parser employing LALR(1) parser tables.

  Boost: Boost/1.76.0
    Boost provides free peer-reviewed portable C++ source libraries.

  Brotli: Brotli/1.0.9
    Brotli is a generic-purpose lossless compression algorithm that compresses data using a combination of a modern variant of the LZ77 algorithm, Huffman coding and 2nd order context modeling, with a
    compression ratio comparable to the best currently available general-purpose compression methods. It is similar in speed with deflate but offers more dense compression. The specification of the Brotli
    Compressed Data Format is defined in RFC 7932.

  CGAL: CGAL/4.14.3
    The goal of the CGAL Open Source Project is to provide easy access to efficient and reliable geometric algorithms in the form of a C++ library.

  CLEASE: CLEASE/0.10.6
    CLuster Expansion in Atomic Simulation Environment (CLEASE) is a package that automates the cumbersome setup and construction procedure of cluster expansion (CE). It provides a comprehensive list of tools
    for specifying parameters for CE, generating training structures, fitting effective cluster interaction (ECI) values and running Monte Carlo simulations.

  CMake: CMake/3.20.1
    CMake, the cross-platform, open-source build system. CMake is a family of tools designed to build, test and package software. 

  CP2K: CP2K/8.2
    CP2K is a freely available (GPL) program, written in Fortran 95, to perform atomistic and molecular simulations of solid state, liquid, molecular and biological systems. It provides a general framework
    for different methods such as e.g. density functional theory (DFT) using a mixed Gaussian and plane waves approach (GPW), and classical pair and many-body potentials. 

  CubeGUI: CubeGUI/4.6
    Cube, which is used as performance report explorer for Scalasca and Score-P, is a generic tool for displaying a multi-dimensional performance space consisting of the dimensions (i) performance metric,
    (ii) call path, and (iii) system resource. Each dimension can be represented as a tree, where non-leaf nodes of the tree can be collapsed or expanded to achieve the desired level of granularity. This
    module provides the Cube graphical report explorer. 

  CubeLib: CubeLib/4.6
    Cube, which is used as performance report explorer for Scalasca and Score-P, is a generic tool for displaying a multi-dimensional performance space consisting of the dimensions (i) performance metric,
    (ii) call path, and (iii) system resource. Each dimension can be represented as a tree, where non-leaf nodes of the tree can be collapsed or expanded to achieve the desired level of granularity. This
    module provides the Cube general purpose C++ library component and command-line tools. 

  CubeWriter: CubeWriter/4.6
    Cube, which is used as performance report explorer for Scalasca and Score-P, is a generic tool for displaying a multi-dimensional performance space consisting of the dimensions (i) performance metric,
    (ii) call path, and (iii) system resource. Each dimension can be represented as a tree, where non-leaf nodes of the tree can be collapsed or expanded to achieve the desired level of granularity. This
    module provides the Cube high-performance C writer library component. 

  DB: DB/18.1.40
    Berkeley DB enables the development of custom data management solutions, without the overhead traditionally associated with such custom projects.

  DBus: DBus/1.13.18
    D-Bus is a message bus system, a simple way for applications to talk to one another. In addition to interprocess communication, D-Bus helps coordinate process lifecycle; it makes it simple and reliable to
    code a "single instance" application or daemon, and to launch applications and daemons on demand when their services are needed. 

  DFTB+: DFTB+/21.1
    DFTB+ is a fast and efficient versatile quantum mechanical simulation package. It is based on the Density Functional Tight Binding (DFTB) method, containing almost all of the useful extensions which have
    been developed for the DFTB framework so far. Using DFTB+ you can carry out quantum mechanical simulations like with ab-initio density functional theory based packages, but in an approximate way gaining
    typically around two order of magnitude in speed.

  DIRAC: DIRAC/22.0
    DIRAC: Program for Atomic and Molecular Direct Iterative Relativistic All-electron Calculations

  Doxygen: Doxygen/1.9.1
    Doxygen is a documentation system for C++, C, Java, Objective-C, Python, IDL (Corba and Microsoft flavors), Fortran, VHDL, PHP, C#, and to some extent D. 

  ELPA: ELPA/2021.05.001
    Eigenvalue SoLvers for Petaflop-Applications .

  ELSI: ELSI/2.7.1-PEXSI
    ELSI provides and enhances scalable, open-source software library solutions for electronic structure calculations in materials science, condensed matter physics, chemistry, and many other fields. ELSI
    focuses on methods that solve or circumvent eigenvalue problems in electronic structure theory. The ELSI infrastructure should also be useful for other challenging eigenvalue problems. 

  Eigen: Eigen/3.3.9
    Eigen is a C++ template library for linear algebra: matrices, vectors, numerical solvers, and related algorithms.

  FFTW: FFTW/3.3.9
    FFTW is a C subroutine library for computing the discrete Fourier transform (DFT) in one or more dimensions, of arbitrary input size, and of both real and complex data.

  FFmpeg: FFmpeg/4.3.2
    A complete, cross-platform solution to record, convert and stream audio and video.

  FLAC: FLAC/1.3.3
    FLAC stands for Free Lossless Audio Codec, an audio format similar to MP3, but lossless, meaning that audio is compressed in FLAC without any loss in quality.

  Flask: Flask/1.1.4
    Flask is a lightweight WSGI web application framework. It is designed to make getting started quick and easy, with the ability to scale up to complex applications. This module includes the Flask
    extensions: Flask-Cors

  FlexiBLAS: FlexiBLAS/3.0.4
    FlexiBLAS is a wrapper library that enables the exchange of the BLAS and LAPACK implementation used by a program without recompiling or relinking it.

  FriBidi: FriBidi/1.0.10
    The Free Implementation of the Unicode Bidirectional Algorithm. 

  GCC: GCC/10.3.0
    The GNU Compiler Collection includes front ends for C, C++, Objective-C, Fortran, Java, and Ada, as well as libraries for these languages (libstdc++, libgcj,...).

  GCCcore: GCCcore/10.3.0
    The GNU Compiler Collection includes front ends for C, C++, Objective-C, Fortran, Java, and Ada, as well as libraries for these languages (libstdc++, libgcj,...).

  GDAL: GDAL/3.3.0
    GDAL is a translator library for raster geospatial data formats that is released under an X/MIT style Open Source license by the Open Source Geospatial Foundation. As a library, it presents a single
    abstract data model to the calling application for all supported formats. It also comes with a variety of useful commandline utilities for data translation and processing.

  GEOS: GEOS/3.9.1
    GEOS (Geometry Engine - Open Source) is a C++ port of the Java Topology Suite (JTS)

  GLPK: GLPK/5.0
    The GLPK (GNU Linear Programming Kit) package is intended for solving large-scale linear programming (LP), mixed integer programming (MIP), and other related problems. It is a set of routines written in
    ANSI C and organized in the form of a callable library.

  GLib: GLib/2.68.2
    GLib is one of the base libraries of the GTK+ project

  GMP: GMP/6.2.1
    GMP is a free library for arbitrary precision arithmetic, operating on signed integers, rational numbers, and floating point numbers. 

  GObject-Introspection: GObject-Introspection/1.68.0
    GObject introspection is a middleware layer between C libraries (using GObject) and language bindings. The C library can be scanned at compile time and generate a metadata file, in addition to the actual
    native C library. Then at runtime, language bindings can read this metadata and automatically provide bindings to call into the C library.

  GPAW: GPAW/21.6.0
    GPAW is a density-functional theory (DFT) Python code based on the projector-augmented wave (PAW) method and the atomic simulation environment (ASE). It uses real-space uniform grids and multigrid methods
    or atom-centered basis-functions.

  GPAW-setups: GPAW-setups/0.9.20000
    PAW setup for the GPAW Density Functional Theory package. Users can install setups manually using 'gpaw install-data' or use setups from this package. The versions of GPAW and GPAW-setups can be
    intermixed.

  GROMACS: GROMACS/2021.3, GROMACS/2021.5
    GROMACS is a versatile package to perform molecular dynamics, i.e. simulate the Newtonian equations of motion for systems with hundreds to millions of particles. This is a CPU only build, containing both
    MPI and threadMPI builds for both single and double precision. It also contains the gmxapi extension for the single precision MPI build. 

  GSL: GSL/2.7
    The GNU Scientific Library (GSL) is a numerical library for C and C++ programmers. The library provides a wide range of mathematical routines such as random number generators, special functions and
    least-squares fitting.

  Ghostscript: Ghostscript/9.54.0
    Ghostscript is a versatile processor for PostScript data with the ability to render PostScript to different targets. It used to be part of the cups printing stack, but is no longer used for that.

  GlobalArrays: GlobalArrays/5.8
    Global Arrays (GA) is a Partitioned Global Address Space (PGAS) programming model

  HDF: HDF/4.2.15
    HDF (also known as HDF4) is a library and multi-object file format for storing and managing data between machines. 

  HDF5: HDF5/1.10.7, HDF5/1.12.1
    HDF5 is a data model, library, and file format for storing and managing data. It supports an unlimited variety of datatypes, and is designed for flexible and efficient I/O and for high volume and complex
    data.

  HarfBuzz: HarfBuzz/2.8.1
    HarfBuzz is an OpenType text shaping engine.

  ICU: ICU/69.1
    ICU is a mature, widely used set of C/C++ and Java libraries providing Unicode and Globalization support for software applications.

  ImageMagick: ImageMagick/7.0.11-14
    ImageMagick is a software suite to create, edit, compose, or convert bitmap images

  JasPer: JasPer/2.0.28
    The JasPer Project is an open-source initiative to provide a free software-based reference implementation of the codec specified in the JPEG-2000 Part-1 standard. 

  Java: Java/11.0.2
    Java Platform, Standard Edition (Java SE) lets you develop and deploy Java applications on desktops and servers.

  JsonCpp: JsonCpp/1.9.4
    JsonCpp is a C++ library that allows manipulating JSON values, including serialization and deserialization to and from strings. It can also preserve existing comment in unserialization/serialization
    steps, making it a convenient format to store user input files. 

  LAME: LAME/3.100
    LAME is a high quality MPEG Audio Layer III (MP3) encoder licensed under the LGPL.

  LLVM: LLVM/11.1.0
    The LLVM Core libraries provide a modern source- and target-independent optimizer, along with code generation support for many popular CPUs (as well as some less common ones!) These libraries are built
    around a well specified code representation known as the LLVM intermediate representation ("LLVM IR"). The LLVM Core libraries are well documented, and it is particularly easy to invent your own language
    (or port an existing compiler) to use LLVM as an optimizer and code generator.

  LMDB: LMDB/0.9.28
    LMDB is a fast, memory-efficient database. With memory-mapped files, it has the read performance of a pure in-memory database while retaining the persistence of standard disk-based databases.

  LibTIFF: LibTIFF/4.2.0
    tiff: Library and tools for reading and writing TIFF data files

  Libint: Libint/2.6.0-lmax-6-cp2k
    Libint library is used to evaluate the traditional (electron repulsion) and certain novel two-body matrix elements (integrals) over Cartesian Gaussian functions used in modern atomic and molecular theory.

  LittleCMS: LittleCMS/2.12
    Little CMS intends to be an OPEN SOURCE small-footprint color management engine, with special focus on accuracy and performance. 

  Lua: Lua/5.4.3
    Lua is a powerful, fast, lightweight, embeddable scripting language. Lua combines simple procedural syntax with powerful data description constructs based on associative arrays and extensible semantics.
    Lua is dynamically typed, runs by interpreting bytecode for a register-based virtual machine, and has automatic memory management with incremental garbage collection, making it ideal for configuration,
    scripting, and rapid prototyping.

  M4: M4/1.4.18, M4/1.4.19
    GNU M4 is an implementation of the traditional Unix macro processor. It is mostly SVR4 compatible although it has some extensions (for example, handling more than 9 positional parameters to macros). GNU
    M4 also has built-in functions for including files, running shell commands, doing arithmetic, etc.

  METIS: METIS/5.1.0
    METIS is a set of serial programs for partitioning graphs, partitioning finite element meshes, and producing fill reducing orderings for sparse matrices. The algorithms implemented in METIS are based on
    the multilevel recursive-bisection, multilevel k-way, and multi-constraint partitioning schemes. 

  MPFR: MPFR/4.1.0
    The MPFR library is a C library for multiple-precision floating-point computations with correct rounding. 

  Mako: Mako/1.1.4
    A super-fast templating language that borrows the best ideas from the existing templating languages

  Mesa: Mesa/21.1.1
    Mesa is an open-source implementation of the OpenGL specification - a system for rendering interactive 3D graphics.

  Meson: Meson/0.58.0
    Meson is a cross-platform build system designed to be both as fast and as user friendly as possible.

  NASM: NASM/2.15.05
    NASM: General-purpose x86 assembler

  NLopt: NLopt/2.7.0
    NLopt is a free/open-source library for nonlinear optimization, providing a common interface for a number of different free optimization routines available online as well as original implementations of
    various other algorithms. 

  NSPR: NSPR/4.30
    Netscape Portable Runtime (NSPR) provides a platform-neutral API for system level and libc-like functions.

  NSS: NSS/3.65
    Network Security Services (NSS) is a set of libraries designed to support cross-platform development of security-enabled client and server applications.

  NTPoly: NTPoly/2.7.0
    is a massively parallel library for computing the functions of sparse, symmetric matrices based on polynomial expansions. For sufficiently sparse matrices, most of the matrix functions in NTPoly can be
    computed in linear time.

  NWChem: NWChem/7.0.2
    NWChem aims to provide its users with computational chemistry tools that are scalable both in their ability to treat large scientific computational chemistry problems efficiently, and in their use of
    available parallel computing resources from high-performance parallel supercomputers to conventional workstation clusters. NWChem software can handle: biomolecules, nanostructures, and solid-state; from
    quantum to classical, and all combinations; Gaussian basis functions or plane-waves; scaling from one to thousands of processors; properties and relativity.

  Ninja: Ninja/1.10.2
    Ninja is a small build system with a focus on speed.

  OPARI2: OPARI2/2.0.6
    OPARI2, the successor of Forschungszentrum Juelich's OPARI, is a source-to-source instrumentation tool for OpenMP and hybrid codes. It surrounds OpenMP directives and runtime library calls with calls to
    the POMP2 measurement interface. 

  ORCA: ORCA/5.0.2
    ORCA is a flexible, efficient and easy-to-use general purpose tool for quantum chemistry with specific emphasis on spectroscopic properties of open-shell molecules. It features a wide variety of standard
    quantum chemical methods ranging from semiempirical methods to DFT to single- and multireference correlated ab initio methods. It can also treat environmental and relativistic effects.

  OTF2: OTF2/2.3
    The Open Trace Format 2 is a highly scalable, memory efficient event trace data format plus support library. It is the new standard trace format for Scalasca, Vampir, and TAU and is open for other tools. 

  OpenBLAS: OpenBLAS/0.3.15
    OpenBLAS is an optimized BLAS library based on GotoBLAS2 1.13 BSD version.

  OpenFOAM: OpenFOAM/v2106
    OpenFOAM is a free, open source CFD software package. OpenFOAM has an extensive range of features to solve anything from complex fluid flows involving chemical reactions, turbulence and heat transfer, to
    solid dynamics and electromagnetics.

  OpenMPI: OpenMPI/4.1.1
    The Open MPI Project is an open source MPI-3 implementation.

  OpenSSL: OpenSSL/1.1
    The OpenSSL Project is a collaborative effort to develop a robust, commercial-grade, full-featured, and Open Source toolchain implementing the Secure Sockets Layer (SSL v2/v3) and Transport Layer Security
    (TLS v1) protocols as well as a full-strength general purpose cryptography library. 

  PAPI: PAPI/6.0.0.1
    PAPI provides the tool designer and application engineer with a consistent interface and methodology for use of the performance counter hardware found in most major microprocessors. PAPI enables software
    engineers to see, in near real time, the relation between software performance and processor events. In addition Component PAPI provides access to a collection of components that expose performance
    measurement opportunites across the hardware and software stack. 

  PCRE: PCRE/8.44
    The PCRE library is a set of functions that implement regular expression pattern matching using the same syntax and semantics as Perl 5. 

  PCRE2: PCRE2/10.36
    The PCRE library is a set of functions that implement regular expression pattern matching using the same syntax and semantics as Perl 5. 

  PDT: PDT/3.25.1
    Program Database Toolkit (PDT) is a framework for analyzing source code written in several programming languages and for making rich program knowledge accessible to developers of static and dynamic
    analysis tools. PDT implements a standard program representation, the program database (PDB), that can be accessed in a uniform way through a class library supporting common PDB operations. 

  PLUMED: PLUMED/2.7.2
    PLUMED is an open source library for free energy calculations in molecular systems which works together with some of the most popular molecular dynamics engines. Free energy calculations can be performed
    as a function of many order parameters with a particular focus on biological problems, using state of the art methods such as metadynamics, umbrella sampling and Jarzynski-equation based steered MD. The
    software, written in C++, can be easily interfaced with both fortran and C/C++ codes. 

  PMIx: PMIx/3.2.3
    Process Management for Exascale Environments PMI Exascale (PMIx) represents an attempt to provide an extended version of the PMI standard specifically designed to support clusters up to and including
    exascale sizes. The overall objective of the project is not to branch the existing pseudo-standard definitions - in fact, PMIx fully supports both of the existing PMI-1 and PMI-2 APIs - but rather to (a)
    augment and extend those APIs to eliminate some current restrictions that impact scalability, and (b) provide a reference implementation of the PMI-server that demonstrates the desired level of
    scalability. 

  PROJ: PROJ/8.0.1
    Program proj is a standard Unix filter function which converts geographic longitude and latitude coordinates into cartesian coordinates

  PSolver: PSolver/1.8.3
    Interpolating scaling function Poisson Solver Library 

  Pango: Pango/1.48.5
    Pango is a library for laying out and rendering of text, with an emphasis on internationalization. Pango can be used anywhere that text layout is needed, though most of the work on Pango so far has been
    done in the context of the GTK+ widget toolkit. Pango forms the core of text and font handling for GTK+-2.x.

  ParaView: ParaView/5.9.1-mpi
    ParaView is a scientific parallel visualizer.

  Perl: Perl/5.32.1-minimal, Perl/5.32.1
    Larry Wall's Practical Extraction and Report Language This is a minimal build without any modules. Should only be used for build dependencies. 

  Pillow: Pillow/8.2.0
    Pillow is the 'friendly PIL fork' by Alex Clark and Contributors. PIL is the Python Imaging Library by Fredrik Lundh and Contributors.

  Pillow-SIMD: Pillow-SIMD/8.2.0
    Pillow is the 'friendly PIL fork' by Alex Clark and Contributors. PIL is the Python Imaging Library by Fredrik Lundh and Contributors.

  PnetCDF: PnetCDF/1.12.2
    Parallel netCDF: A Parallel I/O Library for NetCDF File Access

  PyTorch: PyTorch/1.10.0
    Tensors and Dynamic neural networks in Python with strong GPU acceleration. PyTorch is a deep learning framework that puts Python first.

  PyYAML: PyYAML/5.4.1
    PyYAML is a YAML parser and emitter for the Python programming language.

  Python: Python/2.7.18-bare, Python/3.9.5-bare, Python/3.9.5
    Python is a programming language that lets you work more quickly and integrate your systems more effectively.

  Qhull: Qhull/2020.2
    Qhull computes the convex hull, Delaunay triangulation, Voronoi diagram, halfspace intersection about a point, furthest-site Delaunay triangulation, and furthest-site Voronoi diagram. The source code runs
    in 2-d, 3-d, 4-d, and higher dimensions. Qhull implements the Quickhull algorithm for computing the convex hull. 

  Qt5: Qt5/5.15.2
    Qt is a comprehensive cross-platform C++ application framework.

  QuantumESPRESSO: QuantumESPRESSO/6.8
    Quantum ESPRESSO is an integrated suite of computer codes for electronic-structure calculations and materials modeling at the nanoscale. It is based on density-functional theory, plane waves, and
    pseudopotentials (both norm-conserving and ultrasoft). 

  R: R/4.1.2
    R is a free software environment for statistical computing and graphics.

  Rust: Rust/1.52.1
    Rust is a systems programming language that runs blazingly fast, prevents segfaults, and guarantees thread safety.

  SCOTCH: SCOTCH/6.1.0
    Software package and libraries for sequential and parallel graph partitioning, static mapping, and sparse matrix block ordering, and sequential mesh and hypergraph partitioning.

  SIONlib: SIONlib/1.7.6-tools
    SIONlib is a scalable I/O library for parallel access to task-local files. The library not only supports writing and reading binary data to or from several thousands of processors into a single or a small
    number of physical files, but also provides global open and close functions to access SIONlib files in parallel. This package provides a stripped-down installation of SIONlib for use with performance
    tools (e.g., Score-P), with renamed symbols to avoid conflicts when an application using SIONlib itself is linked against a tool requiring a different SIONlib version. 

  SQLite: SQLite/3.35.4
    SQLite: SQL Database Engine in a C Library

  ScaLAPACK: ScaLAPACK/2.1.0-fb
    The ScaLAPACK (or Scalable LAPACK) library includes a subset of LAPACK routines redesigned for distributed memory MIMD parallel computers.

  Scalasca: Scalasca/2.6
    Scalasca is a software tool that supports the performance optimization of parallel programs by measuring and analyzing their runtime behavior. The analysis identifies potential performance bottlenecks --
    in particular those concerning communication and synchronization -- and offers guidance in exploring their causes. 

  SciPy-bundle: SciPy-bundle/2021.05
    Bundle of Python packages for scientific software

  Score-P: Score-P/7.0
    The Score-P measurement infrastructure is a highly scalable and easy-to-use tool suite for profiling, event tracing, and online analysis of HPC applications. 

  Szip: Szip/2.1.1
    Szip compression software, providing lossless compression of scientific data 

  Tcl: Tcl/8.6.11
    Tcl (Tool Command Language) is a very powerful but easy to learn dynamic programming language, suitable for a very wide range of uses, including web and desktop applications, networking, administration,
    testing and many more. 

  TensorFlow: TensorFlow/2.6.0
    An open-source software library for Machine Intelligence

  Tk: Tk/8.6.11
    Tk is an open source, cross-platform widget toolchain that provides a library of basic elements for building a graphical user interface (GUI) in many different programming languages.

  Tkinter: Tkinter/3.9.5
    Tkinter module, built with the Python buildsystem

  UCX: UCX/1.10.0
    Unified Communication X An open-source production grade communication framework for data centric and high-performance applications 

  UDUNITS: UDUNITS/2.2.28
    UDUNITS supports conversion of unit specifications between formatted and binary forms, arithmetic manipulation of units, and conversion of values between compatible scales of measurement.

  UnZip: UnZip/6.0
    UnZip is an extraction utility for archives compressed in .zip format (also called "zipfiles"). Although highly compatible both with PKWARE's PKZIP and PKUNZIP utilities for MS-DOS and with Info-ZIP's own
    Zip program, our primary objectives have been portability and non-MSDOS functionality.

  VTK: VTK/9.0.1
    The Visualization Toolkit (VTK) is an open-source, freely available software system for 3D computer graphics, image processing and visualization. VTK consists of a C++ class library and several
    interpreted interface layers including Tcl/Tk, Java, and Python. VTK supports a wide variety of visualization algorithms including: scalar, vector, tensor, texture, and volumetric methods; and advanced
    modeling techniques such as: implicit modeling, polygon reduction, mesh smoothing, cutting, contouring, and Delaunay triangulation.

  Valgrind: Valgrind/3.17.0
    Valgrind: Debugging and profiling tools

  Wannier90: Wannier90/3.1.0
    A tool for obtaining maximally-localised Wannier functions

  X11: X11/20210518
    The X Window System (X11) is a windowing system for bitmap displays

  XZ: XZ/5.2.5
    xz: XZ utilities

  Xvfb: Xvfb/1.20.11
    Xvfb is an X server that can run on machines with no display hardware and no physical input devices. It emulates a dumb framebuffer using virtual memory.

  Yasm: Yasm/1.3.0
    Yasm: Complete rewrite of the NASM assembler with BSD license

  Zip: Zip/3.0
    Zip is a compression and file packaging/archive utility. Although highly compatible both with PKWARE's PKZIP and PKUNZIP utilities for MS-DOS and with Info-ZIP's own UnZip, our primary objectives have
    been portability and other-than-MSDOS functionality

  binutils: binutils/2.36.1
    binutils: GNU binary utilities

  bio/GROMACS: bio/GROMACS/2020.4-foss-2020a-Python-3.8.2
    GROMACS is a versatile package to perform molecular dynamics, i.e. simulate the Newtonian equations of motion for systems with hundreds to millions of particles. This is a CPU only build, containing both
    MPI and threadMPI builds for both single and double precision. It also contains the gmxapi extension for the single precision MPI build. 

  bzip2: bzip2/1.0.8
    bzip2 is a freely available, patent free, high-quality data compressor. It typically compresses files to within 10% to 15% of the best available techniques (the PPM family of statistical compressors),
    whilst being around twice as fast at compression and six times faster at decompression. 

  cURL: cURL/7.76.0
    libcurl is a free and easy-to-use client-side URL transfer library, supporting DICT, FILE, FTP, FTPS, Gopher, HTTP, HTTPS, IMAP, IMAPS, LDAP, LDAPS, POP3, POP3S, RTMP, RTSP, SCP, SFTP, SMTP, SMTPS, Telnet
    and TFTP. libcurl supports SSL certificates, HTTP POST, HTTP PUT, FTP uploading, HTTP form based upload, proxies, cookies, user+password authentication (Basic, Digest, NTLM, Negotiate, Kerberos), file
    transfer resume, http proxy tunneling and more. 

  cae/OpenFOAM: cae/OpenFOAM/v2006-foss-2020a
    OpenFOAM is a free, open source CFD software package. OpenFOAM has an extensive range of features to solve anything from complex fluid flows involving chemical reactions, turbulence and heat transfer, to
    solid dynamics and electromagnetics.

  cairo: cairo/1.16.0
    Cairo is a 2D graphics library with support for multiple output devices. Currently supported output targets include the X Window System (via both Xlib and XCB), Quartz, Win32, image buffers, PostScript,
    PDF, and SVG file output. Experimental backends include OpenGL, BeOS, OS/2, and DirectFB

  chem/GAMESS-US: chem/GAMESS-US/20200930-R2-foss-2020a, chem/GAMESS-US/20200930-R2-intel-2020a
    The General Atomic and Molecular Electronic Structure System (GAMESS) is a general ab initio quantum chemistry package. 

  chem/QuantumESPRESSO: chem/QuantumESPRESSO/6.6-intel-2020a
    Quantum ESPRESSO is an integrated suite of computer codes for electronic-structure calculations and materials modeling at the nanoscale. It is based on density-functional theory, plane waves, and
    pseudopotentials (both norm-conserving and ultrasoft). 

  chem/libxc: chem/libxc/4.3.4-iccifort-2020.1.217
    Libxc is a library of exchange-correlation functionals for density-functional theory. The aim is to provide a portable, well tested and reliable set of exchange and correlation functionals.

  compiler/Clang: compiler/Clang/11.0.0-GCCcore-9.3.0
    C, C++, Objective-C compiler, based on LLVM. Does not include C++ standard library -- use libstdc++ from GCC.

  compiler/GCC: compiler/GCC/9.3.0
    The GNU Compiler Collection includes front ends for C, C++, Objective-C, Fortran, Java, and Ada, as well as libraries for these languages (libstdc++, libgcj,...).

  compiler/GCCcore: compiler/GCCcore/9.3.0
    The GNU Compiler Collection includes front ends for C, C++, Objective-C, Fortran, Java, and Ada, as well as libraries for these languages (libstdc++, libgcj,...).

  compiler/LLVM: compiler/LLVM/9.0.1-GCCcore-9.3.0
    The LLVM Core libraries provide a modern source- and target-independent optimizer, along with code generation support for many popular CPUs (as well as some less common ones!) These libraries are built
    around a well specified code representation known as the LLVM intermediate representation ("LLVM IR"). The LLVM Core libraries are well documented, and it is particularly easy to invent your own language
    (or port an existing compiler) to use LLVM as an optimizer and code generator.

  compiler/iccifort: compiler/iccifort/2020.1.217
    Intel C, C++ & Fortran compilers

  cppy: cppy/1.1.0
    A small C++ header library which makes it easier to write Python extension modules. The primary feature is a PyObject smart pointer which automatically handles reference counting and provides convenience
    methods for performing common object operations.

  data/GDAL: data/GDAL/3.0.4-foss-2020a-Python-3.8.2
    GDAL is a translator library for raster geospatial data formats that is released under an X/MIT style Open Source license by the Open Source Geospatial Foundation. As a library, it presents a single
    abstract data model to the calling application for all supported formats. It also comes with a variety of useful commandline utilities for data translation and processing.

  data/HDF: data/HDF/4.2.15-GCCcore-9.3.0
    HDF (also known as HDF4) is a library and multi-object file format for storing and managing data between machines. 

  data/HDF5: data/HDF5/1.10.5-iimpi-2020a, data/HDF5/1.10.6-gompi-2020a, data/HDF5/1.10.6-iimpi-2020a, data/HDF5/1.12.0-gompi-2020a, data/HDF5/1.12.0-iimpi-2020a
    HDF5 is a data model, library, and file format for storing and managing data. It supports an unlimited variety of datatypes, and is designed for flexible and efficient I/O and for high volume and complex
    data.

  data/LAME: data/LAME/3.100-GCCcore-9.3.0
    LAME is a high quality MPEG Audio Layer III (MP3) encoder licensed under the LGPL.

  data/h5py: data/h5py/2.10.0-foss-2020a-Python-3.8.2
    HDF5 for Python (h5py) is a general-purpose Python interface to the Hierarchical Data Format library, version 5. HDF5 is a versatile, mature scientific software library designed for the fast, flexible
    storage of enormous amounts of data.

  data/netCDF: data/netCDF/4.7.4-gompi-2020a, data/netCDF/4.7.4-iimpi-2020a
    NetCDF (network Common Data Form) is a set of software libraries and machine-independent data formats that support the creation, access, and sharing of array-oriented scientific data.

  data/netCDF-C++4: data/netCDF-C++4/4.3.1-gompi-2020a, data/netCDF-C++4/4.3.1-iimpi-2020a
    NetCDF (network Common Data Form) is a set of software libraries and machine-independent data formats that support the creation, access, and sharing of array-oriented scientific data.

  data/netCDF-Fortran: data/netCDF-Fortran/4.5.2-gompi-2020a, data/netCDF-Fortran/4.5.2-iimpi-2020a
    NetCDF (network Common Data Form) is a set of software libraries and machine-independent data formats that support the creation, access, and sharing of array-oriented scientific data.

  data/scikit-learn: data/scikit-learn/0.23.1-foss-2020a-Python-3.8.2, data/scikit-learn/0.23.1-intel-2020a-Python-3.8.2
    Scikit-learn integrates machine learning algorithms in the tightly-knit scientific Python world, building upon numpy, scipy, and matplotlib. As a machine-learning module, it provides versatile tools for
    data mining and analysis in any field of science and engineering. It strives to be simple and efficient, accessible to everybody, and reusable in various contexts.

  debugger/Valgrind: debugger/Valgrind/3.16.1-gompi-2020a
    Valgrind: Debugging and profiling tools

  devel/Autoconf: devel/Autoconf/2.69-GCCcore-9.3.0
    Autoconf is an extensible package of M4 macros that produce shell scripts to automatically configure software source code packages. These scripts can adapt the packages to many kinds of UNIX-like systems
    without manual user intervention. Autoconf creates a configuration script for a package from a template file that lists the operating system features that the package can use, in the form of M4 macro
    calls. 

  devel/Automake: devel/Automake/1.16.1-GCCcore-9.3.0
    Automake: GNU Standards-compliant Makefile generator

  devel/Autotools: devel/Autotools/20180311-GCCcore-9.3.0
    This bundle collect the standard GNU build tools: Autoconf, Automake and libtool 

  devel/Bazel: devel/Bazel/3.6.0-GCCcore-9.3.0
    Bazel is a build tool that builds code quickly and reliably. It is used to build the majority of Google's software.

  devel/Boost: devel/Boost/1.72.0-gompi-2020a
    Boost provides free peer-reviewed portable C++ source libraries.

  devel/CMake: devel/CMake/3.16.4-GCCcore-9.3.0
    CMake, the cross-platform, open-source build system. CMake is a family of tools designed to build, test and package software. 

  devel/DBus: devel/DBus/1.13.12-GCCcore-9.3.0
    D-Bus is a message bus system, a simple way for applications to talk to one another. In addition to interprocess communication, D-Bus helps coordinate process lifecycle; it makes it simple and reliable to
    code a "single instance" application or daemon, and to launch applications and daemons on demand when their services are needed. 

  devel/Doxygen: devel/Doxygen/1.8.17-GCCcore-9.3.0
    Doxygen is a documentation system for C++, C, Java, Objective-C, Python, IDL (Corba and Microsoft flavors), Fortran, VHDL, PHP, C#, and to some extent D. 

  devel/GObject-Introspection: devel/GObject-Introspection/1.64.0-GCCcore-9.3.0-Python-3.8.2
    GObject introspection is a middleware layer between C libraries (using GObject) and language bindings. The C library can be scanned at compile time and generate a metadata file, in addition to the actual
    native C library. Then at runtime, language bindings can read this metadata and automatically provide bindings to call into the C library.

  devel/M4: devel/M4/1.4.18-GCCcore-9.3.0, devel/M4/1.4.18
    GNU M4 is an implementation of the traditional Unix macro processor. It is mostly SVR4 compatible although it has some extensions (for example, handling more than 9 positional parameters to macros). GNU
    M4 also has built-in functions for including files, running shell commands, doing arithmetic, etc.

  devel/Mako: devel/Mako/1.1.2-GCCcore-9.3.0
    A super-fast templating language that borrows the best ideas from the existing templating languages

  devel/PCRE: devel/PCRE/8.44-GCCcore-9.3.0
    The PCRE library is a set of functions that implement regular expression pattern matching using the same syntax and semantics as Perl 5. 

  devel/PCRE2: devel/PCRE2/10.34-GCCcore-9.3.0
    The PCRE library is a set of functions that implement regular expression pattern matching using the same syntax and semantics as Perl 5. 

  devel/Qt5: devel/Qt5/5.14.1-GCCcore-9.3.0
    Qt is a comprehensive cross-platform C++ application framework.

  devel/SQLite: devel/SQLite/3.31.1-GCCcore-9.3.0
    SQLite: SQL Database Engine in a C Library

  devel/SWIG: devel/SWIG/4.0.1-GCCcore-9.3.0
    SWIG is a software development tool that connects programs written in C and C++ with a variety of high-level programming languages.

  devel/flatbuffers: devel/flatbuffers/1.12.0-GCCcore-9.3.0
    FlatBuffers: Memory Efficient Serialization Library

  devel/gperf: devel/gperf/3.1-GCCcore-9.3.0
    GNU gperf is a perfect hash function generator. For a given list of strings, it produces a hash function and hash table, in form of C or C++ code, for looking up a value depending on the input string. The
    hash function is perfect, which means that the hash table has no collisions, and the hash table lookup needs a single string comparison only. 

  devel/intltool: devel/intltool/0.51.0-GCCcore-9.3.0
    intltool is a set of tools to centralize translation of many different file formats using GNU gettext-compatible PO files.

  devel/makeinfo: devel/makeinfo/6.7-GCCcore-9.3.0
    makeinfo is part of the Texinfo project, the official documentation format of the GNU project.

  devel/ncurses: devel/ncurses/6.1, devel/ncurses/6.2-GCCcore-9.3.0
    The Ncurses (new curses) library is a free software emulation of curses in System V Release 4.0, and more. It uses Terminfo format, supports pads and color and multiple highlights and forms characters and
    function-key mapping, and has all the other SYSV-curses enhancements over BSD Curses.

  devel/nsync: devel/nsync/1.24.0-GCCcore-9.3.0
    nsync is a C library that exports various synchronization primitives, such as mutexes

  devel/pkg-config: devel/pkg-config/0.29.2-GCCcore-9.3.0
    pkg-config is a helper tool used when compiling applications and libraries. It helps you insert the correct compiler options on the command line so an application can use gcc -o test test.c `pkg-config
    --libs --cflags glib-2.0` for instance, rather than hard-coding values on where to find glib (or other libraries). 

  devel/pkgconfig: devel/pkgconfig/1.5.1-GCCcore-9.3.0-Python-3.8.2
    pkgconfig is a Python module to interface with the pkg-config command line tool

  devel/protobuf: devel/protobuf/3.13.0-GCCcore-9.3.0
    Google Protocol Buffers

  devel/protobuf-python: devel/protobuf-python/3.13.0-foss-2020a-Python-3.8.2
    Python Protocol Buffers runtime library.

  devel/xorg-macros: devel/xorg-macros/1.19.2-GCCcore-9.3.0
    X.org macros utilities.

  dftd3-lib: dftd3-lib/0.9
    This is a repackaged version of the DFTD3 program by S. Grimme and his coworkers. The original program (V3.1 Rev 1) was downloaded at 2016-04-03. It has been converted to free format and encapsulated into
    modules.

  double-conversion: double-conversion/3.1.5
    Efficient binary-decimal and decimal-binary conversion routines for IEEE doubles.

  expat: expat/2.2.9
    Expat is an XML parser library written in C. It is a stream-oriented parser in which an application registers handlers for things the parser might find in the XML document (like start tags) 

  expecttest: expecttest/0.1.3
    This library implements expect tests (also known as "golden" tests). Expect tests are a method of writing tests where instead of hard-coding the expected output of a test, you run the test to get the
    output, and the test framework automatically populates the expected output. If the output of the test changes, you can rerun the test with the environment variable EXPECTTEST_ACCEPT=1 to automatically
    update the expected output.

  flatbuffers: flatbuffers/2.0.0
    FlatBuffers: Memory Efficient Serialization Library

  flatbuffers-python: flatbuffers-python/2.0
    Python Flatbuffers runtime library.

  flex: flex/2.6.4
    Flex (Fast Lexical Analyzer) is a tool for generating scanners. A scanner, sometimes called a tokenizer, is a program which recognizes lexical patterns in text. 

  fontconfig: fontconfig/2.13.93
    Fontconfig is a library designed to provide system-wide font configuration, customization and application access. 

  foss: foss/2021a
    GNU Compiler Collection (GCC) based compiler toolchain, including OpenMPI for MPI support, OpenBLAS (BLAS and LAPACK support), FFTW and ScaLAPACK.

  freetype: freetype/2.10.4
    FreeType 2 is a software font engine that is designed to be small, efficient, highly customizable, and portable while capable of producing high-quality output (glyph images). It can be used in graphics
    libraries, display servers, font conversion tools, text image generation tools, and many other products as well. 

  futile: futile/1.8.3
    The FUTILE project (Fortran Utilities for the Treatment of Innermost Level of Executables) is a set of modules and wrapper that encapsulate the most common low-level operations of a Fortran code. 

  geo/rgdal: geo/rgdal/1.5-16-foss-2020a-R-4.0.0
    Provides bindings to the 'Geospatial' Data Abstraction Library ('GDAL') (>= 1.11.4 and <= 2.5.0) and access to projection/transformation operations from the 'PROJ.4' library.

  gettext: gettext/0.21
    GNU 'gettext' is an important step for the GNU Translation Project, as it is an asset on which we may build many other steps. This package offers to programmers, translators, and even users, a well
    integrated set of tools and documentation

  giflib: giflib/5.2.1
    giflib is a library for reading and writing gif images. It is API and ABI compatible with libungif which was in wide use while the LZW compression algorithm was patented.

  git: git/2.32.0-nodocs
    Git is a free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency.

  gnuplot: gnuplot/5.4.2
    Portable interactive, function plotting utility

  gompi: gompi/2021a
    GNU Compiler Collection (GCC) based compiler toolchain, including OpenMPI for MPI support.

  gperf: gperf/3.1
    GNU gperf is a perfect hash function generator. For a given list of strings, it produces a hash function and hash table, in form of C or C++ code, for looking up a value depending on the input string. The
    hash function is perfect, which means that the hash table has no collisions, and the hash table lookup needs a single string comparison only. 

  groff: groff/1.22.4
    Groff (GNU troff) is a typesetting system that reads plain text mixed with formatting commands and produces formatted output.

  gzip: gzip/1.10
    gzip (GNU zip) is a popular data compression program as a replacement for compress

  h5py: h5py/3.2.1
    HDF5 for Python (h5py) is a general-purpose Python interface to the Hierarchical Data Format library, version 5. HDF5 is a versatile, mature scientific software library designed for the fast, flexible
    storage of enormous amounts of data.

  help2man: help2man/1.48.3
    help2man produces simple manual pages from the '--help' and '--version' output of other commands.

  hwloc: hwloc/2.4.1
    The Portable Hardware Locality (hwloc) software package provides a portable abstraction (across OS, versions, architectures, ...) of the hierarchical topology of modern architectures, including NUMA
    memory nodes, sockets, shared caches, cores and simultaneous multithreading. It also gathers various system attributes such as cache and memory information as well as the locality of I/O devices such as
    network interfaces, InfiniBand HCAs or GPUs. It primarily aims at helping applications with gathering information about modern computing hardware so as to exploit it accordingly and efficiently. 

  hypothesis: hypothesis/6.13.1
    Hypothesis is an advanced testing library for Python. It lets you write tests which are parametrized by a source of examples, and then generates simple and comprehensible examples that make your tests
    fail. This lets you find more bugs in your code with less work.

  iimpi: iimpi/2021a
    Intel C/C++ and Fortran compilers, alongside Intel MPI.

  imkl: imkl/2021.2.0
    Intel oneAPI Math Kernel Library

  impi: impi/2021.2.0
    Intel MPI Library, compatible with MPICH ABI

  intel: intel/2021a
    Compiler toolchain including Intel compilers, Intel MPI and Intel Math Kernel Library (MKL).

  intel-compilers: intel-compilers/2021.2.0
    Intel C, C++ & Fortran compilers (classic and oneAPI)

  intltool: intltool/0.51.0
    intltool is a set of tools to centralize translation of many different file formats using GNU gettext-compatible PO files.

  iompi: iompi/2021a
    Intel C/C++ and Fortran compilers, alongside Open MPI.

  jbigkit: jbigkit/2.1
    JBIG-KIT is a software implementation of the JBIG1 data compression standard (ITU-T T.82), which was designed for bi-level image data, such as scanned documents.

  lang/Bison: lang/Bison/3.3.2, lang/Bison/3.5.3-GCCcore-9.3.0, lang/Bison/3.5.3
    Bison is a general-purpose parser generator that converts an annotated context-free grammar into a deterministic LR or generalized LR (GLR) parser employing LALR(1) parser tables. 

  lang/FriBidi: lang/FriBidi/1.0.9-GCCcore-9.3.0
    The Free Implementation of the Unicode Bidirectional Algorithm. 

  lang/Java: lang/Java/1.8_191-b26-OpenJDK, lang/Java/11.0.2
    An open-source implementation of the Java Platform, Standard Edition

  lang/Julia: lang/Julia/1.5.1-linux-x86_64
    Julia is a high-level, high-performance dynamic programming language for numerical computing

  lang/Lua: lang/Lua/5.3.5-GCCcore-9.3.0
    Lua is a powerful, fast, lightweight, embeddable scripting language. Lua combines simple procedural syntax with powerful data description constructs based on associative arrays and extensible semantics.
    Lua is dynamically typed, runs by interpreting bytecode for a register-based virtual machine, and has automatic memory management with incremental garbage collection, making it ideal for configuration,
    scripting, and rapid prototyping.

  lang/NASM: lang/NASM/2.14.02-GCCcore-9.3.0
    NASM: General-purpose x86 assembler

  lang/Perl: lang/Perl/5.30.2-GCCcore-9.3.0
    Larry Wall's Practical Extraction and Report Language

  lang/Python: lang/Python/2.7.18-GCCcore-9.3.0, lang/Python/3.8.2-GCCcore-9.3.0
    Python is a programming language that lets you work more quickly and integrate your systems more effectively.

  lang/R: lang/R/4.0.0-foss-2020a
    R is a free software environment for statistical computing and graphics.

  lang/SciPy-bundle: lang/SciPy-bundle/2020.03-foss-2020a-Python-3.8.2, lang/SciPy-bundle/2020.03-intel-2020a-Python-3.8.2
    Bundle of Python packages for scientific software

  lang/Tcl: lang/Tcl/8.6.10-GCCcore-9.3.0
    Tcl (Tool Command Language) is a very powerful but easy to learn dynamic programming language, suitable for a very wide range of uses, including web and desktop applications, networking, administration,
    testing and many more. 

  lang/Yasm: lang/Yasm/1.3.0-GCCcore-9.3.0
    Yasm: Complete rewrite of the NASM assembler with BSD license

  lang/flex: lang/flex/2.6.4-GCCcore-9.3.0, lang/flex/2.6.4
    Flex (Fast Lexical Analyzer) is a tool for generating scanners. A scanner, sometimes called a tokenizer, is a program which recognizes lexical patterns in text. 

  lib/ICU: lib/ICU/66.1-GCCcore-9.3.0
    ICU is a mature, widely used set of C/C++ and Java libraries providing Unicode and Globalization support for software applications.

  lib/JsonCpp: lib/JsonCpp/1.9.4-GCCcore-9.3.0
    JsonCpp is a C++ library that allows manipulating JSON values, including serialization and deserialization to and from strings. It can also preserve existing comment in unserialization/serialization
    steps, making it a convenient format to store user input files. 

  lib/LMDB: lib/LMDB/0.9.24-GCCcore-9.3.0
    LMDB is a fast, memory-efficient database. With memory-mapped files, it has the read performance of a pure in-memory database while retaining the persistence of standard disk-based databases.

  lib/LibTIFF: lib/LibTIFF/4.1.0-GCCcore-9.3.0
    tiff: Library and tools for reading and writing TIFF data files

  lib/NSPR: lib/NSPR/4.25-GCCcore-9.3.0
    Netscape Portable Runtime (NSPR) provides a platform-neutral API for system level and libc-like functions.

  lib/NSS: lib/NSS/3.51-GCCcore-9.3.0
    Network Security Services (NSS) is a set of libraries designed to support cross-platform development of security-enabled client and server applications.

  lib/PMIx: lib/PMIx/3.1.5-GCCcore-9.3.0
    Process Management for Exascale Environments PMI Exascale (PMIx) represents an attempt to provide an extended version of the PMI standard specifically designed to support clusters up to and including
    exascale sizes. The overall objective of the project is not to branch the existing pseudo-standard definitions - in fact, PMIx fully supports both of the existing PMI-1 and PMI-2 APIs - but rather to (a)
    augment and extend those APIs to eliminate some current restrictions that impact scalability, and (b) provide a reference implementation of the PMI-server that demonstrates the desired level of
    scalability. 

  lib/PROJ: lib/PROJ/7.0.0-GCCcore-9.3.0
    Program proj is a standard Unix filter function which converts geographic longitude and latitude coordinates into cartesian coordinates

  lib/SIONlib: lib/SIONlib/1.7.6-GCCcore-9.3.0-tools
    SIONlib is a scalable I/O library for parallel access to task-local files. The library not only supports writing and reading binary data to or from several thousands of processors into a single or a small
    number of physical files, but also provides global open and close functions to access SIONlib files in parallel. This package provides a stripped-down installation of SIONlib for use with performance
    tools (e.g., Score-P), with renamed symbols to avoid conflicts when an application using SIONlib itself is linked against a tool requiring a different SIONlib version. 

  lib/TensorFlow: lib/TensorFlow/2.3.1-foss-2020a-Python-3.8.2
    An open-source software library for Machine Intelligence

  lib/UCX: lib/UCX/1.8.0-GCCcore-9.3.0
    Unified Communication X An open-source production grade communication framework for data centric and high-performance applications 

  lib/double-conversion: lib/double-conversion/3.1.5-GCCcore-9.3.0
    Efficient binary-decimal and decimal-binary conversion routines for IEEE doubles.

  lib/giflib: lib/giflib/5.2.1-GCCcore-9.3.0
    giflib is a library for reading and writing gif images. It is API and ABI compatible with libungif which was in wide use while the LZW compression algorithm was patented.

  lib/libdrm: lib/libdrm/2.4.100-GCCcore-9.3.0
    Direct Rendering Manager runtime library.

  lib/libevent: lib/libevent/2.1.11-GCCcore-9.3.0
    The libevent API provides a mechanism to execute a callback function when a specific event occurs on a file descriptor or after a timeout has been reached. Furthermore, libevent also support callbacks due
    to signals or regular timeouts. 

  lib/libfabric: lib/libfabric/1.11.0-GCCcore-9.3.0
    Libfabric is a core component of OFI. It is the library that defines and exports the user-space API of OFI, and is typically the only software that applications deal with directly. It works in conjunction
    with provider libraries, which are often integrated directly into libfabric. 

  lib/libffi: lib/libffi/3.3-GCCcore-9.3.0
    The libffi library provides a portable, high level programming interface to various calling conventions. This allows a programmer to call any function specified by a call interface description at
    run-time.

  lib/libgd: lib/libgd/2.3.0-GCCcore-9.3.0
    GD is an open source code library for the dynamic creation of images by programmers.

  lib/libgeotiff: lib/libgeotiff/1.5.1-GCCcore-9.3.0
    Library for reading and writing coordinate system information from/to GeoTIFF files

  lib/libglvnd: lib/libglvnd/1.2.0-GCCcore-9.3.0
    libglvnd is a vendor-neutral dispatch layer for arbitrating OpenGL API calls between multiple vendors.

  lib/libiconv: lib/libiconv/1.16-GCCcore-9.3.0
    Libiconv converts from one character encoding to another through Unicode conversion

  lib/libjpeg-turbo: lib/libjpeg-turbo/2.0.4-GCCcore-9.3.0
    libjpeg-turbo is a fork of the original IJG libjpeg which uses SIMD to accelerate baseline JPEG compression and decompression. libjpeg is a library that implements JPEG image encoding, decoding and
    transcoding. 

  lib/libpng: lib/libpng/1.6.37-GCCcore-9.3.0
    libpng is the official PNG reference library

  lib/libreadline: lib/libreadline/8.0-GCCcore-9.3.0
    The GNU Readline library provides a set of functions for use by applications that allow users to edit command lines as they are typed in. Both Emacs and vi editing modes are available. The Readline
    library includes additional functions to maintain a list of previously-entered command lines, to recall and perhaps reedit those lines, and perform csh-like history expansion on previous commands. 

  lib/libsndfile: lib/libsndfile/1.0.28-GCCcore-9.3.0
    Libsndfile is a C library for reading and writing files containing sampled sound (such as MS Windows WAV and the Apple/SGI AIFF format) through one standard library interface.

  lib/libtirpc: lib/libtirpc/1.2.6-GCCcore-9.3.0
    Libtirpc is a port of Suns Transport-Independent RPC library to Linux.

  lib/libtool: lib/libtool/2.4.6-GCCcore-9.3.0
    GNU libtool is a generic library support script. Libtool hides the complexity of using shared libraries behind a consistent, portable interface. 

  lib/libunwind: lib/libunwind/1.3.1-GCCcore-9.3.0
    The primary goal of libunwind is to define a portable and efficient C programming interface (API) to determine the call-chain of a program. The API additionally provides the means to manipulate the
    preserved (callee-saved) state of each call-frame and to resume execution at any point in the call-chain (non-local goto). The API supports both local (same-process) and remote (across-process) operation.
    As such, the API is useful in a number of applications

  lib/libxml2: lib/libxml2/2.9.10-GCCcore-9.3.0
    Libxml2 is the XML C parser and toolchain developed for the Gnome project (but usable outside of the Gnome platform). 

  lib/lz4: lib/lz4/1.9.2-GCCcore-9.3.0
    LZ4 is lossless compression algorithm, providing compression speed at 400 MB/s per core. It features an extremely fast decoder, with speed in multiple GB/s per core.

  lib/nettle: lib/nettle/3.6-GCCcore-9.3.0
    Nettle is a cryptographic library that is designed to fit easily in more or less any context: In crypto toolkits for object-oriented languages (C++, Python, Pike, ...), in applications like LSH or GNUPG,
    or even in kernel space.

  lib/pybind11: lib/pybind11/2.4.3-GCCcore-9.3.0-Python-3.8.2
    pybind11 is a lightweight header-only library that exposes C++ types in Python and vice versa, mainly to create Python bindings of existing C++ code.

  lib/scikit-build: lib/scikit-build/0.10.0-foss-2020a-Python-3.8.2
    Scikit-Build, or skbuild, is an improved build system generator for CPython C/C++/Fortran/Cython extensions.

  lib/snappy: lib/snappy/1.1.8-GCCcore-9.3.0
    Snappy is a compression/decompression library. It does not aim for maximum compression, or compatibility with any other compression library; instead, it aims for very high speeds and reasonable
    compression.

  lib/zlib: lib/zlib/1.2.11-GCCcore-9.3.0, lib/zlib/1.2.11
    zlib is designed to be a free, general-purpose, legally unencumbered -- that is, not covered by any patents -- lossless data-compression library for use on virtually any computer hardware and operating
    system.

  lib/zstd: lib/zstd/1.4.4-GCCcore-9.3.0
    Zstandard is a real-time compression algorithm, providing high compression ratios. It offers a very wide range of compression/speed trade-off, while being backed by a very fast decoder. It also offers a
    special mode for small data, called dictionary compression, and can create dictionaries from any sample set.

  libGLU: libGLU/9.0.1
    The OpenGL Utility Library (GLU) is a computer graphics library for OpenGL. 

  libarchive: libarchive/3.5.1
    Multi-format archive and compression library 

  libcerf: libcerf/1.17
    libcerf is a self-contained numeric library that provides an efficient and accurate implementation of complex error functions, along with Dawson, Faddeeva, and Voigt functions. 

  libdrm: libdrm/2.4.106
    Direct Rendering Manager runtime library.

  libevent: libevent/2.1.12
    The libevent API provides a mechanism to execute a callback function when a specific event occurs on a file descriptor or after a timeout has been reached. Furthermore, libevent also support callbacks due
    to signals or regular timeouts. 

  libfabric: libfabric/1.12.1
    Libfabric is a core component of OFI. It is the library that defines and exports the user-space API of OFI, and is typically the only software that applications deal with directly. It works in conjunction
    with provider libraries, which are often integrated directly into libfabric. 

  libffi: libffi/3.3
    The libffi library provides a portable, high level programming interface to various calling conventions. This allows a programmer to call any function specified by a call interface description at
    run-time.

  libgd: libgd/2.3.1
    GD is an open source code library for the dynamic creation of images by programmers.

  libgeotiff: libgeotiff/1.6.0
    Library for reading and writing coordinate system information from/to GeoTIFF files

  libgit2: libgit2/1.1.0
    libgit2 is a portable, pure C implementation of the Git core methods provided as a re-entrant linkable library with a solid API, allowing you to write native speed custom Git applications in any language
    which supports C bindings.

  libglvnd: libglvnd/1.3.3
    libglvnd is a vendor-neutral dispatch layer for arbitrating OpenGL API calls between multiple vendors.

  libiconv: libiconv/1.16
    Libiconv converts from one character encoding to another through Unicode conversion

  libjpeg-turbo: libjpeg-turbo/2.0.6
    libjpeg-turbo is a fork of the original IJG libjpeg which uses SIMD to accelerate baseline JPEG compression and decompression. libjpeg is a library that implements JPEG image encoding, decoding and
    transcoding. 

  libogg: libogg/1.3.4
    Ogg is a multimedia container format, and the native file and stream format for the Xiph.org multimedia codecs.

  libpciaccess: libpciaccess/0.16
    Generic PCI access library.

  libpng: libpng/1.6.37
    libpng is the official PNG reference library

  libreadline: libreadline/8.1
    The GNU Readline library provides a set of functions for use by applications that allow users to edit command lines as they are typed in. Both Emacs and vi editing modes are available. The Readline
    library includes additional functions to maintain a list of previously-entered command lines, to recall and perhaps reedit those lines, and perform csh-like history expansion on previous commands. 

  libsndfile: libsndfile/1.0.31
    Libsndfile is a C library for reading and writing files containing sampled sound (such as MS Windows WAV and the Apple/SGI AIFF format) through one standard library interface.

  libtirpc: libtirpc/1.3.2
    Libtirpc is a port of Suns Transport-Independent RPC library to Linux.

  libtool: libtool/2.4.6
    GNU libtool is a generic library support script. Libtool hides the complexity of using shared libraries behind a consistent, portable interface. 

  libunwind: libunwind/1.4.0
    The primary goal of libunwind is to define a portable and efficient C programming interface (API) to determine the call-chain of a program. The API additionally provides the means to manipulate the
    preserved (callee-saved) state of each call-frame and to resume execution at any point in the call-chain (non-local goto). The API supports both local (same-process) and remote (across-process) operation.
    As such, the API is useful in a number of applications

  libvdwxc: libvdwxc/0.4.0
    libvdwxc is a general library for evaluating energy and potential for exchange-correlation (XC) functionals from the vdW-DF family that can be used with various of density functional theory (DFT) codes.

  libvorbis: libvorbis/1.3.7
    Ogg Vorbis is a fully open, non-proprietary, patent-and-royalty-free, general-purpose compressed audio format

  libxc: libxc/5.1.5
    Libxc is a library of exchange-correlation functionals for density-functional theory. The aim is to provide a portable, well tested and reliable set of exchange and correlation functionals.

  libxml2: libxml2/2.9.10
    Libxml2 is the XML C parser and toolchain developed for the Gnome project (but usable outside of the Gnome platform). 

  libxslt: libxslt/1.1.34
    Libxslt is the XSLT C library developed for the GNOME project (but usable outside of the Gnome platform).

  libxsmm: libxsmm/1.16.2
    LIBXSMM is a library for small dense and small sparse matrix-matrix multiplications targeting Intel Architecture (x86).

  libyaml: libyaml/0.2.5
    LibYAML is a YAML parser and emitter written in C.

  lmod: lmod
    Lmod: An Environment Module System

  lxml: lxml/4.6.3
    The lxml XML toolkit is a Pythonic binding for the C libraries libxml2 and libxslt.

  lz4: lz4/1.9.3
    LZ4 is lossless compression algorithm, providing compression speed at 400 MB/s per core. It features an extremely fast decoder, with speed in multiple GB/s per core.

  makeinfo: makeinfo/6.7-minimal
    makeinfo is part of the Texinfo project, the official documentation format of the GNU project. This is a minimal build with very basic functionality. Should only be used for build dependencies. 

  math/ELPA: math/ELPA/2019.11.001-intel-2020a
    Eigenvalue SoLvers for Petaflop-Applications .

  math/Eigen: math/Eigen/3.3.7-GCCcore-9.3.0
    Eigen is a C++ template library for linear algebra: matrices, vectors, numerical solvers, and related algorithms.

  math/GEOS: math/GEOS/3.8.1-GCC-9.3.0-Python-3.8.2
    GEOS (Geometry Engine - Open Source) is a C++ port of the Java Topology Suite (JTS)

  math/GMP: math/GMP/6.2.0-GCCcore-9.3.0
    GMP is a free library for arbitrary precision arithmetic, operating on signed integers, rational numbers, and floating point numbers. 

  math/METIS: math/METIS/5.1.0-GCCcore-9.3.0
    METIS is a set of serial programs for partitioning graphs, partitioning finite element meshes, and producing fill reducing orderings for sparse matrices. The algorithms implemented in METIS are based on
    the multilevel recursive-bisection, multilevel k-way, and multi-constraint partitioning schemes. 

  math/MPFR: math/MPFR/4.0.2-GCCcore-9.3.0
    The MPFR library is a C library for multiple-precision floating-point computations with correct rounding. 

  math/NetLogo: math/NetLogo/6.0.4-64
    NetLogo is a multi-agent programmable modeling environment. It is used by tens of thousands of students, teachers and researchers worldwide. It also powers HubNet participatory simulations. It is authored
    by Uri Wilensky and developed at the CCL.

  math/SCOTCH: math/SCOTCH/6.0.9-gompi-2020a
    Software package and libraries for sequential and parallel graph partitioning, static mapping, and sparse matrix block ordering, and sequential mesh and hypergraph partitioning.

  math/Theano: math/Theano/1.0.4-foss-2020a-Python-3.8.2
    Theano is a Python library that allows you to define, optimize, and evaluate mathematical expressions involving multi-dimensional arrays efficiently.

  math/libcerf: math/libcerf/1.13-GCCcore-9.3.0
    libcerf is a self-contained numeric library that provides an efficient and accurate implementation of complex error functions, along with Dawson, Faddeeva, and Voigt functions. 

  matplotlib: matplotlib/3.4.2
    matplotlib is a python 2D plotting library which produces publication quality figures in a variety of hardcopy formats and interactive environments across platforms. matplotlib can be used in python
    scripts, the python and ipython shell, web application servers, and six graphical user interface toolkits.

  mpi/OpenMPI: mpi/OpenMPI/4.0.3-GCC-9.3.0, mpi/OpenMPI/4.0.3-iccifort-2020.1.217
    The Open MPI Project is an open source MPI-3 implementation.

  mpi/impi: mpi/impi/2019.7.217-iccifort-2020.1.217
    Intel MPI Library, compatible with MPICH ABI

  ncurses: ncurses/6.2
    The Ncurses (new curses) library is a free software emulation of curses in System V Release 4.0, and more. It uses Terminfo format, supports pads and color and multiple highlights and forms characters and
    function-key mapping, and has all the other SYSV-curses enhancements over BSD Curses. 

  ncview: ncview/2.1.8
    Ncview is a visual browser for netCDF format files. Typically you would use ncview to get a quick and easy, push-button look at your netCDF files. You can view simple movies of the data, view along
    various dimensions, take a look at the actual data values, change color maps, invert the data, etc.

  netCDF: netCDF/4.8.0
    NetCDF (network Common Data Form) is a set of software libraries and machine-independent data formats that support the creation, access, and sharing of array-oriented scientific data.

  netCDF-C++4: netCDF-C++4/4.3.1
    NetCDF (network Common Data Form) is a set of software libraries and machine-independent data formats that support the creation, access, and sharing of array-oriented scientific data.

  netCDF-Fortran: netCDF-Fortran/4.5.3
    NetCDF (network Common Data Form) is a set of software libraries and machine-independent data formats that support the creation, access, and sharing of array-oriented scientific data.

  nettle: nettle/3.7.2
    Nettle is a cryptographic library that is designed to fit easily in more or less any context: In crypto toolkits for object-oriented languages (C++, Python, Pike, ...), in applications like LSH or GNUPG,
    or even in kernel space.

  networkx: networkx/2.5.1
    NetworkX is a Python package for the creation, manipulation, and study of the structure, dynamics, and functions of complex networks.

  nodejs: nodejs/14.17.0
    Node.js is a platform built on Chrome's JavaScript runtime for easily building fast, scalable network applications. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and
    efficient, perfect for data-intensive real-time applications that run across distributed devices.

  nsync: nsync/1.24.0
    nsync is a C library that exports various synchronization primitives, such as mutexes

  numactl: numactl/2.0.14
    The numactl program allows you to run your application program on specific cpu's and memory nodes. It does this by supplying a NUMA memory policy to the operating system before running your program. The
    libnuma library provides convenient ways for you to add NUMA memory policies into your own program. 

  numlib/CGAL: numlib/CGAL/4.14.3-gompi-2020a-Python-3.8.2
    The goal of the CGAL Open Source Project is to provide easy access to efficient and reliable geometric algorithms in the form of a C++ library.

  numlib/FFTW: numlib/FFTW/3.3.8-gompi-2020a, numlib/FFTW/3.3.8-intel-2020a
    FFTW is a C subroutine library for computing the discrete Fourier transform (DFT) in one or more dimensions, of arbitrary input size, and of both real and complex data.

  numlib/GSL: numlib/GSL/2.6-GCC-9.3.0, numlib/GSL/2.6-iccifort-2020.1.217
    The GNU Scientific Library (GSL) is a numerical library for C and C++ programmers. The library provides a wide range of mathematical routines such as random number generators, special functions and
    least-squares fitting.

  numlib/NLopt: numlib/NLopt/2.6.1-GCCcore-9.3.0
    NLopt is a free/open-source library for nonlinear optimization, providing a common interface for a number of different free optimization routines available online as well as original implementations of
    various other algorithms. 

  numlib/OpenBLAS: numlib/OpenBLAS/0.3.9-GCC-9.3.0
    OpenBLAS is an optimized BLAS library based on GotoBLAS2 1.13 BSD version.

  numlib/ScaLAPACK: numlib/ScaLAPACK/2.1.0-gompi-2020a
    The ScaLAPACK (or Scalable LAPACK) library includes a subset of LAPACK routines redesigned for distributed memory MIMD parallel computers.

  numlib/imkl: numlib/imkl/2020.1.217-iimpi-2020a, numlib/imkl/2020.1.217-iompi-2020a
    Intel Math Kernel Library is a library of highly optimized, extensively threaded math routines for science, engineering, and financial applications that require maximum performance. Core math functions
    include BLAS, LAPACK, ScaLAPACK, Sparse Solvers, Fast Fourier Transforms, Vector Math, and more.

  perf/CubeGUI: perf/CubeGUI/4.4.4-GCCcore-9.3.0
    Cube, which is used as performance report explorer for Scalasca and Score-P, is a generic tool for displaying a multi-dimensional performance space consisting of the dimensions (i) performance metric,
    (ii) call path, and (iii) system resource. Each dimension can be represented as a tree, where non-leaf nodes of the tree can be collapsed or expanded to achieve the desired level of granularity. This
    module provides the Cube graphical report explorer. 

  perf/CubeLib: perf/CubeLib/4.4.4-GCCcore-9.3.0
    Cube, which is used as performance report explorer for Scalasca and Score-P, is a generic tool for displaying a multi-dimensional performance space consisting of the dimensions (i) performance metric,
    (ii) call path, and (iii) system resource. Each dimension can be represented as a tree, where non-leaf nodes of the tree can be collapsed or expanded to achieve the desired level of granularity. This
    module provides the Cube general purpose C++ library component and command-line tools. 

  perf/CubeWriter: perf/CubeWriter/4.4.3-GCCcore-9.3.0
    Cube, which is used as performance report explorer for Scalasca and Score-P, is a generic tool for displaying a multi-dimensional performance space consisting of the dimensions (i) performance metric,
    (ii) call path, and (iii) system resource. Each dimension can be represented as a tree, where non-leaf nodes of the tree can be collapsed or expanded to achieve the desired level of granularity. This
    module provides the Cube high-performance C writer library component. 

  perf/IPM: perf/IPM/2.0.6-iompi-2020a
    IPM is a portable profiling infrastructure for parallel codes. It provides a low-overhead profile of application performance and resource utilization in a parallel program. Communication, computation, and
    IO are the primary focus. 

  perf/OPARI2: perf/OPARI2/2.0.5-GCCcore-9.3.0
    OPARI2, the successor of Forschungszentrum Juelich's OPARI, is a source-to-source instrumentation tool for OpenMP and hybrid codes. It surrounds OpenMP directives and runtime library calls with calls to
    the POMP2 measurement interface. 

  perf/OTF2: perf/OTF2/2.2-GCCcore-9.3.0
    The Open Trace Format 2 is a highly scalable, memory efficient event trace data format plus support library. It is the new standard trace format for Scalasca, Vampir, and TAU and is open for other tools. 

  perf/PAPI: perf/PAPI/6.0.0-GCCcore-9.3.0
    PAPI provides the tool designer and application engineer with a consistent interface and methodology for use of the performance counter hardware found in most major microprocessors. PAPI enables software
    engineers to see, in near real time, the relation between software performance and processor events. In addition Component PAPI provides access to a collection of components that expose performance
    measurement opportunites across the hardware and software stack. 

  perf/PDT: perf/PDT/3.25.1-GCCcore-9.3.0
    Program Database Toolkit (PDT) is a framework for analyzing source code written in several programming languages and for making rich program knowledge accessible to developers of static and dynamic
    analysis tools. PDT implements a standard program representation, the program database (PDB), that can be accessed in a uniform way through a class library supporting common PDB operations. 

  perf/Scalasca: perf/Scalasca/2.5-gompi-2020a
    Scalasca is a software tool that supports the performance optimization of parallel programs by measuring and analyzing their runtime behavior. The analysis identifies potential performance bottlenecks --
    in particular those concerning communication and synchronization -- and offers guidance in exploring their causes. 

  perf/Score-P: perf/Score-P/6.0-gompi-2020a
    The Score-P measurement infrastructure is a highly scalable and easy-to-use tool suite for profiling, event tracing, and online analysis of HPC applications. 

  phys/UDUNITS: phys/UDUNITS/2.2.26-foss-2020a
    UDUNITS supports conversion of unit specifications between formatted and binary forms, arithmetic manipulation of units, and conversion of values between compatible scales of measurement.

  pixman: pixman/0.40.0
    Pixman is a low-level software library for pixel manipulation, providing features such as image compositing and trapezoid rasterization. Important users of pixman are the cairo graphics library and the X
    server. 

  pkg-config: pkg-config/0.29.2
    pkg-config is a helper tool used when compiling applications and libraries. It helps you insert the correct compiler options on the command line so an application can use gcc -o test test.c `pkg-config
    --libs --cflags glib-2.0` for instance, rather than hard-coding values on where to find glib (or other libraries). 

  pkgconfig: pkgconfig/1.5.4-python
    pkgconfig is a Python module to interface with the pkg-config command line tool

  protobuf: protobuf/3.17.3
    Google Protocol Buffers

  protobuf-python: protobuf-python/3.17.3
    Python Protocol Buffers runtime library.

  pybind11: pybind11/2.6.2
    pybind11 is a lightweight header-only library that exposes C++ types in Python and vice versa, mainly to create Python bindings of existing C++ code.

  pycocotools: pycocotools/2.0.4
    Official APIs for the MS-COCO dataset

  re2c: re2c/2.1.1
    re2c is a free and open-source lexer generator for C and C++. Its main goal is generating fast lexers: at least as fast as their reasonably optimized hand-coded counterparts. Instead of using traditional
    table-driven approach, re2c encodes the generated finite state automata directly in the form of conditional jumps and comparisons.

  scikit-build: scikit-build/0.11.1
    Scikit-Build, or skbuild, is an improved build system generator for CPython C/C++/Fortran/Cython extensions.

  scikit-learn: scikit-learn/0.24.2
    Scikit-learn integrates machine learning algorithms in the tightly-knit scientific Python world, building upon numpy, scipy, and matplotlib. As a machine-learning module, it provides versatile tools for
    data mining and analysis in any field of science and engineering. It strives to be simple and efficient, accessible to everybody, and reusable in various contexts.

  settarg: settarg

  snappy: snappy/1.1.8
    Snappy is a compression/decompression library. It does not aim for maximum compression, or compatibility with any other compression library; instead, it aims for very high speeds and reasonable
    compression.

  spglib-python: spglib-python/1.16.1
    Spglib for Python. Spglib is a library for finding and handling crystal symmetries written in C.

  system/hwloc: system/hwloc/2.2.0-GCCcore-9.3.0
    The Portable Hardware Locality (hwloc) software package provides a portable abstraction (across OS, versions, architectures, ...) of the hierarchical topology of modern architectures, including NUMA
    memory nodes, sockets, shared caches, cores and simultaneous multithreading. It also gathers various system attributes such as cache and memory information as well as the locality of I/O devices such as
    network interfaces, InfiniBand HCAs or GPUs. It primarily aims at helping applications with gathering information about modern computing hardware so as to exploit it accordingly and efficiently. 

  system/libpciaccess: system/libpciaccess/0.16-GCCcore-9.3.0
    Generic PCI access library.

  tensorboard: tensorboard/2.8.0
    TensorBoard is a suite of web applications for inspecting and understanding your TensorFlow runs and graphs.

  toolchain/foss: toolchain/foss/2020a
    GNU Compiler Collection (GCC) based compiler toolchain, including OpenMPI for MPI support, OpenBLAS (BLAS and LAPACK support), FFTW and ScaLAPACK.

  toolchain/gompi: toolchain/gompi/2020a
    GNU Compiler Collection (GCC) based compiler toolchain, including OpenMPI for MPI support.

  toolchain/iimpi: toolchain/iimpi/2020a
    Intel C/C++ and Fortran compilers, alongside Intel MPI.

  toolchain/intel: toolchain/intel/2020a
    Compiler toolchain including Intel compilers, Intel MPI and Intel Math Kernel Library (MKL).

  toolchain/iompi: toolchain/iompi/2020a
    Intel C/C++ and Fortran compilers, alongside Open MPI.

  tools/DB: tools/DB/18.1.32-GCCcore-9.3.0
    Berkeley DB enables the development of custom data management solutions, without the overhead traditionally associated with such custom projects.

  tools/GLPK: tools/GLPK/4.65-GCCcore-9.3.0
    The GLPK (GNU Linear Programming Kit) package is intended for solving large-scale linear programming (LP), mixed integer programming (MIP), and other related problems. It is a set of routines written in
    ANSI C and organized in the form of a callable library.

  tools/Ghostscript: tools/Ghostscript/9.52-GCCcore-9.3.0
    Ghostscript is a versatile processor for PostScript data with the ability to render PostScript to different targets. It used to be part of the cups printing stack, but is no longer used for that.

  tools/HPL: tools/HPL/2.3-foss-2020a, tools/HPL/2.3-intel-2020a
    HPL is a software package that solves a (random) dense linear system in double precision (64 bits) arithmetic on distributed-memory computers. It can thus be regarded as a portable as well as freely
    available implementation of the High Performance Computing Linpack Benchmark.

  tools/Meson: tools/Meson/0.55.1-GCCcore-9.3.0-Python-3.8.2
    Meson is a cross-platform build system designed to be both as fast and as user friendly as possible.

  tools/Ninja: tools/Ninja/1.10.0-GCCcore-9.3.0
    Ninja is a small build system with a focus on speed.

  tools/Szip: tools/Szip/2.1.1-GCCcore-9.3.0
    Szip compression software, providing lossless compression of scientific data 

  tools/UnZip: tools/UnZip/6.0-GCCcore-9.3.0
    UnZip is an extraction utility for archives compressed in .zip format (also called "zipfiles"). Although highly compatible both with PKWARE's PKZIP and PKUNZIP utilities for MS-DOS and with Info-ZIP's own
    Zip program, our primary objectives have been portability and non-MSDOS functionality.

  tools/XZ: tools/XZ/5.2.5-GCCcore-9.3.0
    xz: XZ utilities

  tools/Zip: tools/Zip/3.0-GCCcore-9.3.0
    Zip is a compression and file packaging/archive utility. Although highly compatible both with PKWARE's PKZIP and PKUNZIP utilities for MS-DOS and with Info-ZIP's own UnZip, our primary objectives have
    been portability and other-than-MSDOS functionality

  tools/binutils: tools/binutils/2.34-GCCcore-9.3.0, tools/binutils/2.34
    binutils: GNU binary utilities

  tools/bzip2: tools/bzip2/1.0.8-GCCcore-9.3.0
    bzip2 is a freely available, patent free, high-quality data compressor. It typically compresses files to within 10% to 15% of the best available techniques (the PPM family of statistical compressors),
    whilst being around twice as fast at compression and six times faster at decompression. 

  tools/cURL: tools/cURL/7.69.1-GCCcore-9.3.0
    libcurl is a free and easy-to-use client-side URL transfer library, supporting DICT, FILE, FTP, FTPS, Gopher, HTTP, HTTPS, IMAP, IMAPS, LDAP, LDAPS, POP3, POP3S, RTMP, RTSP, SCP, SFTP, SMTP, SMTPS, Telnet
    and TFTP. libcurl supports SSL certificates, HTTP POST, HTTP PUT, FTP uploading, HTTP form based upload, proxies, cookies, user+password authentication (Basic, Digest, NTLM, Negotiate, Kerberos), file
    transfer resume, http proxy tunneling and more. 

  tools/expat: tools/expat/2.2.9-GCCcore-9.3.0
    Expat is an XML parser library written in C. It is a stream-oriented parser in which an application registers handlers for things the parser might find in the XML document (like start tags) 

  tools/gettext: tools/gettext/0.20.1-GCCcore-9.3.0, tools/gettext/0.20.1
    GNU 'gettext' is an important step for the GNU Translation Project, as it is an asset on which we may build many other steps. This package offers to programmers, translators, and even users, a well
    integrated set of tools and documentation

  tools/git: tools/git/2.23.0-GCCcore-9.3.0-nodocs
    Git is a free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency.

  tools/groff: tools/groff/1.22.4-GCCcore-9.3.0
    Groff (GNU troff) is a typesetting system that reads plain text mixed with formatting commands and produces formatted output.

  tools/gzip: tools/gzip/1.10-GCCcore-9.3.0
    gzip (GNU zip) is a popular data compression program as a replacement for compress

  tools/help2man: tools/help2man/1.47.4, tools/help2man/1.47.12-GCCcore-9.3.0
    help2man produces simple manual pages from the '--help' and '--version' output of other commands.

  tools/networkx: tools/networkx/2.4-foss-2020a-Python-3.8.2
    NetworkX is a Python package for the creation, manipulation, and study of the structure, dynamics, and functions of complex networks.

  tools/numactl: tools/numactl/2.0.13-GCCcore-9.3.0
    The numactl program allows you to run your application program on specific cpu's and memory nodes. It does this by supplying a NUMA memory policy to the operating system before running your program. The
    libnuma library provides convenient ways for you to add NUMA memory policies into your own program. 

  tools/qtop: tools/qtop/53-1
    qtop is a nifty command-line tool for monitoring queueing systems, esp. PBS/torque. It tries to fit as much information as possible in your screen's real estate, by stitching together the output of
    commands like pbsnodes -a, qstat & qstat -q. It is possible to write wrappers for other platforms -people have done so for SGE, OAR etc- or, even examine traces offline and present the sampled
    information.

  tools/re2c: tools/re2c/1.3-GCCcore-9.3.0
    re2c is a free and open-source lexer generator for C and C++. Its main goal is generating fast lexers: at least as fast as their reasonably optimized hand-coded counterparts. Instead of using traditional
    table-driven approach, re2c encodes the generated finite state automata directly in the form of conditional jumps and comparisons.

  tools/util-linux: tools/util-linux/2.35-GCCcore-9.3.0
    Set of Linux utilities

  torchvision: torchvision/0.11.1
    Datasets, Transforms and Models specific to Computer Vision

  typing-extensions: typing-extensions/3.10.0.0
    Typing Extensions – Backported and Experimental Type Hints for Python

  util-linux: util-linux/2.36
    Set of Linux utilities

  vis/FFmpeg: vis/FFmpeg/4.2.2-GCCcore-9.3.0
    A complete, cross-platform solution to record, convert and stream audio and video.

  vis/GLib: vis/GLib/2.64.1-GCCcore-9.3.0
    GLib is one of the base libraries of the GTK+ project

  vis/HarfBuzz: vis/HarfBuzz/2.6.4-GCCcore-9.3.0
    HarfBuzz is an OpenType text shaping engine.

  vis/ImageMagick: vis/ImageMagick/7.0.10-1-GCCcore-9.3.0
    ImageMagick is a software suite to create, edit, compose, or convert bitmap images

  vis/JasPer: vis/JasPer/2.0.14-GCCcore-9.3.0
    The JasPer Project is an open-source initiative to provide a free software-based reference implementation of the codec specified in the JPEG-2000 Part-1 standard. 

  vis/LittleCMS: vis/LittleCMS/2.9-GCCcore-9.3.0
    Little CMS intends to be an OPEN SOURCE small-footprint color management engine, with special focus on accuracy and performance. 

  vis/Mesa: vis/Mesa/20.0.2-GCCcore-9.3.0
    Mesa is an open-source implementation of the OpenGL specification - a system for rendering interactive 3D graphics.

  vis/Pango: vis/Pango/1.44.7-GCCcore-9.3.0
    Pango is a library for laying out and rendering of text, with an emphasis on internationalization. Pango can be used anywhere that text layout is needed, though most of the work on Pango so far has been
    done in the context of the GTK+ widget toolkit. Pango forms the core of text and font handling for GTK+-2.x.

  vis/ParaView: vis/ParaView/5.8.0-foss-2020a-Python-3.8.2-mpi
    ParaView is a scientific parallel visualizer.

  vis/Tk: vis/Tk/8.6.10-GCCcore-9.3.0
    Tk is an open source, cross-platform widget toolchain that provides a library of basic elements for building a graphical user interface (GUI) in many different programming languages.

  vis/X11: vis/X11/20200222-GCCcore-9.3.0
    The X Window System (X11) is a windowing system for bitmap displays

  vis/Xvfb: vis/Xvfb/1.20.9-GCCcore-9.3.0
    Xvfb is an X server that can run on machines with no display hardware and no physical input devices. It emulates a dumb framebuffer using virtual memory.

  vis/cairo: vis/cairo/1.16.0-GCCcore-9.3.0
    Cairo is a 2D graphics library with support for multiple output devices. Currently supported output targets include the X Window System (via both Xlib and XCB), Quartz, Win32, image buffers, PostScript,
    PDF, and SVG file output. Experimental backends include OpenGL, BeOS, OS/2, and DirectFB

  vis/fontconfig: vis/fontconfig/2.13.92-GCCcore-9.3.0
    Fontconfig is a library designed to provide system-wide font configuration, customization and application access. 

  vis/freetype: vis/freetype/2.10.1-GCCcore-9.3.0
    FreeType 2 is a software font engine that is designed to be small, efficient, highly customizable, and portable while capable of producing high-quality output (glyph images). It can be used in graphics
    libraries, display servers, font conversion tools, text image generation tools, and many other products as well. 

  vis/gnuplot: vis/gnuplot/5.2.8-GCCcore-9.3.0
    Portable interactive, function plotting utility

  vis/libGLU: vis/libGLU/9.0.1-GCCcore-9.3.0
    The OpenGL Utility Library (GLU) is a computer graphics library for OpenGL. 

  vis/pixman: vis/pixman/0.38.4-GCCcore-9.3.0
    Pixman is a low-level software library for pixel manipulation, providing features such as image compositing and trapezoid rasterization. Important users of pixman are the cairo graphics library and the X
    server. 

  vis/x264: vis/x264/20191217-GCCcore-9.3.0
    x264 is a free software library and application for encoding video streams into the H.264/MPEG-4 AVC compression format, and is released under the terms of the GNU GPL. 

  vis/x265: vis/x265/3.3-GCCcore-9.3.0
    x265 is a free software library and application for encoding video streams into the H.265 AVC compression format, and is released under the terms of the GNU GPL. 

  x264: x264/20210414
    x264 is a free software library and application for encoding video streams into the H.264/MPEG-4 AVC compression format, and is released under the terms of the GNU GPL. 

  x265: x265/3.5
    x265 is a free software library and application for encoding video streams into the H.265 AVC compression format, and is released under the terms of the GNU GPL. 

  xorg-macros: xorg-macros/1.19.3
    X.org macros utilities.

  xxd: xxd/8.2.4220
    xxd is part of the VIM package and this will only install xxd, not vim! xxd converts to/from hexdumps of binary files.

  zlib: zlib/1.2.11
    zlib is designed to be a free, general-purpose, legally unencumbered -- that is, not covered by any patents -- lossless data-compression library for use on virtually any computer hardware and operating
    system.

  zstd: zstd/1.4.9
    Zstandard is a real-time compression algorithm, providing high compression ratios. It offers a very wide range of compression/speed trade-off, while being backed by a very fast decoder. It also offers a
    special mode for small data, called dictionary compression, and can create dictionaries from any sample set.
