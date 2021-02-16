---
title: "Writing Better Python Code Automatically"
excerpt: "Linting and other tools to improve your python code automatically with git."
header:
  teaser: /assets/img/git_logo.png
  image: /assets/img/git_python_workflow_header.png
tags:
  - python
  - lint
  - git
  - continuous improvement
classes:
  - wide
---

{% include toc icon="gears" title="Table of Contents" %}

This is a guide to setting up git repos for python code that take advantage of as many automatic code linting, testing, and continuous improvement tools (CI) as I have found useful. There are certainly more.  I work in a linux-based environment, and the following was tested on Ubuntu 18.04 LTS assuming you have admin privileges.

## Repository configuration

Most repos are built as packages and therefore should take advantage of python's use of `__init__.py` files to indicate structure.  This makes writing documentation, tests, and use of the code itself (importing it externally) rational and readable.  Python includes a nice discussion about this [here](https://docs.python.org/3/tutorial/modules.html) with examples on how to configure your code as a package.  To be brief, here is the basic layout by example:

~~~
my_module/
	__init__.py
	subpackage_1/
		__init__.py
		a.py
		b.py
	subpackage_2/
		__init__.py
		c.py
	tests/
		__init__.py
	doc/
		__init__.py
~~~

The `my_module/__init__.py` file should look like this so that when a used imports my_module they can access the subpackages via "dots", like `my_module.subpackage_1`:

~~~
"""
Load all modules.

@author: username
"""
__all__ = ["subpackage_1", "subpackage_2"]
from . import subpackage_1, subpackage_2
~~~

The `my_module/subpackage_1/__init__.py` file is similar:

