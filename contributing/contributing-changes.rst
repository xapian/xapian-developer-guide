.. _contributing changes:

Contributing changes
====================

.. _patch-guidelines:

Some things that we look for
----------------------------

Beyond looking for changes that improve Xapian, code that works
and so forth, there are a number of things that we aim for when
accepting changes. This then is a list of good practices when
contributing changes.

Code that compiles cleanly and looks like existing code
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We like Xapian to compile without any warnings. In "maintainer
mode", which will be how you're building Xapian if you're
working from a git clone, all warnings will actually become
errors. You should fix the problems rather than change the
compilation settings to ignore these warnings.

Please configure your editor to:

* indent each block of code 4 columns from the containing block

  .. code-block: c++

     {
         // Four columns further
         {
             // And four more
         }
     }

* display the tab character by advancing to the next column that is a
  multiple of 8
* "tab fill" indents: all indents should start with as many tab
  characters as possible followed by as few spaces as possible
* use Unix line endings (so each line ends with just LF, rather than
  CR+LF)

We don't currently have a formal coding standards document, so
you should try to follow the style of the existing
code. In particular, it's a good idea to pay close attention
to code alignment and where we have spaces.

Updated documentation
~~~~~~~~~~~~~~~~~~~~~

If you add a new feature, please ensure that you've documented
it. Don't worry too much about the language you use, or if
English isn't your first language. Others can help get the
documentation into shape, but having a first draft from the
person who wrote the feature is usually the best way to get
started.

* API classes, methods, functions, and types should be
  documented by documentation comments alongside the
  declaration in ``include/xapian/*.h``.

  These are collated by doxygen -- see doxygen's documentation
  for details of the supported syntax.  We've decided to prefer
  to use ``@`` rather than ``\`` to introduce doxygen commands
  (the choice is essentially arbitrary, but ``\`` introduces
  C/C++ escape sequences so ``@`` is likely to make for easier
  to read mark up for C/C++ coders).

* The documentation comments don't give users a good overview,
  so we also need documentation which gives a good overview of
  how to achieve particular tasks.

  If there's relevant documentation already in the `user guide`_,
  then you should update that.  For completely new features,
  you should create either a "how to" or an "advanced feature"
  document in the user manual, so that people can get started
  without having to start with the API documentation.

* Internal classes, etc should also be documented by
  documentation comments where they are declared.

.. _user guide: https://getting-started-with-xapian.readthedocs.org/

Automated tests
~~~~~~~~~~~~~~~

If you're fixing a bug, you should first write a regression
test.  The test will fail on the existing code, then when you
fix the bug it will pass. In the future, the test will make
sure no one accidentally re-introduces the same bug.

If you're adding a new feature, you'll want to write tests that
it behaves correctly. Thinking about the tests you need to
write can often help you plan how to implement the feature; it
can also help when thinking about what API any new classes or
methods should expose.

Updated attributions
~~~~~~~~~~~~~~~~~~~~

If necessary, modify the copyright statement at the top of any
files you've altered. If there is no copyright statement, you may
add one (there are a couple of Makefile.am's and similar that
don't have copyright statements; anything that small doesn't
really need one anyway, so it's a judgement call).  If you've
added files which you've written from scratch, they should
include the GPL boilerplate with your name only.

If you're not in there already, add yourself to the
``xapian-core/AUTHORS`` file.

Consider backporting bug fixes
------------------------------

If there's an active release branch, please check if the bug is present
in that branch, and if the fix is appropriate to backport - if the fix
breaks ABI compatibility or is very invasive, you may need to fix it in
a different way for the release branch, or decide not to backport the fix.

License grant
-------------

We ask everyone contributing changes to Xapian to :ref:`dual-license
<licensing>` under the GPL (which Xapian currently uses) and the MIT/X
license (which we would like to move to in future). The simplest way
to do this is to drop an email to the xapian-devel `mailing list
<https://xapian.org/lists>`_ stating that you own the copyright on your
changes and are happy to dual-license accordingly.

Submit your patch
-----------------

There are two ways of working, depending on whether you want to use
Github or not. In both cases, review and acceptance of the changes
will generally go more easily if you've included tests, updated
documentation and so on :ref:`as discussed earlier<patch-guidelines>`.

Attach a patch directly to the trac ticket
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We find patches in unified diff format easiest to work with. ``git diff``
produces the right output for a single commit (or ``git format-patch``
for a series of commits).

Someone from the community will then be able to review the patch
and decide if it needs further work before integrating. If so,
they'll leave comments on the trac ticket (trac will generally
email you if you're marked as the owner, or you can explicitly
add yourself to the "cc" list for a ticket).

.. _pull requests:

Open a Pull Request on github
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

`Github pull requests`_ provide a web-based interface for review
and discussion of changes before they are accepted into
Xapian. Github's documentation explains how you can go about
opening them.

If your patch is a sub-project in a larger piece of work, then
it's important not to assume the patch is fine as it stands and to
immediately start the next sub-project. Instead you should
concentrate on completing the sub-project before moving on. Since
you'll almost always have to wait at least a little time to get
feedback on any changes, you may want to put the code and tests up
while still working on documentation.

You should add further changes to pull requests by creating
additional commits locally, typically by using ``git commit --fixup``,
and then pushing the branch up to Github. Only once everything's
been approved should you `squash your commits
together`_ to keep the history clean.

.. note::

   Once you've opened a pull request, you shouldn't have to close
   it until it's merged (in which case we'll generally close it for
   you). Even if you need to redo some work, you can either add
   fixup commits or (with agreement from whoever is reviewing the
   PR) unwind your work and create completely new commits, force
   pushing to replace the previous commits in the pull request.

   It makes it much harder to review if you close a pull request in
   the middle of a review only to open another with similar code.

.. _Github pull requests: https://help.github.com/categories/collaborating-on-projects-using-pull-requests/
.. _squash your commits together: https://robots.thoughtbot.com/git-interactive-rebase-squash-amend-rewriting-history
