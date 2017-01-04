.. _Installation/sources:

=========================
Installation from sources
=========================

Introduction
============

Some requirements are needed to install with sources:

* python 2.6 or 2.7 (recommended)
* python-dev


Installation
============

This is the steps:

* Create user *alignak* member of group *alignak*
* Add this user to the sudoers
* Login with this user account::

  adduser alignak
  adduser alignak sudo
  su - alignak

* Get source archive on this page: https://github.com/Alignak-monitoring/alignak/releases ::

    wget https://github.com/Alignak-monitoring/alignak/archive/0.2.tar.gz

* Uncompress the archive ::

    tar -xvf 0.2.tar.gz

* cd to alignak folder ::

    cd alignak-0.2

* Install with python (as sudo)::

    sudo pip install .


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

