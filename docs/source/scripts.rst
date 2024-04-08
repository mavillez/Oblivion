Submission Scripts
==================

The user needs to set the working environment before compiling the codes. That environment will be assumed by the system when job submisison is made using 

.. code-block:: bash

  sbatch script_name.sh
  
The scripts include all the needed information about the requested resources (cores, number of CPUs, memory, etc.,), computing time, and account to be charged. In addition the script can include compilation instructions, setup of system variables, loading modules, etc..

1. Setting System Variables in the Scripts
------------------------------------------

In order to prevent annoying warnings during the course of run it is mandatory to include the following variables in the scripts.

* In Scripts associated to OpenMPI compiled with GCC include

.. code-block:: bash

  export PMIX_MCA_psec=native

This prevents the warnings due to Munge and PSEC similar to 

.. code-block:: bash

   ----------------------------------------------------------------
   A requested component was not found, or was unable to be opened.  
   This means that this component is either not installed or is 
   unable to be used on your system (…)

   Host:      cn061
   Framework: psec
   Component: munge

* In scripts associated to Intel MPI include

.. code-block:: bash

  export I_MPI_FABRICS=shm:ofi 
  export FI_PROVIDER=tcp
  export SLURM_MPI_TYPE=pmi2

This prevents the warning 

.. code-block:: bash
  
  MPI startup(): PMI server not found. Please set I_MPI_PMI_LIBRARY variable if it is not a singleton case.


2. Scripts for OpenMPI + GCC
----------------------------

2.1 Example of a script without modules loading
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

  #!/bin/bash
  #SBATCH --time=00-00:40:00
  #SBATCH --account=ACCOUNTID
  #SBATCH --job-name=JOB_NAME
  #SBATCH --output=zeus_%j.out
  #SBATCH —-error=zeus_%j.error
  #SBATCH --nodes=16
  #SBATCH --ntasks=512
  #SBATCH --cpus-per-task=1
  #SBATCH --ntasks-per-socket=16
  #SBATCH --exclusive
  #SBATCH --partition=PARTITION

  export PMIX_MCA_psec=native

  srun ./executable

In this script we are setting the number of MPI tasks (ntasks), the number of cores per task (cpus-per-task) and the number of tasks per CPU also referred as socket (ntasks-per-socket). So, this script imposes that 1 core executes 1 MPI task. The compute nodes are being used exclusively by this run (option exclusive), and the queue, which in SLURM is called partition, is the debug queue. Finally the code is executed using srun. 


2.2 Example of a script with modules loading
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

  #!/bin/bash
  #SBATCH --time=00-00:40:00
  #SBATCH --account=ACCOUNTID
  #SBATCH --job-name=JOB_NAME
  #SBATCH --output=zeus_%j.out
  #SBATCH —-error=zeus_%j.error
  #SBATCH --nodes=16
  #SBATCH --ntasks=512
  #SBATCH --cpus-per-task=1
  #SBATCH --ntasks-per-socket=16
  #SBATCH --exclusive
  #SBATCH --partition=PARTITION

  export PMIX_MCA_psec=native

  module purge
  module load foss/2021b HDF/4.2.15

  srun ./executable
  

3. Script for Intel MPI 
-----------------------

Follows and example of a script to be used with Intel MPI and modules loading. Similarly scripts can be written without modules.

.. code-block:: bash

  #!/bin/bash
  #SBATCH --time=00-00:40:00
  #SBATCH --account=ACCOUNTID
  #SBATCH --job-name=JOB_NAME
  #SBATCH --output=zeus_%j.out
  #SBATCH —-error=zeus_%j.error
  #SBATCH --nodes=16
  #SBATCH --ntasks=512
  #SBATCH --cpus-per-task=1
  #SBATCH --ntasks-per-socket=16
  #SBATCH --exclusive
  #SBATCH --partition=PARTITION

  export I_MPI_FABRICS=shm:ofi 
  export FI_PROVIDER=tcp
  export SLURM_MPI_TYPE=pmi2

  module purge
  module load iimpi/2022a

  srun ./executable

