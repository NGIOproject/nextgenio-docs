NEXTGenIO Documentation
=======================

This repository contains the documentation (in progress) for the NEXTGenIO prototype, aimed at publication on ReadTheDocs once finalised. 


The text format is `reStructured Text <http://sphinx-doc.org/rest.html>`_ and the HTML document is built using `Sphinx <http://www.sphinx-doc.org/en/master/>`_. You can test your build locally by running ``make html``. The easiest way to learn what files should look like is to read the ``rst`` files already in the repository.

The structure and some of the content of this document is based on the ReadTheDocs entry for Cirrus: `Cirrus Documentation (HTML) on ReadTheDocs <http://cirrus.readthedocs.io/>`_. The instructions listed below are from the Cirrus documentation repository on GitHub.

How to Contribute
-----------------
To contribute to this documentation, first you have to fork it on GitHub and clone it to your machine, see `Fork a Repo <https://help.github.com/articles/fork-a-repo/>`_ for the GitHub documentation on this process.

Once you have the git repository locally on your computer, you will need to install ``sphinx`` to be able to build the documentation. See the instructions below for how to achieve this.

Once you have made your changes and updated your Fork on GitHub you will need to `Open a Pull Request <https://help.github.com/articles/using-pull-requests/>`_.

Building the documentation on a local Windows machine
#####################################################

Install the following:-

* `Anaconda Python <https://store.continuum.io/cshop/anaconda>`_.

To build the HTML documentation run::

    make html

If you want to build the PDF documentation you will need:

* `GNU Make <http://gnuwin32.sourceforge.net/packages/make.htm>`_
* `MikTeX <http://miktex.org/download>`_

Then from the command line, the following will build the .pdf file ::

    make latexpdf

On first run, MikTeX will prompt you to install various extra LaTeX packages.

Building the documentation on a local Linux machine
###################################################

Have

* Python 2
* sphinx

installed, then run ::

     make html

Building the documentation on a local Mac machine
#################################################

For the HTML documentation you will need ``sphinx``. If you do not already have a python distribution installed, we recommend you install `Anaconda Python <https://store.continuum.io/cshop/anaconda>`_.

To build the HTML documentation run::

    make html


Making Changes to the Documentation
-----------------------------------

The documentation consists of a series of `reStructured Text <http://sphinx-doc.org/rest.html>`_ files which have the ``.rst`` extension.
These files are then automatically converted to HTMl and combined into the web version of the documentation by sphinx.
It is important that when editing the files the syntax of the rst files is followed.
If there are any errors in your changes the build will fail and the documentation  will not update, you can test your build locally by running ``make html``.
The easiest way to learn what files should look like is to read the ``rst`` files already in the repository.
