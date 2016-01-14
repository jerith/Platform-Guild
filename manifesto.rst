=============
Our Manifesto
=============


Everything That Can Be Real-Time, Should Be Real-Time
=====================================================

Moving computation into real-time best models the way the data actually
comes in. It also maximizes the compute resources available, as we
amortize our processing costs over time, rather than having workflows
with big peaks and valleys of resource usage. Additionally, real-time
streams are trivially convertible to batch, while the reverse is
non-trivial.


Separate Computation From Persistence
=====================================

Applications should be able to tolerate being brought up and down on
the order of minutes without needing to do lengthy setup, teardown,
or coordination. Clear boundaries between logic and persistence gives
applications the ability to cope with these conditions, as well as
allows for independent scaling of compute resources.


Enrich, Then Expose
===================

Many computations performed in the platform are :dfn:`enrichment`\ s we
calculate about an incoming datum. We have raw data, and we augment this
raw data with information available elsewhere in the platform.

Design these enrichments to be generic, independent and exposed whenever
they might be useful to others, so that these other consumers in the
platform (both other developers and other services) can make use of the
same computation without needing to reimplement it.

Prefer service layers for this exposure rather than sharing code as a
library whenever the specific enrichment is non-pure (or in particular
time-dependent) so that downstream consumers all operate on the same
results.


Schematize Data at the High Level
=================================

The right levels of abstraction and the right names reduces cognitive
burden on anyone who needs to debug, inspect, or use a given schema or
API.

Data points and APIs should be aim to be self descriptive and clear,
even in isolation.


.. rst-class:: grace-note

     Break the rules, but have solid justification when doing so.
