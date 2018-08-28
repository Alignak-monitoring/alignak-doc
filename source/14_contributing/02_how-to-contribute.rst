.. _contributing/how-to-contribute:

=================
How to contribute
=================

There is nothing we don't want, we consider every features / ideas.

Feel free to join

   * `by mail`_
   * `on the IRC`_ or `on gitter chat room`_ to discuss or ask for more information
   * `on Alignak web site`_

.. _by mail: mailto://contact@alignak.net/
.. _on the IRC: http://webchat.freenode.net/?channels=%23alignak
.. _on gitter chat room: https://gitter.im/Alignak-monitoring/alignak?utm_source=share-link&utm_medium=link&utm_campaign=share-link
.. _on Alignak web site: http://alignak.net


.. _contributing/release_cycle:

Roadmap and versioning
======================

Release strategy
----------------

Alignak has no strict schedule for now on release date. The very first main release was released after almost two years of intense cleaning, coding and environment testing. The current version is the second major version (2.0) and it is a strongly improved and stable version.

The project roadmap is managed in the github repository thanks to `milestones <https://github.com/Alignak-monitoring/alignak/milestones>`_ and `specific issues <https://github.com/Alignak-monitoring/alignak/issues?utf8=%E2%9C%93&q=is%3Aissue+is%3Aopen+roadmap>`_ tagged with the `roadmap` label Feature request and specification is discussed there. A technical specification or point of view about a specific feature is discussed in a dedicated issue.

Versioning scheme
-----------------

Alignak is following as much as possible the recommendations of the `Python PEP 440 <https://www.python.org/dev/peps/pep-0440/#semantic-versioning>`_ and uses the semantic versioning scheme.

The main information to retain is that Alignak is released with "Major.Minor.Patch":

   * *Patch* number is incremented when only some fixes or minor modifications are introduced in the current version.

   * *Minor* number is incremented when a new feature is introduced in the current version.

   * *Major* number is incremented when some important modifications are (will be...) introduced in the current version.


The stable current Alignak has a *simple* version number such as **M.m.p**, but some other versions may also be available in the installation repositories:

   - the current `develop` branch version is referenced as M.m.p-dev
   - a specifc branch version (eg. my_branch) is referenced as M.m.p-my_branch
   - a specific tag version (eg. my_tag) is referenced as M.m.p-my_tag


Bug report / feature request
============================

Some bugs or unexpected behaviors may happen ... rarely, but it may happen :)

Bugs are tracked in the `issue list on GitHub <https://github.com/Alignak-monitoring/alignak/issues>`_ . Please, always search for your problem in the existing issues before filing a new one.

When filing a new bug, please remember to include:

   *	A helpful title - use descriptive keywords in the title and body so others can find your bug (avoiding duplicates).
   *	Steps to reproduce the problem, with actual vs. expected results
   *	Alignak version (or if you're pulling directly from the Git repo, your current commit SHA - use git rev-parse HEAD)
   *	OS version
   *	If the problem happens with specific code, link to test files
   *	Screenshots or log are very helpful if you're seeing an error message or a UI display problem. (Just copy log or drag an image into the issue description field to include it).


Documentation
=============

The Alignak documentation that you are currently browsing is also a project that we need contributions to... thus contributions to this project are also welcome and encouraged ;) Feel free to update or add your own writings to this documentation project!

The `issue tracker of the project <https://github.com/Alignak-monitoring/alignak-doc/issues>`_ is the preferred channel for reporting errors and submitting pull requests.

.. note:: A part of this document is built automatically when some tests run in the Alignak project. The *test_daemons_api.py* test builds some RST file and copies the files into the *../alignak-doc/source/07_alignak_features/api* directory.

The documentation is organized in chapters. Each chapter has its own directory into the *source* directory. The *index.rst* file make them all grouped in the main table of content. For a local build of the documentation, run::

      make html

Go to your Web browser and open ``file:///home/alignak/alignak-doc/build/html/index.html``.


Setup environment for Alignak hacking
=====================================

See `on this page <Installation/develop>`_ for the requirements and how to get the source code.

We really suggest using a virtualenv to hack the Alignak code::

   virtualenv alignak
   ./alignak/bin/activate
   pip install -r requirements.txt
   pip install -e .

.. note:: using an IDE such as Pycharm makes all this stuff really easy ;) Feel free to ask for more information on our IRC channel...

If you don't want to use a virtual environment (you are in a docker or something else), install system-wide::

   sudo pip install .

If you do not want to use the Alignak system services for testing, the alignak-arbiter daemon is able to start the other daemons by itself. This makes it easier for some simple testings;) Have a look to the *alignak.ini* configuration file and the `alignak_launched` configuration parameter.


Getting started into the developer documentation
================================================

The `developer documentation <https://alignak.readthedocs.org/>`_ is generated from Alignak source code. Basically, it describes the Alignak application packages and modules. You can see the details of the source code docstrings and jump to the source code if necessary. You also have some class diagrams in the index page to browse code more easily.

