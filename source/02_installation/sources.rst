.. _Installation/sources:

=========================
Installation with sources
=========================

Introduction
============

Some requirements are needed to install with sources:

* python 2.6 or 2.7 (recommanded)
* python-dev


Installation
============

This is the steps:

* Create user *alignak* and connect with
* Get source archive on this page: https://github.com/Alignak-monitoring/alignak/releases 
* Uncompress the archive
* Go in alignak folder
* Run command::

    pip install -r requirements.txt

* Install with python (as sudo)::

    python setup.py install

* Give rights to following fodlers::

    chown -R alignak:alignak /usr/local/var/run/alignak
    chown -R alignak:alignak /usr/local/var/log/alignak


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




