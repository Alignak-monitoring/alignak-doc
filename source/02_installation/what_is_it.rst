.. _Installation/what_is_it:

===========================
What the installation does?
===========================

The setup script
================

The python *setup.py* script used by Alignak does the following:

* test if an `alignak` user and an `alignak` users group exist on the system

* determine the installation directory according to the system (setuptools inner method)

* create a */etc/default/alignak* main configuration file:
    - copy the *bin/default/alignak.in* file
    - update the main variable directories according to the used installation directory

* copy the alignak default configuration files in */etc/alignak* and update some files:
    - update the *alignak.cfg* file according to the used installation directory
    - update the *daemons/*.ini* files according to the used installation directory
    - update the *paths.cfg* file with the Nagios plugins dir and Alignak plugins directory

Files / directories lists
=========================

All the variables introduced here are prefixed with the installation directory. Most often, this intallation directory is */usr/local*.

Variables to update *default/alignak*::

    'ETC': '/etc/alignak',
    'VAR': '/var/lib/alignak',
    'BIN': '/bin',
    'RUN': '/var/run/alignak',
    'LOG': '/var/log/alignak',
    'LIB': '/var/libexec/alignak',

Variables to update *alignak.cfg* and *daemons.ini* files::

    'workdir': '/var/run/alignak',
    'logdir': '/var/log/alignak',
    'modules_dir': '/var/lib/alignak/modules',
    'plugins_dir': '/var/libexec/alignak',

    'lock_file': '/var/run/alignak/arbiterd.pid',
    'local_log': '/var/log/alignak/arbiterd.log',
    'pidfile': '/var/run/alignak/arbiterd.pid',

    'pack_distribution_file': '/var/lib/alignak/pack_distribution.dat'

    'ca_cert': '/etc/alignak/certs/ca.pem',
    'server_cert': '/etc/alignak/certs/server.cert',
    'server_key': '/etc/alignak/certs/server.key',

**Note:** The last three variables concern SSL and they are replaced and stay commented in the configuration files.

Variables to update *resource.d/paths.cfg*::

    'LOGSDIR': '/var/log/alignak',
    'PLUGINSDIR': '/var/libexec/alignak',

**Note:** Those variables are intended for macro definition and they are replaced with *$name$*.
