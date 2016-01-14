===========
Code Review
===========


.. _cr-goals:

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

Without the pretentiousness of the last paragraph: we want to make sure
our code is well written and maintainable, but while minimizing the
effort we collectively need to invest.

An often overlooked or underrepresented desire is to have *authors* perceive
that reviews are conducted in a timely manner -- authors should not feel like
they are *hampered* by the review process, or that there are gaps of silence
while waiting for someone to address their proposed changes. Quality is
important, and moving too fast is harmful in its own ways, but we wish to
account for and encourage a process that allows reviews to be timely so that
developers can get quick feedback on their changes.


Time & Sprint Velocity
======================

Developers, within the guild and at other organizations, spend a significant
amount of time per week (or per sprint) doing reviews of other developers'
code.

Guild members should be mindful of this fact -- when estimating their
capacities for sprints, each developer should account for time spent
doing reviews. As a rough estimate, one might assume 5 hours a week are
spent reading code reviews, but guild members are encouraged to *measure
themselves*.

.. seealso::

    [IPCR]_ (hour result also quoted in [CUCR]_)
        which surveyed developers about a number of qualitative factors
        that affect their opinions of other team members when reviewing
        others' code.

        The same survey found that surveyed developers spent 6 hours a
        week, on average, doing code reviews.


*Who* Should Be a Reviewer?
===========================

[CUCR]_ investigated a number of factors that contributed to the usefulness of
comments in their code reviews:

.. epigraph::

    [Developers] who had made prior changes to files in a change under
    review had a higher proportion of useful comments in four out of the
    five projects ..., but we did not see a difference in effectiveness
    based on the number of times that a developer had worked on a file.

    That is, comments from developers who had changed a file ten times
    had the same usefulness density as from developers who had only
    changed a file once.

    -- [CUCR]_ Section VI: A.1

Part of this realization (that developers learn a lot from having
touched a particular source file or area) also aligns strongly with our
own anecdotal experiences.

In line with our goal to maximize the number of defects we find, we
therefore recommend that changesets be reviewed by developers who have
previously *worked* on a particular file.

Interestingly, [CUCR]_ also investigated the usefulness of comments by
developers who had previously *reviewed* the same file, and found that
it had an even more drastic effect on the likelihood that a particular
comment was useful. We encourage repeat reviewers, but [CUCR]_ caveats
this finding by noting that many organizations require a new developer
to first review new code before being granted the right to change it.

*We therefore encourage that reviews be done by developers who have
previously touched the source file.*

There are 2 additional points worth noting. Our conclusion above is in
direct conflict with the secondary goal of code review discussed `above
<cr-goals>`: knowledge dissemination.

If code reviews are always done by previous reviewers and committers, it
limits the exposure that new developers will have to changesets.

The guild currently *accepts* this realization and chooses to counterbalance
the reduction in knowledge distribution by:

* CC'ing new developers so they can still participate, but do not "block" the
  review

* Using functions other than Code Review (such as pair programming) to
  compensate

* Continuing to encourage new developers to still *work* on new code bases
  despite not having reviewed previous code within it.

.. seealso::

    [CUCR]_
        The entirety of this paper, but particularly Section VI have a number
        of interesting nuances and findings which guild members are encouraged
        to read and think about as we attempt to improve our processes.


*How Many* Reviewers Should There Be?
=====================================


*What* Can Be Reviewed?
=======================

Being able to distill code reviews in to small enough chunks is a skill
-- one that does not necessarily come naturally! It is especially
difficult to take an *already completed* changeset and break it up into
separate chunks in a non-trivial, reviewable manner. As a guild, we
acknowledge this fact but are committed to cultivate this skill due to
the benefits it offers.

[CUCR]_ also identifies a correlation between the total number of files
in the changeset and the number of defects uncovered -- more files in
the review has a negative impact on the number of defects uncovered --
but it is unclear whether this correlation was done after first removing
the correlation with line length. See Section VI, Figure 8.

