Making a release of Xapian
==========================

This is a (hopefully complete) list of the jobs which need doing:

* Email Fabrice Colin and Tim Brody so they can check RPM packaging.

* Check if `config/config.guess` and `config/config.sub` need updating to
  more recent versions from https://git.savannah.gnu.org/gitweb/?p=config.git

* Check the revision currently specified in the bootstrap for the common
  subdirectory.  Unless there's a good reason, we should release
  xapian-core and omega with synchronised versions of the shared files.

* Make sure that any new/changed/removed API methods in xapian-core have been
  wrapped/updated/removed in xapian-bindings.

* Update the lists of deprecated/removed API methods in docs/deprecation.rst

* Update the NEWS files using information from the git logs

* Update the version in configure.ac for each module (xapian-core, omega, and
  xapian-bindings), and the library version info in xapian-core's configure.ac

* Make sure the submitters of fixed bugs are mentioned in the "thanks" list in
  xapian-core/AUTHORS.  Check the list for the appropriate milestone::

   https://trac.xapian.org/query?col=id&col=summary&col=reporter&milestone=1.4.12

* Check for any unfixed bugs on the milestone for the new release, and if they
  aren't blockers, retarget them::

   https://trac.xapian.org/roadmap

* Tag the source trees for the new revision - use the git-tag-release script,
  running it with the new version number, for example:

  .. code-block:: bash

     xapian-maintainer-tools/git-tag-release 1.4.12

  This script also generates tarballs for the new release and copies them
  across to the website.

* Update the wiki:

  Fill in https://trac.xapian.org/wiki/ReleaseOverview/1.4.12

  Also update the roadmap at https://trac.xapian.org/wiki/RoadMap with
  estimated date for next release.

* Update the website: ``generate`` in the ``www.xapian.org`` git repo
  contains the latest version and the date it was released.

* Run ``/home/olly/tmp/xapian-website-update/update_website.sh``

* Announce the new version on xapian-discuss

* Update the version numbers in this checklist ready for the next release
  
* Have a nice cup of tea!

How to make Debian packages for a new release
---------------------------------------------

Debian control files are stored in separate git repositories:

* https://anonscm.debian.org/cgit/collab-maint/xapian-bindings.git
* https://anonscm.debian.org/cgit/collab-maint/xapian-core.git
* https://anonscm.debian.org/cgit/collab-maint/xapian-omega.git

To package a new upstream release, these should be updated as follows:

* If there are any patch files in "debian/patches", check if these have been
  incorporated into the new release, and if so remove them and update
  "debian/patches/series".

* Update the ``debian/changelog`` file, being sure to keep it in the
  standard Debian format (the easiest way is to use the dch utility
  like so: ``dch -v 1.2.19-1``.  The new version number should be the
  version number of the release followed by "-1" (i.e., a debian
  patch number of 1).  The changelog message should indicate that
  there is a new upstream release, and should mention any significant
  changes in the new release.

* Tag using: ``git tag -s -m 1.2.19-1 1.2.19-1``

* FIXME: Document how to make source packages, or update
  ``make-source-packages``.

* FIXME: Document how to build binary packages, or update ``build-packages``.

* Test the packages.

* Run ``debsign build/*_amd64.changes`` to GPG sign the packages.

* Run ``dput build/*_amd64.changes`` to upload them to Debian.

* For the Ubuntu backports::

   ./backport-source-packages xapian-core 1.2.19-1 ubuntu
   ./backport-source-packages xapian-omega 1.2.19-1 ubuntu
   ./backport-source-packages xapian-bindings 1.2.19-1 ubuntu

  And once libsearch-xapian-perl is uploaded to Debian unstable::

   ./backport-source-packages libsearch-xapian-perl 1.2.19.0-1 ubuntu

  Then sign::

   debsign build/*99*_source.changes

  Upload::

   dput xapian-backports build/xapian-core*99*_source.changes

  Wait for that to have a chance to build, and then::

   dput xapian-backports build/xapian-[bo]*99*_source.changes
   dput xapian-backports build/libsearch-xapian-perl*_source.changes
