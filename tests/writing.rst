Writing library tests
=====================

.. note:: Generally we don't "unit test" the lower levels of Xapian

   A common convention in a lot of software is to write `unit tests`_,
   testing individual units of source code. In Xapian we generally
   do not test the lower levels separately. Instead, we write tests
   that use Xapian's public API.

   So for instance, we don't write unit tests for our ``::Internal``
   classes. So if you make a change to :class:`Xapian::Weight::Internal`
   you wouldn't write a test for that change directly. However, your
   changes are motivated by a desire to add certain behaviour to the
   library. This generally will be new functionality via the API, which
   you can write a test for. Sometimes it will be what is sometimes
   called "`non-functional behaviour`_", such as speed of operation or
   memory usage. These may be harder to write tests for, and are
   worth :ref:`discussing with other members of the community <contact>`
   to figure out the best way to test them.

   This means that our tests are often testing a more complex bundle of
   functionality than you may be used to from unit tests. You can think
   of most of Xapian's test suite as automated `functional tests`_.

.. _unit tests:
   https://en.wikipedia.org/wiki/Unit_testing
.. _functional tests:
   https://en.wikipedia.org/wiki/Functional_testing
.. _non-functional behaviour:
   https://en.wikipedia.org/wiki/Non-functional_requirement

Test programs live in ``tests/``. They mostly use a standard test
harness, in ``tests/harness/``, which wraps each test, reports results,
and generally packages things up nicely. The test harness counts how
many testcases pass/fail/skip, catches signals and unhandled exceptions,
and so forth. It can also also check for memory leaks and accesses to
uninitialised values by making use of valgrind, for platforms which
valgrind supports (configure automatically enables use of valgrind if a
suitably recent version is detected).

A typical test program has three parts: the tests themselves (at the
top), a table of tests (at the bottom), and a tiny main which sets the
test harness in motion. It uses the table to figure out what the tests
are called, and what function to call to run them.

Incidentally, when fixing bugs, it's often better to write the test
before fixing the bug. Firstly, it's easier to assure yourself that the
bug is (a) genuine, and (b) fixed, because you see the test go from fail
to pass (though sometimes you don't get the testcase quite right, so
this isn't doesn't always work as well as it should). Secondly you're
more likely to write the test carefully, because once you've fixed
something there's often a feeling that you should commit it for the good
of the world, which tends to distract you.

The framework is done for you, so you don't need to worry about that
much. You are responsible for doing two things:

* writing a minimal test or tests for the feature
* adding that test to the list of tests to be run

Adding the test is simple. There's a ``test_desc`` array in each file that
comprises a set of tests (I'll come to that in a minute), and you just
add another entry. The entry is an array consisting of a name for the
test and a pointer to the function that is the test. Easy. The procedure
is even simpler for apitest tests - there you just use ``DEFINE_TESTCASE``
to define your new testcase, and a script picks it up and makes sure it
is run.

Look at the bottom of ``tests/stemtest.cc`` for the ``test_desc`` array.
Now look up about 20 lines to where the test functions are defined. You
need to write a function like these.  There are a bunch of macros to help you
perform standards testing tasks, such as ``TEST_EQUAL``, which are all in
``tests/harness/testsuite.h``. They're pretty simple to use.

API tests
---------

The most important test system for most people will be ``apitest``. This
also uses the test harness, but has several tables of tests to be run
depending what facilities each backend supports. A lot of the work is
done by macros and helper functions, which may make it hard to work out
quite what is going on, but make life easier once you've grasped what's
going on. The ``main()`` function and other bits are in ``apitest.cc``,
and tests themselves are in various other C++ files starting ``api_``. Each
one of these has its own tables for various different groups of tests
(eg: ``api_db.cc``, which performs tests on the API that require a
database backend, has basic tests, a few specialised groups that only
contain one or two tests, tests that require a writable database, tests
that require a local database, and finally tests that require a remote
database).

To add a new api test, figure out what the test will be dependent on and
put it in the appropriate place (eg: if adding a test for a bug that
occurs while writing to a database, you want a writable database, so you
add a test to ``api_db.cc`` and reference it in the ``writabledb_tests``
table).

Currently, there's ``api_nodb.cc`` (no db required, largely testing
query construction and boundary conditions), ``api_posdb.cc`` (db with
positional information required) and ``api_db.cc`` (everything else,
with lots of subgroups of tests). It's easiest to base a test on an
existing one.

You'll notice in ``apitest.cc`` that it runs all appropriate test groups
against each backend that is being built. The backends are inmemory,
multi, glass, honey, remoteprog and remotetcp. If you need to
create a new test group with different requirements to any current ones,
put it in the appropriate ``api_`` file (or create a new one, and add it
into Makefile.am) and remember to add the group to all pertinent
backends in ``apitest.cc``.

Test databases
--------------

Many of the automated tests work by building a small test database and
then testing a particular library feature against it. To make things
easier, the test harness provides a way of doing this without having
to write indexing code for every test.

Typically you use this by starting your test case with:

.. code-block:: c++

   Xapian::Database db(get_database("dataset"));

which would then index the data in ``tests/testdata/dataset.txt`` and
return an open database object.

You can also get an empty writable database, giving it a name:

.. code-block:: c++

   Xapian::WritableDatabase db(get_named_writable_database("testdbname"));

The actual database files are generally put into ``tests/.<dbtype>``
using either the name (for ``get_named_writable_database()`` or the
dataset(s) used. So you can end up with database paths such as::

  tests/.glass/db__apitest_allterms
  tests/.honey/db__apitest_allterms

The various functions that support this are declared in the header
``tests/apitest.h``, and ``tests/harness/backendmanager.h`` contains doc
comments that will help. (The functions pass through to the particular
backend manager being used.)
