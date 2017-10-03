.. _Installation/sources:

========================
Installation from source
========================

Requirements
============

Some requirements are needed to install from source:

    * python 2.6 or 2.7 (recommended)
    * python-dev
    * pip
    * setuptools

**Note** that Python 2.6 will very soon be deprecated and Alignak is targeted to run with Python 2.7.

**Note** that those required libraries will not be installed with the Alignak pip package.

As of now, Alignak is installable without any compiler installed on the server, except for the *setproctitle*, *ujson*, and *psutil* libraries that requires to be compiled.

To avoid installing the development tools (eg. gcc), you can install the distro packages for those libraries: usually named as `python-psutil`, `python-ujson` and `python-setproctitle`.

Alignak is an application started from a privileged user account and it needs to use a user account. We recommend creating a user account identified as *alignak* member of a group *alignak*.
::

    $ adduser


If you intend to use the Nagios checks plugins and if they are installed on your system, you should invithe the user `alignak` into the `nagios` group. Most often, the installation of the Nagios checks plugins create a `nagios` user member of the `nagios group...


Installation with PIP
=====================

The installation is very simple, use this command::

    sudo pip install alignak


Installation from the git repository
====================================

This procedure is useful only if you want to be able to hack the code base. For only installing, use the next chapter.

* Clone the Alignak repository::

    $ git clone https://github.com/Alignak-monitoring/alignak

* cd to alignak folder::

    $ cd alignak

* Install with python pip (**sudo needed**)::

    $ sudo pip install . -v
        ...
        ...
        Successfully installed CherryPy-14.0.0 alignak-1.1.0 certifi-2018.1.18 chardet-3.0.4 cheroot-6.0.0 idna-2.6 more-itertools-4.1.0 portend-2.2 pytz-2018.3 requests-2.18.4 six-1.11.0 tempora-1.10 urllib3-1.22

* Check installation::

    $ ll /usr/local/bin/alignak*
        -rwxr-xr-x  1 root  wheel  175 Mar  9 11:08 /usr/local/bin/alignak-arbiter*
        -rwxr-xr-x  1 root  wheel  174 Mar  9 11:08 /usr/local/bin/alignak-broker*
        -rwxr-xr-x  1 root  wheel  179 Mar  9 11:08 /usr/local/bin/alignak-environment*
        -rwxr-xr-x  1 root  wheel  174 Mar  9 11:08 /usr/local/bin/alignak-poller*
        -rwxr-xr-x  1 root  wheel  179 Mar  9 11:08 /usr/local/bin/alignak-reactionner*
        -rwxr-xr-x  1 root  wheel  176 Mar  9 11:08 /usr/local/bin/alignak-receiver*
        -rwxr-xr-x  1 root  wheel  177 Mar  9 11:08 /usr/local/bin/alignak-scheduler*

    $ ll /usr/local/etc/alignak/
        total 87
        -rw-r--r--  1 root  wheel   2055 Mar  9 09:40 alignak-logger.json
        -rw-r--r--  1 root  wheel   9238 Mar  9 09:40 alignak.cfg
        -rw-r--r--  1 root  wheel  23841 Mar  9 09:40 alignak.ini
        drwxr-xr-x  7 root  wheel      7 Mar  9 11:08 arbiter/
        drwxr-xr-x  2 root  wheel      3 Mar  9 11:08 certs/
        drwxr-xr-x  3 root  wheel      4 Mar  9 11:08 sample/


**Note:** that installing with root/sudo credentials is needed because some scripts are created in the */usr/local/bin* directory. The drawback is that some files are owned by the root user...


Installation from the source
============================

* Get source archive on this page: https://github.com/Alignak-monitoring/alignak/releases ::

   $ wget https://github.com/Alignak-monitoring/alignak/archive/1.0.0.tar.gz

* Uncompress the archive ::

    $ tar -xvf 1.0.0.tar.gz

* cd to alignak folder ::

    $ cd alignak-1.0.0

* Install with python pip (**sudo needed**)::

    $ sudo pip install .
        ...
        ...
        Successfully installed CherryPy-13.1.0 alignak certifi-2018.1.18 chardet-3.0.4 cheroot-6.0.0 docopt-0.6.2 idna-2.6 importlib-1.0.4 more-itertools-4.1.0 portend-2.2 psutil-5.4.3 requests-2.18.4 setproctitle-1.1.10 six-1.11.0 tempora-1.10 termcolor-1.1.0 ujson-1.35 urllib3-1.22


* Check installation::

    $ ll /usr/local/bin/alignak*
        -rwxr-xr-x 1 root root 166 janv. 30 09:30 /usr/local/bin/alignak-arbiter*
        -rwxr-xr-x 1 root root 165 janv. 30 09:30 /usr/local/bin/alignak-broker*
        -rwxr-xr-x 1 root root 170 janv. 30 09:30 /usr/local/bin/alignak-environment*
        -rwxr-xr-x 1 root root 165 janv. 30 09:30 /usr/local/bin/alignak-poller*
        -rwxr-xr-x 1 root root 170 janv. 30 09:30 /usr/local/bin/alignak-reactionner*
        -rwxr-xr-x 1 root root 167 janv. 30 09:30 /usr/local/bin/alignak-receiver*
        -rwxr-xr-x 1 root root 168 janv. 30 09:30 /usr/local/bin/alignak-scheduler*

    $ ll /usr/local/etc/alignak/
        total 60
        drwxr-xr-x 5 root root  4096 janv. 30 10:19 ./
        drwxr-xr-x 3 root root  4096 janv. 30 10:19 ../
        -rw-rw-r-- 1 root root  9237 janv. 30 10:19 alignak.cfg
        -rw-rw-r-- 1 root root 23092 janv. 30 10:19 alignak.ini
        -rw-rw-r-- 1 root root  2055 janv. 30 10:18 alignak-logger.json
        drwxr-xr-x 7 root root  4096 janv. 30 10:19 arbiter/
        drwxr-xr-x 2 root root  4096 janv. 30 10:19 certs/
        drwxr-xr-x 3 root root  4096 janv. 30 10:19 sample/

    $ ll /usr/local/var/log/alignak/
        total 8
        drwxr-xr-x 2 root root 4096 janv. 30 10:19 ./
        drwxr-xr-x 3 root root 4096 janv. 30 10:19 ../



**Note:** that the created directories are owned by the root user!

**Note:** that installing with root/sudo credentials is needed because some scripts are created in the */usr/local/bin* directory.

**Important note:** because of some pip specific behavior, installing Alignak requires to be connected as a user (and not as root) to run the pip command. If you really need to install from a root account, use ``pip install . -v --install-option='--prefix=/usr/local'``

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


The Python setup script
=======================

**Note** that these information are only available when installing with Python PIP or setup.py. Installation with a distro package may behave differently according to the package choices ;)

The python *setup.py* script used by Alignak does the following:

* test if an ``alignak`` user and an ``alignak`` users group exist on the system

* determine the installation directory ``prefix`` according to the system (most often it is */usr/local*)

* copy the alignak default configuration files in *prefix/etc/alignak*:

    - *alignak.ini* file, the main configuration file
    - *alignak.cfg* file, a default monitored configuration entry-point file
    - *alignak-logger.json* file, the Alignak daemons logger configuration file.
    - *arbiter*, *certs*, *sample* are containing an example configuration and some samples.

