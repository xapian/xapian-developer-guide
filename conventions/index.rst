.. _conventions:

Coding and other conventions in Xapian
======================================

We aim for Xapian to be:

* *Cross-platform*: this means it will compile and run on a range of
  different platforms. We have three sets of automated build systems
  to help us keep track of this: `the Xapian buildbots`_, our `builds
  on Travis CI`_, and finally `builds on AppVeyor`_. The last two will
  be triggered automatically if you submit changes using :ref:`a pull
  request on github <pull requests>`.

.. _the Xapian buildbots: https://buildbot.xapian.org/

.. _builds on Travis CI: https://travis-ci.org/xapian

.. _builds on AppVeyor: https://ci.appveyor.com/project/ojwb/xapian

* *Able to build "cleanly"*, meaning without warnings, on a range of
  compilers. Note that the two main C++ compilers in use these days
  are from `clang`_ and `GCC`_, and they have slightly different sets
  of warnings and behaviours. Our automated builds run against both
  compilers. When you build Xapian, the compiler configuration used by
  default will highlight warnings and refuse to complete the build if
  it finds any.

 .. _clang: https://clang.llvm.org/

 .. _GCC: https://gcc.gnu.org/

* *Documented* and *tested* throughout. Although not all of it is
  fully-documented or tested as it stands, if we add documentation and
  tests every time we add a feature, fix a bug, or work on existing
  code, then we will keep on improving.

  If you'd like to improve our test code coverage, our `code coverage
  report`_ may be helpful in choosing something to add tests
  for. Remember that code coverage isn't the same as having useful
  tests for the code, so even in parts of the codebase that have good
  coverage there may be new tests worth writing.

 .. _code coverage report: http://lcov.xapian.org/

With that in mind, we have some conventions and standards that we try
to adhere to. It's difficult to get away from big lists of things to
do and not do, but we've tried to explain why as much as possible, and
to group things in a way that makes it easier to find useful
information. It's a good idea to at least skim through this material
so you can go back for a detailed look when you need.

.. toctree::
   :maxdepth: 2

   c++
   portability
   other
   deprecation
   api

