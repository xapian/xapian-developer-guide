Testing Xapian
==============

Xapian contains three types of code, which require different kinds of tests.
The C++ libraries (``xapian-core`` and ``xapian-letor``) have a common
test harness, and have extensive test suites written in C++.  Omega,
our pre-packaged web search app, has its own tests.  Finally, the
language bindings have "smoke tests" to check some basic
functionality, and may have further tests for language-specific code,
such as iterators (which generally work differently in language
bindings than they do in C++, to make them more familiar to existing
users of that programming language).

Generally, tests for any part of Xapian can be run using ``make check``.

.. toctree::
   :maxdepth: 2

   libraries
   writing
   debugging
   omega
