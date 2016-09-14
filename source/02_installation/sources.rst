.. _Installation/sources:

=========================
Installation from sources
=========================

Introduction
============

Some requirements are needed to install with sources:

* python 2.6 or 2.7 (recommanded)
* python-dev


Installation
============

This is the steps:

* Create user *alignak* and login with this user account
* Get source archive on this page: https://github.com/Alignak-monitoring/alignak/releases 
* Uncompress the archive
* Go in alignak folder
* Run command::

    pip install -r requirements.txt

* Install with python (as sudo)::

    python setup.py install


Install Alignak as python lib
=============================

In a virtualenv ::

  virtualenv env
  source env/bin/activate
  python setup.py install_lib
  python -c 'from alignak.bin import VERSION; print(VERSION)'

Or directly on your system ::

  sudo python setup.py install_lib
  python -c 'from alignak.bin import VERSION; print(VERSION)'



Data directory
==============

To change the data files installation directory you can run::

    python setup.py install --data-dir /tmp

This command will use */tmp* directory instead of the usual data installation directory determined by the system.