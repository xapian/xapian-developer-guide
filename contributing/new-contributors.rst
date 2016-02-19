Advice for new contributors
===========================

New to Xapian (or even open source)? Don't worry! Here we try to guide
you through your first contribution, but if anything is unclear or you
want to ask a question, please :ref:`get in touch <contact>`.  This
may look a bit daunting the first time, but we're here to help, and a
lot of the details will become natural over time.

Checking out and building Xapian
--------------------------------
              
A good way to start learning about Xapian is to `check out the code
<https://xapian.org/bleeding>`_. and get it to build. It's better to
use the latest code from the repository rather than a release, as
that's what we want the projects to be based on.

We recommend you use Linux or another UNIX-like system for development
work, as we're better set up for development on such platforms. In
particular we use them ourselves, so can more easily help with any set
up issues you may encounter. If you want to run Linux (perhaps
virtualised) for development and have no existing preference, we
suggest ​Debian or ​Ubuntu as our documentation covers these well.

It can take a while to get the code if your network connection is
slow, and it may take a while to build and run the testsuite if your
computer is slow - while you are waiting, you might want to make a
start on the next section.

Learn about Xapian's API
------------------------

It's a good idea to get familiar with Xapian by going through the `user
guide`_. The online version has examples in Python, but you can also `grab
the source`_ and build for other languages; most example code is also
available in C++ and PHP.

For more details on individual classes, you may want to look at
the `automatically generated API documentation`_. If you're building
from git, this will be built for you in ``xapian-core/docs``; the API
may have some changes between the stable release documented on the
website and the latest version in git.

.. _user guide: https://getting-started-with-xapian.readthedocs.org/
.. _grab the source: https://github.com/xapian/xapian-docsprint
.. _automatically generated API documentation:
   http://xapian.org/docs/apidoc/html/annotated.html

Get familiar with the code
--------------------------

If you're going to be writing code, it's a good idea to read some of Xapian's existing sources, particularly in the main library (``xapian-core``). When
you come to write your own, you'll want to follow the style of how the
current code works, both in terms of layout (where spaces go and so
on) and how we use various C++ language features.

.. note::

   There is :ref:`further information on this <other-info>` that hasn't yet
   been added to this guide.

Picking something to start with
-------------------------------

It can be difficult sometimes to find a place to start, so here are some suggestions:

 * Start small.

   By picking a small contribution first, other people can help you
   with details of how Xapian's documentation, code and so on
   work. It's much easier to get feedback on a small change to start
   off with.

   If documentation is your thing, then you might like to take a look at our list of missing documentation_.
 
   On the features side, our`"bite-sized" projects`_ are intended to
   be suitable for someone new to Xapian to pick up. We've tried to
   have a range of things to work on across different parts of Xapian.

.. _documentation: https://trac.xapian.org/wiki/MissingDocumentation
.. _"bite-sized" projects: https://trac.xapian.org/wiki/ProjectIdeas#BiteSize
 
 * Pick something you care about, or which you already have some
   knowledge of.

   For instance, if you've been using Omega, you might want to pick up
   a small bug or feature for that. Or if you've studied (or are
   studying now!) different Information Retrieval weighting schemes,
   you might want to implement one of the ones we don't currently
   support.

 * Don't be afraid to :ref:`ask for help <contact>`.

   Please pop onto the mailing list or IRC if you need any help in
   getting started or picking something to work on.

Do some work
------------

Add or correct some documentation, fix a bug or implement a new feature!

If you're working on a bug or new feature, you'll follow something like the following steps:

 1. Claim the ticket in `trac <https://trac.xapian.org/report/1>`_.

    Trac is our proposed feature and bug tracker. If there isn't a
    ticket for what you want to work on, you can create it.

    You can either reassign the ticket to yourself and then claim it,
    or you can just add a comment saying you're working on it. If you
    haven't already done so, now's a good time to drop a message
    either in IRC or to the mailing list saying what you'l be working
    on.

 2. Create a branch in your local git repository.

    You want somewhere where you can keep your changes until they're
    ready, and that won't get confused with anyone else's work. In git
    these are called branches_.

    Before you create your branch, you will generally want to make
    sure your local copy of the code is up to date with the central
    repository. Generally you can this as follows::

        $ git checkout master
        $ git pull origin/master

    You can check create your new branch::

        $ git checkout -b feature-x

    The branch name doesn't really matter, but you'll probably find it
    easiest to name it something related to the work that you're
    doing.

