===========
Code Review
===========


.. _cr-goals:

Goals
=====

The code review process, in principle and practice, is designed
to identify :dfn:`defects` -- gaps in functionality, testability,
maintainability, readability, resilience, coherence or consistency.

A :dfn:`code reviewer` is tasked with uncovering any such issue
which may have been missed during the initial implementation by the
:dfn:`author` of the changeset.

The perspective offered by a code review offers feedback to the author
for each uncovered defect -- identifying a buggy implementation,
unintentional interactions with existing code, threats to future
maintainability of the codebase, or gaps in error handling, monitoring
and reliability.

In short:

.. rst-class:: grace-note

    A second pair of eyes introduces the opportunity to ensure code is
    correct, simple, maintainable, tested & working as intended.

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

    [IPCR]_
        which surveyed developers about a number of qualitative factors
        that affect their opinions of other team members when reviewing
        others' code.

        The same survey found that surveyed developers spent 6 hours a
        week, on average, doing code reviews.

Reviews often come in asynchronously. A developer -- potentially in another
squad -- may finish a reviewable chunk at any point during a sprint, and at
that point might ask for a review. While it isn't always feasible to drop
everything to do the review, it is in both parties' interests to find ways to
address reviews quickly after they're submitted.

.. sidebar:: Keep Moving

    Unless an author is particularly concerned that a review will change the
    fundamentals of a set of changes, it *often* makes complete sense to
    continue working on further changesets while a review is underway or
    submitted.

    Version control, perhaps particularly :ref:`git <git>` can help
    reconcile the (simultaneous) moving parts.


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

The current recommendation is to limit the official "merge-gating"
reviewer to *one* person and no more. [BKCR]_ found evidence that having
multiple reviewers did not significantly or effectively increase the
number of defects found during reviews.

Anecdotally we note that often in reviews conducted within the guild
with multiple reviewers, long-running discussions often involve only one
of the reviewers. It is our hope that *increasing* the responsibility on
the one reviewer will lead to more careful reviews.

Also from anecdotal evidence, we account for the strong presence of the
:wiki:`Bystander effect <Bystander_Effect>` by encouraging the single
reviewer to be clearly identified, rather than allowing for "any one
person"-styled reviews.


*What* Can Be Reviewed?
=======================

Being able to distill code reviews in to small enough chunks is a skill
-- one that does not necessarily come naturally! It is especially
difficult to take an *already completed* changeset and break it up into
separate chunks in a non-trivial, reviewable manner. As a guild, we
acknowledge this fact but are committed to cultivate this skill due to
the benefits it offers.

.. sidebar:: Ask!

    Can't figure out how to split up a set of changes, either because of
    coherence or understandable lack of :ref:`git <git>`-fu? Ask someone
    to help!

[CUCR]_ also identifies a correlation between the total number of files
in the changeset and the number of defects uncovered -- more files in
the review has a negative impact on the number of defects uncovered --
but it is unclear whether this correlation was done after first removing
the correlation with line length. See Section VI, Figure 8.

The exact number of lines or files beyond which the number of defects
found deteriorates varies within small margins within the cited
articles, but our current recommendation requires reviews be *shorter
than 200 lines*. Developers who complete changesets longer than this
number *must* determine a way to split their changes into multiple
reviews.

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


.. _ready-for-review:

*When* Should Code Be Reviewed
==============================

A set of changes is "ready" for review when:

* It satisfies the conditions mentioned in this document (see the
  :ref:`cr-summary`)

* The repository's :doc:`test suite <ci>` has passed, or
  any failures are explained within the review

The author should ensure that these conditions have been met before
submitting the review.

.. seealso::

    `pre-review`


Style & Static Checkers
-----------------------

We briefly comment on the "importance" of style -- the effect of a style
guide, or of stylistic unison in general on maintainability of a code
base is deserving of closer inspection which we defer longer comments on.

Under the assumption though that this assertion is well-founded,
we further maintain that style guides should be accompanied by
*machine-runnable checkers* whenever possible.

