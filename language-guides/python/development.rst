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

On OS X, ``brew install python`` via `Homebrew` will install the
latest (C)Python to a user-writable location.

.. seealso::

    :ref:`PyPy` below for installation of a separate interpreter used by many
    production components. Its use has especially succeeded CPython for
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

It is used extensively in our production environment.


.. note::

    For new projects, the guild recommendation is to always target a
    deployment directly to PyPy, there are very few disadvantages or
    reasons to start a new project deployed via CPython.


The PyPy `download page <http://pypy.org/download.html>`_ contains
installation instructions for a number of operating systems.

On OS X, ``brew install pypy`` via `Homebrew` will install
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


Legacy Vagrant VMs
==================

While we are migrating away from Chef, it is still possible to run some of our
applications in a Chef-provisioned Vagrant VM. For example Thidwick repo
contains Vagrant configuration that starts the application, the Kyoto database
and the Kafka instance inside a single CentOS VM.

To get started (tested with Thidwick, Vagrant 1.7.4, Chef DK 0.10.0 and
VirtualBox 5.0.4):

1. Install `VirtualBox <https://www.virtualbox.org>`_

#. Install `Vagrant <https://www.vagrantup.com/downloads.html>`_

#. Install `ChefDK <https://downloads.chef.io/chef-dk>`_. For easy integration
   with Vagrant plugins, add this line to your ~/.profile file::

        $ eval "$(chef shell-init bash)"

   This will change your environment so the ChefDK ruby is used instead
   of the system-wide installation. It should be possible to configure
   system-wide ruby to work with vagrant plugins - if you know how to do
   that, please update this README.

#. Navigate to the *vm* directory
#. Install vagrant plugins::

        $ vagrant plugin install berkshelf chef

#. Launch the VM::

        $ vagrant up

   This will download and save the base image, and run chef to
   automatically configure the VM. If you are prompted to select a
   network adapter for bridged networking, choose whichever adapter is
   connected (usually WiFi).

   Virtual machine is configured to use "private network" mode:
   VirtualBox will create a new network adapter on your machine, usually
   called ``vboxnet0``. The VM is connected to the same network and all
   its ports are exposed. To find out the IP of the VM, run 'vagrant
   ssh' and execute 'ifconfig' on the VM.


Vagrant Cheat-Sheet

- *vagrant ssh*: SSH into the VM
- *vagrant halt*: shut down VM
- *vagrant up*: restart the VM
- *vagrant reload*: restart guest
- *vagrant destroy*: delete VM
- *vagrant provision*: re-run Chef


Debuggers
=========

The `pdb` module in the standard library is a debugger included alongside
Python which can be used both to debug running programs via `pdb.set_trace`,
and to inspect the state of the world after exceptions, via `pdb.pm`.

There are however other, more featureful options.

:pypi:`pudb` is a recommended console debugger which can display source code
*while* debugging, along with a number of other useful features. Its interface
matches `pdb` -- i.e. it can be inserted via e.g. ``pudb.set_trace`` and
``pudb.pm``, although it also provides useful helpers like ``pudb.runcall`` for
invoking a callable after dropping into the debugger, and also provides a
command line script (:program:`pudb`) which can enter another script after
first starting the debugger.

.. image:: /static/img/pudb-screenshot.png
    :alt: The pudb debugger

It can be installed in the `usual way <pip>`, via :code:`pip install pudb`.

Many `editor`\ s and `test runner`\ s also integrate with a debugger.

.. note::

    In many, if not all cases, the use of a debugger is a crutch that indicates
    a gap in unit test coverage or general understanding of the code base.

    Guild members are encouraged to follow up uses of a debugger by improving
    the coverage or maintainability of the section of the code that they needed
    to inspect.


Profilers
=========


.. _editor:

Editors
=======

Ah, the world's oldest question -- what editor should one use?

The typical answer to this question should be "whichever you are already
comfortable with".

Given no previous preference, Vim, Emacs, PyCharm, and Sublime Edit are all
fairly popular choices amongst Python developers.

The only strict "requirement" would be to configure your editor to always
insert 4 spaces when the tab key is pressed (rather than a hard tab character
-- note though that a hard tab should always be displayed as 8 spaces). Almost
any editor not named Notepad can be configured to do so (including nano!).


.. seealso::

    :doc:`style`
        Our style guide for more elaborate information.


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
