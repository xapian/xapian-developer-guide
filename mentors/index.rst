Mentoring new contributors
==========================

Xapian frequently participates in `Google Summer of Code`_ (GSoC), which
encourages student developers to contribute to open source
projects. We also welcome new contributors at any time.

This section contains advice and information for anyone within the
community helping newcomers come up to speed as members of our
community. Especially during GSoC, we welcome anyone to act as a
mentor who is prepared to commit enough time. Many of our GSoC
students have gone on to mentor in subsequent years.

.. note::

   If you're looking for help in getting started, then we have `a
   guide for potential GSoC students`_ (which is worth reading through
   even if you aren't participating in GSoC). The :ref:`Contributing`
   section of this guide also has useful pointers.

.. _Google Summer of Code: https://summerofcode.withgoogle.com/

.. _a guide for potential GSoC students: https://trac.xapian.org/wiki/GSoC%20Guide


There's plenty of help
----------------------

You may be daunted by the idea of mentoring someone else, particularly
if you only have a limited amount of expeirence with Xapian
itself. Please don't let this put you off! Those who have been around
in the community for longer are generally happy to provide advice and
assistance, and there's also a `mentor guide`_ written for GSoC
(including contributions from Olly Betts from Xapian).

As with almost everything in life, **it's good to ask for
help**. Xapian as a community has years of experience with Summer of
Code, and some individuals have mentored five or more times. If we
can't figure out a problem together as a community, we can also ask
for help from other projects and mentors, and from the Google Summer
of Code team themselves.


.. _mentor guide: https://developers.google.com/open-source/gsoc/resources/guide


Helping newcomers step by step
------------------------------

Building Xapian
~~~~~~~~~~~~~~~

The first step should always to ensure that a new contributor can
build Xapian on a machine they have access to. Our :ref:`getting
started information<getting started>` is a good guide to follow.

Most Xapian developers use one or more of Debian, Ubuntu, and
macOS. While it's possible to develop for Xapian on a wide range of
operating systems, if someone runs into problems with something else
it's often easier for them to work with a linux virtual machine
running on their computer. As the getting started information says, we
recommend using `Virtual Box <https://www.virtualbox.org/>`_ and the
latest LTS (long-term support) release of Ubuntu.

.. note::

   If a new contributor runs into problems, it's worth getting them to
   explicitly confirm exactly what steps they've taken. It's easy to
   skip a step, or to try something else when you're working through
   issues -- but when it comes to helping someone else, you need to be
   sure you know exactly what they've done.

   A lot of our project communication happens on IRC, and it's
   difficult to read long outputs of commands such as ``make``
   there. It's helpful to have people copy anything substantial into
   something like `Pastebin <https://pastebin.com/>`_ or
   `Gist <https://gist.github.com/>`_ and then provide a link in
   IRC. This can be used for command output, or for the contents of
   files such as ``config.log``.


Getting familiar with the API
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

While the urge to jump right in and start fixing bugs or adding
features is often strong, it's a good idea for a new contributor to
become familiar with Xapian's API early on. This is particularly true
for Summer of Code students, who often won't have used Xapian
previously.

The `Xapian user manual`_ covers the concepts behind Xapian, works
through a practical example of indexing and searching documents using
Xapian, and also covers a range of more advanced features. The online
version uses python, but you can grab the `source code <user manual on
github>`_ and build for a range of languages, including C++.

.. note::

   Almost every contributor should get familiar with Xapian's C++
   API. Anyone who's adding new APIs to Xapian should also think about
   how that API will be used from bindings languages, so it's often
   helpful for them to have at least used Xapian via python or one of
   the other languages we support.

.. _Xapian user manual: https://getting-started-with-xapian.readthedocs.io/

.. _user manual on github: https://github.com/xapian/xapian-docsprint/


Completing a small task
~~~~~~~~~~~~~~~~~~~~~~~

We ask all Summer of Code students to complete at least one small
task, effectively as part of their application. This could be fixing a
bug, tidying up some code, improving test coverage, or completing a
small feature.

A main part of the reason for this is to help new contributors get
used to :ref:`the way we manage changes <contributing changes>` to
Xapian. It's good to pick something small, and go through the entire
process of planning and doing the work, creating a pull request, and
getting everything merged. Firstly, that means that they've become a
contributor to Xapian before Summer of Code even begins. Secondly, it
means that subsequent contributions as part of the GSoC project should
be smoother. Finally, it helps someone new to open source and
collaborative development to start thinking in terms of small changes,
merged quickly.

Most students will have an idea of what project they are interested
in, from our `list of project ideas`_. Some of those will have a small
first step, or sometimes a bug or similar in the general area of the
codebase of the project. For everyone else, we keep a list of `bite
sized projects`_, and there are also some bugs in our tracker marked
as `suitable for newcomers`_ to tackle.

.. _list of project ideas: https://trac.xapian.org/wiki/GSoCProjectIdeas

.. _bite sized projects: https://trac.xapian.org/wiki/ProjectIdeas#BiteSize

.. _suitable for newcomers: https://trac.xapian.org/query?status=!closed&keywords=~GoodFirstBug

