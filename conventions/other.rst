Other conventions
=================

Configure Options
~~~~~~~~~~~~~~~~~

Especially for a library, compile-time options aren't a good solution for
how to integrate a new feature.  An increasingly large number of users install
pre-built binary packages rather than building from source, and unless the
package is capable of being split into modules, the packager has to choose a
set of compile-time options to use.  And they'll tend to choose either the
standard ones, or perhaps a broader set to try to keep everyone happy.  For a
library, similar issues occur when installing from source as well - the
sysadmin must choose the options which will keep all users happy.

Another problem with compile-time options is that it's hard to ensure that
a change doesn't break compilation under some combination of options without
actually building and running the test-suite on all combinations.  The fewer
compile-time options, the more likely the code will compile with every
combination of them.

So please think carefully before adding more compile-time options.  They're
probably OK for experimental features (but should go away once a feature is no
longer experimental).  Options to instrument a build for special purposes
(debug, profiling, etc) are also acceptable.  Disabling whole features probably
isn't (e.g. the ``--disable-backend-XXX`` options we already have are dubious,
though being able to disable the remote backend can be useful when trying to
get Xapian going on a platform).

Naming of Scripts
~~~~~~~~~~~~~~~~~

Scripts generally should *not* have an extension indicating the language they
are currently implemented in (e.g. ``runtest`` rather than ``runtest.sh`` or
``runtest.pl``).  The problem with such an extension is that if we decide
to reimplement the script in a different language, we either have to rename
the script (which is annoying as people will be used to the name, and may
have embedded it in their own scripts), or we have a script with a confusing
name (e.g. a Python script with extension ``.pl``).

The above reasoning doesn't apply to scripts which have to be in a particular
language for some reason, though for consistency they probably shouldn't get
an extension either, unless there's a good reason to have one.
