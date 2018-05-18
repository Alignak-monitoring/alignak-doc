.. _Installation/develop:

============================
Installation for development
============================

Requirements
============

The requirements listed for a `pip` installation are also applicable. See :ref:`this page<Installation/pip>`.


Installation from the git repository
====================================

This procedure is the recommended one if you want to be able to hack the code base. For only installing, see :ref:`this page<Installation/pip>`.

* Clone the Alignak repository::

    $ git clone https://github.com/Alignak-monitoring/alignak

* cd to alignak folder::

    $ cd alignak

* Install with python pip (**sudo needed**)::

   # Install in develop mode
   $ sudo pip install . -e
      ...
      ...
      Successfully installed ...

**Note:** that installing with root/sudo credentials is needed because some scripts are created in the */usr/local/bin* directory. The drawback is that some files are owned by the root user...

**Note:** the provided *setup.py* script contain a list of the required Python dependencies without any version specifier. This allows installing Alignak in development mode and permits update of the required packages. If you wish to have the strict Python dependent packages as for an installation in production mode,  pip install -r requirements.txt .


The Python setup script
=======================

**Note** that these information are only available when installing with Python `pip` or setup.py. Installation with a distro package may behave differently according to the package choices ;)

The python *setup.py* script used by Alignak does the following:

* test if an ``alignak`` user and an ``alignak`` users group exist on the system

* determine the installation directory ``prefix`` according to the system (most often it is */usr/local*)

* copy the alignak default configuration files in *prefix/etc/alignak*:

    - *alignak.ini* file, the main configuration file
    - *alignak.cfg* file, a default monitored configuration entry-point file
    - *alignak-logger.json* file, the Alignak daemons logger configuration file.
    - *arbiter*, *certs*, *sample* are containing an example configuration and some samples.

