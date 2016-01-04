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

On OS X, :command:`brew install python` via `Homebrew` will install the
latest (C)Python to a user-writeable location.

.. seealso::

    :ref:`PyPy` below for installation of a separate interpreter used by many
    production compnents. Its use has especially succeeded CPython for
    any new projects.

.. note::

    Other concerns notwithstanding, use the latest patch release, i.e.
    2.7.X, which as of December 2015 is currently 2.7.11.


.. _PyPy:

PyPy
====

.. todo:: Probably deserves to be extracted out

`PyPy <http://pypy.org/>`__ is a Python interpreter, distinct from
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

On OS X, :command:`brew install pypy` via `Homebrew` will install
the latest release of PyPy. On Linux, using `Portable PyPy
<https://github.com/squeaky-pl/portable-pypy>`_ is recommended, which is
how PyPy is deployed to our production systems.


.. note::

    There are *2* relevant versions when dealing with PyPy.

    There is the release version of the PyPy interpreter (as of December 2015,
    the latest release is 4.0.1).

    Each PyPy release implements a specific version of Python *the language*.

    For instance, PyPy 4.0.1 implements Python 2.7.11.


virtualenv
==========


Debuggers
=========


Profilers
=========


Avoiding Common Gotchas
=======================

.. seealso::

    This section concerns a number of common development environment
    gotchas. `language` has some more general information on possible
    hiccups.


Further Reading
===============

.. seealso::

    `testing-environment` for some guidance on setting up an environment
    suitable for running test suites.


Glossary
========

.. glossary::

    PyPy
        A Python interpreter implemented in `RPython`, distinct from `CPython`.

        .. seealso::

            :ref:`PyPy`

            http://pypy.org
                The PyPy homepage

    CPython
        A Python interpreter implemented in C. CPython is the implementation
        that ships with many operating systems, and generally is what is
        referred to when "Python" is used in an unqualified sense.

    RPython
        *Restricted* Python, a (valid, strict) subset of Python used
        to implement :term:`PyPy` (and other implementations of other
        languages) due to the features of the RPython toolchain, which
        include the ability to leverage a set of existing garbage
        collector implementations, a JIT compiler generator, and other
        useful tools for implementation of programming languages. See
        the :term:`PyPy` documentation for more details.
