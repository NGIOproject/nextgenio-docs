FAQs and Common Issues
======================


- :ref:`ref-qconfig`
- :ref:`ref-qnojobfile`
- :ref:`ref-qsymbollookup`
- :ref:`ref-nomap`
- :ref:`ref-qcastepinstall`
- :ref:`ref-ospray_mpiinit`


FAQs
----

Some Common Issues
------------------

.. _ref-qconfig:

Batch job submission failed: Requested node configuration is not available:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

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
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

When trying to submit a job I receive the error

.. Error::

  Slurmstepd: error: execve(): [exec-name] : No such file or directory

where [exec-name] is the name of my executable.

.. container:: toggle

   .. container:: header

      **Answer**

   .. container:: text

     |

     This error occurs when the job scheduler cannot find the executable to run.
     Make sure that the path specified following mpirun/srun command is correct.
     Note that when the job is not submitted from the directory containing the 
     executable, the full path needs to be specified.

     .. note:: 

        Please note that lines in a batch script starting with *#SBATCH* are not 
        interpreted by bash. It is therefore not possible to reduce the amount of
        repetitious writing by specifying an environment variable for the start of 
        the path (as e.g. $MY_PATH will not be evaluated). Writing the full path 
        is the most reliable method.

|

.. _ref-qsymbollookup:

Symbol Lookup Error
^^^^^^^^^^^^^^^^^^^

When trying to run an executable (using either *srun* or *mpirun*) I receive the 
error (or similar)

.. Error:: 

   ./[exec-name]: symbol lookup error: /opt/intel/compilers_and_libraries_2019.3.199
   /linux/mpi/intel64/lib/libmpifort.so.12: undefined symbol: MPI_UNWEIGHTED

Here [*exec-id*] is the name of my executable:

.. container:: toggle

   .. container:: header

      **Answer**

   .. container:: text

      |

      This error can occur when the executable has been compiled using mpigcc in
      combination with openmpi. Try compiling using:

      .. code:: bash
      
         mpicc mycode.c -o myexec

      Add the ``-fopenmp`` option if the application also uses OpenMP.

|      

.. _ref-nomap:

map: command not found
^^^^^^^^^^^^^^^^^^^^^^

When trying to use the ARM-MAP profiling tool I receive the following error
message:

.. Error::

   /var/spool/slurm/d/[jobID]/slurm_script: line [...]: map: command not found

.. container:: toggle

   .. container:: header

      **Answer**

   .. container:: text

      |

      This error is likely due to the ``arm-forge`` module not being loaded. You
      can check the list of loaded modules by entering ``module list`` and load 
      the required module by entering ``module load arm-forge``

|

.. _ref-qcastepinstall:

Error running *make install* during CASTEP build
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

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

.. _ref-ospray_mpiinit:

Error running OSPRay MPI
^^^^^^^^^^^^^^^^^^^^^^^^

When trying to run OSPRay with the ``--osp:mpi`` setting, I receive the
following error:

.. Error::

   osp:api: could not find module initializer ospray_init_module_mpi

.. container:: toggle

   .. container:: header

      **Answer**

   .. container:: text

      |

      This error may occur when running OSPRay with the ``--osp:mpi`` setting.
      A possible cause is that library path is not linked correctly. In order to
      use MPI with OSPRay it is necessary to set *LD_LIBARY_PATH* to the OSPRay 
      build directory:

      .. code:: bash

         export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/path/to/build/directory

|



















