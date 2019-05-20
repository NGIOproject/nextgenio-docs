.. _sec-ref-scheduler:

Job Scheduler
=============

The NextgenIO system makes use of the an adapted version of the `Slurm 
workload manager <https://slurm.schedmd.com/overview.html>`_. The 
adaptations make it possible for the user to make use of the various
implementations of SCM (Storage Class Memory).

This section will first introduce the basic usage of Slurm, with the help
of some simple examples. The second part of the will expand on this, and 
introduce the new usage of Slurm on the NextgenIO system.

Slurm
~~~~~

This section will provide a short overview of some of the basic commands
required to run jobs with Slurm. 

Please note: the NextgenIO system comes equiped with OpenMPI (version 
3.1.0) and Intel MPI (version 2019.3.199). The usage of Slurm should 
reflect the choice of MPI flavour, as will indicated in the guides in
this section where relevant.

For a detailed manual on the use of Slurm, please consult the `Slurm 
documentation pages <https://slurm.schedmd.com/documentation.html>`_.

Basic Commands
--------------

**System Status**

+---------+--------------------------------------------------------------------------+
| sinfo   || list available partitions, nodes on these partitions and the status of  |
|         || these components. Availability is listed as either *up* or *down*.      |
|         || For a full list of node-by-node status enter: *sinfo --long --Node*     |
+---------+--------------------------------------------------------------------------+
| squeue  || list the jobs currently submitted to the system. Basic functionality    |
|         || returns all jobs, provides the JOBID, the job owner, the requested      |
|         || number of nodes and either the allocated nodes or the reason for being  |
|         || queued.                                                                 |
+---------+--------------------------------------------------------------------------+

**Submitting Jobs**

Submitting a job using the scheduler comprises two steps: allocating 
resources and specifying the job itself. Slurm support three main 
methods for launching jobs: `srun <https://slurm.schedmd.com/srun.html>`_,
`salloc <https://slurm.schedmd.com/salloc.html>`_, and `sbatch <https:
//slurm.schedmd.com/sbatch.html>`_.  

OpenMPI jobs can be run using all three method, and for Intel
MPI jobs the ``srun`` method is recommended. Slurm is compatible
with the mpirun method for both flavours.

Below is a quick overview of the command usage, with more
detailed examples listed further down.

+---------+--------------------------------------------------------------------------+
| srun    || submit a job by allocating resources and specifying the executable.     |
|         || Basic example, running 'mycode' on 2 nodes:                             | 
|         ||    $> srun -N2 mycode                                                   |
+---------+--------------------------------------------------------------------------+
| salloc  || allocate resources. Adding 'sh' at the end of the command launches an   |
|         || interactive shell, whence to start the job. Basic example, requesting 4 |
|         || nodes:                                                                  |
|         ||    $> salloc -N4 sh                                                     |
|         ||    > srun mycode (OR: mpirun mycode)                                    |
+---------+--------------------------------------------------------------------------+
| sbatch  || submit a batch script to the scheduler. Resource allocation can be      |
|         || specified in the command line. The batch file should contain a statement|
|         || the job. A script can contain multiple srun/mpirun commands:            |
|         ||    $> sbatch -N4 myscript.sh                                            |
+---------+--------------------------------------------------------------------------+

The commands and examples listed in this section are intended only
to show the basic functionality of the scheduler. All commands have
many available options and switches, and are therefore highly adaptable
to user requirements. The Slurm documentation provides full details
and examples for the interested reader.

To cancel a submitted job, use `scancel <https://slurm.schedmd.com/
scancel.html>`_, followed either by the JobID or by cancelling all
of a user's jobs with ``scancel -u [username]``.

**Example Subsmission Scripts**

The following examples (based on the helpful tutorial found `here 
<https://support.ceci-hpc.be/doc/_contents/QuickStart/Submitting
Jobs/SlurmTutorial.html>`_) are batch scripts for different examples
of parallel runs.

In all cases the scripts illustrate the basic usage of the scheduler:
allocate resources, specify the job to be run, and then run it. We will
also specify the job output location. The default location for both
standard out and standard error is the file "slurm-j[JOB_ID]". In the
case of using batch scripts, the commands to be passed to the scheduler
are preceded by the the `#SBATCH` comment.

Upon submission of the script the job will be added to the queue, until
the requested resources become available and the job is launched.
 
*1) Submitting a single job many times (without shared memory)*

This type of parallellism is usually associated with embarrassingly
parallel problems.

The first lines of the bash script ("myscript.sh") allocates 
resources. Note that the default allocation of CPUs is one per 
taks. Default units for the memory per CPU are MB. The 'array' 
option creates multiple instances of the same task (in this 
example 10 instances). The task IDs are stored in the variable 
SLURM_ARRAY_TASK_ID. We will also set the maximum run time to
one hour per cpu.

::

    #!/bin/bash
    #SBATCH --ntasks=4
    #SBATCH --time=60:00
    #SBATCH --mem-per-cpu=150
    #SBATCH --array=1-10

The next lines specify the job name and output filename:

::

    #SBATCH --job-name=par_tasks
    #SBATCH --output=op_file.txt

And finally we tell the scheduler to run the job, passing
the task IDs to the ``srun`` command.

::

    srun mycode $SLURM_ARRAY_TASK_ID

We then submit the script with the following command:

::

    sbatch myscript.sh

*2) Submitting a single job with shared memory using OpenMP*

In case the script to be run uses multithreading, multiple 
CPUs can be asigned to the same task. For the job to run 
properly, we do need to explicitly tell Slurm to set the 
OPM_NUM_THREADS environment variable.

The rest of the script looks very similar to the previous
case:

::

   #!/bin/bash
   #SBATCH --ntasks=1
   #SBATCH --cpus-per-task=8
   #SBATCH --time=60:00
   #SBATCH --mem-per-cpu=150
   #
   #SBATCH --job-name=openmp_task
   #SBATCH --output=op_file.txt

   export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
   srun mycode

The script is submitted by entering:

::

    sbatch myscript.sh


*3) Submitting an MPI job*

When submitting an MPI job the script only needs to specify
the number of tasks and the amount of memory per core. 

::

   #!/bin/bash
   #SBATCH --ntasks=10
   #SBATCH --time=60:00
   #SBATCH --mem-per-cpu=150
   #
   #SBATCH --job-name=mpi_task
   #SBATCH --output=op_file.txt

   srun mycode

The script is submitted by entering:

::

    sbatch myscript.sh




Slurm on NextgenIO
~~~~~~~~~~~~~~~~~~

::

    Some examples are probably the quickest to show the way here

