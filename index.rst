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

   This is very early days for this guide, so please let us know any issues
   you spot or how we can improve it in any way. There's a lot of additional
   information about developing for Xapian in the `HACKING file in xapian-core`_
   which we hope to move here in future, and in the meantime that's a good place to
   look for information on coding style, writing and running tests, and so on.

.. _HACKING file in xapian-core:
   https://github.com/xapian/xapian/blob/master/xapian-core/HACKING

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
   contributing/index
   LICENSE

.. some other pieces from HACKING that could live here

   getting-started/index [ getting source, predeps, Windows/VM &c ]
   build-system/index [ configure options, tips for aiding development ]
   tests/index
   writing-code/index [C++ features, naming conventions, code formatting, API structure, portability]
   releases/index [how to make a release, Debian packages]
   deprecation
