.. _Installation/pip:

====================================
Installation with pip or from source
====================================

.. _Installation/python_pip:

If your system does not yet include the Python pip tool, here is an installation reminder.


**For Debian-like Linux**
 ::

      sudo apt install python pip


**For RHEL-like Linux**

On a RHEL (CentOS, Oracle Linux,...), the Python pip installer tool is not included in the standard RHEL repositories but it is part of the EPEL repository. You must enable EPEL repository to install pip.

 ::

      yum install epel-release
      yum -y update

      yum install python-pip


On some older versions, it may be necessary to install extra compiler tools for the python *psutil* package::


      yum install gcc python-devel


**For FreeBSD**
For Python 2.7::

      sudo pkg install py27-pip

For Python 3.6::

      sudo pkg install py36-pip


Installation with pip
=====================

The installation from the PyPi repository is very simple, use this command::

      sudo pip install alignak

This will get and install the most recent `alignak` package found on your default PyPi configuration.


If you wish a specific version, specify the version requirements on the command line::

      sudo pip install alignak==1.1.0rc6


No matter why, but you may need to resinstall all the Python Alignak dependencies. You can easily do it with this command line::

      sudo pip install -r /usr/local/share/alignak/requirements.txt


.. tip:: The Alignak project team maintains Alignak and all the Alignak related stuff on its `PyPI page <https://pypi.org/user/Alignak/>`_. All the published versions are available on the `PyPI repository <https://pypi.org/search/?q=alignak>`_ and are tagged ``alignak``.

.. note:: because of some `pip` specific behavior, installing Alignak requires to be connected as a user (and not as root) to run the `pip` command. If you really need to install from a root account, use ``pip install . -v --install-option='--prefix=/usr/local'``

.. note:: the only downside with the pip installation is that the system services are not automatically installed but you can install *a posteriori* because all necessary files are shipped in the */usr/local/share/alignak/bin* directory


Installation from the source
============================

Get the source archive on the project `GitHub releases page <https://github.com/Alignak-monitoring/alignak/releases>`_ and uncompress the downloaded file then install with python pip.

 ::

      wget https://github.com/Alignak-monitoring/alignak/archive/1.1.0.tar.gz

      tar -xvf 1.1.0.tar.gz

      cd alignak-1.1.0

      sudo pip install -r requirements.txt
      sudo pip install .
        ...
        ...
      Successfully installed ......

.. note:: because of some `pip` specific behavior, installing Alignak requires to be connected as a user (and not as root) to run the `pip` command. If you really need to install from a root account, use ``pip install . -v --install-option='--prefix=/usr/local'``

.. note:: the only downside with the pip installation is that the system services are not automatically installed but you can install *a posteriori* because all necessary files are shipped in the */usr/local/share/alignak/bin* directory
