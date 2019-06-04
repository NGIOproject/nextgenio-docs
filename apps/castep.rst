.. _sec-ref-castep:

CASTEP
======

For full details on the on this software package please refer to the 
`CASTEP documentation <http://www.castep.org/>`_.

Building CASTEP
---------------

Extracting the .tar file will create the CASTEP-[version] directory.
Place the file *linux_x86_64_ifort19.mk* in the directory *obj/platforms*.
From the main CASTEP-[version] directory run the following command:

.. code::  bash

   make COMMS_ARCH=mpi FFTW=fftw3 MATHLIBS=mkl10

and follow the install instructions set in the makefile.


Executing CASTEP jobs
---------------------

Basic information about the content of the input files and way CASTEP
will execute the job can be found by doing a dryrun of the job first:
``castep.serial [job-name] -dryrun``

Here the castep implementation is serial, as the ``castep.mpi`` command
does not appear to be able to perform a dryrun

.. note::

   In order to perform a dryrun on the nextgenio system it is necessary
   to have the serial implementation of castep available. This can be done
   by following the initial build of the application with the ``build`` 
   command specified above, with the command ``build install``.
   This may result in the :ref:`ref-qcastepinstall`, however the castep.serial
   executable should be created nonetheless.

Various parameters for the run can be set in the [job-name].param file.
Possibly relevant parameters (from the perspective of performance) are 
``max_scf_cycles``, the number of iterations over the grid, and 
``write_checkpoint``,  which writes the full analysis to file in a time 
consuming step. The values can be set by adding a line to the param file, 
or by editing the relevant line if the option is already included.

The verbosity of the output files ( [job-name].castep ) can also be set in the
[job-name].param file with the ``iprint`` option, which takes values 0-3
in increasing level of verbosity. For maximum verbosity add the following
line to the .param file :

.. code:: bash

   iprint : 3


The recommended number of cores to request for a CASTEP run
is the total number of k-points (representing the sampling grid
of electron wave vectors). The k-points for a specific run setup can be
found by doing a dryrun first. 

The number of k-points for the standard configuration of the test runs
TiN and DNA are 8 and [...] respectively. 

One level below the number of k-points is the number of plane waves
to be calculated. 


