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

Most \*nix operating systems, including Linux and OS X, come with an installed
Python interpreter because Python is often used by the system.  It is *not*
however recommended to use this Python for development for a number of reasons.

The system Python can differ in minor version. While Python (the language) is
quite reliable when it comes to backwards compatibility between patch and minor
releases, there are a small number of compatibility-breaking changes which
developers are likely to encounter which make more recent minor versions
preferable over an older version which may have been preinstalled
[#noncompat]_.

.. [#noncompat] The most prominent recent incompatible change is the enabling
                of TLS certificate hostname verification, which is now turned
                *on* by default [finally] as of 2.7.10 in :pep:`476`.

Additionally, installing `packages <package>`  directly into the global system
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

On OS X, ``brew install pypy`` via `Homebrew` will install the
latest release of PyPy. On Linux, using :github:`Portable PyPy
<squeaky-pl/portable-pypy>` is recommended, which is how PyPy is
deployed to our production systems.


.. note::

    There are *2* relevant versions when dealing with PyPy.

    There is the release version of the PyPy interpreter (as of December 2015,
    the latest release is 4.0.1).

    Each PyPy release implements a specific version of Python *the language*.

    For instance, PyPy 4.0.1 implements Python 2.7.11.


:program:`virtualenv`
=====================

Each Python interpreter has associated with it a number of directories which it
will search when attempting to import a module (specifically when searching for
a file-backed module). `packages <package>` and `modules <module>` contained in
one of these directories are said to be :dfn:`installed`, and will be locatable
at runtime.  These directories are accessible at runtime via :data:`sys.path`
or can be modified externally via :envvar:`PYTHONPATH` (although modifying it
in either way is *strongly discouraged*).

:pypi:`virtualenv` is an *isolation mechanism* for Python.

It allows for the creation of "virtual", isolated environments which appear to
be full Python installations with their own paths for installed packages.

After installation in the usual manner, via e.g. ``pip install --user
virtualenv``, running ``virtualenv -p pypy venv`` will create a directory named
:file:`venv` (in the current working directory) containing a number of folders.
Most notably, it will contain a :file:`venv/bin/pypy` executable which is
completely isolated from the global Python installation. Installing packages
into that Python, via :file:`venv/bin/pip`, which will also be pre-installed,
will have no effect on the global directories, and vice versa.

.. warning::

    For simplicity's sake, activation of virtualenvs, which can be done via
    ``source venv/bin/activate`` is somewhat discouraged, because it introduces
    even more complexity (shell manipulation) to an already complex (even
    complicated or hacky -- don't read its implementation) tool.

.. note::

    A general note about shells:

    When executing :samp:`{foo}` in a shell, the shell walks :envvar:`PATH`
    looking for binaries in any directory mentioned whose name is
    :samp:`{foo}`. Once such a binary is encountered, the shell executes it,
    and *caches the absolute path to the binary so that it need not do the path
    walking again*.

    This detail often confuses new users (of shells or otherwise).

    Note even further that :samp:`which {foo}` can "lie"! More accurately, the
    path returned by ``which foo`` can be one which is *not* the :samp:`{foo}`
    binary that will be invoked when unqualified.

    Developers are encouraged to prefer :samp:`type -a {foo}` for investigating
    the location(s) of a :samp:`{foo}`, and to familiarize themselves with
    :program:`rehash` (``hash -r`` in :program:`bash`) which purges the
    :envvar:`PATH` cache.

    .. envvar:: PATH

        A (colon-delimited) set of directories which will be searched for
        binary executables.

        .. seealso:: `<https://en.wikipedia.org/wiki/PATH_%28variable%29>`_

.. seealso::

    :pypi:`virtualenvwrapper`
        A utility that wraps :program:`virtualenv` with a number of shell
        functions whose core goal is to manage a central repository of
        virtual environments.

    :pypi:`mkenv`
        A similar tool with a slightly different take.


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

Many `editors <editor>` and `test runners <test runner>` also integrate with a
debugger.

.. seealso::

    `winpdb <http://winpdb.org/>`_

        An unconfusingly named cross-platform debugger with some other
        interesting features.

    :program:`bpdb`

        A debugger that ships with :ref:`bpython <bpython>`

    Your `Editor <editor>`

        Which may offer integration with a debugger. E.g., for PyCharm, see
        `<https://www.jetbrains.com/pycharm/help/python-debugger.html>`_

.. note::

    In many cases, the use of a debugger is a crutch that indicates a gap in
    unit test coverage or general understanding of the code base.

    Guild members are encouraged to follow up uses of a debugger by improving
    the coverage or maintainability of the section of the code that they needed
    to inspect.


REPLs
======

The "vanilla" Python :term:`REPL` (i.e. the program which executes when running
:program:`python` or :program:`pypy`, which differs slightly in offering even
more features) is more than sufficient for many developers. It offers both
:mod:`readline and tab completion <rlcompleter>` support.

.. sidebar:: ...

    ... unless you're on Python 3, then `dabeaz has some bad news for you
    <https://twitter.com/dabeaz/status/618378554812796928>`_ (ed.: has now
    mostly been fixed).

Developers are encouraged to familiarize themselves with its functionality and
command line options. In particular, ``python -c`` and ``python -i`` are useful
for development, and ``python -m`` is an oft-used mechanism for running
executable modules. See :manpage:`python(1)` for more details.

.. _bpython:

Developers looking for more than the above are encouraged to try `bpython
<http://bpython-interpreter.org/screenshots.html>`__, which offers real-time
suggestions, syntax highlighting and other useful features. It is installable
in the usual way, via ``pip install --user bpython``.

:program:`ipython` / :program:`jupyter` is *not* generally recommended for
developers because its behavior differs *significantly* from Python in ways
that often (at least historically) have left beginners in situations where
their code works perfectly fine in a "vanilla" interpreter but does not when
executed in :program:`ipython`. Developers who still choose to use it for its
conveniences are of course more than welcome, but any issues while using it
should first be double checked by running a reproducible example in some other
interpreter.


Profilers
=========

There are a number of broad axes along which a developer might want to profile
a given piece of code.

For timing measurements the :mod:`cProfile` standard library module can offer
basic tracing and timing of function calls and dispatches when executing a
script.

Slightly more powerfully (albeit still a bit immature) is :pypi:`vmprof`.
:pypi:`vmprof` is a :dfn:`statistical profiler` which samples execution of a
running program at frequent intervals, recording the stack frames which were
active when the sample occurs.

Its performance is such that it is *suitable to run in production*, unlike the
aforementioned standard library module, which often introduces 10 or 100x
slowdowns.

.. note::

    Of course, developers should measure the impact of any significant change
    to an application, including the introduction of an online profiler.

:pypi:`line_profiler` is also worthy of mention, as it can give more
granularity than the standard library module by reporting on lines rather than
function calls.

For profiling memory usage, :pypi:`memory_profiler` is available and can offer
insight into heap allocation and object lifetimes.


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

There are a number of small tweaks that developers are encouraged to make, in
the hopes that they will help avoid a number of common gotchas.

The most important of which is to **set** :envvar:`PYTHONDONTWRITEBYTECODE` (to
``true`` or any other non-empty value, via e.g.

.. code-block:: sh

    PYTHONDONTWRITEBYTECODE=true; export PYTHONDONTWRITEBYTECODE

in your shell's configuration.) Pre-compilation of bytecode (which happens on
module import) is generally *more harmful than it is helpful* -- the time
savings for importing modules during development are extremely minimal, whereas
developers are often tripped up by a stale (unremoved, left over) :file:`.pyc`
file that still affects their test runs or runtime.

Disabling the pre-compilation generally therefore has no noticeable effects,
and is highly recommended. Compilation still will happen as part of
installation processes.

.. note::

    :ref:`PyPy`\ 's behavior regarding :file:`.pyc` files is generally more
    helpful even without the above recommendation, as, unlike `CPython`, it
    will `ignore lone pyc files by default
    <http://doc.pypy.org/en/latest/config/objspace.lonepycfiles.html>`_. It
    does so for similar reasoning.

.. warning::

    `tox` currently has some `unfortunate behavior
    <http://lists.idyll.org/pipermail/testing-in-python/2015-April/006376.html>`_
    that *unsets* :envvar:`PYTHONDONTWRITEBYTECODE` when executing tox
    environments, causing :file:`.pyc` files to potentially reappear.

    Be mindful of this until the behavior is fixed upstream.

In addition to disabling bytecode serialization, developers are also encouraged
to change Python's default warning behavior. Somewhat controversially, as of
Python 2.7, Python's behavior is to *hide* many warnings emitted by the
:mod:`warnings` module by default.

The `reasoning
<https://mail.python.org/pipermail/stdlib-sig/2009-November/000789.html>`_
provided for the change was to reduce noise seen by end-users from libraries
invoked on their behalf, which they potentially do not have the ability to
correct.

Developers are encouraged to turn warnings back on! The potential for not
seeing important warnings during development is well worth the noise. Our
recommendation is also ultimately to treat emitted warnings as failures for
guild test suites.

.. code-block:: sh

    PYTHONWARNINGS='default,ignore:Not importing directory:ImportWarning'
    export PYTHONWARNINGS

will cause Python to emit each warning once (and then to hide successive
instances of it). Note, confusingly, that the default behavior is *different*
from setting the above to ``"default"``.

The section at the end (concerning import warnings) will hide a specific,
generally meaningless warning, concerning paths that do not contain
:file:`__init__.py` files encountered during imports, which typically are paths
that simply do not contain Python files, and can safely be ignored.


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

    REPL
        Read/Edit/Print Loop -- loosely, the interactive interpreter where code
        can be entered and evaluated.

        .. seealso::
            `<https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop>`_