Having a style guide be checkable programmatically is often a challenge
because of the prevalence of noisy style checkers, or ones that do not
directly correspond to the particular style settled upon within the
guild, but we consider a reviewer's time better spent on defects that
*cannot* be programmatically detected, and strongly encourage that guild
members collaborate to build or assemble automated tools to reduce the
need to manually read for style.


*How* Should Reviewers Read Changesets
======================================

Being able to read code critically for review is an important skill.

Reviewers should read incoming reviews with an eye towards uncovering
the types of defects mentioned in the introduction.

Read slowly and carefully, until you have a solid understanding
of the changes in front of you.

.. epigraph::

    The more time spent in review, the more defects are detected. This
    might sound obvious; what’s not obvious is that this is by far the
    dominant factor in the number of defects detected.

    -- [BKCR]_, 61

Results fairly consistently ([BKCR_], 60) indicate that reviews
which take longer than approximately an hour sharply drop off in
effectiveness, so limit yourself to no longer than that to prevent
exhaustion, but make the time spent count.

Ask questions in places that need clarification, familiarize yourself
with this document and with any relevant language or style guides, and
provide feedback based on your understanding of the changes and of what
makes quality software.

Review comments are not "commands" -- they are potential openings
for discussions, but ultimately *both* the author and the reviewer's
opinions matter, so in cases where opinions differ, come to an
understanding!

.. seealso::

    [AIPE]_
        An interesting study in which code reviewers' eye movements were
        tracked to attempt to answer whether particular methodologies of
        reviewing code lead to better detection of defects. The results
        are partially reproduced in [ETRA]_. Both papers deserve some
        further inspection.


Commits vs. Diffs
==================

One of the central ideas of :doc:`version control <version-control>` is
the existence of *commits* in their own right -- as encapsulated units
of work.

A `good commit <good-commit>` should be self-contained and informative. We
aspire to adhere to this ideal -- and, ergo, our commits should convey some
additional context or explanation that is not necessarily self-evident from the
actual textual changes to the source code.

Besides providing this context as *help* for the reviewer, a commit message is
*entirely reviewable* and deserves attention -- the presence (or absence) of
good commits, regardless of the overall changeset, should be reviewed to help
authors make better commits.

Ideally, our code review tool would, therefore, include the commit information
along with the diff of the changes. For various technical reasons, our current
tool does not, but guild members are encouraged to include links to remote
branches with their changes, so that the reviewer has access to the full
context of the changes.


.. _pre-review:

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

.. note::

    This incomplete mandate is for leaving *comments* with explanations. We
    recognize an even more basic notion of pre-review -- a simple reading of a
    proposed diff before submission by the author -- as being *self-evidently
    mandatory*.

    Authors should use e.g. ``git diff`` to do so and are encouraged to read
    through their own changes carefully and slowly to ensure they are correct,
    complete, free from unrelated changes and ultimately `ready for review
    <ready-for-review>`.


.. _cr-summary:

Summary
=======

To summarize our current accepted best practice:

Reviews should be done by one developer, and preferably by one who has
previously edited the files under review. This developer's sign-off
gates the change.

The total changeset size should not exceed 200 lines changed, and the
designated reviewer should spend (up to) a dedicated hour reading the
changes carefully in context.

Authors are encouraged to pre-review their own changesets and to leave
comments which will potentially guide the reviewer through the changes,
and highlight any areas where particular choices were made.

Reviewers are encouraged to read for maintainability and correctness.
Stylistic comments are also welcome and encouraged, but should be
accompanied by changes to an automated style checker.

.. rst-class:: grace-note

     We do not consider any of the above to be completely ironclad.

Our hope is to continue to evolve our process as we learn more about
what works for us, and what works for others.


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

.. [BKCR] `Best Kept Secrets of Peer Code Review
    <https://smartbear.com/lp/ebook/collaborator/secrets-of-peer-code-review/>`_
    (2006)

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

.. [AIPE] `Analyzing Individual Performance of Source Code Review Using Reviewers' Eye Movement
    <http://dl.acm.org/citation.cfm?id=1117357>`_ (2006)

.. [ETRA] `An Eye-tracking Study on the Role of Scan Time in Finding Source Code Defects
    <http://www.cs.kent.edu/~jmaletic/papers/ETRA12.pdf>`_ (2012)
