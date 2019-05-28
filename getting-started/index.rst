.. _getting started:

Getting started
===============

.. note::

   Currently this guide is written assuming you are either developing
   on something like Unix (probably either Linux or OS X). You can use
   software suchas `Virtual Box <https://www.virtualbox.org/>`_ to run
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

.. On Fedora, yum install libuuid-devel; we need more to bother
   including this.

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
computer, so it knows how to use them. However there are :ref:`various
options<configure-options>` that allow you to either override the
autodetection (for instance if you wanted to build python bindings
against a particular version of python) or change some defaults. For
now, however, we'll just run it accepting all its defaults:

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

Summary
-------

Now you've got everything working, you probably want to look at
:ref:`writing code<writing-code>`, or if you're trying to fix a bug
then you might want to learn about :ref:`debugging Xapian<debugging>`.

.. todo::

   The other sections of the manual haven't been written yet, so this
   part isn't terribly helpful. Sorry!
