.. _A helpful workflow:

A helpful workflow
==================

If you're working on a bug or new feature, you'll follow something
like the following steps. If you haven't worked on Xapian before, it's
a good idea to adopt this workflow.

Claim the ticket in trac
------------------------

`Trac <https://trac.xapian.org/report/1>`_ is where we keep track of
proposed features and known bugs. You'll have to sign up for an
account, including verifying your email address, in order to make
changes. (This is to prevent unwanted spam on the wiki and in
tickets.)

To 'claim' the ticket so others know you're working on it, you can
either reassign the ticket to yourself and then claim it, or you can
just add a comment saying you're working on it. If you haven't already
done so, now's a good time to drop a message either in the Matrix chatroom
or to the mailing list saying what you'll be working on.

.. note::

   If there isn't a ticket for what you want to work on, you should
   create one. (If you're creating one from something on our `project
   list on the wiki`_ then it's good practice to update the wiki to
   link to the new ticket, so other people can find it easily.)

.. _project list on the wiki: https://trac.xapian.org/wiki/ProjectIdeas

Create a branch in your local git repository
--------------------------------------------

You want somewhere where you can keep your changes until they're
ready, and that won't get confused with anyone else's work. In git
these are called branches_.

Before you create your branch, you will generally want to make sure
your local copy of the code is up to date with the central
repository. Generally you can this as follows:

  .. code-block:: bash

     $ git checkout master
     $ git pull origin/master

You can check create your new branch:

  .. code-block:: bash

     $ git checkout -b feature-x

The branch name doesn't really matter, but you'll probably find it
easiest to name it something related to the work that you're doing.

.. _branches: https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging

.. _Write a plan:

Write a plan
------------

Even the smallest contribution is worth thinking about before starting
to type, and with larger changes it's all but essential. If you put
your plan in the ticket on trac, it will help when someone else comes
to review your patch. Also, if you ask for help, having a plan that
someone else can refer to can let them see how you're thinking, so
they can provide some useful advice or recommendations more easily.

For a larger piece of work, you ideally want to be able to break down
the work into smaller "sub-projects" which can be completed, reviewed
and released in turn. (If you're familiar with agile development,
think of it as a series of development sprints.)
 
When planning work on a bug or new feature, you should bear in mind
that there are a number of :ref:`standards we work to
<patch-guidelines>` when accepting changes into Xapian, and the more
of them you cover yourself the easier it will be to get your changes
into a future release. In particular, it's worth thinking about
documentation and tests in advance.
       
About the only time you don't need to write a plan is when you're
making a small change to some documentation, correcting a spelling
mistake or making something clearer.

Make your changes!
------------------

Now you can start making changes. There's a host of :ref:`other
information that can help you <other-info>` if you're writing code.
As usual, there are :ref:`other people in the community who can help
<contact>` if you need.

If you discover partway through that your plan isn't working, it's a
good idea to stop and write a new plan. You'll have learned something
about the problem that you didn't know when you wrote the initial
plan, so taking the time to think things through from the beginning
can often unblock you and let you start moving again. Of course, if it
doesn't clear things up, that's a good time to ask the community for
help.

Make commits out of your changes
--------------------------------

This is where you probably want to know a little more about git. A
very quick introduction is that you first "stage" changes, then you
"commit" those changes. You don't have to stage all your changes at
once, which means you can keep small notes or parts of future work
lying around while you're creating your commits, without them creeping
into your commits and confusing matters.

To stage changes for your next commit:

  .. code-block:: bash

     $ git add -p

The ``-p`` tells git that you want it to find all the changes, then
one by one ask you if you want each staged. Just type ``y`` to stage a
change (it calls them "hunks"), or ``n`` to skip it this time
round. If the file is completely new, you can run ``git add <path>`` to
stage the whole file. (There are lots of other options available in ``git
add -p``; if you type ``?`` then it will explain what they all do.)

Then to make a commit:

  .. code-block:: bash

     $ git commit -v

git will open your editor for you to write a commit message. The
``-v`` means that your changes will be shown at the bottom of the
editor (although they won't be included in the commit message), which
helps you do a final check that you're committing only what you want,
and everything that is needed.

A good commit in git relies on getting two things right: changes that
do a single thing, and a commit message that describes the thing
clearly. We have some quick tips on each.

Good commits
~~~~~~~~~~~~

Structuring your changes into commits can take a bit of getting used
to, but makes it a lot easier for other people to review, both before
we merge into Xapian and in the future when someone -- which might be
you! -- needs to understand why a change was made in the past, to help
them do whatever work they need to do. There's a
`good article by Anna Shipman <How to raise a good pull request_>`_ that
may help you think about structuring your changes into a set of commits
that are easy for others to read.

* Only make *one change* per commit, and make the *whole change* in
  that commit -- you don't want to end up with essential bits of code
  in a different commit.

  Many people struggle with this at first, and it can be difficult to
  get into the habit of thinking in terms of the distinct changes to
  the system rather than in terms of how you did the work. :ref:`A
  plan <Write a plan>` here can help structure your commits once
  you've finished working.

  One of the reasons we suggest using ``git add -p`` is that it
  enables you to review every single change that goes into a commit,
  which can help you put only the right things into it.

* Avoid committing code that has been commented out. If we need it
  again, it's in the git history.

Why the "size" of commits matters
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We said above that you should only make one change per commit. This is for a few
reasons.

* The most important is that if a change later turns out to be unhelpful for
  whatever reason, git provides a tool (called "`revert <git revert>`_") for
  creating a new commit that "undoes" a previous one. If the original commit did
  more than one thing, then those changes needs to be untangled in order to
  revert. That takes more time, and increases the chance that someone will
  make mistakes when undoing the previous changes.

  .. note:: You can use ``git revert`` with squash and fixup commits

     The trick is to run ``git revert -n``. ``-n`` means that git will revert
     the changes in your working tree and index, but not make a commit out of
     them. You can then use ``git commit --fixup`` or similar as usual.

     You can also revert several different commits as one commit this way.

* Where it makes sense, we like to apply fixes and smaller improvements to the
  current release series of Xapian. Some larger improvements, including
  completely new features, are generally not suitable for this, so if you make a
  commit which say fixes a bug and adds a new method to one of our core classes,
  it makes it harder to "backport" just the bug fix. If you separate that into
  two commits, we can use a git tool called "`cherry picking <git cherry
  pick_>`_" to pull just the bugfix commit in for the next stable release.

  In fact, with separate commits, it's easier to spot candidates for backporting
  in the first place.

* In a similar vein, some changes that aren't pulled into a release series may
  be of interest to someone else who doesn't want to, or cannot, use the
  "bleeding edge" git master. With separate commits, they can use the patches
  for just the functionality they are looking for. This can be particularly
  useful for people packaging Xapian for various operating system distributions.

.. _git cherry pick: https://git-scm.com/docs/git-cherry-pick
.. _git revert: https://git-scm.com/docs/git-revert

Good commit messages
~~~~~~~~~~~~~~~~~~~~

Writing a great commit message is important both for people reviewing
your code now to help get it ready for a future Xapian release, and
for when someone needs to understand how and why a particular change
was made, months or years in the future -- when that someone might be
you!

* Start with a short (50 characters) summary line.

  git (and github) are designed to work better this way. The summary
  should be in the imperative ("Fix bug on macOS" rather than "Fixed
  bug on macOS"). This matches git's automatic messages around
  merges, reverts and so on.

* Follow that with more detail as needed, wrapping long lines at
  72 characters (one exception is that long URLs are best not wrapped).

* Describe the effect, not the code. The important thing is for
  people to be able to read the commit message and understand what
  you were trying to achieve when you made those changes. That way,
  if someone needs to work on that part of the code in future, they
  can understand the purpose of it, and not accidentally remove some
  useful functionality. (Tests help here, but the commit message is
  very important.)

There are a few articles around on writing good commit messages;
Thoughtbot's `"5 Useful Tips For A Better Commit Message"`_ has some
good advice.

.. warning::

   Lots of online git tutorials will tell you to write commit
   messages on the command line, using ``git commit -m <message>``.
   If you do that, you'll never write really good commit messages.

For more details on using git, there are free books and resources
online, such as `Pro Git`_.

.. A paid book that we can recommend is
   `Goal-Oriented Git`_ by George Brocklehurst.

.. _"5 Useful Tips For A Better Commit Message":
   https://thoughtbot.com/blog/5-useful-tips-for-a-better-commit-message
.. _Goal-Oriented Git: https://gumroad.com/l/gWds
.. _Pro Git: https://git-scm.com/book/en/v2
.. _How to raise a good pull request:
   https://www.annashipman.co.uk/jfdi/good-pull-requests.html

Contribute your changes
-----------------------

We have :ref:`detailed information <Contributing changes>` to help you
here.
