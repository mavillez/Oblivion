Quick Start
===========

1. Project Folder
-----------------

After logged in the user finds himself in his home directory. Change to the project directory

.. code-block:: console

  cd /mnt/beegfs/projects/PROJECT_ID/USER_NAME
  
where PROJECT_ID is the project folder and USER_NAME is the user's username.

2. Working Environment
----------------------

Check the available modules in the machine by using

.. code-block:: console

  module av
  
Load the needed modules to create the working environment (:ref:`see the modules section<Environment Modules>`)
      
.. code-block:: console  
    
    module load <module_name>

3. Software folder
------------------

.. code-block:: console

  cd /mnt/beegfs/apps/4.7.x/software
  
  
4. Compilation and Job Submission
---------------------------------

In the project directory compile the code and submit it for execution using a SLURM script

.. code-block:: console

  sbatch <script_name.sh>.

A script example for runs with OpneMPI:

.. code-block:: console

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
  private*     up 3-00:00:00     10  alloc cn[001-004,021-028,032-033]
  private*     up 3-00:00:00     78   idle cn[005-020,029-031,034-088]
  debug        up 2-00:00:00     10  alloc cn[001-004,021-028,032-033]
  debug        up 2-00:00:00     78   idle cn[005-020,029-031,034-058]
  medium       up 2-00:00:00     10  alloc cn[021-028,032-033]
  medium       up 2-00:00:00     28   idle cn[029-031,034-058]
  short        up 3-00:00:00      4  alloc cn[001-004]
  short        up 3-00:00:00     16   idle cn[005-020]
  
  
To check if the job is in the queue to run just execute

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
