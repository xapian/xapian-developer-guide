.. _getting started:

Getting started
===============

.. note::

   Currently this guide is written assuming you are either developing
   on something like Unix (probably either Linux or OS X). You can use
   software such as `Virtual Box <https://www.virtualbox.org/>`_ to run
   a 'virtual' Linux machine on another operating system; in this case
   we recommend using the most recent `Ubuntu LTS
   release <https://wiki.ubuntu.com/LTS>`_.

   It should be possible to build and develop Xapian on Windows,
   but we currently don't have any documentation on doing so, or
   any active developers with suitable experience.

Getting the source code
-----------------------

First off, let's make sure you have a copy of the Xapian source and
can build it and run the tests. This is generally a little different
to if you're just installing Xapian to use it, because you'll be
working with the entire source tree rather than individual pieces. The
Xapian build system has some support for this, but let's get you a
copy of everything first:

.. code-block:: bash

   $ git clone git://git.xapian.org/xapian
   $ cd xapian

This will 'clone' a complete copy of the Xapian source code, including
not only the core library but also the various language bindings (for
use from Python, Lua, Ruby and so on) and the self-contained web
search system 'Omega'. It also contains all the tests for those
various components.

Installing the dependencies
---------------------------

Debian / Ubuntu
~~~~~~~~~~~~~~~

For a recent version of Debian or Ubuntu, this command should ensure you have
all the necessary tools and libraries:

.. code-block:: bash

   $ apt-get install build-essential m4 perl python zlib1g-dev uuid-dev \
     wget bison tcl libpcre3-dev libmagic-dev valgrind ccache eatmydata \
     doxygen graphviz help2man python-docutils pngcrush python-sphinx \
     python3-sphinx mono-devel openjdk-8-jdk lua5.2 liblua5.2-dev \
     php-dev php-cli python-dev python3-dev ruby-dev tcl-dev texinfo

OS X
~~~~

You need to install Apple's XCode tools, which contain their compiler,
debugger and various other tools. You can do that from within the
AppStore.

We recommend using `homebrew <https://brew.sh/>`_ to install and manage
additional libraries and tools on OS X. Once you've installed XCode
and homebrew, you can get all the dependencies you need for Xapian
using:

.. code-block:: bash

   $ brew install libmagic pcre \
     lua mono perl php python python3 ruby tcl-tk \
     doxygen help2man graphviz pngcrush
   # and some python-specific documentation tools
   $ pip install sphinx docutils
   $ pip3 install sphinx

(We install documentation tools for both python2 and python3, in the
same way we build the bindings for both of them.)

Windows
~~~~~~~

Building using MSVC is supported by the autotools build system.  You need
to install a set of Unix-like tools first -- we recommend `MSYS2
<https://www.msys2.org/>`_.

For details of how to specify MSVC to ``configure`` see the "INSTALL" document
in ``xapian-core``.

When building from git, by default you'll need some extra tools to generate
Unicode tables (Tcl) and build documentation (doxygen, help2man, sphinx-doc).
We don't currently have detailed advice on how to do this (if you can provide
some then please send a patch).

You can avoid needing Tcl by copying ``xapian-core/unicode/unicode-data.cc``
from another platform or a release which uses the same Unicode version.  You
can avoid needing most of the documentation tools by running configure with
the ``--disable-documentation`` option.

On other platforms
~~~~~~~~~~~~~~~~~~

You will need the following tools installed to build from git:

* GNU m4 >= 4.6 (for autoconf)
* perl >= 5.6 (for automake; also for various maintainer scripts)
* python >= 2.3 (for generating the Python bindings)
* GNU make (or another make which support VPATH for explicit rules)
* GNU bison (for building SWIG, used for generating the bindings)
* Tcl (to generate unicode/unicode-data.cc)

There are also a number of libraries you'll need available, including
development headers:

* `zlib1g <https://zlib.net>`_
* `libuuid <https://git.kernel.org/pub/scm/utils/util-linux/util-linux.git/tree/libuuid>`_
* `PCRE <https://www.pcre.org>`_
* libmagic (which is part of `the open source implementation of file(1) <https://www.darwinsys.com/file/>`_)

.. On Fedora, yum install libuuid-devel -- can we get a more complete list?

If you're doing much development work, you'll probably also want the following
tools installed:

* `valgrind <http://valgrind.org/>`_ for better testsuite error finding
* `ccache <https://ccache.dev>`_ for faster rebuilds
* `eatmydata <https://www.flamingspork.com/projects/libeatmydata/>`_ for faster testsuite runs

If you want to be able to build distribution tarballs (with ``make dist``) then
you'll also need some further tools:

* doxygen (v1.8.8 is used for 1.3.x snapshots and releases; 1.7.6.1 fails to
  process git master after ``PL2Weight`` was added).
