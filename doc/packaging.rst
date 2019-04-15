Releasing VON Packages
=====================================================================

This engineering document describes the process packaging and releasing VON Anchor and VON Tails.

Prerequisites
++++++++++++++++++++

Export Environment Variables
----------------------------

For all operations, ensure that environment variables have correct values; issue::

    $ export TEST_POOL_IP=10.0.0.2
    $ export RUST_LOG=error
    $ export PIPENV_MAX_DEPTH=16

at the prompt or adjust ``$HOME/.profile`` accordingly and source it.

Install and Run pylint if Necessary
-----------------------------------

To run pylint to check for newly introduced poor code, issue::

    $ pipenv install pylint

at the prompt and edit ``$HOME/.pylintrc.txt`` to taste. The project uses 120 character lines in source code, and variables ``rc``, ``rv`` throughout for return codes and return values, among other house rules.

Issue::

    $ pipenv run pylint <src.py>

at the prompt to check file ``<src.py>``.

Install twine if Necessary
--------------------------

If not yet present in the pipenv virtual environment, issue::

    $ pipenv install twine

at the prompt to install twine.

Configure pypi Interaction
--------------------------

Be sure that file ``$HOME/.pypirc`` is up to date::

    [distutils]
    index-servers=
        pypi

    [pypi]
    username=sri-von
    password=Apple1995!

Install sphinx if Necessary
---------------------------

If not yet present in the pipenv virtual environment, issue::

    $ pipenv install sphinx
    $ pipenv install sphinx_rtd_theme

at the prompt to install sphinx.

VON Anchor
++++++++++

This section outlines the process to test and release ``von_anchor``.

Set Version
...........

Set the current version number as the only line in file ``VERSION.txt``.

A version number takes the form ``<x.y.z>``. The ``<x.y>`` major and minor revisions should match the underlying indy-sdk version that its underlying master revision anticipates; e.g., 1.6.z would correspond to indy-sdk 1.6.* release.

Test ``von_anchor``
---------------------------

Issue at the prompt::

    $ cd $HOME/von_anchor/test
    $ pipenv run pytest -s test_pool.py
    $ pipenv run pytest -s test_wallet.py
    $ pipenv run pytest -s test_anchors.py
    ...

for all current unit tests and check the results.

Update sphinx Documentation
----------------------------

The following subsections outline the update operation for sphinx documentation.

Inspect Document Content
........................

To create a clean target directory and build documentation into it, issue::

    $ rm -rf $HOME/_sandbox/sphinx
    $ mkdir -p $HOME/_sandbox/sphinx
    $ cd $HOME/von_anchor/docs/source
    $ pipenv run sphinx-build . $HOME/_sandbox/sphinx

at the prompt and inspect the content starting from ``$HOME/_sandbox/sphinx/index.html`` in a browser.

Push Changes
-----------------------------------------------------

If there are any code changes required in the ``von_anchor`` source code, update the version as above and enter::

    $ cd $HOME/von_anchor
    $ rm -rf dist
    $ rm -rf von_anchor.egg-info
    $ pipenv run python setup.py sdist
    $ pipenv run twine upload dist/von_anchor-<x.y.z>.tar.gz

to re-release the package.

If there are any changes required in the ``von_anchor`` source code, update the version in ``setup.py`` in the ``von_anchor`` installation directory. Use git to commit and push any code changes::

    $ cd $HOME/von_anchor
    $ git add .
    $ git commit -m'create a lucid comment'
    $ git push

and then tag the release as per ``tagging.rst`` in this directory.

VON Tails
++++++++++

This section outlines the process to test and release ``von_tails``.

Test ``von_tails``
---------------------------

Consult https://von-tails.readthedocs.io/en/latest/von_tails/installation.html to install and validate the operation of the VON Tails server.

Update sphinx Documentation
----------------------------

The following subsections outline the update operation for sphinx documentation.

Set Version in sphinx Document
...............................

Adjust the ``version`` and ``release`` values in ``docs/source/conf.py`` within the ``von_tails`` installation directory: they must match the tag marking the correct ``von_anchor`` version.

Inspect Document Content
........................

To create a clean target directory and build documentation into it, issue::

    $ rm -rf $HOME/_sandbox/sphinx
    $ mkdir -p $HOME/_sandbox/sphinx
    $ cd $HOME/von_tails/docs/source
    $ pipenv run sphinx-build . $HOME/_sandbox/sphinx

at the prompt and inspect the content starting from ``$HOME/_sandbox/sphinx/index.html`` in a browser.

Push Changes
-----------------

Use git to commit and push any resulting code changes::

    $ cd $HOME/von_tails
    $ git add .
    $ git commit -m'create a lucid comment'
    $ git push

Tag the release as per ``tagging.rst`` in this directory.
