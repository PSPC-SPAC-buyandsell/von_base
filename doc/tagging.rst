Tagging von_base and von_anchor
===============================

Tag each release against the ``von_anchor`` version number.

Create and Push Tags
--------------------

At the bash prompt, after committing and pushing all changes, issue:

.. code-block:: bash

    $ cd ~/von_base
    $ git tag <x.y.z> -m'von_anchor <x.y.z>'
    $ git push --tags

    $ cd ~/von_anchor
    $ git tag <x.y.z> -m'von_anchor <x.y.z>'
    $ git push --tags

where <x.y.z> represents the ``von_anchor`` version number (e.g., 1.6.6).

Move Existing Tags
------------------

If committing changes that do not affect ``von_anchor`` in pypi, effectively move the existing tag by deleting and recreating it.

At the bash prompt, issue the following to delete the tag in ``von_base`` and ``von_anchor``:

.. code-block:: bash

    $ cd ~/von_base
    $ git tag -d <x.y.z>
    $ git push origin :refs/tags/<x.y.z>
    $ git tag <x.y.z> -m'von_anchor <x.y.z>'
    $ git push origin <x.y.z>

    $ cd ~/von_anchor
    $ git tag -d <x.y.z>
    $ git push origin :refs/tags/<x.y.z>
    $ git tag <x.y.z> -m'von_anchor <x.y.z>'
    $ git push origin <x.y.z>

where <x.y.z> represents the ``von_anchor`` version number (e.g., 1.6.6).

Then create and add the tags to ``von_base`` and ``von_anchor`` as above.
