======================
Continuous Integration
======================

Fundamentals
============

As overarching guidelines for CI builds, we recommend that:

* All builds should be runnable locally for developers *identically* as
  they run on the build servers. Builds that require investigation on a
  build server or job machine indicate missing isolation or understanding
  of the environment in which the software is being tested, which damages
  the confidence of the build.
* Builds and their steps should be version controlled in the same way as
  any other software, which promotes transparency and reproduceability.

Software Correctness and Testing Builds
=======================================
