Testing Omega
=============

Omega's test suite can be run in the normal fashion:
``make check`` within the ``omega`` directory. It runs a number of
smaller tests, most of them unit testing code that Omega relies on.

``atomparsetest``, ``htmlparsetest``
    Test the ``AtomParser`` and ``MyHtmlParser`` classes respectively,
    which parse Atom and HTML files as part of ``omindex``.

``csvesctest``, ``jsonesctest``, ``urlenctest``
    Test CSV, JSON, and URL escaping, used by the ``$csv{}``, ``$json{}``,
    and ``$url{}`` OmegaScript commands.

``md5test``
    Test our MD5 hashing routines, used both by the ``$hash{}`` OmegaScript
    command and for keeping a hash of indexed file contents (in Xapian
    document value 1), which allows duplicate documents to be collapsed.

``utf8convertest``
    Test our UTF8 conversion routines.


Clickmodel tests
----------------

All the clickmodel code lives in ``omega/clickmodel``, with tests for
each model in ``omega/clickmodel/tests`` and sample data in
``omega/clickmodel/testdata``.

Omegascript and functionality tests
-----------------------------------

We test most Omega functionality by running it against a specific database
with a particular Omegascript template. The ``omegatest`` script
provides a lightweight test harness (in shell) for writing test cases.
Test databases are created using ``scriptindex`` (which is easier to drive
programmatically than ``omindex``).

It takes a bit of time to get used to the way these tests are written,
but once you get the hang of it, writing tests for new Omegascript commands,
or other Omega functionality, is usually quite easy.

Currently, we do not have any tests for ``omindex``.
