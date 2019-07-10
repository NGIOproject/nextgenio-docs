.. _sec-ref-castep:

CASTEP
======

For full details on the on this software package please refer to the 
`CASTEP documentation <http://www.castep.org/>`_.

Building CASTEP
---------------

The first requirement is a copy of the .tar file containing the CASTEP
libraries and installer. Extracting the .tar file will create the 
CASTEP-[version] directory. 

.. code:: bash

   tar -xzf  CASTEP-[version].tar

Place the file *linux_x86_64_ifort19.mk* in the directory 
*CASTEP-[version]/obj/platforms*. From within the
CASTEP-[version] directory run the following command:

.. code::  bash

   make COMMS_ARCH=mpi FFTW=fftw3 MATHLIBS=mkl10

and follow the install instructions when prompted (the instructions
are set in the makefile).

In the following we will make use of two trial datasets, provided on
the CASTEP website: `TiN <http://www.castep.org/CASTEP/TiN>`_,
`Al3x3 <http://www.castep.org/CASTEP/Al3x3>`_, and `DNA 
<http://www.castep.org/CASTEP/DNA>`_.

::

   How will the required linux makefile be made available to users?

Preparing a CASTEP job
----------------------

Basic information about the content of the input files and way CASTEP
will execute the job can be found by doing a dryrun of the job first:
``castep.serial [job-name] -dryrun``

Here the castep implementation is serial, as the ``castep.mpi`` command
does not appear to be able to perform a dryrun

.. note::

   In order to perform a dryrun on the nextgenio system it is necessary
   to have the serial implementation of castep available. This can be done
   conducting the initial build of the application that uses the ``make`` 
   command specified above, and follow this with the command ``make install``.
   This may result in the :ref:`ref-qcastepinstall`, however the castep.serial
   executable should be created nonetheless.

Various parameters for the run can be set in the *[job-name].param file*.
Possibly relevant parameters (from the perspective of performance) are 
``max_scf_cycles``, the number of iterations over the grid, and 
``write_checkpoint``,  which writes the full analysis to file in a time 
consuming step. The values can be set by adding a line to the .param file, 
or by editing the relevant line if the option is already included.

The verbosity of the output files ( [job-name].castep ) can also be set in the
[job-name].param file with the ``iprint`` option, which takes values 0-3
in increasing level of verbosity. For maximum verbosity add the following
line to the .param file :

.. code:: bash

   iprint : 3

The recommended minimum number of cores to request for a CASTEP run
is the total number of k-points (representing the sampling grid
of electron wave vectors). The k-points for a specific run setup can be
found by doing a dryrun first. Per k-point the calculation is spread
further (to solve for the plane waves), and split among the available
threads.

Executing a CASTEP job
----------------------

The following is an example batch script to submit to the job scheduler
that will run the analysis of the TiN dataset:

.. code:: bash

   #!/bin/bash

   #SBATCH --nodes=1
   #SBATCH --ntasks=8
   #SBATCH --cpus-per-task=6

   #SBATCH -D /path/to/TiN-directory
   #SBATCH -o /path/to/TiN-directory/TiN.out.%A.%N.log
   #SBATCH -e /path/to/TiN-directory/TiN.err.%A.%N.log
   #SBATCH --job-name=castep-tin

   export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK

   srun "/path/to/castep-build/CASTEP-18.1/obj/linux_x86_64_ifort19/castep.mpi" "path/to/TiN-directory/TiN-mp"


For the TiN, Al3x3, and DNA test sets, in the standard configuration, the 
number of k-points is 8, 2, and 1 respectively. In the example above
one MPI process is assigned per k-point, and the plane wave calculations
are spread over six cpus, each running one thread.

Performance of the system will depend both on the parallelisation of
the run and on the memory mode selected.

The type of parallelisation can be controlled in the batch script using
the ``--ntasks`` option and the ``OMP_NUM_THREADS`` variable. Some effects
of different parallelisation are explored in :ref:`sec-ref-perftools`.

Initial results indicate that running CASTEP on a node, or nodes, in App
Direct mode has a considerable performance benefit over a run in Memory
mode.




