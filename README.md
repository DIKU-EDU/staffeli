# `staffeli` — DIKU Support Tools for Canvas LMS

These tools leverage the [Canvas LMS REST
API](https://canvas.instructure.com/doc/api/index.html) to create a more
pleasant environment for working with [Absalon](https://absalon.ku.dk/).

<p align="center"><img src ="logo.jpg" width="300" alt="An Easel" title="Image
license: CC0; Source:
https://pixabay.com/en/art-painting-modern-art-mural-1027828/"/></p>

<p align="center">"Staffeli" is Danish for "easel" — a support frame for
holding up a canvas.</p>

[![Documentation Status](https://readthedocs.org/projects/staffeli/badge/)](http://staffeli.readthedocs.io/en/latest/)
[![Travis CI (Linux + macOS) Status](https://travis-ci.org/DIKU-EDU/staffeli.svg)](https://travis-ci.org/DIKU-EDU/staffeli)
[![License: EUPL v1.1](https://img.shields.io/badge/license-EUPL%20v1.1-blue.svg)](https://github.com/DIKU-EDU/Staffeli/blob/master/LICENSE.md)

## Purpose

The purpose of Staffeli is two-fold:

1. Leverage the [Canvas LMS REST
API](https://canvas.instructure.com/doc/api/index.html) to get things done
better, faster, stronger.
1. Quick prototyping of new features for [Canvas
LMS](https://www.canvaslms.com/).

Initially, Staffeli is _not_ intended for managing course content, merely to
snapshot course data (e.g. enrolled students, groups, sections, submissions),
_and_ to get grading done efficiently.

## Status

This is a somewhat coherent summoning of scripts developed during courses using
Canvas. Brace yourself. Lend a hand.

## Installation

1. Clone, or
   [download](https://github.com/DIKU-EDU/staffeli/archive/master.zip) this
   repository.
2. `pip3 install .`

## Getting Started

With Staffeli, we work with local course clones. We aim to keep these clones
compatible with git.

We recommend that you create a local directory `canvas`, `absalon`, or similar,
for all of you Canvas-related local course clones. Staffeli needs some initial
help to be able to login with your credentials. You need to [generate a
token](https://guides.instructure.com/m/4214/l/40399-how-do-i-obtain-an-api-access-token-for-an-account)
for Staffeli to use, and save it as `.token`, `token`, or `token.txt` in this
high-level directory.

**NB!** This is your personal token so **do not** share it with others, else
they can easily impersonate you using a tool like Staffeli. Unfortunately, to
the best of our knowledge, Canvas has no means to segregate or specialize
tokens, so this is really "all or nothing".

### Cloning a Course

To clone a course:

```
$ staffeli clone <course name>
```

The `<course name>` may be a substring of the course name as it appears on your
dashboard. Casing is not important. If there are multiple conflicting names, or
no matching course names, Staffeli will complain and let you try again.

### Fetch Submissions for a New Assignment

Use `staffeli fetch`. For instance, to fetch all submissions for "A3":

```
$ staffeli fetch subs/A3
```

To fetch just the metadata for all submissions, but not the submissions
themselves:

```
$ staffeli fetch subs
```

### Fetch Groups

This is a good idea to make sure you are up-to-date with canvas.

```
$ staffeli fetch groups
```

### Split According to Some Group Category

Assuming you already fetched groups and submissions,

```
$ staffeli groupsplit subs/A3 groups/Team.yml
```

### NB

If you are using git to share course internals, remember to push both changes
to [`groups`](groups), [`subs`](subs), and [`splits`](splits), when making
splits for some new assignment.

### Grade a Submission

Assuming you are in the submission directory, you can use `staffeli grade` to
grade the submission:

```
staffeli grade GRADE [-m COMMENT] [FILEPATH]...

Where
    GRADE           pass, fail, or an int.
    [-m COMMENT]    An optional comment to write.
    [FILEPATH]...   Optional files to upload alongside.
```

## Contributing

First, take a look at our [design guide](DESIGN.md) and [style
guide](STYLE.md).

Contact us at dikunix at dikumail dot dk.

Take a look at our on-going [issues](https://github.com/DIKU-EDU/Staffeli/issues).

# Testing

Currently, [Travis CI](https://travis-ci.org/DIKU-EDU/staffeli) will only check
that you roughly conform to the [PEP 8 Python Style
Guide](https://www.python.org/dev/peps/pep-0008/) (using
[flake8](http://flake8.pycqa.org/)), and perform static type-checking with
[mypy](http://mypy-lang.org/), all only for selected Python files in this
repository. See (and run?) [`static_tests.py`](static_tests.py) for further
details.

Before you do that however, you might want to do this:

```
$ pip3 install -r test-requirements.txt
```

This will also install what you need to run the dynamic tests we have in store
under [tests](tests), except for Docker.

[`start_local_canvas.py`](start_local_canvas.py) will fire up a Docker image
with a local Canvas instance for use with our [tests](tests). You will also
find it in your browser under the address `localhost:3000`. The user is
`canvas@example.edu` and the password is `canvas`.

## Static Testing Framework

We use [flake8](http://flake8.pycqa.org/) for style-checking and
[mypy](http://mypy-lang.org/) for static type checking.

Assuming you have these tools installed, you can do this:

```
$ ./static_tests.py
```

## Dynamic Testing Framework

We use [pytest](https://docs.pytest.org/) together with
[hypothesis](https://hypothesis.readthedocs.io/).

Assuming you have these tools installed, you can do this:

```
$ pytest
```

### Dynamic Test Coverage

Run `pytest` with the option `--cov=staffeli` to get an idea of the test
coverage of Staffeli proper. It is pretty lousy ATM. As of 2016-05-16, the
numbers were:

```
Name                          Stmts   Miss  Cover
-------------------------------------------------
staffeli/assignment.py           28     28     0%
staffeli/cachable.py             22     13    41%
staffeli/canvas.py              326    326     0%
staffeli/canvasTA-subs.py        58     58     0%
staffeli/cli.py                 295    295     0%
staffeli/course.py               37      4    89%
staffeli/files.py                57     41    28%
staffeli/listed.py               31     13    58%
staffeli/names.py                 3      1    67%
staffeli/resubmissions.py       121    121     0%
staffeli/speedgrader_url.py       9      9     0%
staffeli/submission.py           22     22     0%
staffeli/typed_canvas.py        102      4    96%
staffeli/upload.py               17     17     0%
-------------------------------------------------
TOTAL                          1128    952    16%
```

## Git Hooks

The static and dynamic tests are also part of the
[`pre-commit`](hooks/pre-commit) and [`pre-push`](hooks/pre-push) hooks,
respectively. Install these hooks by executing
[`hooks/install.sh`](hooks/install.sh). Unfortunately, neither these hooks, nor
the hooks installer will work on Windows.
