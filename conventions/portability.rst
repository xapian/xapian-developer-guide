Portability
===========

C++ Portability Issues
~~~~~~~~~~~~~~~~~~~~~~

Web Resources
-------------

The `C++ Super-FAQ`_ covers many frequently asked C++ questions.

.. _C++ Super-FAQ: https://isocpp.org/faq

Header Portability Issues
-------------------------

``<fcntl.h>``

  Don't directly ``#include <fcntl.h>`` - instead ``#include "safefcntl.h"``.

  The main reason for this is that when using certain compilers on
  certain versions of Solaris, fcntl.h does ``#define open open64``.
  Sadly this breaks C++ code which has methods called open (as we do).
  There's a cunning workaround for this problem in
  ``common/safefcntl.h``.

  Also, ``safefcntl.h`` ensures the ``O_BINARY`` is defined (to 0 if
  not required) so calls to ``open()`` and ``creat()`` can specify
  ``O_BINARY`` unconditionally for the benefit of platforms which
  discriminate between text and binary files.

``<windows.h>``

  Don't directly ``#include <windows.h>`` - instead ``#include
  "safewindows.h"`` which reduces the bloat of header files included
  and prevents some of the more egregious namespace pollution.  It
  also defines any constants we need which might be missing in older
  versions of the mingw headers.

``<winsock2.h>``

  Don't directly ``#include <winsock2.h>`` - instead ``#include
  "safewinsock2.h"``.  This ensure that safewindows.h is included
  before <winsock2.h> to avoid winsock2.h including windows.h without
  our namespace pollution reducing workarounds.

``<sys/select.h>``

  Don't directly ``#include <sys/select.h>`` - instead ``#include
  "safesysselect.h"`` which supports older UNIX platforms which
  predate POSIX 1003.1-2001 and works around a problem on Solaris.

``<sys/socket.h>``

  Don't directly ``#include <sys/socket.h>`` - instead ``#include
  "safesyssocket.h"`` which supports older UNIX platforms which
  predate POSIX 1003.1-2001 and works on Windows too.

``<sys/stat.h>``

  Don't directly ``#include <sys/stat.h>`` - instead ``#include
  "safesysstat.h"`` which under MSVC enables stat to work on files >
  2GB, defines the missing POSIX macros S_ISDIR and S_ISREG, pulls in
  <direct.h> for mkdir() (which is provided by sys/stat.h under UNIX)
  and provides a compatibility wrapper for mkdir() which takes 2
  arguments (so code using mkdir can always just pass two arguments).

``<sys/wait.h>``

  To get `WEXITSTATUS` or `WIFEXITED` defined, ``#include
  "safesyswait.h"``.  Note that this won't provide `waitpid()`, etc on
  Microsoft Windows, since these functions are only really useful to
  use when `fork()` is available.

``<unistd.h>``

  Don't directly ``#include <unistd.h>`` - instead ``#include
  "safeunistd.h"`` -- MSVC doesn't even HAVE unistd.h!

The various "safe" headers are maintained in ``xapian-core/common``,
but also used by Omega.  Currently bootstrap sorts out setting up a
copy of this subdirectory via a secondary git checkout.


Warning-Free Compilation
------------------------

Compiling without warnings on every platform is our goal, though it's not
always possible to achieve.  For example, some GCC 3.x compilers produce the
occasional bogus warning (e.g.  warning that a variable may be used
uninitialised, despite it being initialised at the point of declaration!)

You should consider configure-ing with:

.. code-block:: bash

   ./configure CXXFLAGS=-Werror

when doing development work on Xapian.  This promotes warnings to errors,
which should ensure you at least don't introduce new warnings for the compiler
you're using.

If you configure with ``--enable-maintainer-mode``, and are using GCC
4.1 or newer, this is done for you automatically.  This is intended to
be an aid rather than a form of automated punishment - it's all too
easy to miss a new warning as once a file is compiled, you don't see
it unless you modify that file or one of its dependencies.