Helping them through their first contribution
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Just as with getting Xapian built for the first time, new contributors
may need support in getting through our contribution flow. There are
some common things to watch out for.

Pull requests where the automated tests aren't passing

  This includes a special test run which checks that the code diff
  follows some of our conventions, such as around spaces and blank
  lines. Generally, the error messages should help track down the
  problem.

  It's possible (and a good idea) to run all the tests locally before
  opening a pull request. The code diff checks can be run by piping
  the output of ``git diff`` through the
  ``xapian-maintainer-tools/xapian-check-patch`` script. Something
  like this is often what you want:

  .. code-block: bash

     git diff master..HEAD | xapian-maintainer-tools/xapian-check-patch

  (The ``git diff`` command there will output the changes in your
  local commits compared to the "master" branch.)

Not following our :ref:`coding conventions <conventions>`

  We can't automate checks for all of these, but we also don't expect
  anyone to be able to spot all possible problems. One of the reasons
  pull request reviews are open is so that several different people
  can help spot and straighten out issues, and get a contribution over
  the line.

Not following our conventions for :ref:`pull request flow <pull
requests>`

  In particular, first-time contributors often need reminding not to
  force-push branches once a PR is open. It feels tidier to have a
  tidy list of commits. However, it makes it harder for reviewers to
  check that earlier comments have been addressed. Contributors should
  use "fixup" commits, as described in our documentation, and only
  tidy up commits right before a pull request is merged.

  A similar problem is a pull request with lots of commits without
  good commit messages. Each commit in a pull request should make a
  single, well-described change, including any necessary tests and
  documentation.

Some of these take a long time to get used to, and even experienced
developers will make mistakes. That means that it's worth checking for
the basics on every pull request.

Expectations of mentors
-----------------------

We operate a "group mentoring" approach, which means you can -- and
should! -- help any students you can during the summer. Where
possible, we expect mentors to find time every week to engage with
Xapian and our Summer of Code students. Here are some ways to do that.

Answer questions on the mailing list and IRC

  It's demotivating to ask a question and get no reply. Sometimes even
  just a response that says you don't know can help reassure a new
  contributor that they aren't on their own.

  Particularly during the early phases of Summer of Code, there are a
  lot of questions that come up repeatedly. New contributors regularly
  need help getting Xapian built and installed on their
  computers. People often need pointing at our guidance for potential
  students (the GSoC site sends people straight to our ideas list, and
  it's easy to miss the links we provide to furhter information). So
  something as simple as chipping in to point people to existing
  information and documentation can be incredibly valuable.

Help review pull requests

  The core of a contribution to Xapian is often a pull request. Before
  it's merged by one of the Xapian team, we want to make sure it's in
  :ref:`good shape <patch-guidelines>`. Anyone can check over our
  guidelines on what we're looking for, and provide feedback to a
  contributor on how to improve their pull request.

Provide feedback on design ideas

  Most Summer of Code projects have a knotty or interesting problem at
  the heart of them. That's what makes them appealing to work on over
  a period of months. However, that means that there's often one or
  more points during the project where some decisions have to be
  made. APIs need designing, data structures need choosing, and
  sometimes different competing algorithms need assessing.

  While we expect our students to do most of the work here, getting
  timely feedback and input from the rest of the community is often
  important in keeping a project on track. As with reviewing pull
  requests, anyone can look over a proposal and provide their
  thoughts.

  .. note::

     API design is a particularly difficult problem, and we generally
     do not expect any one person (student or not!) to design a great
     API on their own. It's not always obvious the best approach until
     you've written code that uses an API in a range of different
     situations.

     We generally recommend that projects that require a new API start
     by implementing a very simple one.  Ideally this will leave time
     later in the project to revise the initial version based on
     feedback and experience of actually using it. (Summer of Code
     students also include a section on possible future improvements
     in their project write-up. If there isn't enough time to improve
     an API based on feedback, that can always become a future
     project!)

Encourage small, regular contributions that can be merged

  We recommend structuring any project as a series of small
  sub-projects, each of which can be submitted as a pull request,
  reviewed, and merged. It's usually possible to start work on the
  next sub-project while the previous one is going through review.

  As well as encouraging contributors to submit small changes, it's
  also important that they address review comments quickly. It's all
  too easy to move on to the next sub-project, but never actually get
  the previous one merged. It's far better to spend time getting two
  or three sub-projects merged than have pull requests for four or
  five none of which is in a good enough state to be merged.

  Our best Summer of Code students have often had early contributions
  released during their project. Our experience shows that
  contributors are more likely to become longer-term members of the
  Xapian community if their early work can be merged and released.


As well as group mentoring, every student has a specific mentor
assigned, who is there to make sure there is always someone looking
out for them. You should keep in regular contact with the student
you're assigned to, making sure that they're getting the support they
need from the community.

.. note::

   Mentors, as well as students, can have something unexpected come up
   during the summer. Plans change, work becomes busier, or any one of
   a hundred things can mean you suddenly have less time than you
   anticipated.

   If something comes up, please let Xapian's "org admins" for Summer
   of Code know as soon as possible. Our group mentoring approach
   makes it easier to cope with people who have to step away from
   Summer of Code, but we need to know to ensure we can support all
   our students as best we can.
