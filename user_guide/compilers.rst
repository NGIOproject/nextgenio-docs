Compilers
=========

C and C++
~~~~~~~~~

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

Fortran
~~~~~~~