With Intel's C++ compiler, ``--enable-maintainer-mode`` also enables
``-Werror``.  If you know the equivalent of ``-Werror`` for other
compilers, please add a note here, or tell us so that we can add a
note.

Miscellaneous Portability Issues
--------------------------------

Make sure that the last line of any source file ends with a linefeed character
since it's undefined behaviour if it doesn't (most compilers accept it, though
at least GCC gives a warning).

Makefile Portability
~~~~~~~~~~~~~~~~~~~~

We don't want to force those building Xapian from the source distribution to
have to use GNU make.  Requiring GNU make for "make dist" isn't such a problem
but it's probably better to use portable constructs everywhere to avoid
problems when people move or copy code between targets.  If you do make use
of non-portable constructs where it's OK, add a comment noting the special
circumstances which justify doing so.

Here's an incomplete list of things to avoid:

* Don't use "$(RM)" - it's defined by GNU make, but using it actually harms
  portability as other makes don't define it.  Use plain "rm" instead.

* Don't use "%" pattern rules - these are GNU make specific.  Use an
  implicit rule (e.g. ".c.o:") if you can.  Otherwise, write out each version
  explicitly.

* Don't use "$<" except in implicit rules.  This is an annoying restriction,
  as using "$<" makes it much easier to make VPATH builds work.  But it's only
  portable in implicit rules.  Tips for rewriting - if it's a source file,
  write it as:

    .. code-block:: bash

        $(srcdir)/foo.ext

  If it's a generated object file or similar, just write the name as is.  The
  tricky case is a generated file which isn't in git but is shipped in the
  distribution tarball, as such a file could be in either the source or build
  tree.  Use this trick to make sure it's found whichever directory it's in:

    .. code-block:: bash

        `test -f foo.ext || echo '$(srcdir)/'`foo.ext

* Don't use "exit 0" to make a rule fail.  Use "false" instead.  BSD make
  doesn't like "exit 0" in a rule.

* Don't use make conditionals.  Automake offers conditionals which may be
  of use, and these are implemented to work with any make.  See the automake
  manual for details, and a few caveats.

* The list of portable utilities is:

    .. code-block:: bash

       cat cmp cp diff echo egrep expr false grep install-info
       ln ls mkdir mv pwd rm rmdir sed sleep sort tar test touch true

  Note that versions of these (GNU versions in particular) support switches
  which aren't portable - notably, "test -r" isn't portable; neither is
  "cp -a".  And note that "mkdir -p" isn't portable - the semantics vary.
  The autoconf manual has some `useful information about writing portable
  shell code`_ (most of it not specific to autoconf).

.. _useful information about writing portable shell code:
   https://www.gnu.org/software/autoconf/manual/autoconf.html#Portable-Shell

