Sumission Scripts
=================

1. Scripts for OpenMPI Compiled with GCC
-----------------------------------------



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
