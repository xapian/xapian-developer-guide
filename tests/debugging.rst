.. _debugging:

Debugging the libraries
=======================

Extra options to give to configure
----------------------------------

.. note::

   Non-developer configure options are described in ``INSTALL`` files
   in the source tree.

You will probably want to use some of these if you're going to be
developing Xapian.

``--enable-assertions``
	This enables compiling of assertion code which will throw
	Xapian::AssertionError if the code detects violating of
	preconditions, postconditions, or fails other consistency checks.

``--enable-assertions=partial``
	This option enables a subset of the assertions enabled by
	"--enable-assertions", but not the most expensive.  The intention is
	that it should be suitable for use in a real-world system for tracking
	down problems without imposing too much of an overhead (but note that
	we haven't yet performed timings to measure the overhead...)

``--enable-log``
	This enables compiling code into the library which generates verbose
	debugging messages.  See :ref:`debugging messages`.

``--enable-log=profile``
	In 1.2.0 and earlier, this used to use the debug logging macros to
	report to stderr how long each method takes to execute.  This feature
	was removed in 1.2.1 - you are likely to get better results using
	dedicated profiling tools - for more information see:
	https://trac.xapian.org/wiki/ProfilingXapian

.. _debugging messages:

Debugging messages
------------------

If you configure with ``--enable-log``, lots of places in the code generate
debugging messages to tell us what they're up to - this information can be
very useful for debugging both the Xapian library and code which uses it.  But
the quantity of information generated is potentially vast so there's a
mechanism to allow you to select where to store the log and which types of
message you're interested by setting environment variables.  You can:

* set ``XAPIAN_DEBUG_LOG`` to be the path to a file that you would
  like debugging output to be appended to, or to the special
  value ``-`` to indicate that you would like debugging output to be
  sent to stderr.  Unless ``XAPIAN_DEBUG_LOG`` is set, no debug logging
  will be performed.  Occurrences of ``%p`` in
  ``XAPIAN_DEBUG_LOG`` will be replaced with the current process-id.

  If you're debugging a crash and want to avoid losing the most recent log
  messages then include ``%!`` in XAPIAN_DEBUG_LOG (which is replaced with
  the empty string).  This will cause the log file to be opened with
  ``O_DSYNC`` or ``O_SYNC`` or similar if running on a platform that supports
  a suitable mechanism.  In 1.4.10 and earlier this was on by default (and
  ``%!`` has no special meaning) but it can incur a significant performance
  overhead and in most cases isn't necessary.

* set ``XAPIAN_DEBUG_FLAGS`` to a string of capital letters indicating the types
  of debugging message you would like to display (the default is to log calls
  to API functions and methods).  These letters are shown in the first column
  of the log output, and are also listed in ``common/debuglog.h``.  If the
  first character is ``-``, then the letters indicate those categories of
  message *not* be shown instead.  As a consequence of this, setting
  ``XAPIAN_DEBUG_FLAGS=-`` will give you all debugging messages.

These environment variables only have any effect if you ran configure with the
``--enable-log`` option.

The format is::

    <message type> <pid> [<this>] <message>

For example::

    A 16747 [0x57ad1e0] void Xapian::Query::Internal::validate_query()

Each nested call adds another space before the ``[`` so you can easily see
which function call and return messages correspond.

Using various debugging, profiling, and leak-finding tools
----------------------------------------------------------

.. note::

   If you have runes for using other tools, please add them to this section,
   or send them to us so we can.


``libstdc++`` debug mode
~~~~~~~~~~~~~~~~~~~~~~~~

GCC's ``libstdc++`` supports a debug mode, which checks for various misuses of
the STL - to enable this, define _GLIBCXX_DEBUG when building Xapian:

.. code-block:: bash

   ./configure CPPFLAGS=-D_GLIBCXX_DEBUG

See `their documentation of this option
<https://gcc.gnu.org/onlinedocs/libstdc++/manual/debug_mode.html>`_.

.. note::

   All C++ code must be compiled with this defined or you'll get problems.
   Xapian's API headers include a check that the same setting is used when
   building code using Xapian as was used to build Xapian.

Using gdb
~~~~~~~~~

To use `gdb <https://www.gnu.org/software/gdb/>`_, no special build options are
required, but make sure you compile with debugging information (on by default
for GCC).  You'll probably find debugging easier if you compile without
optimisation (with optimisation, line numbers in error messages can be
confusing due to code inlining, etc, and the values of some variables can't be
printed because they've been eliminated from the code completely):

.. code-block:: bash

   ./configure CXXFLAGS='-O0 -g'

Using gprof and other profiling tools
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To enable profiling for gprof:

.. code-block:: bash

   ./configure CXXFLAGS=-pg LDFLAGS=-pg

To use Purify (a proprietary tool):

.. code-block:: bash

   ./configure CXXLD='purify c++' --disable-shared

To use Insure (another proprietary tool):

.. code-block:: bash

   ./configure CXX=insure

Using lcov
~~~~~~~~~~

You can use lcov (at least version 1.10) to generate a test coverage report.
You ideally want lcov 1.11 or later, since 1.11 includes patches to reduce
memory usage significantly - lcov 1.10 would run out of memory in a 1GB VM.

See `lcov.xapian.org <http://lcov.xapian.org/>`_ for automatically generated
reports for git master.

If you use ccache, you'll need ccache >= 3.2.2 for coverage builds to actually
be cached.  Since ccache 3.0 (released 2010-06-20) coverage builds are
supported, but initially by disabling caching if the coverage options are used.
See below for a workaround for ccache < 3.0.

There are three make targets (currently supported in the ``xapian-core`` and
``xapian-letor`` directories):

``make coverage-reconfigure``
  This reruns configure in the source tree with options to configure the build
  to generate coverage information.  See ``Makefile.am`` for details of the
  configure options used and why they are needed.

  You can specify additional options via ``COVERAGE_CONFIGURE_ARGS`` on the
  ``make`` command line.  For example:

  * To configure ``xapian-letor`` to use the in-tree ``xapian-core`` use::

      make coverage-reconfigure COVERAGE_CONFIGURE_ARGS=XAPIAN_CONFIG="`pwd`/../xapian-core/xapian-config

  * If you're using ccache < 3.0 this doesn't support coverage builds.  To
    work around this you can disable use of ccache with::

      make coverage-reconfigure COVERAGE_CONFIGURE_ARGS=CCACHE_DISABLE=1

  * On older systems, coverage reports don't seem to work with shared
    libraries.  To work around this disable use of shared libraries with::

      make coverage-reconfigure COVERAGE_CONFIGURE_ARGS=--disable-shared

``make coverage-reconfigure-maintainer-mode``
  This does the same thing, except the tree is configured in "maintainer mode",
  which is what you want if generating coverage reports while working on the
  code.

``make coverage-check``
  This runs ``make check`` and generates an HTML report in a directory called
  ``lcov``.

  You can specify extra arguments to pass to the ``genhtml`` tool using
  ``GENHTML_ARGS``, so for example if you plan to serve the generated HTML
  coverage report from a webserver, you can tell ``genhtml`` to gzip all
  generated HTML files and add a suitable ``.htaccess`` file using::

    make coverage-check GENHTML_ARGS=--html-gzip

Using valgrind
~~~~~~~~~~~~~~

The testsuite can make use of `valgrind <http://www.valgrind.org/>`_ 3.3.0
or newer to check for memory leaks, reads from uninitialised memory,
and some other bugs during tests.

Valgrind doesn't support every platform, but Xapian contains very
little platform specific code (and most of what there is is Microsoft
Windows specific) so even just testing with valgrind on one platform
gives good coverage.

If you have a new enough version of valgrind installed, it's
automatically detected by configure and used when running the
testsuite. No special build options are required, but make sure you
compile with debugging information (on by default for GCC) and the
valgrind documentation recommends disabling optimisation (with
optimisation, line numbers in error messages can be confusing due to
code inlining, etc):

.. code-block:: bash

   ./configure CXXFLAGS='-O0 -g'

The testsuite runs more slowly under valgrind, so if you wish to
disable this auto-detection you can run configure with:

.. code-block:: bash

   ./configure VALGRIND=

Or you can disable use of valgrind during a particular run of "make check"
like so:

.. code-block:: bash

   make check VALGRIND=

Or disable it while running a test directly (under sh or bash):

.. code-block:: bash

   VALGRIND= ./runtest ./apitest

Current versions of valgrind result in false positives on current versions
of macOS, so on this platform configure only enables use of valgrind if
it's specified explicitly, for example if valgrind is on your ``PATH``
you can just use:

.. code-block:: bash

   ./configure VALGRIND=valgrind


