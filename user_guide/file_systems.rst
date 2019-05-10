.. _sec-ref-filesystems:

File Systems
============

In order to make use non-volatile memory (NMV) NextgenIO
makes use of a variety of file systems.

Management of data transfer by the Data Scheduler is 
facilitated by two newly developed file systems: `echofs`_ 
and `GekkoFS`_.

DAX
~~~


DevDAX
~~~~~~


DataClay
~~~~~~~~


echofs
~~~~~~

This file system allows for POSIX-like NVRAM based 
storage, and is used by the Data Scheduler to prefetch
(stage-in) data from the parallel file system (PFS/
storage) to the computing nodes, and to write data
from the computing nodes to the PFS upon completion
(stage-out).

echofs operates as a temporary, ad-hoc file system on
the computing nodes and on the PFS. It exists only as 
long as the batch job needs it. It hides any memory
hierarchies present in the computing node (section
:ref:`sec-ref-memmodes`), and presents the node to the OS
as a virtual storage device with a single mount point.

Following the submission of a batch job, provided the 
required resources are available, the Data Scheduler creates
an instance on echofs across the allocated computing 
nodes. echofs prefetches the required data, based on the
job's data requirements passed on by the Scheduler.

Once stage-in has been completed, the batch job can 
execute. The application can access the loaded files
via standard POSIX I/O functions, making the system
compatible with legacy applications. The NVRAM will
function as a faster form of traditional storage.

Any new files created during the job are written into 
the NVRAM buffers, only transferring to storage (stage-
out) once the job is completed. echofs therefore 
operates as a burst buffer.

GekkoFS
~~~~~~~




::

    Questions:
    - When using App Direct reserved NVDIMM space in Platform Memory Mode,
      what filing system is used to mount this memory?
    - Are DAX and DataClay both used?
