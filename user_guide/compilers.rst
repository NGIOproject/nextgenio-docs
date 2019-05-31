Modules and Compilers
=====================

Modules
~~~~~~~
The application development environment on NextgenIO is primarily
controlled through the *modules* environment. By loading and switching
modules you control the compilers, libraries and software available.

This means that for compiling on Cirrus you typically set the compiler
you wish to use using the appropriate modules, then load all the
required library modules (e.g. numerical libraries, IO format libraries).

Additionally, if you are compiling parallel applications using MPI 
you will need to load one of the MPI environments
and use the appropriate compiler wrapper scripts.

Basic usage of the ``module`` command is covered below. For
full documentation please see the  `Linux manual page on modules
<http://linux.die.net/man/1/module>`__.

Information on the available modules
------------------------------------

Finding out which modules (and hence which compilers, libraries and
software) are available on the system is performed using the
``module avail`` command:

::

    $>  module avail
    ...

This will list all the names and versions of the modules available on
the service. Not all of them may work in your account though due to,
for example, licencing restrictions. You will notice that for many
modules we have more than one version, each of which is identified by a
version number. One of these versions is the default. As the
service develops the default version will change.

You can list all the modules of a particular type by providing an
argument to the ``module avail`` command. For example, to list all
available versions of the MPI library:

::

    $> module avail mpi
 
    ---------- /opt/ohpc/pub/moduledeps/intel-impi ----------
       mpiP/3.4.1

    ----------- /opt/ohpc/pub/moduledeps/intel --------------
       impi/2019.3.199 (L)    mpich/3.3    openmpi/1.10.7

    ------------ /home/software/modulefiles -----------------
       mpi4py/python2-openmpi    mpi4py/python2    mpi4py/python3-openmpi    mpi4py/python3 (D)

       Where:
        D:  Default Module
        L:  Module is loaded


If you want more info on any of the modules, you can use the
``module help`` command:

::

    $> module help impi

    ---------- Module Specific Help for "impi/2019.3.199" ------------
 
    This module loads the Intel MPI environment
 
    mpiifort     (Fortran source)
    mpiicc       (C   source)
    mpiicpc      (C++ source)
 
    Version 2019.3.199

The simple ``module list`` command will give the names of the modules
and their versions you have presently loaded in your envionment:

::

    $> module list

    Currently Loaded Modules:
    1) autotools   2) prun/1.3   3) intel/19.0.3.199   4) impi/2019.3.199   5) ohpc   6) packages-nextgenio

Loading, unloading and swapping modules
---------------------------------------

To load a module to use ``module add`` or ``module load``. For example,
to load the intel-compilers-17 into the development environment:

::

    module load openmpi

This will load the default openmpi version. If there is more than one
version available, it is possible to specify the version in the 
module load command, bypassing the default settings if so desired.

If you want to clean up, ``module remove`` will remove a loaded module:

::

    module remove openmpi

(or ``module rm openmpi`` or ``module unload openmpi``) will unload 
whichever version of openmpi you have loaded. 

There are many situations in which you might want to change the presently 
loaded version to a different one, such as trying the latest version which 
is not yet the default, using a legacy version to keep compatibility with old data,
or because the new module to be loaded is incompatible with the old one.
Swapping can be achieved most easily by using "module swap oldmodule newmodule". 

Suppose you wish to swap ``openmpi`` for ``impi``: 

::

    module swap openmpi impi

In case two modules are incompatible, simply trying to load the conflicting
module will result in an error.

C/C++ Compilers
~~~~~~~~~~~~~~~

Various compilers are available on NextgenIO. Different
combinations of compilers for multithreading and MPI code 
can be used, although performance may vary.

The available C/C++ compilers are ``icc`` (the Intel
compiler) and ``gcc`` (the GNU compiler), and the available
MPI libraries are *openmpi* and *impi* (Intel's MPI 
library). The MPI libraries are used by the compiler 
through the ``mpicc`` and ``mpigcc`` wrappers, and the
library is selected by loading the relevant module.

The MPI modules are simply named ``openmpi`` and ``impi``.
The modules use ``icc`` as the standard C/C++ compiler,
but ``gcc`` is available for both. To switch the compiler
used by the ``mpicc`` wrapper to ``gcc`` in the ``openmpi``
module:

::

   export OMPI_CC=gcc

This option may be the preferred way of using the ``gcc``
compiler when running mpi code, as executables created
with the ``mpigcc`` wrapper appear to have trouble 
running on the NextgenIO system.

Fortran Compilers
~~~~~~~~~~~~~~~~~

Compiling Examples
~~~~~~~~~~~~~~~~~~

Below we consider several examples of ways to compile
code that uses both MPI and multithreading, using the
`xthi <https://github.com/olcf/XC30-Training/blob/master/
affinity/Xthi.c>`_ example, as well as possible effects on 
performance, using the :ref:`sec-ref-castep` as a trial
application.

OpenMP
------

Code including OpenMP based multithreading can be compiled
using ``gcc`` and the flag *-fopenmp*:

::

    gcc openmp_code.c -fopenmp -o openmp_exec

MPI
---

MPI code can be compiled as follows:

::

    mpicc mpi_code.c -o mpi_exec

And for combined MPI and OpenMP applications:

::

    mpicc mix_code.c -fopenmp -o mix_exec


::

    - How to run mpicc with gcc rather than icc?


