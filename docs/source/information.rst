User Information, Partition Quotas & Accounting
===============================================

1. Users & Projects
-------------------

* Accounts are set for users associated to a project. Each project can have several users. Users can have several projects

* For project extension contact the OBLIVION Support Team at support@oblivion.uevora.pt.

* The users accounts associated to a project are closed 3 months after its completion. The accounts, project and data in the filesystems are removed at that time.


2. Available Filesystems & Storage Limits
-----------------------------------------

.. list-table:: 

  * - $HOME	
    - Home directory for user 
    - 20 GB
  * - $PROJECT	
    - Project directory
    - 10/20 TB → Can be increased upon demand
  * - $DATA
    - Massive volumes of data
    - 100 TB → Can be increased upon demand


3. Backups
----------

There are no backups for the filesystems. The user must download the data as soon as it is available.

4. Scheduler & Prioritization
-----------------------------

* Job scheduler: SLURM

* Job prioritization:	fairshare, waiting time in queues, job size, QOS.
 

5. Nodes Exclusivity
--------------------

User can set exclusivity of the compute nodes by using ``#SBATCH --exclusive`` in the submission script. Otherwise the resources manager (SLURM) may also allocate the compute nodes to other jobs.

6. Partitions
-------------

.. list-table::

  * - private
    - All compute nodes; Maxtime=72 hours; Exclusive for sys admin or national urgency runs (high priority)
  * - debug
    - 88 compute nodes: cn[001-088]; Maxtime=48 hours
  * - medium
    - 58 compute nodes: cn[001-058]; Maxtime=72 hours
  * - short
    - 30 compute nodes: cn[059-088]; Maxtime=48 hours

7. Accounting
-------------

* CPU time is charged for wall-clock time, that is, for the time nodes are occupied.

* CPU time is charged for complete nodes, regardless the number of cores used.

* Monthly average CPU time set for each project.

* Sliding usage window of 2 months: after two months the hours of the first month that have not been consumed will be accounted and deducted from the total available compute hours. The sliding window is moved every two months, so hours will be deducted on the 2nd, 4th and 6th, etc, months of the project.

* CPU time transited to other months will have a slower priority than the corresponding monthly time.

* CPU time not consumed at the end of the project will not be usable, unless there is a project extension.
