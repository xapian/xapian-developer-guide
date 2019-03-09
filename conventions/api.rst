API Structure Notes
===================

We use reference counted pointers for most API classes.  These are
implemented using ``Xapian::Internal::intrusive_ptr``, the
implementation of which is exposed for efficiency, and because it's
unlikely we'll need to change it frequently, if at all.

For the reference counted classes, the API class
(e.g. ``Xapian::Enquire``) is really just a wrapper around a reference
counted pointer.  This points to an internal class
(e.g. ``Xapian::Enquire::Internal``).  The reference counted pointer
is a member variable of the API class called internal.  Conceptually
this member is private, though it typically isn't declared as private
(this is to avoid littering the external headers with friend
declarations for non-API classes).

There are a few exceptions to the reference counted structure, such as
``MSetIterator`` and ``ESetIterator`` which have an exposed
implementation.  Tests show this makes a substantial difference to
speed (it's ~20% faster) in typical cases of iterator use.

The postfix ``operator++`` for iterators should be implemented inline in terms
of the prefix form as described by Joe Buck on the gcc mailing list:

   .. code-block:: c++

      class some_iterator {
      public:
          // ...
          some_iterator& operator++();
          some_iterator operator++(int) {
              some_iterator tmp = *this;
              operator++();
              return tmp;
          }
      };

   The compiler is allowed to assume that the copy constructor only
   does a copy, and to optimize away unneeded copy operations.  The
   result in this case should be that, for some_iterator above, using
   the postfix operator without using the result should give code
   equivalent to using the prefix operator.

   [With modern compilers], you should find that this style comes very
   close to eliminating any penalty from "incorrect" use of the
   postfix form.

Xapian's PostingIterator, TermIterator, PositionIterator, and ValueIterator all
have only one data member which fits in a register.