4. Software Compilation and Modules Loading
-------------------------------------------

Compilation instructions are allowed in a script and the path for the executable can be set.

.. code-block:: bash

  #!/bin/bash
  #SBATCH --time=00-00:40:00
  #SBATCH --account=ACCOUNTID
  #SBATCH --job-name=JOB_NAME
  #SBATCH --output=zeus_%j.out
  #SBATCH —-error=zeus_%j.error
  #SBATCH --nodes=16
  #SBATCH --ntasks=512
  #SBATCH --cpus-per-task=1
  #SBATCH --ntasks-per-socket=16
  #SBATCH --exclusive
  #SBATCH --partition=PARTITION

  export I_MPI_FABRICS=shm:ofi 
  export FI_PROVIDER=tcp
  export SLURM_MPI_TYPE=pmi2

  module purge
  module load iimpi/2022a

  mpicc mpitest.c -o mpitest_exec

  srun ./executable


5. Modules Path
---------------

The modules path, declared in the variable ``MODULEPATH``, is loaded into the user environment at login. 
However, the user can add MODULEPATH into the batch script. This is useful when there are different software 
stacks or there is a need to test different setups. 

So, the user can add int othe script the following line ``export MODULEPATH=<PATH TO THE MODULES CORE>`` before 
the list of modules to be loaded. Here is an example:

.. code-block:: bash

  #!/bin/bash
  #SBATCH --time=00-00:40:00
  #SBATCH --account=ACCOUNTID
  #SBATCH --job-name=zeus
  #SBATCH --output=zeus_%j.out
  #SBATCH —-error=zeus_%j.error
  #SBATCH --nodes=16
  #SBATCH --ntasks=512
  #SBATCH --cpus-per-task=1
  #SBATCH --ntasks-per-socket=16
  #SBATCH --exclusive
  #SBATCH --partition=PARTITION

  export PMIX_MCA_psec=native
  export MODULEPATH=/mnt/beegfs/stack/mn02470/modules/all/Core 
  
  module purge
  module load foss/2021b 
  module load TensorFlow/2.8.4 scikit-learn/1.0.2 matplotlib/3.4.3

  srun ./executable
  
Note in this example we included modules like TensorFlow, etc., but the user can add any modules he/she wishes. 

In this particular example when ``TensorFlow/2.8.4`` is loaded the same happens to ``SciPy-bundle/2021.10``. This can be checked in the command line terminal by issuing the following commands:

.. code-block:: console

   module purge
   module load foss/2021b TensorFlow/2.8.4
   module list

obtaining

.. code-block:: console

  Currently Loaded Modules:
    1) GCCcore/11.2.0     14) PMIx/4.1.0          27) libffi/3.4.2             40) JsonCpp/1.9.4                   
    2) zlib/1.2.11        15) OpenMPI/4.1.1       28) Python/3.9.6             41) NASM/2.15.05                    
    3) binutils/2.37      16) OpenBLAS/0.3.18     29) pybind11/2.7.1           42) libjpeg-turbo/2.0.6             
    4) GCC/11.2.0         17) FlexiBLAS/3.0.4     30) SciPy-bundle/2021.10     43) LMDB/0.9.29                     
    5) numactl/2.0.14     18) FFTW/3.3.10         31) Szip/2.1.1               44) nsync/1.24.0                    
    6) XZ/5.2.5           19) ScaLAPACK/2.1.0-fb  32) HDF5/1.12.1              45) protobuf/3.17.3                 
    7) libxml2/2.9.10     20) foss/2021b          33) h5py/3.6.0               46) protobuf-python/3.17.3          
    8) libpciaccess/0.16  21) bzip2/1.0.8         34) cURL/7.78.0              47) flatbuffers-python/2.0          
    9) hwloc/2.5.0        22) ncurses/6.2         35) dill/0.3.4               48) libpng/1.6.37                   
   10) OpenSSL/1.1        23) libreadline/8.1     36) double-conversion/3.1.5  49) snappy/1.1.9                    
   11) libevent/2.1.12    24) Tcl/8.6.11          37) flatbuffers/2.0.0        50) networkx/2.6.3                  
   12) UCX/1.11.2         25) SQLite/3.36         38) giflib/5.2.1             51) TensorFlow/2.8.4                
   13) libfabric/1.13.2   26) GMP/6.2.1           39) ICU/69.1             


