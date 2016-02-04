==================
Python for the Web
==================

.. _wsgi:

WSGI
====

:ref:`WSGI <python:wsgi>` is a specification that defines an *interface*
between web servers and applications.

Succinctly, the WSGI specification defines two components: :dfn:`WSGI
container`\ s and :dfn:`WSGI application`\ s. Containers are web servers. They
accept HTTP(S) connections by accepting socket connections, parsing the
incoming requests, and assembling a collection of Python objects with the
resulting information. There are a variety of WSGI containers in existence, and
a Python developer will typically choose one to use.

WSGI applications are the entry points for functionality implemented by a
developer. A WSGI application is a *single Python object with a defined
interface*: it is a `callable` taking exactly 2 arguments, and which must
return an `iterable`:


.. code-block:: python

    def application(environ, start_response):
        start_response("200 OK", [])
        return ["Hello WSGI!"]


Specifically, its interface is:


.. function:: application(environ, start_response)

    :argument `collections.Mapping` environ: the :dfn:`WSGI environment`, a
        mapping containing all of the parsed information from the HTTP request,
        along with arbitrary additional objects populated by the WSGI
        container or by any `WSGI middleware`.

    :argument `callable` start_response: A callable (a callback) which the
        application can call, taking 2 arguments, a response status and an
        iterable of 2-`tuple`\s representing the response headers. When called,
        this argument causes the WSGI container to record these details for use
        in assembling the HTTP response. (The callable may be called multiple
        times, and the last call "wins").

    :returns: an `iterable`, the response body, which will be sent back to the
        client.



The name of the callable is unimportant, as are the names used for the two
parameters (they're passed positionally into the callable by the WSGI
container as discussed below) though the ones presented above are typical.

.. seealso::

    :wsgi:`What is WSGI? <what>`
        Further detailed documentation on the WSGI specification and its
        ecosystem.
    `wsgi-frameworks`
        Higher level frameworks which encapsulate a WSGI application, providing
        tools like routing and request/response objects, amongst other
        potential features.


.. _wsgi-frameworks:

WSGI-Based Frameworks
---------------------


.. seealso::

    :python:`howto/webservers`
        An outdated-but-interesting document from the standard library
        documentation, with more contextual and background information about
        Python on the web.


.. glossary::

    WSGI middleware
        Abstractly, any callable which takes a WSGI application as an argument
        and returns another WSGI application.

        Middleware is a mechanism for composing features onto WSGI
        applications.
