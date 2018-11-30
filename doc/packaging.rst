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

Install sphinx if Necessary
---------------------------

If not yet present in the pipenv virtual environment, issue::

    $ pipenv install sphinx
    $ pipenv install sphinx_rtd_theme

at the prompt to install sphinx.

VON Anchor
++++++++++

This section outlines the process to test and release ``von_anchor``.

Test ``von_anchor``
---------------------------

Issue at the prompt::

    $ cd ~/von_anchor/test
    $ pipenv run pytest -s test_pool.py
    $ pipenv run pytest -s test_wallet.py
    $ pipenv run pytest -s test_anchors.py
    ...

for all current unit tests and check the results.

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

Build Sphinx Generation If Necessary
--------------------------------------------------

To create a clean target directory and build documentation into it, issue::

    $ rm -rf $HOME/_sandbox/sphinx
    $ mkdir -p $HOME/_sandbox/sphinx
    $ cd $HOME/von_anchor/docs/source
    $ pipenv run sphinx-build . $HOME/_sandbox/sphinx

at the prompt and inspect the content starting from ``$HOME/_sandbox/sphinx/index.html`` in a browser.

Use git to commit and push any resulting code changes. Tag the release as per ``tagging.rst`` in this directory.

Update, Build, and Upload ``von_anchor`` if Necessary
-----------------------------------------------------

If there are any code changes required in the ``von_anchor`` source code, update the version in ``setup.py`` and re-release::

    $ cd ~/von_anchor
    $ rm -rf dist
    $ rm -rf von_anchor.egg-info
    $ pipenv run python setup.py sdist
    $ pipenv run twine upload dist/von_anchor-<x.y.z>.tar.gz

where ``<x.y.z>`` represents the new version number. The ``<x.y>`` major and minor revisions should match the underlying indy-sdk version that its underlying master revision anticipates; e.g., 1.6.z would correspond to indy-sdk 1.6.* release. Be sure that file ``$HOME/.pypirc`` is up to date::

    [distutils]
    index-servers=
        pypi

    [pypi]
    username=sri-von
    password=Apple1995!

Use git to commit and push any resulting code changes. Tag the release as per ``tagging.rst`` in this directory.

VON Tails
++++++++++

This section outlines the process to test and release ``von_tails``.

Test ``von_tails``
---------------------------

Consult https://von-tails.readthedocs.io/en/latest/von_tails/installation.html to install and validate the operation of the VON Tails server.

Build Sphinx Documentation If Necessary
--------------------------------------------------

To create a clean target directory and build documentation into it, issue::

    $ rm -rf $HOME/_sandbox/sphinx
    $ mkdir -p $HOME/_sandbox/sphinx
    $ cd $HOME/von_tails/docs/source
    $ pipenv run sphinx-build . $HOME/_sandbox/sphinx

at the prompt and inspect the content starting from ``$HOME/_sandbox/sphinx/index.html`` in a browser.

Use git to commit and push any resulting code changes. Tag the release as per ``tagging.rst`` in this directory.
