Xapian Developer Guide |version|
================================

Xapian is an open source search engine library, which allows developers
to add advanced indexing and search facilities to their own applications.
This manual aims to explain how to work on and contribute to Xapian
itself; if you want to use Xapian in your own project, you should look
at our `Xapian user manual`_.

.. _Xapian user manual: https://getting-started-with-xapian.readthedocs.org/

.. _other-info:

.. note::

   This is early days for this guide, so please let us know any
   issues you spot or how we can improve it in any way.

Since Xapian is an open source project, it is entirely dependent on
contributions -- many of them from people like you! These
contributions come in many forms, from spotting when some
documentation isn't clear and maybe improving it, through fixing bugs
up to designing and implementing completely new features. We need all
types of contribution for Xapian to continue to evolve and serve its
users.

This guide is intended to:

* help you understand how to make and contribute changes to Xapian

* lay out some of our "rules of the road", which are there to make it
  easier for everyone to collaborate on Xapian

We recommend you at least skim through all of this guide, so you know
what information is available to you. However, if you absolutely must
jump in head first, you should at least read our :ref:`advice for new
contributors<advice for new contributors>`.

.. todo::

   Talk about how to manage security issues. `This article`_ may be helpful.

   .. _This article: https://thoughtbot.com/blog/handling-security-issues-in-open-source-projects

.. _contact:

Getting in touch
----------------

The Xapian community typically works "in the open", via mailing lists and an
IRC channel:

* Our `mailing lists <https://xapian.org/lists>`_ are open for anyone to
  join, although (because we don't want to relay spam to everyone) if you
  aren't subscribed to the list someone will have to manually approve your
  message. Please be patient and don't resend a message just because it
  doesn't appear right away.

* We're on `#xapian <https://webchat.freenode.net/?channels=%23xapian>`_ on
  ``irc.freenode.net``. IRC is a simple text chat system where many members
  of the community hang out; although because we're distributed around the
  world, you may not get an instant response. That doesn't mean we're
  ignoring you, so either hang on for a reply, or you can use the mailing
  list instead.

.. This would be a good place to talk about a CoC.

Contents
--------

.. toctree::
   :maxdepth: 3

   getting-started/index
   conventions/index
   documentation/index
   tests/index
   contributing/index
   mentors/index
   releases/index
   LICENSE

.. some other pieces from HACKING that could live here

   note that the central section (from 'Building from git' through to
   tips for aiding development) is now in here, although not always
   tidied up or in the best place.

   The section on dependencies is deliberately trimmed to just
   install everything and not explain what's going on. So we
   might want to include the details somewhere.

   There needs to be a section about macOS and bindings, somewhere.

   build-system/index [ configure options ]
   tests/index [ eatmydata, debugging, valgrind, make targets, running individual tests, lldb/gdb, gprof/purify/insure/lcov, data -> db indexing (tests/testdata & code in tests/harness/)), where the dbs live (eg tests/.glass) ]
   releases/index [how to make a release, Debian packages]

.. todo:: There needs to be a section about macOS and bindings, somewhere.
