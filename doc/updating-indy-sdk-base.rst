Updating the indy-sdk Base Library in ``von_base`` and ``von_anchor``
=====================================================================

This engineering document describes the process of updating the indy-sdk base library within the ``von_base`` distribution for the ``von_anchor`` library.

Build ``libindy.so`` Shared Library
-----------------------------------

At the bash prompt, issue:

.. code-block:: bash

    $ cd
    $ git clone https://github.com/hyperledger/indy-sdk
    $ cp indy-sdk/ci/indy-pool.dockerfile ~/von_base/files
    $ cd indy-sdk/libindy

If rust is already on the host, issue:

.. code-block:: bash

    $ rustup update

otherwise issue:

.. code-block:: bash

    $ curl https://sh.rustup.rs -sSf | sh

and continue:

.. code-block:: bash

    $ sudo apt install cargo
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

Issue at the prompt:

.. code-block:: bash

    $ cd ~/von_anchor/test
    $ pipenv run pytest -s test_pool.py
    $ pipenv run pytest -s test_wallet.py
    $ pipenv run pytest -s test_anchors.py
    ...

for all current unit tests and check the results. If the indy-sdk deltas drive new behaviour, new test code may be necessary.

Update and Release ``von_anchor`` if Necessary
----------------------------------------------

Consult ``packaging.rst`` in this directory for directions on building and uploading the VON Anchor package.
