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
X11 forwarding to be enable when connecting to NextgenIO with 
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



Usage is illustrated with examples.

.. figure:: ../images/armmap_prof.png
   :align: center
   :scale: 80%
   :alt: Screenshot of ARM profiling GUI


MAP profiling an OpenMP programme
---------------------------------

Job submission can be done either directly, by selecting 
*Run*, or by submitting a batch job to the queue (see section
see :ref:`sec-ref-scheduler`).


MAP profiling an MPI programme
------------------------------

::

   - Which modules need to be loaded for use?
   - Are there more tools that should be described here?
   - Include examples of use?