* Don't use "include" - it's not present in BSD make (at least some versions
  have ".include" instead, but that doesn't really seem to help...)  Automake
  provides a configure-time include, which may provide a replacement for some
  uses of "include".

* It appears that BSD make only supports VPATH for implicit rules (e.g.
  ".c.o:") - there's certainly a restriction there which is not present in GNU
  make.  We used to try to work around this, but now we use AM_MAINTAINER_MODE
  to disable rules which are only needed by those developing Xapian (these were
  the rules which caused problems).  And we recommend those developing Xapian
  use GNU make to avoid problems.

* Rules with multiple targets can cause problems for parallel builds.  These
  rules are really just a shorthand for multiple rules with the same
  prerequisites and commands, and it is fine to use them in this way.  However,
  a common temptation is to use them when a single invocation of a command
  generates multiple output files, by adding each of the output files as a
  target.  Eg, if a swig language module generates xapian_wrap.cc and
  xapian_wrap.h, it is tempting to add a single rule something like:

    .. code-block:: make

       # This rule has a problem
       xapian_wrap.cc xapian_wrap.h: xapian.i
               SWIG_commands

  This can result in SWIG_commands being run twice, in parallel.  If
  SWIG_commands generates any temporary files, the two invocations can
  interfere causing one of them to fail.

  Instead of this rule, one solution is to pick one of the output files as a
  primary target, and add a dependency for the second output file on the first
  output file:

    .. code-block:: make

       # This rule also has a problem
       xapian_wrap.h: xapian_wrap.cc
       xapian_wrap.cc: xapian.i
               SWIG_commands

  This ensures that make knows that only one invocation of SWIG_commands is
  necessary, but could result in problems if the invocation of SWIG_commands
  failed after creating xapian_wrap.cc, but before creating xapian_wrap.h.
  Instead, we recommend creating an intermediate target:

    .. code-block:: make

       # This rule works in most cases
       xapian_wrap.cc xapian_wrap.h: xapian_wrap.stamp
       xapian_wrap.stamp: xapian.i
               SWIG_commands
               touch $@

  Because the intermediate target is only touched after the commands have
  executed successfully, subsequent builds will always retry the commands if an
  error occurs.  Note that the intermediate target cannot be a "phony" target
  because this would result in the commands being re-run for every build.

  However, this rule still has a problem - if the xapian_wrap.cc and
  xapian_wrap.h files are removed, but the xapian_wrap.stamp file is not, the
  .cc and .h files will not be regenerated.  There is no simple solution to
  this, but the following is a recipe taken from the automake manual which
  works.  For details of *why* it works, see the section in the automake manual
  titled "Multiple Outputs":

    .. code-block:: make

       # This rule works even if some of the output files were removed
       xapian_wrap.cc xapian_wrap.h: xapian_wrap.stamp
       ## Recover from the removal of $@.  A full explanation of these rules is in
       ## the automake manual under the heading "Multiple Outputs".
               @if test -f $@; then :; else \
                 trap 'rm -rf xapian_wrap.lock xapian_wrap.stamp' 1 2 13 15; \
                 if mkdir xapian_wrap.lock 2>/dev/null; then \
                   rm -f xapian_wrap.stamp; \
                   $(MAKE) $(AM_MAKEFLAGS) xapian_wrap.stamp; \
                   rmdir xapian_wrap.lock; \
                 else \
                   while test -d xapian_wrap.lock; do sleep 1; done; \
                   test -f xapian_wrap.stamp; exit $$?; \
                 fi; \
               fi
       xapian_wrap.stamp: xapian.i
               SWIG_commands
               touch $@

* This is actually a robustness point, not portability per se.  Rules which
  generate files should be careful not to leave a partial file in place if
  there's an error as it will have a timestamp which leads make to believe it's
  up-to-date.  So this is bad:

  .. code-block:: make

     foo.cc: script.pl
             $PERL script.pl > foo.cc

  This is better:

  .. code-block:: make

     foo.cc: script.pl
             $PERL script.pl > foo.tmp
             mv foo.tmp foo.cc

  Alternatively, pass the output filename to the script and make sure you
  delete the output on error or a signal (although this approach can leave
  a partial file in place if the power fails).  All used Makefile.am-s and
  scripts have been checked (and fixed if required) as of 2003-07-10 (didn't
  check xapian-bindings).

* Another robustness point - if you add a non-file target to a makefile, you
  should also list it in ".PHONY".  Otherwise your target won't get remade
  reliably if someone creates a file with the same name in their tree.  For
  example:

  .. code-block:: make

     .PHONY: hello goodbye

     hello:
           echo hello

     goodbye:
           echo goodbye

And lastly a style point - using "@" to suppress echoing of commands being
executed removes choice from the user - they may want to see what commands
are being executed.  And if they don't want to, many versions of make support
the use "make -s" to suppress the echoing of commands.

Using ``@echo`` on a message sent to stdout or stderr is acceptable
(since it avoids showing the message twice).  Otherwise don't use
``@`` - it makes it harder to track down problems in the makefiles.
