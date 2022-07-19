Resources and Job Management
============================

Resources and job management are managed by `SLURM Work Manager <https://slurm.schedmd.com>`_ providing insight, among others, on:

#. Available resources
#. Job management
#. Accounting

Available Resources
-------------------

To check the available resources the user should execute ``sinfo``

.. code-block:: julia

  $ sinfo

  PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
  debug        up 1-00:00:00      8  alloc cn[001-008,012-013]
  debug        up 1-00:00:00     54   idle cn[009-011,014-058]
  private*     up 2-00:00:00      4  alloc cn[001-008,012-013]
  private*     up 2-00:00:00     54   idle cn[009-011,014-058]
  medium       up 2-00:00:00     48   idle cn[011-058]
  short        up 3-00:00:00      4  alloc cn[001-008]
  short        up 3-00:00:00      6   idle cn[009-010]

and execute ``squeue`` that gives the compute nodes in use and the jobs status. In some systems one can see all jobs while in others it is limited to the user. (see example below)

Job Management
--------------

Job Submission
~~~~~~~~~~~~~~

The user submits the job to the system by using a SLURM script using the command

.. code-block:: julia
  
  $ sbatch <slurm_script_name>

The script can have the form

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

The script sets 1024 cores (``ntasks``), 1 MPI task per core (``cpus-per-task``), and 16 cores per CPU/Socket (``ntasks-per-socket``). The compute nodes are being used exclusively in this run (``exclusive``), and the queue, which in SLURM is called ``partition``, is ``debug``. The code is executed using srun.

Job Information
~~~~~~~~~~~~~~~

After submitting the job the user can check the compute nodes under use or the job status by issuing the command ``squeue`` as

.. code-block:: julia

  $ squeue | grep USER_NAME
 
    JOBID PARTITION     NAME       USER ST       TIME  NODES  NODELIST(REASON)
    16868     debug     job1  USER_NAME  R    5:54:10      1  cn013
    16867     debug     job2  USER_NAME  R    5:54:15      1  cn012
    16866     debug     job3  USER_NAME  R    5:54:21      8  cn[001-008]

He/She can learn further detailed information on the submitted job, e.g., used resources, paths, scripts, etc., by executing ``scontrol show jobid <JOBID>``, with <JOBID> being the job id:

.. code-block:: julia
  
  $ scontrol show jobid 16951

  JobId=16951 JobName=<JOB NAME>
   UserId=<UserID> GroupId=<GroupID> MCS_label=N/A
   Priority=2484 Nice=0 Account=<ProjID> QOS=normal
   JobState=RUNNING Reason=None Dependency=(null)
   Requeue=1 Restarts=0 BatchFlag=1 Reboot=0 ExitCode=0:0
   RunTime=02:07:25 TimeLimit=1-00:00:00 TimeMin=N/A
   SubmitTime=2022-07-19T09:15:43 EligibleTime=2022-07-19T09:15:43
   AccrueTime=2022-07-19T09:15:43
   StartTime=2022-07-19T09:15:43 EndTime=2022-07-20T09:15:43 Deadline=N/A
   SuspendTime=None SecsPreSuspend=0 LastSchedEval=2022-07-19T09:15:43
   Partition=debug AllocNode:Sid=mn01:9703
   ReqNodeList=(null) ExcNodeList=(null)
   NodeList=cn[005-006]
   BatchHost=cn005
   NumNodes=2 NumCPUs=72 NumTasks=72 CPUs/Task=1 ReqB:S:C:T=0:0:*:*
   TRES=cpu=72,node=2,billing=72
   Socks/Node=* NtasksPerN:B:S:C=0:0:18:* CoreSpec=*
   MinCPUsNode=1 MinMemoryCPU=4600M MinTmpDiskNode=0
   Features=(null) DelayBoot=00:00:00
   OverSubscribe=OK Contiguous=0 Licenses=(null) Network=(null)
   Command=<PROJECT_PATH>/<USER_FOLDERS>/slurm.sh
   WorkDir=<PROJECT_PATH>/<USER_FOLDERS>
   StdErr=<PROJECT_PATH>/<USER_FOLDERS>/slurm-16951.err
   StdIn=/dev/null
   StdOut=<PROJECT_PATH>/<USER_FOLDERS>/slurm-16951.out
   Power=
   

Hold and Release Jobs
~~~~~~~~~~~~~~~~~~~~~
   
Submitted jobs that are not running yet, because they are in a pending state, can be put on hold by using the command

.. code-block:: julia

  $ scontrol hold <jobid>
  
The same job can be released using

.. code-block:: julia

  $ scontrol release <jobid>

Accounting
----------

The user can always use ``sacct`` to see the CPU time used by his/her jobs by using, for example,

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


For more information on the command sacct options at the terminal execute ``man sacct``
 
The total computing time consumed by the users of a project, say ProjID, over a period of time, say from 01.01.2022 through 18.07.2022 is obtained using the command ``sreport``

.. code-block:: julia
  
  $ sreport -t Hours cluster AccountUtilizationByUser Accounts=projID start=1/1/22 format=Accounts,Login,Used,Energy

  --------------------------------------------------------------------------------
  Cluster/Account/User Utilization 2022-01-01T00:00:00 - 2022-07-18T23:59:59
  Usage reported in CPU Hours
  --------------------------------------------------------------------------------
          Account     Login      Used     Energy 
  --------------- --------- --------- ---------- 
           projID              211007    2217368 
           projID    user01     4030       45434 
           projID    user01      1711      23285 
           projID    user01     41505     525459 
           projID    user02     58204     542022 
           projID    user02    105558    1081168
 
This shows the computing time (Hours) and energy (Joules) consumed by the project members, user01 and user02 and by the project.

For further information see the user manual using ``man sreport``.

Most Commonly Used Slurm Commands
---------------------------------

.. list-table::   
    
  * - sbatch
    - Submit a batch script (which can be a bash, Perl or Python script. 
  * - salloc
    - Request an allocation. 
  * - srun
    - Create a job step within an job
  * - squeue
    - Query the list of pending and running jobs
  * - scancel
    - Cancel pending or running jobs or to send signals to processes in running jobs or job steps. Use ``scancel <jobid>``
  * - scontrol
    - Query information about compute nodes and running or recently completed jobs. Can use ``scontrol show job <jobid>``
  * - sacct
    - Retrieve accounting information for jobs and job steps
  * - sinfo
    - Retrieve information about the partitions and node states
  * - sprio
    - Query job priorities
  * - smap
    - Graphical display of the state of the partitions and nodes using a curses interface
  * - sattach
    - Attach to the standard input, output or error of a running job
  * - sstat
    - Query information about a running job
