Releasing VON Packages
=====================================================================

This engineering document describes the process packaging and releasing VON Anchor and VON Tails.

VON Anchor
++++++++++

This section outlines the process to test and release ``von_anchor``.

Export Environment Variables
----------------------------

For all operations, ensure that environment variables have correct values; issue

.. code-block:: bash

    $ export TEST_POOL_IP=10.0.0.2
    $ export RUST_LOG=error
    $ export PIPENV_MAX_DEPTH=16

at the prompt or adjust ``$HOME/.profile`` accordingly and source it.

Test ``von_anchor``
---------------------------

Issue at the prompt:

.. code-block:: bash

    $ cd ~/von_anchor/test
    $ pipenv run pytest -s test_pool.py
    $ pipenv run pytest -s test_wallet.py
    $ pipenv run pytest -s test_anchors.py
    ...

for all current unit tests and check the results.

Install twine if Necessary
--------------------------

If not yet present in the pipenv virtual environment, issue

.. code-block:: bash

    $ pipenv install twine

at the prompt to install twine.

Test Sphinx Documentation Generation If Necessary
-------------------------------------------------

If not yet present in the pipenv virtual environment, issue

.. code-block:: bash

    $ pipenv install sphinx
    $ pipenv install sphinx_rtd_theme

at the prompt to install sphinx.

To create a clean target directory and build documentation into it, issue

.. code-block:: bash

    $ rm -rf $HOME/_sandbox/sphinx
    $ mkdir -p $HOME/_sandbox/sphinx
    $ cd $HOME/von_anchor/docs/source
    $ pipenv run sphinx-build . $HOME/_sandbox/sphinx

at the prompt and inspect the content starting from ``$HOME/_sandbox/sphinx/index.html`` in a browser.

Update, Build, and Upload ``von_anchor`` if Necessary
-----------------------------------------------------

If there are any code changes required in the ``von_anchor`` source code, update the version in ``setup.py`` and re-release:

.. code-block:: bash

    $ cd ~/von_anchor
    $ rm -rf dist
    $ rm -rf von_anchor.egg-info
    $ pipenv run python setup.py sdist
    $ pipenv run twine upload dist/von_anchor-<x.y.z>.tar.gz

where ``<x.y.z>`` represents the new version number. The ``<x.y>`` major and minor revisions should match the underlying indy-sdk version that its underlying master revision anticipates; e.g., 1.6.z would correspond to indy-sdk 1.6.* release. Be sure that file ``$HOME/.pypirc`` is up to date:

.. code-block::

    [distutils]
    index-servers=
        pypi

    [pypi]
    username=sri-von
    password=Apple1995!

Use git to commit and push any resulting code changes. Tag the release as per ``tagging.rst`` in this directory.










Get twine
-----------------------------------

At the bash prompt, issue:

.. code-block:: bash

    $ export PIPENV_MAX_DEPTH=16
    $ pipenv install twine

reate 

Update Virtual Environment
--------------------------

Visit ``https://pypi.org/python3-indy``, ascertain the current version (e.g., ``python3-indy-1.4.0-dev-527``).

Edit ``~/von_anchor/requirements.txt`` and set the current version; e.g.,``python3-indy==1.4.0-dev-527``

Then issue:

.. code-block:: bash

    $ export PIPENV_MAX_DEPTH=16
    $ export RUST_LOG=error
    $ export TEST_POOL_IP=10.0.0.2
    $ cd ~/von_anchor
    $ pipenv install -r requirements.txt

and use git to commit and push the delta to the ``requirements.txt`` file.

Test and Fix ``von_anchor``
---------------------------

Issue:

.. code-block:: bash

    $ cd ~/von_anchor/test
    $ pipenv run pytest -s test_pool.py
    $ pipenv run pytest -s test_wallet.py
    $ pipenv run pytest -s test_anchors.py
    ...

for all current unit tests and check the results. If the indy-sdk deltas drive new behaviour, new test code may be necessary.

Update ``von_anchor`` if Necessary
----------------------------------

If there are any code changes required in the ``von_anchor`` source code, update the version in ``setup.py`` and re-release:

.. code-block:: bash

    $ cd ~/von_anchor
    $ rm -rf dist
    $ rm -rf von_anchor.egg-info
    $ pipenv run python setup.py sdist
    $ pipenv install twine
    $ pipenv run twine upload dist/von_anchor-<x.y.z>.tar.gz

where ``<x.y.z>`` represents the new version number. The ``<x.y>`` major and minor revisions should match the indy-sdk version that its underlying master revision anticipates; e.g., 1.6.z would correspond to indy-sdk 1.5.0-dev-nnn, converging toward an indy-sdk 1.6 release. Be sure that file ``~/.pypirc`` is up to date:

.. code-block::

    [distutils]
    index-servers=
        pypi

    [pypi]
    username=sri-von
    password=Apple1995!

Use git to commit and push any resulting code changes.

