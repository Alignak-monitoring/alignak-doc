.. _extending/alignak_backend:

===============
Alignak backend
===============

The Alignak backend is a REST backend dedicated to the Alignak framework. It is used to:

    - manage monitoring configuration (hosts, services, contacts, timeperiods...)

        * creates and updates monitoring objects configuration
        * user rights management
        * template management for hosts and services

    - manage retention

        * loads and saves retention information for checks/hosts/services

    - manage live states

        * creates and updates states for hosts and services
        * manages acknowledges, downtimes, re-checks

    - feed time series databases with performance data

        * get performance data from the elements check to feed Graphite and/or Influx

    - system activity log

        * store all the system activity in a timestamped log


More information is available in the `project repository <https://github.com/Alignak-monitoring-contrib/alignak-backend>`_.

The backend full documentation is available on http://alignak-backend.readthedocs.io/en/latest/intro.html

REST API
========

The Alignak backend, based on the Python Eve frameworks, implements a complete REST API to manage the monitored objects data model.

The detailed API is available in the backend documentation.


Using the backend with Alignak
==============================

Installing the Alignak backend is an easy operation.
The different installation methods are detailed in the documentation but the most easy way is to::

    pip install alignak-backend


Once installed, the backend is ready to run and you can start with::

    ./bin/run.sh


Configuring the Alignak backend is made easy thanks to a *settings.json* file.
The default configuration makes the backend run on the local interface, on port 5000, and it creates an admin user that can be used to log-in.

**Note**: to be updated after the backend packaging ...

Once installed, the backend needs to be related to Alignak daemons. This is achieved thanks to the `Alignak backend`_ modules.

A backend installation tutorial exists on the `SysAdmin.cool web site <http://sysadmin.cool/>`_.

Feeding the backend
===================

On start, the backend creates some elements if they do not yet exist:

    * a default super administrator user named `admin` with an `admin` password
    * a default Realm (`All`)
    * default groups (users groups, hosts groups and services groups) named `All`
    * default time periods: `24x7` and `None`

Those default elements are used as default values for the newly created elements to insure data model integrity.

Creating or updating data in the backend is made simple thanks to:

    * direct curl commands
    * using a REST client like PostMan
    * using the Alignak backend client Python library
    * using the Alignak backend client PHP library
    * a flat-files Nagios configuration importation script

Backend clients
---------------
Several backend client or libraries exist to use the REST API. They will not be described in this document.

For more information, see:

    - http://alignak-backend-client.readthedocs.io/en/latest/, for the Python library
    -

**To be completed...**


Backend importation script
--------------------------

The easiest and fastest way to populate data in the Alignak backend is to import your existing configuration into the backend.

The `alignak_backend_import` makes it as easy as::

    alignak_backend_import /etc/my_config/config.cfg

Once this script has finished its execution, you will be able to browse your contacts, hosts, services, ... in the Alignak backend.


The most often `alignak_backend_import` command is::

    alignak_backend_import -d -m -b http://backend:5000 -u username -p password /etc/my_config/config.cfg

where you specify that you want to delete existing data (*-d*), import the existing templates (*-m*) and indicate where is located you backend (*-b*) and how to log-in (*-u*, *-p*).


More information is available in this `documentation <http://alignak-backend-import.readthedocs.io/en/latest/>`_.

A backend feeding tutorial exists on the `SysAdmin.cool web site <http://sysadmin.cool/>`_.
