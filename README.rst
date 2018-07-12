von_base
========
This repository prepares an Ubuntu 16+ VM for the ``von_anchor`` component enabling VON components.

To install it, clone it from github, then issue::

  $ cd von_base
  $ sudo ./setup

at the command prompt. The operation will:

- set up the docker ``indy_pool`` image and ``indy_pool_network`` network if necessary
- set up the base pipenv in the user directory
- set up then uncompress and install the ``libindy.so`` shared library into ``/usr/lib/``.

Once ``von_base`` is set up, logout and login to pick up membership in group ``docker``. Then, issue::

  $ logout
  (login)
  $ test/test_base

to test the success of the installation (``test_base`` should echo ``0``). Proceed to the installation of component ``von_anchor``.

Documentation
=============
The design document at ``doc/anchor-design.doc`` discusses in detail the packages comprising the technology demonstrator project:

- ``von_base``
- ``von_anchor``.
