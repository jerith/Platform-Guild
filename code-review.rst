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


Knowledge Dissemination
=======================


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


Does Pair Programming Substitute?
=================================


References
==========

.. [CRCS] `Code Review at Cisco Systems
        <http://support.smartbear.com/support/media/resources/cc/book/code-review-cisco-case-study.pdf>`_