.. _branches: https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging

 3. Write a plan.

    Even the smallest contribution is worth thinking about before
    starting to type, and with larger changes it's all but
    essential. If you put your plan in the ticket on trac, it will
    help when someone else comes to review your patch. Also, if you as
    for help, having a plan that someone else can refer to can let
    them see how you're thinking, so they can provide some useful
    advice or recommendations more easily.

    For a larger piece of work, you ideally want to be able to break
    down the work into smaller "sub-projects" which can be completed,
    reviewed and released in turn. (If you're familiar with agile
    development, think of it as a series of development sprints.)
 
    When planning work on a bug or new feature, you should bear in
    mind that there are a number of :ref:`standards we work to
    <patch-guidelines>` when accepting changes into Xapian, and the
    more of them you cover yourself the easier it will be to get your
    changes into a future release. In particular, it's worth thinking
    about documentation and tests in advance.
       
    About the only time you don't need to write a plan is when you're
    making a small change to some documentation, correcting a spelling
    mistake or making something clearer.
    
 4. Make your changes!

    Now you can start making changes. There's a host of :ref:`other
    information that can help you <other-info>` if you're writing code.
    As usual, there are :ref:`other people in the community who can help
    <contact>` if you need.

    If you discover partway through that your plan isn't working, it's
    a good idea to stop and write a new plan. You'll have learned
    something about the problem that you didn't know when you wrote
    the initial plan, so taking the time to think things through from
    the beginning can often unblock you and let you start moving
    again. Of course, if it doesn't clear things up, that's a good
    time to ask the community for help.

 5. Make commits out of your changes

    This is where you probably want to know something about git. A
    very quick introduction is that you first "stage" changes, then
    you "commit" those changes. You don't have to stage all your
    changes at once, which means you can keep small notes or parts of
    future work lying around while you're creating your commits,
    without them creeping into your commits and confusing matters.

    To stage changes for your next commit::

       $ git add -p

    The ``-p`` tells git that you want it to find all the changes,
    then one by one ask you if you want each staged. Just type ``y``
    to stage a change (it calls them "hunks"), or``n`` to skip it this time
    round. If the file is completely new, you can run ``git add <path>``
    to stage the whole file. (There are lots of other options available
    in ``git add -p``; if you type ``?`` then it will explain what they
    all do.)

    Then to make a commit::

       $ git commit -v

    git will open your editor for you to write a commit message. The
    ``-v`` means that your changes will be shown at the bottom of the
    editor (although they won't be included in the commit message),
    which helps you do a final check that you're committing only what
    you want, and everything that is needed. Writing a great commit
    message is important both for people reviewing your code now to
    help get it ready for a future Xapian release, and for when
    someone needs to understand how and why a particular change was
    made, months or years in the future -- when that someone might be
    you! There are a few articles around on writing good commit
    messages; Thoughtbot's `"5 Useful Tips For A Better Commit Message"`_
    has some good advice.

    .. warning::

       Lots of online git tutorials will tell you to write commit
       messages on the command line, using ``git commit -m <message>``.
       If you do that, you'll never write really good commit messages.

    For more details on using git, there are free books and resources
    online, such as `Pro Git`_.

.. A paid book that we can recommend is
   `Goal-Oriented Git`_ by George Brocklehurst.

.. _"5 Useful Tips For A Better Commit Message":
   https://robots.thoughtbot.com/5-useful-tips-for-a-better-commit-message
.. _Goal-Oriented Git: https://gumroad.com/l/gWds
.. _Pro Git: https://git-scm.com/book/en/v2

 6. Contribute your changes

    We have :ref:`detailed information <Contributing changes>` to help you
    here.
