=======================
Language Tips & Gotchas
=======================

Common (& Uncommon) Gotchas
===========================

.. glossary::

    gotcha
        a valid construct in a system, programm or programming language that:

            1)
                .. epigraph::
                    "works as documented but is counter-intuitive and almost
                    invites mistakes because it is both easy to invoke and
                    unexpected or unreasonable in its outcome"

                    -- https://en.wikipedia.org/wiki/Gotcha_(programming)

            2) doesn't work exactly like :samp:`{XYZ other programming
            language} I know`


.. seealso::
    `Facts and myths about Python names and values
    <http://nedbatchelder.com/text/names.html>`_

    `<http://eev.ee/blog/2012/05/23/python-faq-passing/>`_
        "How do I pass by reference? Does Python pass by reference or pass by
        value?"

    `<http://docs.python-guide.org/en/latest/writing/gotchas/>`_
        Language Gotchas from The Hitchhiker's Guide to Python


.. _eq-is-hard:

Implementing Equality & Comparison Checks
-----------------------------------------


Useful (& Unuseful) Libraries
=============================

collections.namedtuple
----------------------

Guild members are strongly encouraged to *avoid* using
`collections.namedtuple`.

:func:`~collections.namedtuple` is a subtle object with a very
specific use case, but its misuse is somewhat prevalent due often to
a combination of laziness (read: misplaced convenience) or lack of
awareness.

The typical reason to reach for :func:`~collections.namedtuple` is as a
mechanism for generating *boilerplate-less classes with field names*.
I.e., rather than writing:

.. code-block:: python

    class Point(object):
        def __init__(self, x, y):
            self.x = x
            self.y = y

a developer may initially be tempted to instead elect to use
:func:`~collections.namedtuple`, where our simple :code:`Point` class
can be defined as :code:`Point = namedtuple("Point", ["x", "y"])` or the
like. The savings from such a thing are even more pronounced when one
considers the fact that :func:`~collections.namedtuple`, a subclass of
`tuple`, will therefore give you implementations of :code:`__eq__` and
:code:`__repr__` (equality and debug representations, amongst others),
for free.

This is a noble goal, especially given additional complications like the
`trickiness of a correct equality implementation <eq-is-hard>`!

There are a number of problems with it however. Here's one:

.. testsetup::

    from collections import namedtuple

.. testcode::

    Point = namedtuple("Point", ["x", "y"])
    print Point(123, 789) == Point(123, 789)

.. testoutput::

    True

Great, so far so good.

.. testcode::

    House = namedtuple("House", ["street_number", "rating"])
    print House(123, 789) == Point(123, 789)

.. testoutput::

    True

Ouch.

This gotcha is also an illustrative example of the dangers of
inheritance in general, and more specifically of the dangers of overly
loose type comparisons.

Another common reason offered for reaching for
:func:`~collections.namedtuple` is to leverage the memory efficiency
of `tuple`\ s, and specifically of `__slots__`. However, defining
`__slots__` is essentially `completely unnecessary on PyPy
<http://morepypy.blogspot.ca/2010/11/efficiently-implementing-python-objects.html>`_.
(Its use on `CPython` is an optimization which should be
applied in the usual way -- after benchmarks have been written).

.. seealso::

    :ref:`Why Not... <characteristic:why>`
        Similar arguments proposed in the :mod:`characteristic` documentation
