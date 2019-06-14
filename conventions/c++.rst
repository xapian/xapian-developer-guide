.. _c++ conventions:

C++ conventions
===============

Code layout
~~~~~~~~~~~

Editor configuration
--------------------

vim
^^^

If you use the ``vim`` or ``neovim`` editor, then these settings will
help you lay out code to match Xapian C++ coding conventions::

   set sw=4
   set sts=4
   set noet
   set cinoptions=l1,g0.5s,h0.5s,:0.5s,=0.5s,t0,(0

Indentation
-----------

Indent C++ code by 4 spaces for a new indentation level, and set your
editor to tab-fill indentation (with a tab being 8 spaces wide).

As an exception, "public", "protected" and "private" declarations in
classes and structs should be indented by 2 spaces, and the following
code should be indented by 2 more spaces::

    class Foo {
      public:
        method();
    };

The rationale for this exception is that class definitions in header
files often have fairly long lines, so losing an indent level to the
access specifier tends to make class definitions less readable.

The default access for a class is always "private", so there's no need
to specify that explicitly - in other words, write this::

    class Foo {
        int internal_method();

      public:
        int external_method();
    };

Don't write this::

    class Foo {
      private:
        int internal_method();

      public:
        int external_method();
    };

If a class only contains public methods and data, consider declaring
it as a "struct" (the only difference in C++ is that the default
access for a struct is "public").

Spaces and line breaks
----------------------

Put a space before the ``(`` after control flow constructs like
``for``, ``if``, ``while``, and so on.  So write ``if (strlen(p) >
10)`` not ``if(strlen (p) > 10)``.  Don't put a space before the ``(``
in function calls.

When ``if``, ``else``, ``for``, ``while``, ``do``, ``switch``,
``case``, ``default``, ``try``, or ``catch`` is followed by a block
enclosed in braces, the opening brace should be on the same line, like
so::

    if (x > 12) {
        foo(x);
        x = 12;
    } else {
        bar(x);
    }

The rationale for this is that it conserves vertical space (allowing more
code to fit on screen) without reducing readability.


C++ idioms in Xapian
~~~~~~~~~~~~~~~~~~~~

* If you have an empty loop body, use ``{ }`` rather than ``;`` as the
  former stands out more clearly to the reader (but also consider if
  the code might be clearer written a different way).

* Prefer ``++i;`` to ``i++;``, ``i += 1;``, or ``i = i + 1``.  For
  simple integer variables these should generate equivalent (if not
  identical) code, but if ``i`` is an iterator object then the
  pre-increment form can be more efficient in some cases with some
  compilers.  It's simpler and more consistent to always use the
  pre-increment form (unless you make use of the old value which the
  post-increment form returns).  For the same reasons, prefer ``--i;``
  to ``i--;``, ``i -= 1;``, or ``i = i - 1;``.

* Prefer ``container.empty()`` to ``container.size() == 0`` (and
  ``!container.empty()`` to ``container.size() != 0`` or
  ``container.size() > 0``).

  Some containers (e.g. ``std::forward_list``) support ``empty()`` but not
  ``size()``.  Pre-C++11 finding the size of a container wasn't
  necessarily a constant time operation for some containers
  (e.g. ``std::list`` with GCC) - that's no longer the case for any STL
  containers since C++11, but it could still be true for non-STL
  containers.

  Also the ``empty()`` form makes the intent of the test more explicit.

* Prefer not to use ``else`` when the control flow is diverted
  elsewhere at the end of the ``if`` block (e.g. by ``return``,
  ``continue``, ``break``, ``throw``).  This eliminates a level of
  indentation from the code in the ``else`` block, and typically makes
  the control flow logic clearer.  For example::

    if (x == 0) {
        foo();
        return;
    }

    while (x--) {
        bar();
    }

  rather than::

    if (x == 0) {
        foo();
        return;
    } else {
        while (x--) {
            bar();
        }
    }

* For standard ISO C headers, prefer the C++ form for ISO C headers
  (e.g.  ``#include <cstdlib>`` rather than ``#include <stdlib.h>``)
  unless there's a good reason (e.g. portability) to do otherwise.  Be
  sure to document such exceptions to avoid another developer changing
  them to the standard form.  Global exceptions: <signal.h> (lots of
  POSIX stuff which e.g. Sun's compiler doesn't provide in <csignal>).

* For standard ISO C++ headers, *always* use the ISO C++ form
  ``#include <list>`` (pre-ISO compilers used ``#include <list.h>``, but
  GCC has generated a warning for this form for years, and GCC 4.3
  dropped support entirely).

* Prefer ``new SomeClass`` to ``new SomeClass()``, since the latter
  tends to lead one to write ``SomeClass foo();`` which is a function
  prototype, and not equivalent to the variable definition ``SomeClass
  foo``.  However, note that ``new SomePODType()`` is *not* the same
  as ``new SomePODType`` (if SomePODType is a Plain Old Data type) -
  the former will zero-initialise scalar members of SomePODType.

* RTTI (``dynamic_cast<>``, ``typeid``, ``std::typeinfo``): Needing to
  use RTTI features in the library most likely indicates a design
  flaw, and you should avoid use of these features.  Where necessary,
  you can use a technique similar to
  ``Database::as_networkdatabase()`` to replace ``dynamic_cast<>``.

* ``using namespace std;`` and ``using std::XXX;`` - it's OK to use
  these in applications, library code, and internal library headers.
  But in externally visible headers (such as anything included by
  ``#include <xapian.h>``) you *MUST* use explicit ``std::``
  qualifiers - it's not acceptable to pull anything from namespace std
  into the namespace of an application which uses Xapian.

* Use C++ style casts (``static_cast<>``, ``reinterpret_cast<>``, and
  ``const_cast<>``) or constructor-syntax (e.g. ``double(value)``) in
  preference to C style casts.  The syntax of the C++ casts is ugly,
  but they do make the intent much clearer which is definitely a good
  thing, and they avoid issues such as casting away const when you
  only meant to cast the type of a pointer.

* ``std::pair<>`` with an STL class as one (or both) of the members
  can produce very long symbols (over 4KB!) after name mangling - long
  enough to overflow the size limits of some vendor compilers or
  toolchains (so this can affect GCC if it is using the system ld or
  as).  Even where the compiler works, the symbol bloat in an
  unstripped build is probably best avoided, so it's preferable to use
  a simple two member struct instead.  The code is probably more
  readable anyway, and easier to extend if more members are needed
  later.

* We try to avoid putting the full definition of virtual methods in header
  files.  This is because current compilers can't (as far as we know) inline
  virtual methods, so putting the definition in the header file simply slows
  down compilation (and, because method definitions often require further
  header files to be included, this can result in many more files needing
  recompilation after a change to a header file than is really necessary).
  Just put the declaration in the header file, and put the definition in a .cc
  file with the same basename.


Efficient use of ``std::string``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* When passing an empty string to a method expecting ``const
  std::string &`` prefer ``std::string()`` to ``""`` or
  ``std::string("")`` (it is more efficient with some compilers).

* To make a string object empty, ``s.resize(0)`` (if you want to keep
  the current reserved space) or ``s = string()`` (if you don't) seem
  the best options.

* Use ``std::string::assign()`` rather than building a temporary
  string object and assigning that.  For example, ``foo =
  std::string(ptr, len);`` is better written as ``foo.assign(ptr,
  len);``.

* It's generally better to build up strings using ``+=`` rather than
  combining series of components with ``+``.  So ``foo = a + " and " +
  c`` is better written as ``foo = a; foo += " and "; foo += c;``.
  It's possible for compilers to handle the former without a lot of
  temporary string objects by returning a proxy object to allow the
  concatenation to happen lazily, but not all compilers do this, and
  it's likely to still have some overhead.  Note that GCC 4.1 seems to
  produce larger code in some cases for the latter approach, but it's
  a definite win with GCC 4.4.

* ``std::string(1, '\0')`` seems to be slightly more efficient than
  ``std::string("", 1)`` for constructing a ``std::string`` containing
  a single ASCII nul character.


Use of C++ Features
~~~~~~~~~~~~~~~~~~~

C++11
-----

As of Xapian 1.3.3, a compiler with decent support for C++11 is
required to build Xapian.  We currently aim to allow users to use a
non-C++11 compiler to build code which uses Xapian.

There are now several compilers with good C++11 support, but there are
a few shortfalls in commonly deployed versions of most of them.  Often
we can work around this, and we should do where the effort is low
compared to the gain (so a compiler version which is widely used is
more worth supporting than one which is hardly used by anyone).

However, we shouldn't have to jump through hoops to cater for
compilers where their authors aren't putting in the effort to keep up
with the language standards.

Please avoid the following C++11 features for the time being:

* ``std::to_string()`` - this is completely missing on current
  versions of mingw and cygwin - in the library, you can ``#include
  "str.h"`` and then use the ``str()`` function instead for most
  cases.  This is also usually faster than ``std::to_string()``.


C++ features we assume
----------------------

We assume that all compilers will correctly implement the following,
so it's safe to rely on them when working on Xapian.

* We assume ``<sstream>`` is available.  GCC < 2.95.3 didn't have it
  but GCC 2.95.3 includes a backported version.  We aren't aware of
  any other compilers still in use which lack it.

* Non-".h" versions of standard ISO C++ headers (e.g. ``#include
  <list>`` rather than ``#include <list.h>``).  We aren't aware of any
  compiler still in use which lacks these, and GCC 4.3 no longer has
  the old versions.  If there are any, we could add a directory full
  of forwarding headers to work around this issue.

* Standard header ``<limits>`` (for ``numeric_limits<>``) - for GCC, this was
  added in GCC 3.0.

* Standard header ``<streambuf>`` (GCC < 3.0 only has ``<streambuf.h>``).


Exceptions
~~~~~~~~~~

When catching an exception which is an object, do it by const reference, so
like this::

      try {
	  foo();
      } catch (const ErrorClass& e) {
	  bar(e);
      }

Catching by value is bad because it "slices" the object if an object
of a derived type is thrown.  Even if derived types aren't a worry, it
also causes the copy constructor to be called needlessly. More
information is available in a `Standard C++ FAQ entry`_.

.. _Standard C++ FAQ entry: https://isocpp.org/wiki/faq/exceptions#what-to-catch

A const reference is preferable to a non-const reference as it stops
the object being inadvertently modified.  In the rare cases when you
want to modify the caught object, a non-const reference is OK.

Exceptions should be avoided except for truly exceptional situations,
since throwing and handling them has a significant cost. It also
generally makes the API easier to understand, and client code easier
to read.


Include ordering for source files
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To help us move towards a consistent ordering of ``#include`` lines in
source files, please follow the following policy when ordering them:

* ``#include <config.h>`` should be first, and use <> not "" (as
  recommended by the autoconf manual).  Always include ``config.h``
  from C/C++ source files, but don't include it from header files -
  the autoconf manual recommends that it should be included first, so
  including it from headers is either redundant, or may hide a missing
  config.h include in the source file the header was included from
  (better to get an error in this case).

* The header corresponding to the source file should be next. This means that
  compilation of the library ensures that each header with a corresponding
  source file is "self supporting" (i.e. it implicitly or explicitly includes
  all of the headers it requires).

* External xapian-core headers, alphabetically. When included from
  other external headers, use <> to reduce problems with finding
  headers in the user's source tree by mistake. In sources and
  internal headers, use "" (?) - practically this makes no difference
  as we have ``-I`` for srcdir and builddir, but <> suggests installed
  header files so "" seems more natural).

* Internal headers, alphabetically (using "").

* "Safe" versions of library headers (include these first to avoid issues if
  other library headers include the ones we want to wrap). Use "" and order
  alphabetically.

* Library headers, alphabetically.

* Standard C++ headers, alphabetically. Use the modern (no .h suffix) names.


Branch Prediction Hints
~~~~~~~~~~~~~~~~~~~~~~~

For compilers which support ``__builtin_expect()`` (GCC >= 3.0 and some others)
you can provide manual hints to assist branch prediction.  We've wrapped these
in macros which evaluate to just their argument for compilers which don't
support ``__builtin_expect()__``.

Within the xapian-core library code, you can mark the expressions in ``if`` and
``while`` statements as ``rare`` (if the condition is rarely true) or ``usual``
(if the condition is usually true).

For example:

.. code-block:: c++

   if (rare(something_unusual())) deal_with_it();

   while (usual(!end_condition()) keep_going();

It's easy to make incorrect assumptions about where hotspots are and which
branches are usually taken or not, so except for really obvious cases (such
as ``if (!consistency_check()) throw_exception();``) you should benchmark
that new ``rare`` and ``usual`` hints help rather than hinder before committing
them to the repository.  It's also likely to be a waste of effort to add them
outside of areas of code which are executed very frequently.

Don't expect miracles - the first 15 uses added saved approximately 1%.

If you know how to implement the ``rare`` and ``usual`` macros for other
compilers, please let us know.

Use of Assert
~~~~~~~~~~~~~

Use Assert to perform internal consistency checks, and to check for
invalid arguments to functions and methods (e.g. passing a NULL
pointer when this isn't permitted).  It should *NOT* be used to check
for error conditions such as file read errors, memory allocation
failing, etc (since we want to perform such checks in non-debug builds
too).

File format errors should also not be tested with Assert - we want to catch
a corrupted database or a malformed input file in a non-debug build too.

There are several variants of Assert:

- ``Assert(P)`` -- asserts that expression P is true.

- ``AssertRel(a,rel,b)`` -- asserts that (a rel b) is true - rel can
  be a boolean relational operator, i.e. one of ``==``, ``!=``, ``>``,
  ``>=``, ``<``, ``<=``.  The message given if the assertion fails
  reports the values of a and b, so ``AssertRel(a,<,b);`` is more
  helpful than ``Assert(a < b);``

- ``AssertEq(a,b)`` -- shorthand for ``AssertRel(a,==,b)``.

- ``AssertEqDouble(a,b)`` -- asserts a and b differ by less than
  ``DBL_EPSILON``

- ``AssertParanoid(P)`` -- a particularly expensive assertion.  If you
  want a build with Asserts enabled, but without a great performance
  overhead, then passing --enable-assertions=partial to configure and
  AssertParanoids won't be checked, but Asserts will.  You can also
  use ``AssertRelParanoid`` and ``AssertEqParanoid``.

An earlier assert, ``CompileTimeAssert(P)``, has now been removed,
since we require C++11 support from the compiler, and C++11 added
``static_assert``.
