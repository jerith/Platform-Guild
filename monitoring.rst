=============================
Monitoring Real Time Services
=============================


Application Metrics
===================


Collection
----------

Push vs. Pull
-------------

Dashboards
----------


Exceptions & Tracebacks
=======================


In-Process Attachment
=====================

One potentially unique or useful form of runtime introspection is the ability
to attach and inspect running processes by *executing code*.

In interpreted languages such as Python, there is at least one "out of the box"
solution for exposing in-process attachment via `twisted.conch.manhole
<http://twistedmatrix.com/documents/current/api/twisted.conch.manhole.html>`_.

Instructions for doing so in :ref:`WSGI <WSGI>` applications, such as many our
own, can be found at
`<http://crochet.readthedocs.org/en/latest/introduction.html#ssh-into-your-server>`_.

Developers should consider the option of enabling such functionality when
deemed useful for the particular application.

Statistical Profiling
=====================


.. seealso::

    * `strace`
