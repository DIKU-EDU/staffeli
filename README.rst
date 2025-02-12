``staffeli`` — DIKU Support Tools for Canvas LMS
================================================

These tools leverage the `Canvas LMS REST
API <https://canvas.instructure.com/doc/api/index.html>`__ to create a
more pleasant environment for working with
`Absalon <https://absalon.ku.dk/>`__.

"Staffeli" is Danish for "easel" — a support frame for holding up a
canvas.

|Documentation Status| |Travis CI (Linux + macOS) Status| |License: EUPL
v1.1| |PyPI|

.. |Documentation Status| image:: https://readthedocs.org/projects/staffeli/badge/
   :target: http://staffeli.readthedocs.io/en/latest/
.. |Travis CI (Linux + macOS) Status| image:: https://travis-ci.org/diku-dk/staffeli.svg
   :target: https://travis-ci.org/diku-dk/staffeli
.. |License: EUPL v1.1| image:: https://img.shields.io/badge/license-EUPL%20v1.1-blue.svg
   :target: https://github.com/DIKU-EDU/Staffeli/blob/master/LICENSE.md
.. |PyPI| image:: https://img.shields.io/pypi/v/staffeli.svg
   :target: https://pypi.python.org/pypi/staffeli

.. contents::

.. section-numbering::

Purpose
-------

The purpose of Staffeli is two-fold:

1. Leverage the `Canvas LMS REST
   API <https://canvas.instructure.com/doc/api/index.html>`__ to get
   things done better, faster, stronger.
2. Quick prototyping of new features for `Canvas
   LMS <https://www.canvaslms.com/>`__.

Initially, Staffeli is *not* intended for managing course content,
merely to snapshot course data (e.g., enrolled students, groups,
sections, submissions), *and* to get grading done efficiently.

Although Staffeli is written in Python 3, it is not intent on forcing you to
manage your course using Python, or to have to get intimate with the Staffeli
API to get things done. Staffeli extensively uses YAML files for storage,
enabling the easy use of both *command-line utilities* and *the programming
language of your choice*, to get things done quickly, and efficiently.

Status
------

Staffeli is maturing. It is being transitioned to be annotated with `type hints
<https://www.python.org/dev/peps/pep-0484/>`__, with the types checked
statically with `mypy <http://mypy-lang.org/>`__, and a test-suite `has been
set up <tests>`__, but full-blown continuous integration remains to be set up.

We are still covering a fairly small subset of the API. Brace yourself. Lend a
hand.

Installation
------------

These are instructions for installing Staffeli on your system.

1. `Download this repository <https://github.com/DIKU-EDU/staffeli/archive/master.zip>`__
   or ``git clone https://github.com/DIKU-EDU/staffeli.git``
2. Enter directory and run ``pip3 install -e .``

These are instructions for installing Staffeli in a user directory.

1. Check that you have Python 3 and pip installed: ``which python3 pip3``
2. Install `virtualenv <https://virtualenv.pypa.io/en/stable/>`__ globally,
   e.g. with ``sudo pip3 install virtualenv``
3. `Download this repository <https://github.com/DIKU-EDU/staffeli/archive/master.zip>`__
   or ``git clone https://github.com/DIKU-EDU/staffeli.git``
4. Enter directory and create virtualenv with ``virtualenv .``
5. Activate virtualenv with ``source bin/activate``
6. Install Staffeli in virtualenv, ``pip3 install -e .``

Getting Started
---------------

With Staffeli, we work with local course clones. We aim to keep these
clones compatible with git.

We recommend that you create a local directory ``canvas``, ``absalon``,
or similar, for all of you Canvas-related local course clones. Staffeli
needs some initial help to be able to login with your credentials. You
need to `generate a
token <https://guides.instructure.com/m/67952/l/1018197-how-do-i-manage-api-access-tokens-in-my-user-account>`__
for Staffeli to use, and save it as ``.token``, ``token``, or
``token.txt`` in this high-level directory.

**NB!** This is your personal token so **do not** share it with others,
else they can easily impersonate you using a tool like Staffeli.
Unfortunately, to the best of our knowledge, Canvas has no means to
segregate or specialize tokens, so this is really "all or nothing".

Cloning a Course
^^^^^^^^^^^^^^^^

To clone a course:

::

    $ staffeli clone '<course name>' [dir]

The ``'course name'`` is any case-insensitive substring of the course name as
it appears on your dashboard. Use quotes in case the substring contains spaces.
If there are multiple conflicting names matching the substring, Staffeli will
complain and let you try again.

The ``[dir]`` is an optional destination directory for the local working area,
in case it should not be named ``<course string>``. For example::

    $ staffeli clone 'Advanced programming' ap17
    Cloning '5100-B1-1E17;Advanced programming' into 'ap17'...

Fetch Submissions for a New Assignment
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Use ``staffeli fetch``. For instance, to fetch all submissions for "A3":

::

    $ staffeli fetch subs/A3

