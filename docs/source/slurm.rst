Most Commonly Used Slurm Commands
=================================

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
