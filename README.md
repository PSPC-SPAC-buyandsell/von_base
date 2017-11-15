# von_base

This repository prepares an Ubuntu 16+ VM for the `von_agent` and `von_connector` components implementing the VON demonstrator project.

To install it, clone it from github, then issue
```
$ cd von_base
$ sudo ./setup
```
at the command prompt. The operation will set up the docker `indy_pool` image and `indy_pool_network` network if necessary, then uncompress and install the `libindy.so` shared library into `/usr/lib/`.

Once `von_base` is set up, proceed to the installation of the python virtual environment and components `von_agent` and `von_connector`.