To fetch just the metadata for all submissions, but not the submissions
themselves:

::

    $ staffeli fetch subs

Fetch Groups
^^^^^^^^^^^^

This is a good idea to make sure you are up-to-date with canvas.

::

    $ staffeli fetch groups


Grade a Submission
^^^^^^^^^^^^^^^^^^

Assuming you are in the submission directory, you can use
``staffeli grade`` to grade the submission:

::

    staffeli grade GRADE [-m COMMENT] [FILEPATH]...

    Where
        GRADE           pass, fail, or an int.
        [-m COMMENT]    An optional comment to write.
        [-f FILEPATH]   Upload the contents of a file as a comment.
        [FILEPATH]...   Optional files to upload alongside.

Split Submissions among TAs
^^^^^^^^^^^^^^^^^^^^^^^^^^^

See `the groups contrib <https://github.com/DIKU-EDU/staffeli/tree/master/contrib/groups>`_.

Documentation
-------------

It is up-and-coming on
[ReadTheDocs](http://staffeli.readthedocs.io/en/latest/). The source files for
that page are under [docs/source](docs/source), and they are, much like this
README, written in `reStructured Text
<http://www.sphinx-doc.org/en/stable/rest.html>`_. It is also suggested to
follow the `Python documentation style guide
<https://docs.python.org/devguide/documenting.html#style-guide>`_.

Contributing
------------

First, take a look at our `design guide <DESIGN.md>`__ and `style
guide <STYLE.md>`__.

Contact us at dikunix at dikumail dot dk.

Take a look at our on-going
`issues <https://github.com/DIKU-EDU/Staffeli/issues>`__.

Testing
-------

Currently, `Travis CI <https://travis-ci.org/DIKU-EDU/staffeli>`__ will
only check that you roughly conform to the `PEP 8 Python Style
Guide <https://www.python.org/dev/peps/pep-0008/>`__ (using
`flake8 <http://flake8.pycqa.org/>`__), and perform static type-checking
with `mypy <http://mypy-lang.org/>`__, all only for selected Python
files in this repository. See (and run?)
```static_tests.py`` <static_tests.py>`__ for further details.

Before you do that however, you might want to do this:

::

    $ pip3 install -r test-requirements.txt

This will also install what you need to run the dynamic tests we have in
store under `tests <tests>`__, except for **Docker**:
```start_local_canvas.py`` <start_local_canvas.py>`__ will fire up a
Docker image with a local Canvas instance for use with our
`tests <tests>`__. You will also find it in your browser under the
address ``localhost:3000``. The user is ``canvas@example.edu`` and the
password is ``canvas``.

The static and dynamic tests are also part of the
```pre-commit`` <hooks/pre-commit>`__ and
```pre-push`` <hooks/pre-push>`__ hooks, respectively. Install these
hooks by executing ```hooks/install.sh`` <hooks/install.sh>`__.
Unfortunately, neither these hooks, nor the hooks installer will work on
Windows.

Static Testing Framework
^^^^^^^^^^^^^^^^^^^^^^^^

We use `flake8 <http://flake8.pycqa.org/>`__ for style-checking and
`mypy <http://mypy-lang.org/>`__ for static type-checking.

Assuming you have these tools installed, you can do this:

::

    $ ./static_tests.py

This is also part of the ```pre-commit`` <hooks/pre-commit>`__ hook.

Dynamic Testing Framework
^^^^^^^^^^^^^^^^^^^^^^^^^

We use `pytest <https://docs.pytest.org/>`__ together with
`hypothesis <https://hypothesis.readthedocs.io/>`__.

Assuming you have these tools installed, you can do this:

::

    $ pytest

This is also part of the `pre-push <hooks/pre-push>`__ hook.

Dynamic Test Coverage
^^^^^^^^^^^^^^^^^^^^^

Run ``pytest`` with the option ``--cov=staffeli`` to get an idea of the
test coverage of Staffeli proper.

It is pretty lousy ATM. As of `2017-05-24
<https://github.com/DIKU-EDU/staffeli/commit/e5a0811edf26dc70eaad680e54b5763fc64f90fc>`__,
the numbers were:

::

    Name                          Stmts   Miss  Cover
    -------------------------------------------------
    staffeli/assignment.py           28     28     0%
    staffeli/cachable.py             22     13    41%
    staffeli/canvas.py              326    326     0%
    staffeli/cli.py                 295    295     0%
    staffeli/course.py               43      4    91%
    staffeli/files.py                57     41    28%
    staffeli/gcat.py                 29      5    83%
    staffeli/group.py                23     23     0%
    staffeli/listed.py               31      8    74%
    staffeli/names.py                 3      1    67%
    staffeli/resubmissions.py       121    121     0%
    staffeli/speedgrader_url.py       9      9     0%
    staffeli/submission.py           22     22     0%
    staffeli/typed_canvas.py        112      4    96%
    staffeli/upload.py               17     17     0%
    -------------------------------------------------
    TOTAL                          1138    917    19%