.. tip:: A good entry point could be the daemon package where you can find the python files used to launch the Alignak daemons (arbiter, scheduler ...) and then follow the code...

When some classes got modified, one must update the developer documentation::

   rm -rf doc/source/reference/* && sphinx-apidoc -o doc/source/reference/ alignak/


Git and GitHub
==============

Before starting to dig into Alignak code, you should be able to use git with ease. If you are new to it, we can suggest you the following links:

   * http://www.git-tower.com/blog/git-cheat-sheet/
   * http://www.cheat-sheets.org/saved-copy/git-cheat-sheet.pdf

We recommend following as much as possible a standard git forking workflow `as documented here <https://www.atlassian.com/git/tutorials/comparing-workflows/forking-workflow`. Take a moment to read this, it is really interesting ...

If you are already familiar with this, here are some of useful commands we use quite often. Indeed, we suggest to use and IDE such as Pycharm because it will hide the git stuff behind a nice interface ;)

Else, if you are a command line addict, what is following is for you...

.. note:: Consider "origin" as the remote branch from Alignak-monitoring organization

Add the fork you have made by pressing the "Fork" button on GitHub::

   # Add your remote git
   git remote add <yournick> git@github.com:<yournick>/alignak.git
   # Fetch data from this remote
   git fetch <yournick>
   # Create new branch named mydevelop linked to the remote develop branch of your fork
   git checkout <yournick>/develop -b mydevelop


Synchronize alignak develop with your current branch (considering there is no conflicts)::

   git fetch origin
   # You should have a merge commit to confirm
   git merge origin/develop
   # Pick the id of the commit before the merging commit you have just done
   git log
   # git will automatically try to stack commit over the commit you specified (develop HEAD)
   git rebase <commit-id>
   # This push the new tree upstream (we have to force push as your local and remote have drifted)
   git push -f <yournick> <current_branch>


Avoid merging a commit because you forgot to pull before committing::

   # This will fetch and merge remote branch with your local one creating a merge commit
   git pull
   # Pick the id of the commit corresponding to the remote HEAD (usually 2 commit before)
   git log
   # Git will revert you commit(s) and stack it after the remote HEAD
   git rebase  <commit-id>
   # Don't need to force you have only added one commit over remote.
   git push


When you’ve finished a feature on a local branch and it’s time to commit your changes to the develop or master branch, you might prefer merging over rebasing. Clean your pull request before submitting it::

   # Pick the id of the current origin develop (see synchronize)
   git rebase -i  <commit-id>

   # Here you can: squash, fix, reword or edit order of commit the way you want.
   # At the end git will try to make the tree the way you ask (if no conflict)
   # In case of a conflict, git will stop were the conflict is and let you deal with it
   # git mergetools may help for that
   # One you are done (git status says there no modified file neither added file)
   git rebase --continue



.. _contributing/testing:

Unit and integration tests
==========================

Alignak is using `py.test <https://docs.pytest.org/en/latest/>`_ for its unit and integration tests. The tests are currently dispatched in two directories / test suites:

   * *tests* for the unit tests: base class, functions tests, etc.
   * *tests_integ* for the integration tests (indeed the one that need to run at least one daemon...)

Almost every test uses *alignak_test.py* module and inherit from the **AlignakTest** class. This class provides a set of function to help tests::

    * scheduler_loop : used to fake a scheduler loop (run check, create broks, raise notification etc..)
    * show_logs : Dump logs (broks with type "log")
    * show_actions : Dump actions (notification, event handler)
    * assert_log_match / assert_any_log_match / ... : Find regexp into logs
    * add : add a brok or external command

.. note:: this file contains many useful functions for Alignak testing... the functions are documented to explain what they are used for.

The best solution to add a test case is:
   * check the existing *test_.py* files names (the files are named according to the main tested features...), and choose the appropriate one. Else, create a new *test_feature.py* file ...
   * duplicate an existing test case and change the expected behavior
   * contact us to ask for more information and we will help digging into the Alignak tests suites

Running the same tests as the one that are executed during a Travis build is an easy stuff. Run the following script in the corresponding directory to run the whole test suite::

      pytest --verbose --durations=10 --no-print-logs --cov=alignak --cov-config .coveragerc test_*.py

      # A more simple form (without verbose and code coverage)
      pytest test_*.py

Because Alignak is intended to run on multiple Python interpreters, the best solution ot run all the tests is to use the `Tox tests automation tool <http://tox.readthedocs.io/en/latest/index.html>`. We provide a *tox.ini* file in the main directory of the project repository. Running all the tests in the exact same conditions as the Travis build and production environment is as simple as::

      tox


.. _contributing/packaging:

Application packaging
=====================

Python packaging
----------------

The Python package is built thanks to the Travis CI deployment feature. You can find the built package in the Alignak profile on the `Python packages repository PyPi <https://pypi.org>`_.

How to build and publish an Alignak python package::

   # Set Alignak version in alignak/version.py
   VERSION = "1.1.0rc0"

   # In alignak repo main directory:
   # for a source distribution
   python setup.py sdist

   # for a wheel distribution
   python setup.py bdist_wheel

   # Upload the package to the test pypi
   # There is an Alignak user account (alignak) on the official PyPi repository
   twine upload --repository-url https://test.pypi.org/legacy/ dist/*

   # Test packaging
   sudo pip install --index-url https://test.pypi.org/simple/ --extra-index-url https://pypi.org/simple alignak==1.1.0rc0
   ...
   ...
   Successfully installed CherryPy-15.0.0 alignak-1.1.0b0 backports.functools-lru-cache-1.5 certifi-2018.4.16 chardet-3.0.4 cheroot-6.3.1 docopt-0.6.2 idna-2.6 importlib-1.0.4 more-itertools-4.1.0 numpy-1.14.3 portend-2.2 psutil-5.4.5 pytz-2018.4 requests-2.18.4 setproctitle-1.1.10 six-1.11.0 tempora-1.11 termcolor-1.1.0 ujson-1.35 urllib3-1.22

   # Check files copy
   ls -al /usr/local/etc/alignak
   # Contains the default configuration files (same as the etc repo directory)


Distro packaging
----------------

Packaging Alignak for Linux/Unix is done thanks to the `fpm packaging tool <https://github.com/jordansissel/fpm>`_ and a specific shell script (*package.sh*) that allows to choose which package is to be built.

The project repository includes three `.bintray-*.json` files that are used to publish the built packages to the Alignak dedicated repositories on the `Bintray software distribution <https://bintray.com/alignak/>`_.

The *package.sh* script creates the packages and updates the `.bintray-*.json` files to update:

   - the target repository
      It replaces `sed_version_repo` with the appropriate repository name: *alignak-deb-testing* / *alignak-deb-stable*, or *alignak-rpm-testing* / *alignak-rpm-stable*

   - the version name, description and release date
      It replaces `sed_version_name`, `sed_version_desc` and `sed_version_released` with fresh information from the aplication


The *package.sh* script command line parameters:

   - git branch name:
      - `master` will build a stable version (alignak_deb-stable repository)
         -> python-alignak_x.x.x_all.deb
      - `develop` will build a develop version (alignak_deb-testing repository)
         -> python-alignak_x.x.x-dev_all.deb
      - any other will build a develop named version (alignak_deb-testing repository)
         -> python-alignak_x.x.x-mybranch_all.deb

   - python version:
      2.7, 3.5 (default)

   - package type:
      deb (default), rpm, freebsd, apk, pacman, ...
      Indeed all the package types supported by fpm may be used ... but it may give some unexpected results.

.. note:: it is not recommended to use anything else than alphabetic characters in the # branch name according to the debian version name policy! Else, the package will not even install on the system!


As an example::

      # Installing fpm
      sudo apt-get install ruby ruby-dev rubygems build-essential
      sudo apt-get install rpm
      sudo gem install --no-ri --no-rdoc fpm

      # Packaging
      sudo pip install virtualenv virtualenv-tools
      sudo pip install --upgrade distribute

      # Get Alignak repo
      git clone http://github.com/alignak-monitoring/alignak
      cd alignak
      # Build package from a virtualenv
      ./package.sh test
      # package.sh is commented for all options


The packages are built thanks to the Travis CI deployment feature. You can find the built packages in an Alignak dedicated repository on the `Bintray software distribution <https://bintray.com/alignak/>`_.

To proceed with installation, you must register the alignak repository and store its public key on your system. This script is an example (for Ubuntu 16) to be adapted to your system::

    # Create an apt source with the content according to your Linux distribution.
    # Get the development repository URL
    $ echo "deb https://dl.bintray.com/alignak/alignak-deb-testing {distribution} main" | sudo tee -a /etc/apt/sources.list

    # Get the stable repository URL
    $ echo "deb https://dl.bintray.com/alignak/alignak-deb-stable {distribution} main" | sudo tee -a /etc/apt/sources.list

**Note:** According to your OS, use the following {distribution} in the former script example:
    - Debian 8: ``jessie``
    - Ubuntu 16.04: ``xenial``
    - Ubuntu 14.04: ``trusty``
    - Ubuntu 12.04: ``precise``


The Alignak packages repositories contain several version of the application. Some information about the versioning scheme are `available on this page <contributing/release_cycle>`_.

For Travis build deploying to Bintray:
   - let the ``alignak`` subject in the bintray json files
   - create a secure key with the ``travis encrypt`` tool. Use yor Bintray API key to generate the key, see https://docs.travis-ci.com/user/deployment/bintray/
   - copy the secure key into the *travis.yml* file