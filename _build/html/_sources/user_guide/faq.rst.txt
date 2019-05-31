FAQs and Common Issues
======================


- :ref:`ref-qconfig`
- :ref:`ref-nojobfile`

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
     cores in a single node, split over two MPI processes , the following code 
     will request the correct resources:

     .. code:: python

        #SBATCH --nodes=1            ##Just for clarity here
        #SBATCH --ntasks=2
        #SBATCH --overcommit
        #SBATCH --cpus-per-task=48

     For this example, the total number of logical cpus requested is ntasks * 
     cpus-per-task = 96. This is the maximum number available on a node.

.. _ref-nojobfile:

Slurmstepd: error: execve(): hello: No such file or directory
-------------------------------------------------------------

.. container:: toggle

   .. container:: header

      **Answer**

   .. container:: text

     |

     This error occurs when the job scheduler cannot find the executable to run.
     Make sure that the path specified following mpirun/srun command is correct.
     Note that when the job is not submitted from the directory containin the 
     executable, the full path needs to be specified.
