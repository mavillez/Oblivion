Environment Modules
===================


1. Introduction
---------------

There are several conflicting software packages installed in the Oblivion supercomputer. In order for the user to use the adequate software there is a need to set the paths for the binaries, libraries, manuals, and includes. Either the user sets these paths for each package or the system provides an easy way to set those paths. The latter is the preferable and makes use of environment modules. 


2. Working with Modules
-----------------------

The user sets the software environment by loading the modules associated to the packages he needs to use. This is easily done by using module load or module add. Software dependences are set in the same way. Oblivion uses a hierarchical module naming scheme (hmns) in which modules availability follows the software hierarchy Core/Compiler/MPI.

Core refers to the basic core modules that have to be loaded in order to have access to next levels of software compiled against a specific compiler and a MPI API.

After logging into the machine the user should execute the command module av (av for available) obtaining the list of core modules:

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


The list shows the toolchains gompi/2021a, foss/2021a, intel/2021a, iimpi/2021a, iompi/2021a. One of the toolchains has to be loaded using, say for foss/2021a

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
   DB/18.1.40                          flatbuffers-python/2.0
   DBus/1.13.18                        flatbuffers/2.0.0
   Doxygen/1.9.1                       flex/2.6.4                 (D)
   Eigen/3.3.9                         fontconfig/2.13.93
   FFmpeg/4.3.2                        freetype/2.10.4
   FLAC/1.3.3                          gettext/0.21               (D)
   Flask/1.1.4                         giflib/5.2.1
   FriBidi/1.0.10                      git/2.32.0-nodocs
   GLPK/5.0                            gnuplot/5.4.2
   GLib/2.68.2                         gperf/3.1
   GMP/6.2.1                           groff/1.22.4
   GObject-Introspection/1.68.0        gzip/1.10
   Ghostscript/9.54.0                  help2man/1.48.3
   HDF/4.2.15                          hwloc/2.4.1                (L)
   HarfBuzz/2.8.1                      hypothesis/6.13.1
   ICU/69.1                            intltool/0.51.0
   ImageMagick/7.0.11-14               jbigkit/2.1
   JasPer/2.0.28                       libGLU/9.0.1
   JsonCpp/1.9.4                       libarchive/3.5.1
   LAME/3.100                          libcerf/1.17
   LLVM/11.1.0                         libdrm/2.4.106
   LMDB/0.9.28                         libevent/2.1.12            (L)
   LibTIFF/4.2.0                       libfabric/1.12.1           (L)
   LittleCMS/2.12                      libffi/3.3
   Lua/5.4.3                           libgd/2.3.1
   M4/1.4.18                           libgeotiff/1.6.0
   METIS/5.1.0                         libgit2/1.1.0
   MPFR/4.1.0                          libglvnd/1.3.3
   Mako/1.1.4                          libiconv/1.16
   Mesa/21.1.1                         libjpeg-turbo/2.0.6
   Meson/0.58.0                        libogg/1.3.4
   NASM/2.15.05                        libpciaccess/0.16          (L)
   NLopt/2.7.0                         libpng/1.6.37
   NSPR/4.30                           libreadline/8.1
   NSS/3.65                            libsndfile/1.0.31
   Ninja/1.10.2                        libtirpc/1.3.2
   OPARI2/2.0.6                        libtool/2.4.6
   OTF2/2.3                            libunwind/1.4.0
   PAPI/6.0.0.1                        libvorbis/1.3.7
   PCRE/8.44                           libxml2/2.9.10             (L)
   PCRE2/10.36                         libxslt/1.1.34
   PDT/3.25.1                          libyaml/0.2.5
   PMIx/3.2.3                   (L)    lxml/4.6.3
   PROJ/8.0.1                          lz4/1.9.3
   Pango/1.48.5                        makeinfo/6.7-minimal
   Perl/5.32.1-minimal                 ncurses/6.2                (D)
   Perl/5.32.1                  (D)    nettle/3.7.2
   Pillow-SIMD/8.2.0                   nodejs/14.17.0
   Pillow/8.2.0                        nsync/1.24.0
   PyYAML/5.4.1                        numactl/2.0.14             (L)
   Python/2.7.18-bare                  pixman/0.40.0
   Python/3.9.5-bare                   pkg-config/0.29.2          (D)
   Python/3.9.5                 (D)    pkgconfig/1.5.4-python
   Qhull/2020.2                        protobuf-python/3.17.3
   Qt5/5.15.2                          protobuf/3.17.3
   Rust/1.52.1                         pybind11/2.6.2
   SIONlib/1.7.6-tools                 re2c/2.1.1
   SQLite/3.35.4                       scikit-build/0.11.1
   Szip/2.1.1                          snappy/1.1.8
   Tcl/8.6.11                          typing-extensions/3.10.0.0
   Tk/8.6.11                           util-linux/2.36
   Tkinter/3.9.5                       x264/20210414
   UCX/1.10.0                   (L)    x265/3.5
   UDUNITS/2.2.28                      xorg-macros/1.19.3
   UnZip/6.0                           xxd/8.2.4220
   X11/20210518                        zlib/1.2.11                (L,D)
   XZ/5.2.5                     (L)    zstd/1.4.9
   Xvfb/1.20.11

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


The top row displays the modules for software compiled against OpenMPI, which in turn was compiled with GCC compiler (second row of modules). The third row displays the modules of software compiled with GCC/10.3.0. Finally, the core modules already seen before are displayed

Now the user only needs to load the modules of interest. For example, if a user wants to use TensorFlow he/she executes the following command:

.. code-block:: console

  module load TensorFlow/2.6.0

or if he/she wants to use GROMACS/2021.5 then just execute

.. code-block:: console

  module load GROMACS/2021.5

In the latter case the loaded modules, given by "module list", are

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

 
4. List of commonly used commands
---------------------------------

.. list-table::
   :widths: 25 50
   :header-rows: 1

  * - Command	
    - Function
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
