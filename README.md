# von_base
This repository prepares an Ubuntu 16+ VM for the `von_agent` and `von_connector` components implementing the VON demonstrator project.

To install it, clone it from github, then issue
```
$ cd von_base
$ sudo ./setup
```
at the command prompt. The operation will:
  - set up the docker `indy_pool` image and `indy_pool_network` network if necessary
  - set up the base pipenv in the user directory
  - set up then uncompress and install the `libindy.so` shared library into `/usr/lib/`.

Once `von_base` is set up, issue
```
$ newgrp docker
$ test/test_base
```
to test the installation (`test_base` should echo `0`) and proceed to the installation of components `von_agent` and `von_connector`.

## Documentation
The design document at `doc/agent-design.doc` discusses in detail the packages comprising the technology demonstrator project:
  - `von_base`
  - `von_agent`
  - `von_connector`.
