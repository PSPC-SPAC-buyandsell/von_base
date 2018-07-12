Updating the indy-sdk Base Library in ``von_base`` and ``von_anchor``
=====================================================================
This engineering document describes the process of updating the indy-sdk base library within the ``von_base`` distribution for the ``von_anchor`` library.

Build ``libindy.so`` Shared Library
-----------------------------------
At the bash prompt, issue::

  $ cd
  $ git clone https://github.com/hyperledger/indy-sdk
  $ cp indy-sdk/ci/indy-pool.dockerfile ~/von_base/files
  $ cd indy-sdk/libindy
  $ curl -sSf https://static.rust-lang.org/rustup.sh | sh
  $ cargo build --release
  $ cp target/debug/libindy.so ~/von_base/files
  $ cd ~/von_base/files
  $ sudo chown root:root libindy.so
  $ tar -cpvzf libindy.so.tgz libindy.so
  $ sudo mv libindy.so /usr/lib

Use git to commit and push any changes.

Update Virtual Environment
--------------------------
Visit ``https://pypi.org/python3-indy``, ascertain the current version (e.g., ``python3-indy-1.4.0-dev-527``).

Edit ``~/von_anchor/requirements.txt`` and set the current version; e.g.,``python3-indy==1.4.0-dev-527``

Then issue::

  $ export PIPENV_MAX_DEPTH=16
  $ export RUST_LOG=error
  $ export TEST_POOL_IP=10.0.0.2
  $ cd ~/von_anchor
  $ pipenv install -r requirements.txt

and use git to commit and push the delta to the ``requirements.txt`` file.

Test and Fix ``von_anchor``
---------------------------
Issue::

  $ cd ~/von_anchor/test
  $ pipenv run pytest -s test_pool.py
  $ pipenv run pytest -s test_wallet.py
  $ pipenv run pytest -s test_anchors.py
  ...

for all current unit tests and check the results. If the indy-sdk deltas drive new behaviour, new test code may be necessary.

Update ``von_anchor`` if Necessary
----------------------------------
If there are any code changes required in the ``von_anchor`` source code, update the version in ``setup.py`` and re-release::

  $ cd ~/von_anchor
  $ rm -rf dist
  $ rm -rf von_anchor.egg-info
  $ pipenv run python setup.py sdist
  $ pipenv install twine
  $ pipenv run twine upload dist/von_anchor-<x.y.z>.tar.gz

where ``<x.y.z>`` represents the new version number. The ``<x.y>`` major and minor revisions should match the indy-sdk version that its underlying master revision anticipates; e.g., 1.6.z would correspond to indy-sdk 1.5.0-dev-nnn, converging toward an indy-sdk 1.6 release. Be sure that file ``~/.pypirc`` is up to date::

  [distutils]
  index-servers=
      pypi

  [pypi]
  username=sri-von
  password=Apple1995!

Use git to commit and push any resulting code changes in the VON connector project source or test code bases.

