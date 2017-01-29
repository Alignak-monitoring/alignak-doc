.. _Installation/sources:

========================
Installation from source
========================

Requirements
============

Some requirements are needed to install from source:

* python 2.6 or 2.7 (recommanded)
* python-dev
* pip
* setuptools
* git (only for installing from the git repository)


Installation from the git repository
====================================

This procedure is useful only if you want to be able to hack the code base. For only installing, use the next chapter.

Follow these steps:

* Create user *alignak* member of group *alignak*
* Add this user to the sudoers
* Login with this user account::

  adduser alignak
  adduser alignak sudo
  su - alignak

* Clone the Alignak repository::

    git clone https://github.com/Alignak-monitoring/alignak

* cd to alignak folder::

    cd alignak

* Install with python (sudo needed)::

    sudo pip install .


Installation from the source
============================

Follow these steps:

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

* Install with python (sudo needed)::

    sudo pip install .


Install as a python lib
=======================

In a virtualenv ::

  virtualenv env
  source env/bin/activate
  python setup.py install_lib
  python -c 'from alignak.bin import VERSION; print(VERSION)'

Or directly on your system ::

  sudo python setup.py install_lib
  python -c 'from alignak.bin import VERSION; print(VERSION)'