* dot (part of Graphviz.  Doxygen's ``DOT_MULTI_TARGETS`` option apparently needs
  ">1.8.10")
* help2man
* rst2html or rst2html.py (``pip install docutils``)
* pngcrush (optional - used to reduce the size of PNG files in the HTML
  apidocs)
* sphinx-doc (``pip install sphinx`` should do)

Building Xapian
---------------

Bootstrapping the code
~~~~~~~~~~~~~~~~~~~~~~

Xapian needs to set up a few things with a fresh clone of the code, as
well as downloading and building some tools for which we require very
precise versions. You should run this command in the ``xapian``
directory that was created earlier when you cloned the source code:

.. code-block:: bash

   $ ./bootstrap

To download tools, bootstrap will use ``wget``, ``curl`` or
``lwp-request`` if installed.  If not, it will give an error telling
you the URL to download from by hand and where to copy the file to.

.. note::

   As well as installing some tools, bootstrap will also run
   ``autoreconf`` on each of the checked-out subdirectories, and
   generate a top-level ``configure`` script.  This configure script
   allows you to configure xapian-core and any other modules you've
   checked out with a single simple command, such that the other modules
   link against the uninstalled xapian-core (which is very handy for
   development work and a bit fiddly to set up by hand).  It
   automatically passes ``--enable-maintainer-mode`` to the
   subprojects so that the autotools will be rerun if
   ``configure.ac``, ``Makefile.am``, etc are modified.

.. warning::

   If you are tracking development in git, there will sometimes be
   changes to the build system sources which require regeneration of
   the generated makefiles and associated machinery.  We aim to make
   the build system automatically regenerate the necessary files, but
   in the event that a build fails after an update, it may be worth
   re-running the bootstrap script to regenerate the build system from
   scratch, before looking for the cause of the error elsewhere.

Configuring the code
~~~~~~~~~~~~~~~~~~~~

Configuring the code is mostly about Xapian's build system
automatically detecting where all its dependencies are on your
computer, so it knows how to use them. However there are various
options that allow you to either override the autodetection
(for instance if you wanted to build python bindings
against a particular version of python) or change some defaults.
To find out about the configure options available, you can run
``configure --help``. For now, however, we'll just run it accepting
all its defaults:

.. code-block:: bash

   $ ./configure

Note that on OS X you probably want to turn off the Perl and TCL8
bindings when developing, as there are some complexities when
developing against the system versions, and the homebrew versions are
slightly awkward:

.. code-block:: bash

   $ ./configure --without-perl --without-tcl

Building Xapian
~~~~~~~~~~~~~~~

Building Xapian is just a matter of typing:

.. code-block:: bash

   $ make

First it will build xapian-core, the core library. Then it will build
Omega and the language bindings, using the version of xapian-core
you've just built, but not yet installed. (This is the bit that causes
some problems on OS X if you use system versions of any of the
languages.)

Running the tests
-----------------

Xapian has a comprehensive test suite, and it's a good idea to get
into the habit of running it. From the top of the clone, just run:

.. code-block:: bash

   $ make check

Again, the tests for xapian-core are run first, then Omega and then
the language bindings. If any test fails, the build system will stop
there.

A quick note about the build system
-----------------------------------

Here, we've been working from a clone of the Xapian git repository,
which means that the following options are on by default.
However if you are ever building from a source tarball,
the following may be of use.

``--enable-maintainer-mode``
	This tells configure to enable make dependencies for
	regenerating build system files (such as ``configure``,
	``Makefile.in``, and ``Makefile``) and other generated files (such as
	the stemmers and query parser) when required.  These are
	disabled by default as some make programs try to rebuild them
	when it's not appropriate (e.g. BSD make doesn't handle VPATH
	except for implicit rules).  For this reason, we recommend GNU
	make if you enable maintainer mode.

        You'll also need a non-cross-compiling C compiler for
	compiling the Lemon parser generator and the Snowball stemming
	algorithm compiler.  The configure script will attempt to
	locate one, but you can override this autodetection by passing
        ``CC_FOR_BUILD`` on the command line like so:

        .. code-block:: bash

           ./configure CC_FOR_BUILD=/opt/bin/gcc

``--enable-documentation``
	This tells configure to enable make dependencies for regenerating
	documentation files.  By default it uses the same setting as
	``--enable-maintainer-mode``.

Xapian's build system has a lot of other options you can use to
control exactly what gets built and in what ways. Check out help
information for the various tools for more information, such as
``./bootstrap --help`` and ``./configure --help``.

Summary
-------

Now you've got everything working, you probably want to look at
:ref:`contributing to Xapian<contributing>`, or if you're trying to fix a bug
then you might want to learn about :ref:`debugging Xapian<debugging>`.
