========================
Commonly Useful Software
========================

.. _homebrew:

Homebrew
--------

`Homebrew <http://brew.sh/>`__ is the "missing" package manager for OS X.

Whereas `Macports <https://www.macports.org/>`_ preceded it and is another
extant package manager for OS X, Homebrew is (subjectively) easier to use, and
easier to author formulae for. It's focused on single-user installations (i.e.
formulae it installs go into :file:`/usr/local` and are available for all
users) but this generally is the case for OS X users.

The recommendation is generally to install it for developers using OS X, as it
will be the primary mechanism for installing software (binaries or otherwise).

.. warning::

    Do not run the installation command on the page linked above.

    You should never execute software you download off of the internet without
    first reading it, even when using a tool that does certificate validation,
    like :program:`curl` might. Doing so is a security risk which you should
    not tolerate. Resist the temptation for laziness -- even if you are not a
    ruby programmer, glancing at the code you're about to execute is better
    than nothing.

    Either download the file and open it (to read) before allowing
    :program:`ruby` to execute it, or use :program:`vipe` to pipe the download
    into before execution.

After installation, you can use homebrew to install a large number of packages,
such as :ref:`pypy`. See :manpage:`brew(1)` for further info, or the
documentation from the above website.

.. seealso::

    `brew cask <http://caskroom.io/>`_

        An extension for :ref:`homebrew` which can install various :file:`.app`
        bundles in addition to packages.

GNU :program:`Parallel`
-----------------------

:program:`awscli`
-----------------

:program:`diffutils`
--------------------

:program:`mtr`
------------------

:program:`par`
--------------

:program:`dstat`
------------------

.. _strace:

:command:`strace`
------------------
