User Information, Partition Quotas & Accounting
===============================================

1. User/Project
---------------

* Accounts are set for users associated to a project. Each project can have several users.

* The accounts are closed 3 months after completion of the project. The accounts, project and data in the filesystems are removed at that time.


2. Available Filesystems
------------------------

.. list-table:: 

  * - $HOME	
    - Home directory for user
  * - $PROJECT	
    - Project directory related source code, binaries, etc.
  * - $DATA	
    - Storage location for large data
 

3. Storage Limits per User and Project 
--------------------------------------

.. list-table::

  * - $HOME	
    - 10 GB
  * - $PROJECT	
    - 10 TB → Can be increased upon demand
  * - $DATA	
    - 100 TB → Exclusive for large projects can be increased upon demand
    
4. Backups
----------

There are no backups for the filesystems. The user must download the data as soon as it is available.

5. Scheduler & Prioritization
-----------------------------

* Job scheduler	SLURM

* Job prioritization	fairshare, waiting time in queues, job size
 

6. Nodes Exclusivity
--------------------

Default exclusivity of compute nodes starts at 144 cores (4 compute nodes).

User can set exclusivity for the first 4 compute nodes.

7. Partitions
-------------

.. list-table::

  * - private
    - All compute nodes; Maxtime=72 hours; Exclusive for sys admin or national urgency runs (high priority)
  * - highmem
    - 30 compute nodes; Maxtime=72 hours cn[059-088] - Not available to users yet
  * - short	
    - 20 compute nodes; Maxtime=72 hours cn[001-020]
  * - medium	
    - 38 compute nodes; Maxtime=48 hours cn[021-058]
  * - debug	
    - 58 compute nodes; Maxtime=48 hours cn[001-058]
 

8. Accounting
-------------

* CPU time is charged for wall-clock time, that is, for the time nodes are occupied.

* CPU time is charged for complete nodes, regardless the number of cores used.

* Monthly average CPU time set for each project.

* Sliding usage window of 2 months: after two months the hours of the first month that have not been consumed will be accounted and deducted from the total available compute hours. The sliding window is moved every two months, so hours will be deducted on the 2nd, 4th and 6th, etc, months of the project.

* CPU time transited to other months will have a slower priority than the corresponding monthly time.

* CPU time not consumed at the end of the project will not be usable, unless there is a project extension.
