
Specific Software Modules
=========================

Running a user's code or software installed in OBLIVION requires the loading of specific modules or a toolchain. This section deal with examples on the modules to be loaded for specific software execution.

1. Julia Language
~~~~~~~~~~~~~~~~~

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
default). Further information on these versions is obtained with ``module spider Julia`` or module spider julia``.

Note that Julia's package manager (Pkg) is available directly from the REPL or via
`julia --project` workflows; users are encouraged to install packages into their
own project environments under their home or project directories, since the
central Julia installation is read-only. 

For MPI-parallel Julia workloads, load a toolchain (e.g. `foss/2025b`) or MPI framework (OpenMPI, MPICH) modules, before loading Julia so that MPI.jl can pick up the system OpenMPI/MPICH libraries. 

For example the PAMNEI (**\P**\ arallel Block **\A**\ daptive Mesh Refinement **\M**\ HD **\M**\ ultispecies **\N**\ on-**\E**\ quilibrium **\I**\ onization; de Avillez+ 2026) code is a Julia based plasma astrophysics code uses MPI (either OpenMPI or MPICH), HDF5, NetCDF, and VTK data formats and ADIOS2 (ADaptable I/O System 2) which is an I/O framework/middleware for HPC that includes its own format (BP). So, to run this code one needs to load the different modules in the submission scripts or in an interactive session:

.. code-block:: julia

  module purge
  module load site/langs Julia/1.12.7
  module load GCC/14.3.0
  module load OpenMPI/5.0.8
  module load HDF5/1.14.6
  module load netCDF/4.9.3
  module load ADIOS2/2.10.2

Here we start by purging the modules and then load the needed modules. Note thta to load Julia/1.12.7 we load the module site/langs.

2. mpi4py
~~~~~~~~~

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


3. scipy, numpy, numexpr, pandas
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

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


4. GROMACS
~~~~~~~~~~

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

