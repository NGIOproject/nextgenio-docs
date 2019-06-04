FAQs and Common Issues
======================


- :ref:`ref-qconfig`
- :ref:`ref-qnojobfile`
- :ref:`ref-qcastepinstall`

.. _ref-qconfig:

Batch job submission failed: Requested node configuration is not available:
---------------------------------------------------------------------------

When trying to submit my batch job, I receive the error message:
  
.. Error:: sbatch: error: Batch job submission failed: Requested node configuration is not available  


.. container:: toggle

   .. container:: header

      **Answer:**

   .. container:: text

     |

     It is possible that the batch scripts requests more cores per node than can 
     be allocated. When using the ``cpus-per-task`` option in the batch script, 
     the maximum number of cores to be requested per task is 48 (the number of 
     physical cpus on the node). 

     If the intention is (e.g.) to use the total number of available logical 
     cores in a single node, by using hyperthreading, the following code can be
     used to request the correct resources. In this example we request a single
     node, with two MPI processes (one process per socket, unless the scheduler 
     is instructe otherwise):

     .. code:: python

        #SBATCH --nodes=1            ##Just for clarity here
        #SBATCH --ntasks=2
        #SBATCH --overcommit
        #SBATCH --cpus-per-task=48

     For this example, the total number of logical cpus requested is ntasks * 
     cpus-per-task = 96. This is the maximum number available on a node.

|

.. _ref-qnojobfile:

Slurmstepd: error: execve(): [exec-name] : No such file or directory
--------------------------------------------------------------------

.. container:: toggle

   .. container:: header

      **Answer**

   .. container:: text

     |

     This error occurs when the job scheduler cannot find the executable to run.
     Make sure that the path specified following mpirun/srun command is correct.
     Note that when the job is not submitted from the directory containin the 
     executable, the full path needs to be specified.

|

.. _ref-qcastepinstall:

Error running *make install* during CASTEP build
------------------------------------------------

.. container:: toggle

   .. container:: header

      **Answer**

   .. container:: text

      |

      The error is likely to occur when building some of the support utilities
      for CASTEP. However, the initial command ``make COMMS_ARCH=mpi FFTW=fftw3 
      MATHLIBS=mkl10`` should have built an executable ``castep.mpi`` and similarly 
      ``make install`` will have produced a functional executable ``castep.serial``.
      The latter may be of use for performing dryruns on any input files.

|