~~~
"""
Load all modules.

@author: nam
"""
__all__ = ["a", "b]
~~~

The `__init__.py` files in tests/ and doc/ are generally empty, but should contain a docstring header (as above) to be consistent with the pre-commit documentation linters used below.  

A user can now import is the standard way:

~~~ python
>>> import my_module
>>> from my_module.subpackage_1.a import some_function
~~~

Even if a "subpackage" is really just your source code, this keeps your tests (which you should always write) conveniently separated.
Perhaps most importantly, **this structure ensures that it is easy to automatically run those tests,** as will be discussed more below.

## Documentation

Writing documentation is often neglected, however, it is critically important to ensuring proper use for future reproducibility; even bad documentation is better than none.  In the end, this **always** saves time.  In python there are many different styles and levels of formality to create documentation, but in practice docstrings are the most helpful.  They are simple and easy to write as you go, and are what most programmers and users practically interact with. These can be accessed via the `help(my_function)` function or `?my_function` in a Jupyer notebook. The pre-commit tool listed below has docstring linters to help automatically format these docstrings.  Datacamp has an excellent and more elaborate [tutorial](https://www.datacamp.com/community/tutorials/documenting-python-code) on how to write them.  In practice, a basic dosctring format I often use looks like (numpy style):

~~~ python
def my_function(a, b):
	"""
	A one-line description of what my_function does this in imperative case.

	More detailed descrition which can span
	multiple lines and have helpful information.

	Notes
	-----
	Anything else pertaining to tricks, tips, etc.

	Parameters
	----------
	a : type
		What is a?
	b : type
		What is b?

	Returns
	-------
	result : type
		What is being returned?
	"""
	...
~~~

There are other ways to format these; see the [Google style guide](https://google.github.io/styleguide/pyguide.html) for example, and this [tutorial](https://www.datacamp.com/community/tutorials/docstrings-python) for a comparison between Sphinx vs. Google styles.  

When you are creating a code for public release and want it to include more formal examples, illustrations, discussions, etc. I usually use [Sphinx](https://www.sphinx-doc.org/en/master/) which relies on reStructured Text.  This is the most configurable, but takes a bit of practice.  Their website contrains greate tutorials and examples.  For most codes and projects, this is not necessary; extensive docstrings, a good README file and some examples are usually enough for anything personal or intended for use by a user group with specialty knowledge of the code.  

Regardless, at a bare minimum all functions, classes, and files should have docstrings throughout.  The pydoc linters are helpful to ensure this.

Also worth mentioning is [pdoc](https://pdoc3.github.io/pdoc/0) is based on markdown instead of reST and is more lightweight which can be a valuable intermediate between docstrings (exclusively) and Sphinx.

## pre-commit

[![pre-commit](https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit&logoColor=white)](https://github.com/pre-commit/pre-commit)

### Installation
The first, and principal automatic tool you should use is [pre-commit](https://pre-commit.com) which is a framework for managing your code by using hooks to catch simple issues before submission to code review.  This largely handles stylistic issues to standardize the code and catch simple mistakes.  Installation is simple.

~~~ bash
$ pip install pre-commit
~~~

Alternatively, in a specific conda environment:

~~~ bash
$ conda activate myenv
$ conda install -c conda-forge pre-commit
~~~

### Usage
The basic procedure, following installation, is to:
1. Specify pre-commit as a requirement (add pre-commit to requirements.txt)
2. Add a pre-commit configuration file to your repo (.pre-commit-config.yaml)
3. Install the hook scripts
4. (optional) Run against all files when adding new hooks - usually pre-commit only runs on the changed files.

A sample configuration file can be generated manually, or by starting from the built in example which defaults to python formatters (at least at the time of writing).

~~~ bash
$ cd path/to/repo
$ pre-commit sample-config > .pre-commit-config.yaml
$ git add .pre-commit-config.yaml
$ pre-commit install
$ pre-commit run --all-files
~~~

### Example
At the top level you can add things like file include/exclude patterns and default language version. See [documentation](https://pre-commit.com/#plugins).  An example file might look like this:

~~~ bash
$ cat .pre-commit-config.yaml
---
---
fail_fast: false
default_language_version:
    python: python3.7
repos:
-   repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v3.4.0
    hooks:
    -   id: trailing-whitespace
    -   id: end-of-file-fixer
    -   id: check-yaml
    -   id: check-added-large-files
    -   id: check-ast
    -   id: check-docstring-first
    -   id: check-merge-conflict
    -   id: check-symlinks
    -   id: check-yaml
    -   id: debug-statements
    -   id: requirements-txt-fixer

-   repo: https://github.com/pycqa/isort
    rev: 5.7.0
    hooks:
    -   id: isort
        args: ["--profile", "black", "--filter-files"]
        language: python
        types: [python]
-   repo: https://gitlab.com/pycqa/flake8
    rev: 3.8.4
    hooks:
    -   id: flake8
        args: ["--max-line-length", "90"]
        language: python
        types: [python]
-   repo: https://github.com/psf/black
    rev: 20.8b1
    hooks:
    -   id: black
        args: ["--line-length", "80"]
        language: python
        types: [python]
-   repo: https://github.com/pycqa/bandit
    rev: 1.7.0
    hooks:
    -   id: bandit
        args: ["-ll", "--ini", "setup.cfg", "--recursive"]
        language: python
        types: [python]
-   repo: https://github.com/pre-commit/pygrep-hooks
    rev: v1.7.0
    hooks:
    -   id: python-use-type-annotations
-   repo: https://github.com/adrienverge/yamllint
    rev: v1.25.0
    hooks:
    -   id: yamllint
        args: ["-d", "{ignore: .pre-commit-config.yaml}"]
        language: python
        types: [yaml]
-   repo: https://github.com/pycqa/pydocstyle
    rev: 5.1.1
    hooks:
    -   id: pydocstyle
        language: python
        types: [python]
-   repo: https://github.com/asottile/blacken-docs
    rev: v1.9.1
    hooks:
    -   id: blacken-docs
        language: python
        types: [python]
~~~

The repos indicate which repo to git clone from, the rev specifies the revision/tag to clone at, and the hooks are mappings that configure which hook from the repository is used and can be customized. The "args" key indicates which command line arguments, if any, to send to the specific linter. Black, for example, defaults to try to reach a max line length of 88, not 80 as stipulated by flake8.  As a result, we specify that black should try to reach 80, though we allow flake8 to tolerate up to 90 if it is not successful. Optional keys default to the settings of that specific repo's configuration so refer to each for details. A list of supported hooks can be found on their [website](https://pre-commit.com/hooks.html).  Check this over to see which ones make sense for your project.

Hooks can be automatically updated by running:

~~~ bash
$ pre-commit autoupdate
~~~

You can run all the pre-commit hooks, or individual ones based on their id:

~~~ bash
$ pre-commit run --all-files
$ pre-commit run <hook_id>
~~~

If you need to skip a hook during a commit, you specify a comma separated list of hook ids as an environment variable, SKIP.

~~~ bash
$ SKIP=flake8 git commit -m "foo"
~~~

Otherwise, simply running `git commit` triggers these checks, many of which automatically fix your code. There are many specific configurations and changes you can make.  Refer to the documentation for examples of these and more details. The example pre-commit configuration file above includes hooks to additional repos, links to some are provided below and more details can be found therein.

* [![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black) - uncompromising code formatting tools to standardize your code.
* [![Imports: isort](https://img.shields.io/badge/%20imports-isort-%231674b1?style=flat&labelColor=ef8336)](https://pycqa.github.io/isort/) - sorts your "import" statements.
* [Flake8](https://github.com/PyCQA/flake8) - style guide enforcement, see this [blog post]((https://medium.com/python-pandemonium/what-is-flake8-and-why-we-should-use-it-b89bd78073f2)) for more details.
* [mypy](https://mypy.readthedocs.io/en/stable/) - static type checker.

## Coverage and Unittests
Now that the code is documented and automatically checked for style and other salient issues, we need to turn to more custom code tests to ensure correct operation.  Python comes with the [unittest](https://docs.python.org/3/library/unittest.html) framework built in, and because of this I almost always use this.  Still, there are a lot of other options including:
* [hypothesis](https://hypothesis.readthedocs.io/en/latest/)
* [doctest](https://docs.python.org/3/library/doctest.html) - part of standard library.
* [py.test](https://docs.pytest.org/en/stable/)
* [mock](https://docs.python.org/3/library/unittest.mock.html) - part of standard library  as of Python 3.3
* [nose](https://nose.readthedocs.io/en/latest/)

Hypothesis is particularly neat, but for most things I still just use unittests.  These can be stored in the tests/ directory if you lay your package out as discussed above.  In tests/ files should follow the pattern `test_*.py` where the asterisk is filled with whatever function, class, file, etc. you want to write tests about.  Here is an example file:

~~~
"""
Unittests for MyClass.

author: username
"""
import unittest

from my_module.subpackage_1.a import MyClass


class TestMyClass(unittest.TestCase):
    """Test MyClass class."""

    def test_sklearn_compatibility(self):
        """Check compatible with sklearn's estimator API."""
        from sklearn.utils.estimator_checks import check_estimator

        try:
            check_estimator(MyClass())
        except Exception as e:
            error = str(e)
        else:
            error = None
        self.assertIsNone(error, msg=error)

    def test_another_thing(self):
        ...
~~~

There are many different [assert methods](https://docs.python.org/3/library/unittest.html#assert-methods), and you can have certain functions run once at setUp as well.  Refer to Python's documentation for more details and [here](https://www.tutorialspoint.com/unittest_framework/unittest_framework_assertion.htm) for an abbreviated summary.  

These tests (all files) can be all discovered and run automatically from the [command line](https://stackoverflow.com/questions/1732438/how-do-i-run-all-python-unit-tests-in-a-directory), assuming you have placed your test scripts in tests/ which includes an `__init__.py` file; from the my_module/ level you can simply call:

~~~ bash
$ python -m unittest discover tests/
~~~

See [here](https://www.patricksoftwareblog.com/python-unit-testing-structuring-your-project/) for another blog post about structuring your module for convenient testing.  Unittests need to be written in the form: set up something, execute, test output against expectation.  However, this doesn't always help if you haven't thought of a way the code could fail or be used incorrectly; tests like [hypothesis](https://hypothesis.readthedocs.io/en/latest/) are geared at trying to solve this.  Another way to help is to use [coverage](https://github.com/nedbat/coveragepy) to estimate how much of you code is executed (and therefore tested) when you run these tests.  Conveniently, you can just change the syntax above a little bit after installing the code.

~~~ bash
$ pip install coverage
$ coverage run -m unittest discover tests/
$ coverage report
~~~

Refer to their documentation for further customization.  Note that you can add a badge to your repo by using [coverage-badge](https://pypi.org/project/coverage-badge/).

~~~ bash
$ pip install coverage-badge
$ coverage run -m unittest discover tests/ # Stores .coverage folder with results
$ coverage-badge -o coverage.svg # Must be run where .coverage is
~~~

Simply add [![](coverage.svg)]() to your README.md file and it will render.

## Travis CI

[Continuous integration](https://en.wikipedia.org/wiki/Continuous_integration) is the practice of merging developers' contributions over time into a single main copy.  This is easy nowadays with [git](https://git-scm.com/), and I regularly use [github](https://www.github.com) and [gitlab](https://www.gitlab.com); gitlab has CI built in, but free CI tools are available for github also.  Perhaps the most common is [Travis CI](https://travis-ci.com/).  To use this service, sign up for a free account and log in.  They offer a nice [tutorial](https://docs.travis-ci.com/user/tutorial/#to-get-started-with-travis-ci-using-github) on getting started with github which you can refer to for more details.  Basically, from their dashboard, activate repository access as you deem fit, following their instructions.  Next you need to add a CI configuration file, which is just a yaml file in the root of your repo.  A standard `.travis.yml` example for python is below:

~~~
language: python
python:
  - 3.7
branches:
  only:
  - main
before_install:
  - python --version
  - pip install -U pip
  - pip install -r requirements.txt
  - pip install codecov
env:
  - CODECOV_TOKEN="asdfghjklqwertyuiopzxcvbnm"
script: python -m unittest discover tests/
after_success:
  - codecov
~~~

Simply insert your token above to automatically submit reports from Travis-CI builds to the free service [codecov.io](https://codecov.io); this takes the results of your tests and generates a dynamic badge so you don't have to run the steps manually (previous section).  Create an account on codecov.io and link your repository.  Go to Settings > Badge to find code to put in your README.md file to display the result of these tests. You can do the same for the Travis-CI status if you click on the "build status" badge on your repo's dashboard there - see [here](https://docs.travis-ci.com/user/status-images/).

## Summary

Your code should now be:
* well organized
* documented
* easy to test and write tests for
* linted upon git commit for code compliance
* tested upon git push with CI


all within a relatively automatic fashion.  Other tools which can be helpful for code writing, debugging, etc. include:
* [git kraken](https://www.gitkraken.com/)
* [spyder](https://www.spyder-ide.org/)
