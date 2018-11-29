VON Base
========

This repository prepares an Ubuntu 16+ VM for the ``von_anchor`` component, which enables VON components.

To install it, clone it from github, then issue::

.. code-block:: bash

  $ cd von_base
  $ sudo ./setup

at the command prompt. The operation will:

- set up the docker ``indy_pool`` image and ``indy_pool_network`` network if necessary
- set up the base pipenv in the user directory
- set up then uncompress and install the ``libindy.so`` shared library into ``/usr/lib/``.

Once ``von_base`` is set up, logout and login to pick up membership in group ``docker``. Then, issue::

.. code-block:: bash

  $ logout
  (login)
  $ test/test_base

to test the success of the installation (``test_base`` should echo ``0``). Proceed to the installation of component ``von_anchor``.

Documentation
=============

Visit https://von-agent.readthedocs.io/en/latest/index.html to view the design documentation detailing the VON Anchor project in depth.
