.. _sec-ref-perftools:

Performance Analysis Tools
==========================

Various performance analysis tools are installed on the 
NextgenIO system. 

Score-P
~~~~~~~

Score-P provides an infrastructure to use various measurement
tools, including `Vampir`_, for parallel applications. 
The official site for Score-P can be found `here
<https://www.vi-hps.org/projects/score-p/>`__, and the 
documentation is located `here <http://scorepci.pages.
jsc.fz-juelich.de/scorep-pipelines/docs/scorep-5.0/html/>`__.

::

    - Need to include Score-p at compilation time


Vampir
~~~~~~

Vampir provides an interactive event trace visualiser, allowing
the post mortem analysis of parallel application runs. The website
for Vampir can be found `here <https://vampir.eu>`__, and a tutorial
can be accessed `here <https://vampir.eu/tutorial>`__.


Allinea/ARM MAP
~~~~~~~~~~~~~~~

ARM Map is a source level application profiler, designed to 
evaluate parallel applications. The official website is located 
`here <https://www.arm.com/products/development-tools/server-and
-hpc/forge/map>`__, and an introduction can be found `here <https:
//developer.arm.com/docs/101136/latest/map/getting-started>`__.

On NextgenIO ARM Map can be accessed after loading the ``arm-forge`` 
module:

::

    $> module load arm-forge

When the module is loaded, the easiest use of MAP is through the
GUI. This can be openend by entering 

::

    $> map

This will open the basic interface (note that this will require 
X11 forwarding to be enabled when connecting to NextgenIO with 
SSH - :ref:`sec-ref-connect`):

.. figure:: ../images/armmap_begin.png
   :align: center
   :scale: 50%
   :alt: Screenshot of ARM GUI opening screen

After selecting 'profiling' the menu shown below will load.
By selecting the application to be profiled, MAP will auto-detect
the type of application (e.g. OpenMP, MPI) the associated menu
options will be available. These options can also be set 
manually.

.. figure:: ../images/armmap_prof.png
   :align: center
   :scale: 80%
   :alt: Screenshot of ARM profiling GUI

Alternatively one can simply run MAP from the command line (or 
from a batch script) by prepending ``map`` to the command to run
the executable and adding the option ``--profile`` to disable the
GUI:

.. code:: bash

   map srun --nodes=2 --tasks-per-node=2 /path/to/myexec --profile


.. note::

   MAP can show a line by line usage report of the submitted code 
   over the course of the job's runtime. To enable this feature it
   is necessary to compile the code using the debug flag. For 
   example:

   .. code:: bash

      mpicc mycode.c -fopenmp -o myexec -g

   After this compilation MAP can be called as usual upon execution.
      

MAP profiling example: CASTEP
-----------------------------

This example will use the application :ref:`sec-ref-castep` to
illustrate the usage of MAP for benchmarking.

Job submission can be done either directly, by selecting 
*Run*, or by submitting a batch job to the queue (see section
see :ref:`sec-ref-scheduler`).

The following batch script submits a CASTEP job analysing the 
practice TiN dataset, moves the resulting files to the *output*
subdirectory, and calls MAP to profile the performance (the 
configuration is arbitrary, but has been choses such that the
number of tasks matches the number of k-points):

.. code:: bash

  #!/bin/bash

  #SBATCH --nodes=2
  #SBATCH --ntasks=8
  #SBATCH --cpus-per-task=12
  #SBATCH -p 2lm               #Request a node in Memory Mode

  #SBATCH -D /path/to/TiN
  #SBATCH -o /path/to/TiN/output/TiN.out.%A.%N.log
  #SBATCH -e /path/to/TiN/output/TiN.err.%A.%N.log
  #SBATCH --job-name=tin-job

  export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
  export KMP_AFFINITY=compact

  DIR="/path/to/castep-dir/"
  J="TiN-mp"                         #Name of the executable (and the assciated files in the TiN directory)

  map srun "${DIR}/CASTEP-18.1/obj/linux_x86_64_ifort19/castep.mpi" "${DIR}/test/TiN/$J" --profile

  mv "${DIR}test/TiN/${J}.castep" "${DIR}test/TiN/output"
  mv "${DIR}test/TiN/${J}.bands" "${DIR}test/TiN/output"
  mv "${DIR}test/TiN/${J}.bib" "${DIR}test/TiN/output"
  mv "${DIR}test/TiN/${J}.cst_esp" "${DIR}test/TiN/output"

  => This seems to result in an interminable run

::

   - Which modules need to be loaded for use?
   - Are there more tools that should be described here?
   - Include examples of use?