.. seealso::

    [INTF]_
        Particularly Section IV.A, which discusses similar results about
        patch size and its effect on *acceptance* time.

        Section IV.C also notes results about the effect a particular
        *component* has on *response* time -- i.e., some code bases are
        harder to review than others.

        A number of other factors were also found to be statistically
        significant in the dataset collected in the paper.


The Difficulties of Configuration Changes
-----------------------------------------

Configuration changes are examples of particularly "risky" or unique
changesets. A configuration change often is short but impactful.

In these cases we stress our above recommendation to have changes
reviewed by seasoned guild members, and to acknowledge the care needed
to ensure that configuration changes are done properly.

Developers reviewing configuration changesets should look carefully at
the failsafe mechanisms in the surrounding code to ensure that systems
are hardened to at least help identify potential configuration issues if
possible, should a human miss a potential issue.


*When* Should Code Be Reviewed
==============================


*How* Should Reviewers Read Changesets
======================================


Commits vs. Diffs
==================

One of the central ideas of :doc:`version control <version-control>` is
the existence of *commits* in their own right -- as encapsulated units
of work.

A `good commit <good-commit>` should be self-contained and informative. We
aspire to adhere to this ideal -- and, ergo, our commits should convey some
additional context or explanation that is not necessarily self-evident from the
actual textual changes to the source code.

Ideally, our code review tool would, therefore, include the commit information
along with the diff of the changes. For various technical reasons, our current
tool does not, but guild members are encouraged to include links to remote
branches with their changes, so that the reviewer has access to the full
context of the changes.


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


Summary
=======

To summarize our current accepted best practice:

Reviews should be done by one developer, and preferably by one who has
previously edited the files under review. This developer's sign-off
gates the change.

The total changeset size should not exceed 200 lines changed, and
reviewers should spend (up to) a dedicated hour reading them carefully
in context.

Authors are encouraged to pre-review their own changesets and to leave
comments which will help the reviewer through the changes and their
reasoning.

Reviewers are encouraged to read for maintainability and correctness.
Stylistic comments are also welcome and encouraged, but should be
accompanied by changes to an automated style checker.


Further Questions
=================

There are a number of further questions which we propose as worthy of
consideration, without making recommendations about their answers:

* How should pair programming affect the code review process? Is
  software written while pair programming (either informally or in the
  formal XP programming sense) less likely to produce defects that would
  be caught by the code review process?
* Would encouraging a *checklist* to be created for each file or module improve
  the detection of potential issues within it when the file was under
  re-review?
* Would encouraging or enforcing a *workflow* for individual comments be
  beneficial -- e.g., asking authors to transition each comment thread to
  "Addressed", "Won't Fix", "ACKed", etc.?
* How would asking developers to artificially *re-review already-merged*
  changesets affect reliability? Such a practice could be used to familiarize
  new developers with the code review process and its contextual code, but also
  might provide a mechanism for doing retrospective evaluation of a changeset
  after some amount of time has passed, and might also remind developers of
  technical debt that might have been noticed but left asunder.


.. seealso::

    [CUCR]_
        In which the authors created and trained a classifier to rate
        the *usefulness* of comments (post-hoc) and inspected how
        the usefulness of a comment affected its likelihood of being
        addressed.


References
==========

.. [CRCS] `Code Review at Cisco Systems
    <http://support.smartbear.com/support/media/resources/cc/book/code-review-cisco-case-study.pdf>`_
    (2006)

.. [CUCR] `Characteristics of Useful Code Reviews:
    An Empirical Study at Microsoft <http://research.microsoft.com/apps/pubs/default.aspx?id=249224>`_
    (2015)

.. [IPCR] `Impact of Peer Code Review on Peer Impression Formation: A Study
    <http://www.amiangshu.com/papers/Bosu-ESEM-2013.pdf>`_ (2013)

.. [INTF] `The Influence of Non-technical Factors on Code Review
    <http://ieeexplore.ieee.org/xpl/login.jsp?tp=&arnumber=6671287>`_ (2013)