6. Scripts for Specific Software
--------------------------------

6.1 Dalton
~~~~~~~~~~

For Dalton compiled with GCC and OpenMPI and available in the foss toolchain use a script similar to the following

.. code-block:: bash

  #!/bin/bash
  #SBATCH --time=00-00:40:00
  #SBATCH --account=ACCOUNTID
  #SBATCH --job-name=JOB_NAME
  #SBATCH --output=%x_%j.out
  #SBATCH --error=%x_%j.error
  #SBATCH --ntasks=180
  #SBATCH --cpus-per-task=1
  #SBATCH --ntasks-per-socket=18
  #SBATCH --exclusive
  #SBATCH --partition=PARTITION

  export PMIX_MCA_psec=native

  export DALTON_LAUNCHER=srun
  export DALTON_TMPDIR=`pwd`/temp
  mkdir -p $DALTON_TMPDIR

  module purge
  module load foss/2021b Dalton/2020.0

  input_file=example.dal
  molecule_file=example.mol

  dalton -t ${DALTON_TMPDIR} ${input_file} ${molecule_file}


6.2 GPAW
~~~~~~~~

For GPAW compiled with GCC and OpenMPI and found in the foss toolchain use a script similar to the following

.. code-block:: bash

  #!/bin/bash
  #SBATCH --time=00-00:40:00
  #SBATCH --account=ACCOUNTID
  #SBATCH --job-name=JOB_NAME
  #SBATCH --output=%x_%j.out
  #SBATCH --error=%x_%j.error
  #SBATCH --ntasks=180
  #SBATCH --cpus-per-task=1
  #SBATCH --ntasks-per-socket=18
  #SBATCH --exclusive
  #SBATCH --partition=PARTITION

  export PMIX_MCA_psec=native

  module purge
  module load foss/2021b GPAW/22.8.0

  srun gpaw python calculation_script.py script_args

Do not use the following script or similar - you end up having error messages and not running the code

.. code-block:: bash

  #!/bin/bash

  gpaw sbatch -- \
  --time=00:40:00 \
  --account=ACCOUNTID \
  --job-name=JOB_NAME \
  --output=vermi_%j.out \
  --error=vermi_%j.error \
  --ntasks=180 \
  --cpus-per-task=1 \
  --ntasks-per-socket=18 \
  --exclusive \
  --partition=PARTITION \
  config_file.py input_file

6.3 GROMACS
~~~~~~~~~~~

Below is a script example to run GROMACS/2021.5 which, in OBLIVION, is installed under the foss/2021b toolchain.


.. code-block:: bash

  #!/bin/bash
  #SBATCH --time=01:00:00
  #SBATCH --account=MY_ACCOUNT
  #SBATCH --job-name=MY_JOB_NAME
  #SBATCH --output=%x_%j.out
  #SBATCH --error=%x_%j.error
  #SBATCH --ntasks=180
  #SBATCH --cpus-per-task=1
  #SBATCH --ntasks-per-socket=18
  #SBATCH --exclusive
  #SBATCH --partition=PARTITION

  export PMIX_MCA_psec=native

  module purge
  module load foss/2021b GROMACS/2021.5

  # Run all needed preparation steps like 'gmx grompp ...'

  srun gmx_mpi mdrun -ntomp 1 -v -deffnm MY_JOB_NAME

