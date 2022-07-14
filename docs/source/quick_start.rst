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
  
Load the needed modules to create the working environment 
      
.. code-block:: console  
    
    module load <module_name>


3. Compilation and Job Submission
---------------------------------

In the project directory compile the code and submit it for execution using a SLURM script

.. code-block:: console

  sbatch <script_name.sh>.

A script example:

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
  
  srun ./code_executable

In this script we are setting the number of MPI tasks (ntasks), the number of cores per task (cpus-per-task) and the number of tasks per CPU also referred as socket (ntasks-per-socket). So, this script imposes that 1 core executes 1 MPI task. The compute nodes are being used exclusively by this run (option exclusive), and the queue, which in SLURM is called partition, is the debug queue. Finally the code is executed using srun.


4. Available Resources and Jobs in the Queue
--------------------------------------------

To see what compute nodes ara vailable use

.. code-block:: console

  $ sinfo
  
  
To check if the job is in the queue to run just execute

.. code-block:: console

  $ squeue | grep USER_NAME
  

5. Consumed CPU time
--------------------

The user can always use sacct to see the CPU time used by the job by using, for example,

.. code-block:: console
  
  sacct –format JobIdRaw,User,Partition,Submit,Start,Elapsed,AllocCPUS,CPUTime,CPUTimeRaw,MaxRSS,State,NodeList -S 2021-02-01 -E 2021-02-02


For more information on the command sacct options at the terminal execute

.. code-block:: console

  man sacct
