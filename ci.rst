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
  any other software, which promotes transparency and reproducibility.

Software Correctness and Testing Builds
=======================================

Notifications
-------------

Builds that run in our CI infrastructure are encouraged to notify
potentially "interested" parties, with the goal of promoting awareness
of work on a particular component or project. While there is a competing goal
of avoiding general noisiness in developer notifications, we attempt to strike
a balance between concerns.

Our current recommendation, which attempts to at least notify developers during
periods of potential risky change is to notify:

* all *chapter members* for **status changes** of a particular job --
  i.e., only when a build first fails and subsequently first succeeds, but
  not for repeated successes or repeated failures, which often generate
  "inbox noise" while a developer works to restore a working build
* all *commiting developers* for **every failure**, since these developers
  are most likely to be the ones responsible for the failure, and will be
  needed to address it

This notification scheme can be implemented on a build job by configuring the
*Editable Email Notification* plugin on a job to have the following
triggers:

.. figure:: /static/img/ci-notifications.png
    :alt: Email Notification Settings

    Notification settings can be found by expanding the *Advanced
    Settings* panel for the Editable Email Notification plugin.

    Each job should add this plugin to their post-build steps, found on
    the configuration page for the job.
