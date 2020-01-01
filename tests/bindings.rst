.. _testing-bindings:

Testing the language bindings
=============================

We generally don't test the entire API with bindings; we rely on the existing
tests for ``xapian-core`` to make sure that the library is working. So
bindings tests should concentrate on three things:

1. A basic "smoketest" that all is well. This checks that the bindings can
   actually be loaded, and that a basic index and search is possible.

2. Anything specific to these bindings. For instance, when writing bindings using
   SWIG, you usually write a number of "typemaps" that convert between the
   bindings language's types and the C++ types used by the Xapian API. Testing
   the use of each typemap at least once is important, and more valuable than
   trying to test the entire API.

   Note that it's helpful to test use of SWIG's built-in typemaps for the
   binding language, so that any SWIG changes that cause problems cause test
   failures and can be identified and addressed.

3. Regression tests for bugs in the bindings.

It's usually practical to do this all in one test file, although for languages
that have more sophisticated test frameworks built in it may be easier to split
across multiple files.
