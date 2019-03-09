Marking Features as Deprecated
==============================

In the API headers, a feature (a class, method, function, enum,
typedef, etc) can be marked as deprecated by using the
``XAPIAN_DEPRECATED()`` or ``XAPIAN_DEPRECATED_CLASS`` macros.  Note
that you can't deprecate a preprocessor macro.

For compilers with a suitable mechanism (such as GCC, clang and MSVC)
this causes compile-time warning messages to be emitted for any use of
the deprecated feature.  For compilers without support, the macro just
expands to its argument.

Sometimes a deprecated feature will also be removed from the library
itself (particularly something like a typedef), but if the feature is
still used inside the library (for example, so we can define class
methods), then use ``XAPIAN_DEPRECATED_EX()`` or
``XAPIAN_DEPRECATED_CLASS_EX`` instead, which will only issue a
warning in user code (this relies on user code including xapian.h and
library code including individual headers)

You must add this line to any API header which uses
``XAPIAN_DEPRECATED()`` or ``XAPIAN_DEPRECATED_CLASS``::

    #include <xapian/deprecated.h>

When marking a feature as deprecated, document the deprecation in
``docs/deprecation.rst``.  When actually removing deprecated features,
please tidy up by removing the inclusion of ``<xapian/deprecated.h>``
from any file which no longer marks any features as deprecated.

The ``XAPIAN_DEPRECATED()`` macro should wrap the whole declaration
except for the semicolon and any "definition" part, for example::

    XAPIAN_DEPRECATED(int old_function(double arg));

    class Foo {
      public:
        XAPIAN_DEPRECATED(int old_method());

        XAPIAN_DEPRECATED(int old_const_method() const);

        XAPIAN_DEPRECATED(virtual int old_virt_method()) = 0;

        XAPIAN_DEPRECATED(static int old_static_method());

        XAPIAN_DEPRECATED(static const int OLD_CONSTANT) = 42;
    };

Mark a class as deprecated by inserting ``XAPIAN_DEPRECATED_CLASS``
after the class keyword like so::

    class XAPIAN_DEPRECATED_CLASS Foo {
      public:
        Foo() { }

        // ...
    };

You can simply mark a method defined inline in a class with
``XAPIAN_DEPRECATED()`` like so::

    class Foo {
      public:
        // This failed to compile with GCC 3.3.5.
        XAPIAN_DEPRECATED(int old_inline_method()) { return 42; }
    };
