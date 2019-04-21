Testing the libraries
=====================

Both ``xapian-core`` and ``xapian-letor`` have automated test suites
covering their APIs and various internals of the API
implementations. They are implemented using the same C++ test harness,
and so have similar features and tests in both should feel familiar
once you get used to one.

There are a few environment variables which the test harness checks for
which you might find useful:

``XAPIAN_TESTSUITE_SIG_DFL``
    By default, the testsuite harness catches signals and handles them
    gracefully - the current test is failed, and the testsuite moves onto the
    next test.  If you want to suppress this (some debugging tools may work
    better if the signal is not caught) set the environment variable
    ``XAPIAN_TESTSUITE_SIG_DFL`` to any value to prevent the testsuite harness
    from installing its own signal handling.

``XAPIAN_TESTSUITE_OUTPUT``
    By default, the testsuite harness uses ANSI escape sequences to give
    colour output if stdout is a tty.  You can disable this feature by setting
    ``XAPIAN_TESTSUITE_OUTPUT=plain``. Alternatively, piping the output, such as
    through ``cat`` or ``more``, will have the same effect.

    Auto-detection can be explicitly specified with
    ``XAPIAN_TESTSUITE_OUTPUT=auto`` (or empty). Any other value
    forces the use of colour.

    Colour output is always disabled on Microsoft Windows, so
    ``XAPIAN_TESTSUITE_OUTPUT`` has no effect there.

``XAPIAN_TESTSUITE_LD_PRELOAD``
    The runtest script will add this to ``LD_PRELOAD`` if it is set, allowing
    you to easily load ``LD_PRELOAD`` libraries when running the testsuite.
    The original intended use was to allow use of `libeatmydata
    <https://www.flamingspork.com/projects/libeatmydata/>`_ which makes
    ``fsync`` and related calls no-ops, but configure now checks for
    the eatmydata wrapper script and this is used automatically.

    However, there may be other ``LD_PRELOAD`` libraries which are useful,
    so we've left the machinery in place.

Running test programs
---------------------

To run all tests, use ``make check``.  You can also run just the subset of
tests which exercise the inmemory, remote progserver, remote TCP,
multi-database, glass or honey backends using ``make check-inmemory``,
``make check-remoteprog``, ``make check-remotetcp``, ``make check-multi``,
``make check-glass`` or ``make check-honey`` respectively.

Also, ``make check-remote`` will run the tests on both variants of the remote
backend, and ``make check-none`` will run those tests which don't use any
backend.  These are handy shortcuts when doing development work on a particular
backend.

Running individual tests
~~~~~~~~~~~~~~~~~~~~~~~~

The runtest script (in the tests subdirectory) takes care of the details of
running the test programs (including setting up the environment so they work
when srcdir != builddir and handling libtool dynamically linked binaries).  To
run a test program by hand (rather than via make) just use:

.. code-block:: bash

   ./runtest ./apitest

You can specify options and arguments.  Individual test programs optionally
take one or more test names as arguments, and you can also pass ``-v`` to get
more verbose output from failing tests, e.g.:

.. code-block:: bash

   ./runtest ./apitest -v deldoc1

If the number of the test is omitted, all tests with that basename are run,
so to run deldoc1, deldoc2, etc:

.. code-block:: bash

   ./runtest ./apitest deldoc

Running under a debugger or other tool
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can also use runtest to run a test program under gdb (or most other tools):

.. code-block:: bash

   ./runtest gdb ./apitest -v deldoc1
   ./runtest valgrind ./apitest -v deldoc1

Some test programs take special arguments - for example, you can restrict
apitest to the glass backend using ``-bglass``.


Speeding up the testsuite with eatmydata
----------------------------------------

The testsuite does a lot of small database operations, and the calls to fsync,
fdatasync, etc which Xapian makes by default can slow down testsuite runs
substantially.  There's a handy ``LD_PRELOAD`` library called `eatmydata
<https://www.flamingspork.com/projects/libeatmydata/>`_, which can help here,
by turning fsync and related calls into no-ops.

You need a version of eatmydata with the eatmydata wrapper script (version 37
or newer), and then configure should auto-detect it and it'll get used when
running the testsuite (via runtest).  If you wish to disable this
auto-detection for some reason, you can run configure with:

.. code-block:: bash

   ./configure EATMYDATA=

Or you can disable use of eatmydata during a particular run of "make check"
like so:

.. code-block:: bash

   make check EATMYDATA=

Or disable it while running a test directly (under sh or bash):

.. code-block:: bash

   EATMYDATA= ./runtest ./apitest
