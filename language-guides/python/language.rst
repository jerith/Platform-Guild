=======================
Language Tips & Gotchas
=======================

Common (& Uncommon) Gotchas
===========================

.. glossary::

    gotcha
        a valid construct in a system, program or programming language that:

            1)
                .. epigraph::
                    "works as documented but is counter-intuitive and almost
                    invites mistakes because it is both easy to invoke and
                    unexpected or unreasonable in its outcome"

                    -- `<https://en.wikipedia.org/wiki/Gotcha_(programming)>`_

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


Useful (& Useless) Libraries
=============================

.. _discouraged-builtins:

Discouraged Built-ins
---------------------

There are a number of built-in objects (i.e. objects in the :mod:`__builtin__`
namespace, available without any imports) whose use is *strongly discouraged*.

The following is a brief list:

* :func:`basestring`
* :func:`callable`
* :func:`cmp`
* :func:`compile`
* :func:`eval`
* :func:`execfile`
* :func:`file`
* :func:`filter`
* :func:`globals` & :func:`locals`
* :func:`hasattr`
* :func:`id`
* :func:`input`
* :func:`isinstance`
* :func:`issubclass`
* :class:`long`
* :func:`reload`
* :func:`round`
* @\ :func:`staticmethod`
* :class:`type`
* :func:`vars`
* :func:`__import__`

The following built-ins are also of questionable use, and their use is
cautioned unless their limitations are understood:

* :func:`bin`, :func:`hex` & :func:`oct`
* :func:`delattr`
* :func:`dir`
* :func:`map`
* :func:`print` & :func:`raw_input`
* :func:`reduce`
* :func:`super`
* :func:`unicode`


.. _discouraged-stdlib:

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

The explanation for the above is that :func:`~collections.namedtuple`\ s are
first and foremost, *tuples*. They will degrade into tuples for comparisons,
and field names are strictly for readability -- they assign names to ultimately
*positional* components. Introducing the concept of positionality to a fresh,
new class is not often intentional -- an arbitrary class' fields should not be
orderable in some arbitrary order.

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

For these reasons, the use of :func:`~collections.namedtuple` is appropriate
only to convert an *existing* API that returned tuples into one that preserves
backwards compatibility but which returns more readable objects with field
names (a use case that is fairly rare at Magnetic).

And the noble goal of terseness? It's for this reason that libraries like
`characteristic`_ exist --
as ways of decreasing the lines of boilerplate necessary to define a class with
a number of "characteristic" fields or attributes, and upon which things like
comparisons are defined fairly "trivially". Guild members are encouraged to use
these libraries when value classes are desired.

.. seealso::

    :ref:`Why Not... <characteristic:why>`
        Similar arguments proposed in the `characteristic`_ documentation

.. _characteristic: http://characteristic.readthedocs.org/en/stable/


Frequently Asked Questions
==========================


How Do I...
-----------

... reload a Python module at runtime?
    Generally speaking, you don't, can't and shouldn't. You need to restart
    your process. If you wish to do so "regularly" in some specific domain,
    like while writing an IRC bot, the correct solution is often to handle
    respawning a subprocess via a bouncer, which will restart with the new
    version of the particular module or object.

    The reasons for this are fairly simple: in order to "properly" accomplish
    what developers generally expect out of reloading, the entire Python object
    graph would need to be visited. Consider a module ``foo`` which was to be
    reloaded -- any object in the entire object graph that held a reference to
    any "previous" version of objects from ``foo`` would need to have its
    reference reconciled in some way.

    Don't be misled by the confusing presence of :func:`reload` in the builtin
    namespace! Its usage is `strongly discouraged <discouraged-builtins>`
    because its behavior in the above situation is essentially to do the
    simplistic thing, and not update any references.

... (copy|serialize) arbitrary Python objects?
    Generally speaking, you don't, can't and shouldn't. You need to know what
    kinds of objects you wish to copy or serialize, and to have them know (in
    the OO sense) how they need to be copied.

    Serializing or copying arbitrary Python objects is not a generally solvable
    problem. Consider, for the most trivial example, an object with an
    attribute ``foo`` that points at an open :func:`file` object.

    This attribute (and therefore this object) is not in any reasonable sense
    serializable or (deep-)copyable.

    Don't be misled by the confusing presence of :mod:`pickle`, :mod:`shelve`
    and :mod:`copy` in the standard library! Their usage is `strongly
    discouraged <discouraged-stdlib>` for the reasons mentioned above.

    Developers are *very strongly discouraged* from serializing arbitrary
    objects, in favor of serializing *data* that can be used to construct the
    objects that are needed.
