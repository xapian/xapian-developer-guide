Handy tips for aiding development
=================================

Disabling documentation builds
------------------------------

If you are find you are repeatedly changing the API headers (in include/)
during development, then you may become annoyed that the docs/ subdirectory
will rebuild the doxygen documentation every time you run "make" since this
takes a while.  You can disable this temporarily (if you're using GNU make),
by creating a file "docs/GNUmakefile" containing these two lines:

.. code-block:: make

   %:
       @echo "Skipping 'make $@' in docs"

Note that the whitespace at the start of the second line needs to be a
single "tab" character!

Don't forget to remove (or rename) this and check the documentation builds
before committing or generating a patch though!


Integration syntax checking with your editor
--------------------------------------------

If you are using an editor or other tool capable of running syntax checks as you
work there you can use the `make` target 'check-syntax'. For 'emacs' users this
works well with 'flymake'. Usage from a shell:

.. code-block:: bash

   make check-syntax check_sources=api/omdatabase.cc
