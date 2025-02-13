.. _maintainer-workflow:

************************
Workflow for Maintainers
************************

This page is for maintainers who can merge our own or other peoples' changes into the upstream repository.

Seeing as how you're a maintainer, you should be completely on top of the basic git workflow in :ref:`newcomers` and Astropy's `git workflow`_.

.. _git workflow: https://docs.astropy.org/en/stable/development/workflow/development_workflow.html#development-workflow

Integrating changes via the web interface (recommended)
=======================================================

Whenever possible, merge pull requests automatically via the pull request manager on GitHub.
Merging should only be done manually if there is a really good reason to do this!

Make sure that pull requests do not contain a messy history with merges, etc.
If this is the case, then follow the manual instructions, and make sure the fork is rebased to tidy the history before committing.

To check out a particular pull request to test out locally::

    $ git checkout pr/999
    Branch pr/999 set up to track remote branch pr/999 from upstream.
    Switched to a new branch 'pr/999'

When to remove or combine/squash commits
----------------------------------------

In all cases, be mindful of maintaining a welcoming environment and be helpful with advice, especially for new contributors.
It is expected that a maintainer would offer to help a contributor who is a novice git user do any squashing that that maintainer asks for, or do the squash themselves by directly pushing to the PR branch.

Pull requests **must** be rebased and at least partially squashed (but not necessarily squashed to a single commit) if large (approximately >10KB) non-source code files (e.g. images, data files, etc.) are added and then removed or modified in the PR commit history (The squashing should remove all but the last addition of the file to not use extra space in the repository).

Combining/squashing commits is **encouraged** when the number of commits is excessive for the changes made.
The definition of "excessive" is subjective, but in general one should attempt to have individual commits be units of change, and not include reversions.
As a concrete example, for a change affecting < 50 lines of source code and including a changelog entry, more than a two commits would be excessive.
For a larger pull request adding significant functionality, however, more commits may well be appropriate.

As another guideline, squashing should remove extraneous information but should not be used to remove useful information for how a PR was developed.
For example, 4 commits that are testing changes and have a commit message of just "debug" should be squashed.
But a series of commit messages that are "Implemented feature X", "added test for feature X", "fixed bugs revealed by tests for feature X" are useful information and should not be squashed away without reason.

When squashing, extra care should be taken to keep authorship credit to all individuals who provided substantial contribution to the given PR, e.g. only squash commits made by the same author.

When to rebase
--------------

Pull requests **must** be rebased (but not necessarily squashed to a single commit) if:

* There are commit messages include offensive language or violate the code of conduct (in this case the rebase must also edit the commit messages)

Pull requests **may** be rebased (either manually or with the rebase and merge button) if:

* There are conflicts with main
* There are merge commits from upstream/main in the PR commit history (merge commits from PRs to the user's fork are fine)

Asking contributors who are new to the project or inexperienced with using git is **discouraged**, as is maintainers rebasing these PRs before merge time, as this requires resetting of local git checkouts.


A few commits
-------------

If there are only a few commits, consider rebasing to upstream::

    # Fetch upstream changes
    $ git fetch upstream-rw

    # Rebase
    $ git rebase upstream-rw/main

A long series of commits
------------------------

If there are a longer series of related commits, consider a merge instead::

    $ git fetch upstream-rw
    $ git merge --no-ff upstream-rw/main

Note the ``--no-ff`` above.
This forces git to make a merge commit, rather than doing a fast-forward, so that these set of commits branch off trunk then rejoin the main history with a merge, rather than appearing to have been made directly on top of trunk.

Check the history
-----------------

Now, in either case, you should check that the history is sensible and you have the right commits::

    $ git log --oneline --graph
    $ git log -p upstream-rw/main..

The first line above just shows the history in a compact way, with a text representation of the history graph.
The second line shows the log of commits excluding those that can be reached from trunk (``upstream-rw/main``), and including those that can be reached from current HEAD (implied with the ``..`` at the end).
So, it shows the commits unique to this branch compared to trunk.
The ``-p`` option shows the diff for these commits in patch form.

Push to open pull request
-------------------------

Now you need to push the changes you have made to the code to the open pull request::

    $ git push git@github.com:<username>/sammi.git HEAD:<name of branch>

You might have to add ``--force`` if you rebased instead of adding new commits.

Issue Milestones and Labels
============================

Current milestone guidelines:

* Only confirmed issues or pull requests that are release critical or for some other reason should be addressed before a release, should have a milestone.
  When in doubt about which milestone to use for an issue, do not use a milestone and ask other the maintainers.

Current labelling guidelines:

* Issues that require fixing in main, but that also are confirmed to apply to supported stable version lines should be marked with a "Affects Release" label.
* All open issues should have a "Priority <level>", "Effort <level>" and "Package <level>", if you are unsure at what level, pick higher ones just to be safe.
  If an issue is more of a question or discussion, you can omit these labels.
* If an issue looks to be straightforward, you should add the "Good first issue" and "Hacktoberfest" label.
* For other labels, you should add them if they fit, like if an issue affects the net submodule, add the "net" label or if it is a feature request etc.

Updating and Maintaining the Changelog
======================================

The changelog will be read by users, so this description should be aimed at SWxSOC users instead of describing internal changes which are only relevant to the developers.

The current changelog is kept in the file "CHANGELOG.rst" at the root of the repository.

Releases
========

We have a `step by step checklist`_ on the Wiki on how to make a release.

.. _step by step checklist: https://github.com/HERMES-SOC/hermes_core/wiki/Release-Process