Updating the indy-sdk Base Library in ``von_base`` and ``von_agent``
====================================================================
This engineering document describes the process of updating the indy-sdk base library within the ``von_base`` distribution for the ``von_agent`` library.

Build ``libindy.so`` Shared Library
-----------------------------------
At the bash prompt, issue::

  $ cd
  $ git clone https://github.com/hyperledger/indy-sdk
  $ cp indy-sdk/ci/indy-pool.dockerfile ~/von_base/files
  $ cd indy-sdk/libindy
  $ cargo build
  $ cp target/debug/libindy.so ~/von_base/files
  $ cd ~/von_base/files
  $ sudo chown root:root libindy.so
  $ tar -cpvzf libindy.so.tgz libindy.so
  $ sudo mv libindy.so /usr/lib

Use git to commit and push any changes.

Update Virtual Environment
--------------------------
Visit ``https://pypi.org/python3-indy``, ascertain the current version (e.g., ``python3-indy-1.4.0-dev-527``).

Edit ``~/von_agent/requirements.txt`` and set the current version; e.g.,``python3-indy==1.4.0-dev-527``

Then issue::

  $ export PIPENV_MAX_DEPTH=16
  $ export RUST_LOG=error
  $ export TEST_POOL_IP=10.0.0.2
  $ cd ~/von_agent
  $ pipenv install -r requirements.txt

and use git to commit and push the delta to the ``requirements.txt`` file.

Test and Fix ``von_agent``
--------------------------
Issue::

  $ cd ~/von_agent/test
  $ pipenv run pytest -s test_pool.py
  $ pipenv run pytest -s test_wallet.py
  $ pipenv run pytest -s test_agents.py
  ...

for all current unit tests and check the results. If the indy-sdk deltas drive new behaviour, new test code may be necessary.

Update ``von_agent`` if Necessary
---------------------------------
If there are any code changes required in the ``von_agent`` source code, update the version in ``setup.py`` and re-release::

  $ cd ~/von_agent
  $ rm -rf dist
  $ rm -rf von_agent.egg-info
  $ pipenv run python setup.py sdist
  $ pipenv install twine
  $ pipenv run twine upload dist/von_agent-<x.y.z>.tar.gz

where ``<x.y.z>`` represents the new version number; be sure that file ``~/.pypirc`` is up to date::

  [distutils]
  index-servers=
      pypi

  [pypi]
  username=sri-von
  password=Apple1995!

Use git to commit and push any resulting code changes in the VON connector project source or test code bases.

