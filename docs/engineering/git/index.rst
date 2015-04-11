.. Simple Energy Onboarding documentation master file, created by
   sphinx-quickstart on Sat Apr 11 11:15:55 2015.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Git
===


git-config
----------

A git config is a file, typically located in your home directory, that applies
global settings to all of your projects.  We recommend using some format of the
following ``git-config``

http://grimoire.ca/git/config

.. code-block:: ini
    
    [push]
        # Fewer potentially dangerous surprises when pushing to remotes with
        # respect to what branch you are pushing to.
        default = simple
    [merge]
        # Makes `git merge` and `git rebase` behave consistently when you do
        # not specify what you are merging/rebasing into/onto.
        defaultToUpstream = true
    [branch]
        # Makes your branches track the correct **thing** when you make them.
        # Please note that the value `true` and `always` mean different things
        # for `autosetupmerge`.
        autosetupmerge = always
    [rerere]
        # Prevents you from having to re-resolve merge conflicts (most of the
        # time).
        enabled = true


Branching
---------

Anytime you intend make modifications to a codebase, you should do so via a
branch.  The accepted naming convention at Simple Energy has been the format
``<name>/<ticket-number>-<short-description>``.  An example of this would be
``piper/MP-123-implement-some-sweet-new-api``.


Commiting
---------

You should commit early and often.  The work you do will typically be done on a
branch where you are able to rewrite the commit history before pushing the
changes and making them public.

Commit Messages
---------------

The generally accepted standard for git commit messages is present tense
active.  A good guideline is to complete the sentence **If you apply this commit
it will ________**.

* fix the issue with authentication **(good)**
* fixes the issue with authentication **(bad)**
* fixing the issue with authentication **(bad)**

Rebasing
--------

While working on a branch, it is often the case that new code will be added to
the upstream codebase.  Using the ``merge`` functionality to get these changes
into your branch can be problematic.

The established **best practice** is to instead use ``rebase``.  This
functionality replays the changes you have made on top of the upstream
codebase.

Assuming that your local master branch is in sync and up to date with the
upstream version, the following would replay any work already done on the
branch ``piper/cool-new-feature`` on top of the master branch.

.. code-block:: bash

    #piper/cool-new-feature $ git rebase master


Interactive Rebasing
--------------------

Interactive rebasing is a technique for cleaning up your commit history.  This
method is often only recommended for commit that have not been shared with
others.

Consider the following commit history

.. code-block:: bash

    174e970 MP-123 - Add cool new feature
    a0473ff oops, forgot something.
    30cffdf fix unexpected test failures

In an ideal world, we would have taken care of everything in a single commit,
but that clearly is not the case.

.. code-block:: bash

    $ git rebase master -i


This will open a file which allows you to specify how you want to re-play the
commit history.

.. code-block:: bash

    pick 174e970 bumping version
    f a0473ff add mixin classes
    f 30cffdf Add ajax and dom fixture helpers

This will cause the last two commits to be merged with the first commit,
resulting in a single commit that contains all of our changes.
