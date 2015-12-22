=================
Testing in Python
=================

.. _testing-environment:

Test Running
============

Broadly speaking, there are two important parts to the test running
process: a `test framework` and a `test runner`.

The guild has generally standardized on using `unittest` as the test framework
of choice for a number of reasons:

    * portability of test suites (across `test runner`\ s) -- most test
      runners are compatible with test suites written for the standard
      library framework.

    * wide selection of test runners

    * simplicity of the framework

Other choices include the frameworks included with
`py.test <http://pytest.org/latest/>`_ and `nose
<https://nose.readthedocs.org/en/latest/>`_, along with the various
`BDD <https://en.wikipedia.org/wiki/Behavior-driven_development>`_
frameworks.

Given this choice of framework, there are a wide variety of test runners which
generally can successfully execute guild-written test suites.

If you have no previous preferences, the guild recommendation is to use
:program:`trial`, the runner included with `Twisted
<https://twistedmatrix.com/>`_. It can be installed in the usual manner by
running ``pip install --user Twisted``.

Tests for a given project can then be executed by running :samp:`trial
{some_project}` to execute the tests contained within the particular
project.

.. note::

    Running tests for a project often requires an additional set of
    `requirements <requirements.txt>` which should first be installed
    via ``pip install -r test-requirements.txt`` when present.

.. note::

    Executing tests, at least via :program:`trial`, does *not* require
    being in the same directory as the package, but generally *should*
    be done after first installing the package to be tested, typically
    via :samp:`pip install -e {the_repository}`.

.. seealso::

    `packaging`
        Guidelines for how to structure the package layout of a project.

    `green <https://pypi.python.org/pypi/green>`_
        An alternative test runner similar to :program:`trial`.

    `py.test <http://pytest.org/latest/>`_
        A test runner somewhat different from :program:`trial`, which
        includes (is built around) an alternate test framework but which
        can run `unittest` test suites as well.

Tox
---


Test Writing
============

.. seealso::

    `How to Write Docstrings for Tests
    <https://jml.io/pages/test-docstrings.html>`_

Glossary
========

.. glossary::
    Test Framework
        A Python library that exposes mechanisms for writing test cases.

        Beyond defining the API for writing an executable test, test
        frameworks offer features related to executing common setup and
        tear down for test cases along with other execution-specific
        functionality.

    Test Runner
        An executable whose job it is to :term:`discover <test
        discovery>`, :term:`assemble <test assembly>`, and execute a
        suite of Python tests.

        Runners support a variety of features related to the discovery
        and execution process. Some come with their own test framework,
        and some simply execute tests using the :mod:`unittest` standard
        library module.

    Test Assembly
       The process of assembling a `unittest.TestSuite` out of a
       collection of discovered tests, in order to execute them all at
       once.

       This process is a function generally performed by a
       `test runner`, obsoleting the need to manually create
       `unittest.TestSuite` instances in *most* cases.

        .. note::

            Not a widely used term, but no particularly standard term
            exists.

    Test Discovery
        The process of locating executable tests "within" a given Python
        object.

        There are slight differences in implementation between different
        `test runner`\ s, but generally for tests written using the
        `unittest` `test framework`, the discovery implementation will
        find tests in:

            * `modules` whose name begins with :file:`test_{something}`

                * within which, it will look for subclasses of
                  `unittest.TestCase`

                    * and execute any method whose name begins with
                      :samp:`test_{it_does_something}`
