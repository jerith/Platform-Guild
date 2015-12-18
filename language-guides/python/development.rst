================================
A Python Development Environment
================================

Development environments are *highly* personal.

That being said, what follows are a number of ideas for assembling
one possible coherent development environment suitable for developing
production systems.


Goals
=====

Our goal is to download and install an interpreter (which we'll use to
run and develop our code), to install and configure a way to isolate
projects from each other, and to configure a preferred editor for
development.


Bullet version:

    * download & install versions of Python that will be used for development
    * provide isolation mechanisms for developing distinct Python projects
    * avoid a small number of common gotchas
    * editor setup


Interpreters
============

Most \*nix operating systems, including Linux and OS X, come with an
installed Python interpreter because Python is often used by the system.
It is *not* however recommended to use this Python for development for a
number of reasons.

The system Python can differ in minor version. While Python (the
language) is quite reliable when it comes to backwards compatibility
between patch and minor releases, there are a small number of exceptions
(most prominently TLS certificate verification, which is now turned *on*
by default [finally] as of 2.7.10 in :pep:`476`).

Additionally, installing packages directly into the global system
Python can conflict with package installations done by a system package
manager, or worse with dependencies of the operating system itself.

For these reasons, we recommend installing a non-system Python
installation to do development with.

On OS X, :command:`brew install python` via `Homebrew`_ will install the
latest (C)Python to a user-writeable location. However, see below about
PyPy.

.. seealso::

    PyPy below for installation of a separate interpreter used by many
    production compnents.

.. note::

    Other concerns notwithstanding, use the latest patch release, i.e.
    2.7.X, which as of December 2015 is currently 2.7.11.


PyPy
====

.. todo:: Probably deserves elaboration as its own topic

`PyPy <http://pypy.org/>`_ is a Python interpreter, distinct from
`CPython <https://en.wikipedia.org/wiki/CPython>`_, that offers
significant speed and memory benefits (amongst others), allowing Python
to compete more fairly with the performance of "fully precompiled"
languages.

It is used extensibly in our production environment.


.. note::

    For new projects, the guild recommendation is to always target a
    deployment directly to PyPy, there are very few disadvantages or
    reasons to start a new project deployed via CPython.


The PyPy `download page <http://pypy.org/download.html>`_ contains
installation instructions for a number of operating systems.

On OS X, :command:`brew install pypy` via `Homebrew`_ will install
the latest release of PyPy. On Linux, using `Portable PyPy
<https://github.com/squeaky-pl/portable-pypy>`_ is recommended, which is
how PyPy is deployed to our production systems.


.. note::

    There are *2* relevant versions when dealing with PyPy.

    There is the release version of the PyPy interpreter (as of December 2015,
    the latest release is 4.0.1).

    Each PyPy release implements a specific version of Python *the language*.

    For instance, PyPy 4.0.1 implements Python 2.7.11.


Avoiding Common Gotchas
=======================

.. seealso::

    This section concerns a number of common development environment
    gotchas. `Language Tips & Gotchas`_ has some more general
    information on possible hiccups.
