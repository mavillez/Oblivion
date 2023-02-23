Quick Start
===========

1. Project Folder
-----------------

After logged in the user finds himself in his home directory. Change to the project directory

.. code-block:: julia

  cd /mnt/beegfs/projects/PROJECT_ID/USER_NAME
  
where PROJECT_ID is the project folder and USER_NAME is the user's username.

2. Working Environment
----------------------

Check the available modules in the machine by using

.. code-block:: julia

  module av
  
Load the needed modules to create the working environment (:ref:`see the modules section<Environment Modules>`)
      
.. code-block:: julia
    
    module load <module_name>

3. Software folder
------------------

The software directories are located at

.. code-block:: julia

    /mnt/beegfs/stack/cn01470/software
  
Using ``ls /mnt/beegfs/stack/cn01470/software`` a list of all software directories is displayed

.. code-block:: julia

  ABINIT              FlexiBLAS              Julia          NCO                   re2c
  AmberTools          Flye                   kim-api        ncurses               Rust
  Anaconda3           FMS                    LAME           ncview                SAMtools
  ANSYS_CFD           fontconfig             LAMMPS         netCDF                ScaFaCoS
  ant                 foss                   LAPACK         netcdf4-python        ScaLAPACK
  ANTLR               freetype               libarchive     netCDF-C++4           scikit-bio
  archspec            FriBidi                libcerf        netCDF-Fortran        scikit-build
  arpack-ng           futile                 libdap         nettle                scikit-learn
  Arrow               GATK                   libdrm         networkx              SciPy-bundle
  ArviZ               GCC                    libepoxy       Ninja                 SCOTCH
  ASE                 GCCcore                libevent       NLopt                 Siesta
  ATK                 GDAL                   libfabric      nodejs                SimPEG
  at-spi2-atk         Gdk-Pixbuf             libffi         NSPR                  snakemake
  at-spi2-core        GEOS                   libFLAME       NSS                   snappy
  attr                gettext                libgd          nsync                 spglib-python
  Autoconf            Ghostscript            libgeotiff     numactl               SPOTPY
  ...
  

4. Compilation and Job Submission
---------------------------------

In the project directory compile the code and submit it for execution using a SLURM script

.. code-block:: julia

  sbatch <script_name.sh>.

A script example for runs with OpneMPI compiled with GCC:

.. code-block:: bash

  #!/bin/bash
  #SBATCH --time=00:40:00
  #SBATCH --account=astro_00
  #SBATCH --job-name=JOB_NAME
  #SBATCH --output=JOB_NAME_%j.out
  #SBATCH --error=JOB_NAME_%j.error
  #SBATCH --nodes=32
  #SBATCH --ntasks=1024
  #SBATCH --cpus-per-task=1
  #SBATCH --ntasks-per-socket=16
  #SBATCH --exclusive
  #SBATCH --partition=debug
  
  export PMIX_MCA_psec=native
  
  srun ./code_executable

In this script we are setting the number of MPI tasks (ntasks), the number of cores per task (cpus-per-task) and the number of tasks per CPU also referred as socket (ntasks-per-socket). So, this script imposes that 1 core executes 1 MPI task. The compute nodes are being used exclusively by this run (option exclusive), and the queue, which in SLURM is called partition, is the debug queue. Finally the code is executed using srun. 


5. Available Resources and Jobs in the Queue
--------------------------------------------

To see what compute nodes ara vailable use

.. code-block:: julia

  $ sinfo

  PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
  private*     up 3-00:00:00      2  down* cn[076,080]
  private*     up 3-00:00:00      2    mix cn[025,030]
  private*     up 3-00:00:00     12  alloc cn[013-018,021,023,026-029]
  private*     up 3-00:00:00     71   idle cn[002-012,019-020,022,024,031-075,077-079,081-088]
  private*     up 3-00:00:00      1   down cn001
  debug        up 2-00:00:00      2    mix cn[025,030]
  debug        up 2-00:00:00     12  alloc cn[013-018,021,023,026-029]
  debug        up 2-00:00:00     43   idle cn[002-012,019-020,022,024,031-058]
  debug        up 2-00:00:00      1   down cn001
  short        up 3-00:00:00      6  alloc cn[013-018]
  short        up 3-00:00:00     13   idle cn[002-012,019-020]
  short        up 3-00:00:00      1   down cn001
  medium       up 2-00:00:00      2    mix cn[025,030]
  medium       up 2-00:00:00      6  alloc cn[021,023,026-029]
  medium       up 2-00:00:00     30   idle cn[022,024,031-058]

To learn the meaning of states down, mix, alloc, and idle read the manual pages by issuing the command ``man sinfo``. 
  
To check if a job is in the queue to run just execute

.. code-block:: console

  $ squeue | grep USER_NAME
 
    JOBID PARTITION     NAME       USER ST       TIME  NODES  NODELIST(REASON)
    16868     debug     job1  USER_NAME  R    5:54:10      1  cn013
    16867     debug     job2  USER_NAME  R    5:54:15      1  cn012
    16866     debug     job3  USER_NAME  R    5:54:21      8  cn[001-008]


6. Consumed CPU time
--------------------

The user can always use sacct to see the CPU time used by the job by using, for example,

.. code-block:: console
 
  $ sacct --format=JobIdRaw,User,Partition,Submit,Start,Elapsed,AllocCPUS,CPUTime,CPUTimeRaw,MaxRSS,State,NodeList -S 2021-02-01 -E 2021-02-02

  JobIDRaw      User  Partition              Submit               Start    Elapsed  AllocCPUS    CPUTime CPUTimeRAW     MaxRSS      State           NodeList 
  ------------ --------- ---------- ------------------- ------------------- ---------- ---------- ---------- ---------- ---------- ---------- --------------- 
  2002              USER      debug 2021-02-01T15:42:30 2021-02-01T15:42:30   00:14:17        576 5-17:07:12     493632             COMPLETED     cn[029-044] 
  2002.batch                        2021-02-01T15:42:30 2021-02-01T15:42:30   00:14:17         36   08:34:12      30852      8792K  COMPLETED           cn029 
  2002.0                            2021-02-01T15:42:30 2021-02-01T15:42:30   00:14:17        512 5-01:53:04     438784    174720K  COMPLETED     cn[029-044] 
  2003              USER      debug 2021-02-01T15:44:13 2021-02-01T15:56:47   00:07:43       1152 6-04:09:36     533376             COMPLETED cn[020-027,029+ 
  2003.batch                        2021-02-01T15:56:47 2021-02-01T15:56:47   00:07:43         36   04:37:48      16668     10104K  COMPLETED           cn020 
  2003.0                            2021-02-01T15:56:47 2021-02-01T15:56:47   00:07:43       1024 5-11:41:52     474112    134972K  COMPLETED cn[020-027,029+ 


For more information on the command sacct options at the terminal execute

.. code-block:: console

  man sacct