Note that a 1 MPI task per core is set (actually this is the default), using 18 cores per CPU, hence 36 per 
compute node and 5 compute nodes. As can be seen in the script no call to ``mpirun`` is made - all processes 
distribution is managed by ``srun``.


6.4 QuantumEspresso
~~~~~~~~~~~~~~~~~~~

QuantumEspresso by default sets shared memory threads through OpenMP. Therefore, the user should control the 
number of threads to be used by setting in the script the variable ``OMP_NUM_THREADS``. In order to prevent the
use of threads in a compute node the script should include ``export OMP_NUM_THREADS=1``. Below is a script example.

.. code-block:: bash
   
  #!/bin/bash
  #SBATCH --time=00-00:40:00
  #SBATCH --account=ACCOUNTID
  #SBATCH --job-name=JOB_NAME
  #SBATCH --output=%x_%j.out
  #SBATCH --error=%x_%j.error
  #SBATCH --ntasks=180
  #SBATCH --cpus-per-task=1
  #SBATCH --ntasks-per-socket=18
  #SBATCH --exclusive
  #SBATCH --partition=PARTITION

  export OMP_NUM_THREADS=1
  export PMIX_MCA_psec=native

  module purge
  module load foss/2021b QuantumESPRESSO/7.0

  srun pw.x -i MY_INPUT.in


6.5 VASP
~~~~~~~~

Running the Vienna Ab-initio Simulation Package (VASP) requires some consideration in the number of cores to be used as well as on the shared memory threads (through OpenMP) to take into account. Similarly to QuantumEspresso one has to include in the script ``export OMP_NUM_THREADS=1``. Another issue relates to the the use of part or all cores available per socket. Below is a script to run VASP compiled against OpenMPI and OpenBLAS, Scalapack, etc..

.. code-block:: bash

  !/bin/bash

  #SBATCH --time=48:00:00
  #SBATCH --account=hpcc001
  #SBATCH --job-name=VASP_PuX
  #SBATCH --output=output
  #SBATCH --error=job_name_%j.error
  #SBATCH --ntasks=144
  #SBATCH --cpus-per-task=1
  #SBATCH --ntasks-per-socket=18
  #SBATCH --exclusive
  #SBATCH --partition=debug
  
  export OMP_NUM_THREADS=1

  module purge
  module load foss/2021b

  export PMIX_MCA_psec=native
  
  srun vasp_ncl 
  #srun vasp_std

where vasp_std and vasp_ncl denote the standard and non-collinear (including spin-orbit, etc.) versions, respectively.

If VASP is compiled with HDF5 then the script must also include ``module load HDF5/1.12.1``.

Using VASP compiled against Intel compilers requires the use of all cores in a socket for the solution to converge. 

6.6 STATA
~~~~~~~~~

For a job on a single node using 4 cores the script can look like

.. code-block:: bash

  !/bin/bash

  #SBATCH --time=48:00:00
  #SBATCH --account=ACCOUNT_NAME
  #SBATCH --job-name=stata
  #SBATCH --output=job_name_%j.out
  #SBATCH --error=job_name_%j.error
  #SBATCH --nodes=1
  #SBATCH --ntasks-per-node=4
  #SBATCH --exclusive
  #SBATCH --partition=debug
  
  module purge
  module load stata

  export PMIX_MCA_psec=native

  #run stata
  stata -b filename.do 

Note that the filename.do contains the list of commands you need to execute stata ifyou were running on the command line.




Acknowledgements
---------------

Alfredo Palace Carvalho (U. Évora & HPC Chair) provided the scripts for Dalton, GPAW, GROMACS, and QuantumEspresso.
Estelina Lora da Silva (U. Porto & HPC Chair) provided the script for VASP.
José Coutinho (U. Aveiro), not only provided the scripts, but is investigating the compilation modes and use of VASP with foss/2021b and Intel compilers.
