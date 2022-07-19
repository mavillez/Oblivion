Resources and Job Management
============================

1. Projects and Users
---------------------

A computational project can have several users. A user can participate in several projects.

The project has a specific folder to be eecuted and has a initial disk quota of 10 TB. 

Each user has folders in the users and project directories.


2. Available Resources
----------------------

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

In order to see what compute nodes are being used by user USER_NAME execute ``squeue``

.. code-block:: julia

  $ squeue | grep USER_NAME
 
    JOBID PARTITION     NAME       USER ST       TIME  NODES  NODELIST(REASON)
    16868     debug     job1  USER_NAME  R    5:54:10      1  cn013
    16867     debug     job2  USER_NAME  R    5:54:15      1  cn012
    16866     debug     job3  USER_NAME  R    5:54:21      8  cn[001-008]


Accounting
----------

Job Submission & Information
----------------------------

The user submits the job to the system by using a SLURM script using the command

.. code-block:: julia
  
  $ sbatch <slurm_script_name>
     
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
   UserId=dianagomes(15051) GroupId=dianagomes(15051) MCS_label=N/A
   Priority=2484 Nice=0 Account=cpca21a208 QOS=normal
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


