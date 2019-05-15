.. _sec-ref-pycompss:

PyCOMPSs
========

PyCOMPSs is the Python binding for the COMPSs library, which
aims to ease the use of parallel programming for distributed 
infrastructures. The COMPSs library provides a programming 
model and a runtime. A brief description of the framework is
given below. Please refer to the `PyCOMPSs documentation
<https://pypi.org/project/pycompss/>`_ for a full overview. 

The PyCOMPSs programming model allows users to write code for
applications sequentially, splitting the operations of the 
programme over PyCOMPSs tasks. During execution the runtime
library will automatically (i.e. without required user input)
split these tasks over any available nodes, attempting to optimise
usage of resources.

On the NextgenIO system PyCOMPSs can be implemented by users by
importing the library. The object store :ref:`ref-sec-dataclay` 
has been optmised for use in combination with PyCOMPSs.

::

   - Do users need to load a module in order to ne able to load
     the library?
   - Does PyCOMPSs have special implementations involving SCM?
   => Include worked examples of applications using PyCOMPSs


