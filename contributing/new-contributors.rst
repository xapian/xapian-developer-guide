.. _advice for new contributors:

Advice for new contributors
===========================

New to Xapian (or even open source)? Don't worry! Here we try to guide
you through your first contribution, but if anything is unclear or you
want to ask a question, please :ref:`get in touch <contact>`.  This
may look a bit daunting the first time, but we're here to help, and a
lot of the details will become natural over time.

.. todo:: Should be easier to find this section.

.. todo:: Point to some git & github advice.

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
   https://xapian.org/docs/apidoc/html/annotated.html

Get familiar with the code
--------------------------

If you're going to be writing code, it's a good idea to read some of
Xapian's existing sources, particularly in the main library
(``xapian-core``). When you come to write your own, you'll want to
follow the style of how the current code works, both in terms of
layout (where spaces go and so on) and how we use various C++ language
features.

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

   If documentation is your thing, then you might like to take a look
   at our list of missing documentation_.
 
   On the features side, our `bite-sized projects`_ are intended to
   be suitable for someone new to Xapian to pick up. We've tried to
   have a range of things to work on across different parts of Xapian.

.. note::

   No change is too small to consider! Some people's first
   contributions to an open source project are fixing a single stray
   letter in documentation, or adding a line break where one is
   needed.

.. _documentation: https://trac.xapian.org/wiki/MissingDocumentation
.. _bite-sized projects: https://trac.xapian.org/wiki/ProjectIdeas#BiteSize
 
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

Add or correct some documentation, fix a bug or implement a new
feature! You'll probably find our :ref:`suggested workflow <A helpful
workflow>` helpful. We also have :ref:`detailed information
<Contributing changes>` on contributing your changes back to Xapian
for inclusion in future releases.
