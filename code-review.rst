===========
Code Review
===========


Goals
=====

The code review process, in principle and practice, is designed to identify
*defects*.

A :dfn:`code reviewer` is tasked with uncovering issues that may have
been missed during the initial implementation by the :dfn:`author` of
the changeset.

The perspective offered by a code review offers feedback to the author
for each uncovered issue, where issues may be anything from buggy
implementations, unintentional interactions with existing code, threats
to future maintainability of the codebase, or gaps in error handling,
monitoring and reliability.

Distilled, a code review should focus on ensuring that the proposed new
code is *correct*, *simple*, *maintainable*, *tested* and *working as
intended*.

A *secondary* goal of code review is to disseminate knowledge of changes
and the codebase in general throughout the guild, but when in conflict
with its primary goal, we consider this goal to be... secondary.


Constraints & Desires
=====================

Given our above motivation to uncover defects in code during the review, the
constraints we place on our process should focus on maximizing our return on
investment of reviewer time.


How Many Reviewers?
===================


Who Should Be a Reviewer?
=========================


Commits vs. Diffs
==================


Line Length Limits
==================


Time Goal & Sprint Velocity
===========================


Author Pre-review
=================

An author pre-review is a pre-submission attempt to annotate the
source code performed *by its author*. The author reviews the chunks
or commits that are about to be submitted within the code review tool,
and populates comments whose goals are to guide reviewers through the
changeset and to explain particular changes or choices made.

.. epigraph::

    Clearly author preparation is correlated with low defect densities.
    But there are at least two ways to explain this correlation, each
    leading to opposite conclusions about whether author preparation
    should be mandatory.

    -- [CRCS]_, pp. 81

The sourced article (which members are encouraged to read), proposes that
pre-review either promotes self-consideration by authors, reducing defects, or
numbs reviewers' attention spans, possibly increasing them. The authors (and
the guild) find the former to be more tenable.

The guild therefore *strongly encourages* but does not mandate
pre-review by the author of a code review.


Style & Static Checkers
=======================


Re-Review
=========


Further Questions
=================

There are a number of further questions which we propose as worthy of
consideration, without making recommendations about their answers:

* How should pair programming affect the code review process?
* Would encouraging a *checklist* to be created for each file or module improve
  the detection of potential issues within it when the file was under
  re-review?


References
==========

.. [CRCS] `Code Review at Cisco Systems
        <http://support.smartbear.com/support/media/resources/cc/book/code-review-cisco-case-study.pdf>`_
