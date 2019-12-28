Adding support for other programming languages
==============================================

Ambition
--------

We'd like Xapian to be usable from as many languages as possible. However,
this means more than just having bindings code. To truly support a language,
we need to have:

* working bindings that can be built against a recent release of Xapian or git
  master, as well as against the latest version of the target language

* documentation to help people familiar with the target language to start using
  the bindings

* example code -- ideally translations of the code in the `Xapian user guide`_.
  You can start from the `user guide source code`_, which already supports a few
  languages.

* automated tests for the bindings; at least a "smoketest" hooked into the tests
  for the build system, so we'll notice if something significant breaks. See
  :ref:`the section on testing bindings <testing-bindings>`.

.. _Xapian user guide: https://getting-started-with-xapian.readthedocs.io/en/latest/

.. _user guide source code: https://github.com/xapian/xapian-docsprint

Some things you need to know
----------------------------

Many languages can be done using SWIG, and it's probably easier to do so
even though some languages may have better tools available just because it's
less overall work.  SWIG makes it particularly easy to wrap a new method for
all the supported languages at once - in many cases just adding the method
to the C++ API is enough, since SWIG parses the C++ API headers.

To give an idea of how much work a set of bindings might be, the author of the
Ruby bindings estimated they took about 25 hours, starting from not knowing
SWIG.  However, the time taken could vary substantially depending on the
language, how well you know it, and how well SWIG supports it.

What's really needed is someone interested in bindings for a particular
language who knows that language well and will be using them actively.
We can help with the Xapian and SWIG side, but without somebody who knows
the language well it's hard to produce a solid, well tested set of bindings
rather than just a token implementation.

Experiments and partially-completed bindings
--------------------------------------------

XS bindings for Perl have been contributed for Perl, and these are available
on CPAN as ``Search::Xapian``.  We also have SWIG-based Perl bindings which are
in the ``perl`` subdirectory here, as a ``Xapian`` module.  These are replacing
the XS-based ``Search::Xapian``, and will eventually be on CPAN.

These are languages which SWIG supports and which people have done some work
on producing Xapian bindings for:

Go
	There are some basic Go bindings written by Marius Tibeica in the
	"golang" branch, which is currently based on the ``RELEASE/1.4`` branch.

Ocaml
	Dan Colish did `some initial work on Ocaml support <ocaml_>`_.

Pike
	Bill Welliver has written some `Pike bindings for Xapian <pike_>`_ covering
	some of the API.

	These bindings appear to be hand-coded rather than generated using SWIG.
	SWIG 4.0.0 has disabled Pike support due to lack of recent maintenance,
	so SWIG is probably not a good option here.

.. _ocaml: https://trac.xapian.org/ticket/588
.. _pike: http://modules.gotpike.org/module_info.html?module_id=42

There are a number of other languages which SWIG supports, but which nobody has
yet (to our knowledge!) worked on producing Xapian bindings for -- see `the SWIG
documentation <swig_>`_ for a list of supported languages. It may be possible to
support a language which isn't listed above, but it's likely to be harder unless
you're already familiar with the tools available for wrapping a C++ library for
use with that language.

.. _swig: http://www.swig.org/compare.html
