Xapian documentation
====================

Available documentation
-----------------------

There are a number of types of documentation available for Xapian.

1. User documentation that explains how people will use Xapian,
   typically either as a "full worked example", or as a deep dive into
   a particular feature.

   Most of this documentation lives in the *Xapian user guide*, which
   is available `online`_ (built with python examples)
   and as `a git repo on Github`_ which
   you can build for a range of different languages.
   
2. API documentation, which is built from specially-formatted comments
   in Xapian's C++ header files. This allows people writing code that
   uses Xapian to check on details of the classes and methods they are
   using, as well as to explore what features there are in Xapian.

3. Developer documentation, for people who want to
   contribute directly to Xapian.
   `This guide`_ is part of that
   (and is also `available on github`_).

   There is also some developer documentation available elsewhere,
   particularly in the main Xapian source repository -- look in
   particular in ``xapian-core/docs``. Some of this is being migrated
   to the developer guide. (Some is also being migrated to the user
   guide.)

4. `The Xapian wiki`_ also contains information, including links
   to articles and talks that people have written about using Xapian
   in various ways. Although this isn't always official Xapian
   documentation, it may be helpful. Some of it may now be quite out
   of date, so if something you find there doesn't work, you may want
   to check the official documentation to see if you can figure out
   how to change the advice or code you find to work with a recent
   version of Xapian.

5. Omega, a standalone indexing and web search tool built using
   Xapian, has its own documentation bundled with its source code,
   and `available on our website`_.

6. The language bindings,
   which allow you to use Xapian from programming languages others than C++,
   have `their own documentation`_
   which mostly focusses on ways that
   the API is different from C++ in those languages.
   
.. _online: https://getting-started-with-xapian.readthedocs.org/
.. _a git repo on Github: https://github.com/xapian/xapian-docsprint/
.. _This guide: https://xapian-developer-guide.readthedocs.org/
.. _available on github: https://github.com/xapian/xapian-developer-guide/
.. _The Xapian wiki: https://trac.xapian.org/wiki
.. _available on our website: https://xapian.org/docs/omega/
.. _their own documentation: https://xapian.org/docs/bindings/


Building the documentation
~~~~~~~~~~~~~~~~~~~~~~~~~~

The user guide and developers guide can both be built using Sphinx;
there is more information in a ``README.md`` in each repository.

If you're building from a clone of Xapian, then the API docs will be
built automatically as part of building Xapian. (If you've downloaded
a source tarball, then the API docs will already have been built for
you.) The API docs of the latest release are also available
`on the Xapian website`_, along with some of the developer
documentation that isn't in this developer guide.

Similarly, documentation for Omega and the bindings will be built when
you build them from source, and `is also available online`_.

.. _on the Xapian website: https://xapian.org/docs/apidoc/html/annotated.html
.. _is also available online: https://xapian.org/docs/


Updating the documentation
--------------------------

As you work on changes to Xapian, it's important
to write or update documentation
so people will know how to use your work.
Looking through the list of documentation above,
you can see there are a number of places
where you could add helpful information.
The main ones are:

* The user guide, which is where most new users expect to learn how
  to use Xapian.

* The API documentation, in doxygen comments in the source code.

* The developer guide, if you make changes which future developers
  will need to know.

  Note that you can also provide information to future developers in
  code comments, and in the commit messages you write. For many types
  of information, they are more convenient for other developers to
  find when they need. Try to add information where you yourself would
  expect to find it.

Don't worry about getting the words exactly right, particularly if
English isn't your first language. If you can get down the information
that needs to be there, someone else can tidy up the language if
necessary.


PDF versions of docs
--------------------

If you want PDF versions of the API documentation, you'll need to
install some more tools:

* gs (part of Ghostscript)
* pdflatex (in texlive-latex-base on Debian/Ubuntu)
* epstopdf (in texlive-extra-utils on Debian/Ubuntu)
* makeindex (in texlive-binaries on Debian/Ubuntu, or
  texlive-base-bin for older releases)

Note that pdflatex, epstopdf, gs, and makeindex must all currently be on your
path (as specified by the environmental variable PATH), since doxygen will look
for them there.

For a recent version of Debian or Ubuntu, this command should work:

.. code-block:: bash

   apt-get install ghostscript texlive-latex-base texlive-extra-utils \
       texlive-binaries texlive-fonts-extra texlive-fonts-recommended \
       texlive-latex-extra texlive-latex-recommended

On OS X, you can install `MacTeX <https://www.tug.org/mactex/>`_.

Once you've got the extra tools you need, you can build the PDF documentation:

.. code-block:: bash

   (cd xapian-core && make -C docs apidoc.pdf)
