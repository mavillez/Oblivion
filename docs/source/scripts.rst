Sumission Scripts (Examples)
============================

The user needs to set the working environment before compiling the codes. That environment will be assumed by the system when job submisison is made using 

.. code-block:: console

  sbatch script_name.sh
  
The scripts include all the needed information about the requested resources (cores, number of CPUs, memory, etc.,), computing time, and account to be charged. In addition the script can include compilation instructions, setup of system variables, loading modules, etc..

1. Setting System Variables in the Scripts
------------------------------------------

In order to prevent anoying warnings during the course of run it is manadatory to include the following variables in the scripts.

* In Scripts associated to OpenMPI compiled with GCC include

.. code-block:: console

  export PMIX_MCA_psec=native

This prevents the warnings due to Munge and PSEC similar to 

.. code-block:: console

   ----------------------------------------------------------------
   A requested component was not found, or was unable to be opened.  
   This means that this component is either not installed or is 
   unable to be used on your system (…)

   Host:      cn061
   Framework: psec
   Component: munge

* In scripts associated to Intel MPI include

.. code-block:: console

  export I_MPI_FABRICS=shm:ofi 
  export FI_PROVIDER=tcp
  export SLURM_MPI_TYPE=pmi2

This prevents the warning 

.. code-block:: console
  
  MPI startup(): PMI server not found. Please set I_MPI_PMI_LIBRARY variable if it is not a singleton case.


2. Scripts for OpenMPI compiled with GCC 
----------------------------------------

Example of a script without modules loading
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: console

  #!/bin/bash
  #SBATCH --time=00-00:40:00
  #SBATCH --account=benchmarks
  #SBATCH --job-name=zeus
  #SBATCH --output=zeus_%j.out
  #SBATCH —-error=zeus_%j.error
  #SBATCH --nodes=16
  #SBATCH --ntasks=512
  #SBATCH --cpus-per-task=1
  #SBATCH --ntasks-per-socket=16
  #SBATCH --exclusive
  #SBATCH --partition=medium

  export PMIX_MCA_psec=native

  srun ./executable

In this script we are setting the number of MPI tasks (ntasks), the number of cores per task (cpus-per-task) and the number of tasks per CPU also referred as socket (ntasks-per-socket). So, this script imposes that 1 core executes 1 MPI task. The compute nodes are being used exclusively by this run (option exclusive), and the queue, which in SLURM is called partition, is the debug queue. Finally the code is executed using srun. 


Example of a script with modules loading
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: console

  #!/bin/bash
  #SBATCH --time=00-00:40:00
  #SBATCH --account=benchmarks
  #SBATCH --job-name=zeus
  #SBATCH --output=zeus_%j.out
  #SBATCH —-error=zeus_%j.error
  #SBATCH --nodes=16
  #SBATCH --ntasks=512
  #SBATCH --cpus-per-task=1
  #SBATCH --ntasks-per-socket=16
  #SBATCH --exclusive
  #SBATCH --partition=medium

  export PMIX_MCA_psec=native

  module purge
  module load foss/2021b HDF/4.2.15

  srun ./executable
  

3. Script for Intel MPI 
-----------------------

Follows and example of a script to be used with Intel MPI and modules loading. Similarly scripts can be written without modules.

.. code-block:: console

  #!/bin/bash
  #SBATCH --time=00-00:40:00
  #SBATCH --account=benchmarks
  #SBATCH --job-name=zeus
  #SBATCH --output=zeus_%j.out
  #SBATCH —-error=zeus_%j.error
  #SBATCH --nodes=16
  #SBATCH --ntasks=512
  #SBATCH --cpus-per-task=1
  #SBATCH --ntasks-per-socket=16
  #SBATCH --exclusive
  #SBATCH --partition=medium

  export I_MPI_FABRICS=shm:ofi 
  export FI_PROVIDER=tcp
  export SLURM_MPI_TYPE=pmi2

  module purge
  module load iimpi/2022a

  srun ./executable

4. Script with Compilation and Modules Loading
----------------------------------------------

Compilation instructions are allowed in a script and the path for the executable can be set.

.. code-block:: console

  #!/bin/bash
  #SBATCH --time=00-00:40:00
  #SBATCH --account=benchmarks
  #SBATCH --job-name=zeus
  #SBATCH --output=zeus_%j.out
  #SBATCH —-error=zeus_%j.error
  #SBATCH --nodes=16
  #SBATCH --ntasks=512
  #SBATCH --cpus-per-task=1
  #SBATCH --ntasks-per-socket=16
  #SBATCH --exclusive
  #SBATCH --partition=medium

  export I_MPI_FABRICS=shm:ofi 
  export FI_PROVIDER=tcp
  export SLURM_MPI_TYPE=pmi2

  module purge
  module load iimpi/2022a

  mpicc mpitest.c -o mpitest_exec

  srun ./executable

5. Script for GPAW
------------------

For GPAW compiled with GCC and OpenMPI and found in the foss toolchain use a script similar to the following

.. code-block:: console

  #!/bin/bash
  #SBATCH --time=00-00:40:00
  #SBATCH --account=benchmarks
  #SBATCH --job-name=vermi
  #SBATCH --output=vermi_%j.out
  #SBATCH --error=vermi_%j.error
  #SBATCH --ntasks=180
  #SBATCH --cpus-per-task=1
  #SBATCH --ntasks-per-socket=18
  #SBATCH --exclusive
  #SBATCH --partition=short

  export PMIX_MCA_psec=native

  module purge
  module load foss/2021b GPAW/22.8.0

  srun gpaw python config_file.py input_file


Do not use the following script or similar - you end up having error messages and not running the code

.. code-block:: console

  #!/bin/bash

  gpaw sbatch -- \
  --time=00:40:00 \
  --account=benchmarks \
  --job-name=vermi \
  --output=vermi_%j.out \
  --error=vermi_%j.error \
  --ntasks=180 \
  --cpus-per-task=1 \
  --ntasks-per-socket=18 \
  --exclusive \
  --partition=short \
  config_file.py input_file

