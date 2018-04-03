# Updating the indy-sdk Base Library in von_base and von_agent
This engineering document describes the process of updating the indy-sdk base library within the `von_base` distribution for the `von_agent` library.

## Build libindy.so Shared Library
At the bash prompt, issue:
```
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
```
Use git to commit and push any changes.

## Update Virtual Environment
Visit `https://pypi.python.org/pypi/python3-indy`, ascertain the current version (e.g., python3-indy-1.3.1-dev-408)
Edit `~/von_agent/requirements.txt` and set the current version; e.g.,
```
python3-indy==1.3.1-dev-408
```

Then issue:
```
$ export PIPENV_MAX_DEPTH=16
$ export RUST_LOG=error
$ export TEST_POOL_IP=10.0.0.2
$ cd ~/von_agent
$ pipenv install -r requirements.txt
```
and use git to commit and push the delta to the `requirements.txt` file.

## Test and Fix von_agent
Issue:
```
$ cd ~/von_agent/test
$ pipenv run pytest -s test_pool.py
$ pipenv run pytest -s test_wallet.py
...
$ pipenv run pytest -s test_agents.py
```
for all current unit tests and check the results. If the indy-sdk deltas drive new behaviour, new test code may be necessary.

## Update von_agent and von_connector if Necessary
If there are any code changes required in the `von_agent` source code, update the version in `~/von_agent/setup.py` and re-release:
```
$ cd ~/von_agent
$ rm -rf dist
$ rm -rf von_agent.egg-info
$ pipenv run python setup.py sdist
$ pipenv install twine
$ pipenv run twine upload dist/von_agent-<x.y.z>.tar.gz
```
where `<x.y.z>` represents the new version number; be sure that file `~/.pypirc` is up to date:
```
[distutils]
index-servers=
    pypi

[pypi]
username=sri-von
password=Apple1995!
```

Reflect the new version number in `~/von_connector/service_wrapper_project/requirements.txt`, plus both `~von_conx/src/requirements.txt` and `~von_conx/src/test/requirements.txt`. Then at the prompt, issue
```
$ cd ~/von_connector
$ pipenv install -r service_wrapper_project/requirements.txt
$ cd ~/von_conx
$ pipenv install -r src/test/requirements.txt
```
and use git to commit, then push, the delta to the `requirements.txt` files of the VON connector projects.

If the indy-sdk delta drives any changes to the high-level von_agent APIs or the VON protocol, update the code bases of the VON connector projects (`~/von_connector/service_wrapper_project/wrapper_api/*`, `~/von_connector/service_wrapper_project/wrapper_api/test/test_wrapper.py, `~/von_conx/src/app/`, and `~/von_conx/src/test/test_wrapper.py`; at the prompt, issue the following to test:
```
$ cd ~/von_connector/service_wrapper_project/wrapper_api/test
$ pipenv run pytest -s test_wrapper.py
$ cd ~/von_conx/docker
$ ./manage stop
$ ./manage rm
$ ./manage build
$ ./manage bg
$ cd ~/von_conx/src/test
$ pipenv run pytest -s test_wrapper.py
```
(the operator may omit `./manage bg` above to have the test suite start docker containers in-band).

Use git to commit and push any resulting code changes in the VON connector project source or test code bases.
