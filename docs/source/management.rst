Resources and Job Management
============================

1. Projects and Users
---------------------

A computational project can have several users. A user can participate in several projects.

The project has a specific folder to be eecuted and has a initial disk quota of 10 TB. 

Each user has folders in the users and project directories.


2. Available Resources
----------------------

To check the available resources the user should execute

.. code-block:: julia

  $ sinfo

obtaining

.. code-block:: julia

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



.. code-block:: console

