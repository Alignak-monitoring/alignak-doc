.. _configuration/default_configuration:

=====================
Default configuration
=====================

Main information
================

The default configuration installed with Alignak is a quite good start to build your own configuration
because it defines helpful stuff to set-up a configuration

Note that this configuration allows you to run Alignak because it includes a *localhost* host that is self-checked

More information on the content of this configuration and how to adapt to your needs in the `_next chapter<05_extending_alignak/updating_default>`_.


Folders hierarchy
=================

The default configuration hierarchy is as following.

Main configuration file
-----------------------
::

    /usr/local/etc/
    /usr/local/etc/default
        -> main configuration file used by the init scripts (only on Linux distros)


Running scripts
---------------
::

    /usr/local/etc/init.d
        -> init scripts to start, stop, alignak (only on Linux distros)

    /usr/local/etc/rc.d
        -> init scripts to start, stop, alignak (only on non-Linux distros)


Framework configuration
-----------------------
::

    /usr/local/etc/alignak
        -> configuration entry point...


Sample
~~~~~~
The *etc/alignak/sample* directory contain many samples for the configuration of the different
elements defined in the configuration. Please note that they are only samples and that they will
probably not be fully functional out-of-the-box...

SSL
~~~
Used to store the SSL files if you enable SSL communication between the daemons
::

    /usr/local/etc/alignak/certs
        -> ... for SSL connexion between the daemons

Daemons
~~~~~~~
To configure the daemons that are running on the system. In a distributed environment, only those
files need to be updated to reflect the daemons configuration.
::

    /usr/local/etc/alignak/daemons
        -> ... for the daemons

Arbiter
~~~~~~~
This part of the configuration is used to describe the whole framework environment: daemons, hosts, services, ...
The Alignak arbiter uses this configuration to dispatch
::

    /usr/local/etc/alignak/arbiter
        -> ... for the main monitoring configuration file (alignak.cfg)
    /usr/local/etc/alignak/arbiter/resource.d
        -> ... for the global macros and resources
    /usr/local/etc/alignak/arbiter/modules
        -> ... for the installed modules
    /usr/local/etc/alignak/arbiter/daemons
        -> ... for the daemons
    /usr/local/etc/alignak/arbiter/objects
        -> ... for the monitored objects
    /usr/local/etc/alignak/arbiter/objects/contactgroups
    /usr/local/etc/alignak/arbiter/objects/services
    /usr/local/etc/alignak/arbiter/objects/hostgroups
    /usr/local/etc/alignak/arbiter/objects/contacts
    /usr/local/etc/alignak/arbiter/objects/realms
    /usr/local/etc/alignak/arbiter/objects/timeperiods
    /usr/local/etc/alignak/arbiter/objects/sample
    /usr/local/etc/alignak/arbiter/objects/sample/services
    /usr/local/etc/alignak/arbiter/objects/sample/hosts
    /usr/local/etc/alignak/arbiter/objects/sample/triggers.d
    /usr/local/etc/alignak/arbiter/objects/commands
    /usr/local/etc/alignak/arbiter/objects/packs
    /usr/local/etc/alignak/arbiter/objects/notificationways
    /usr/local/etc/alignak/arbiter/objects/escalations
    /usr/local/etc/alignak/arbiter/objects/templates
    /usr/local/etc/alignak/arbiter/objects/servicegroups
    /usr/local/etc/alignak/arbiter/objects/hosts
    /usr/local/etc/alignak/arbiter/objects/dependencies
    /usr/local/etc/alignak/arbiter/templates
        -> ... for the monitored objects templates

    /usr/local/etc/alignak/arbiter/packs
        -> ... for the installed monitoring checks packs
    /usr/local/etc/alignak/arbiter/packs/resource.d
        -> ... for the installed monitoring checks packs global macros

    /usr/local/var/log/alignak
        -> ... for the alignak daemons log files

    /usr/local/var/lib/alignak
        -> ... for the alignak libraries

    /usr/local/var/libexec/alignak
        -> ... for the alignak external checks plugins

    /usr/local/var/run/alignak
        -> ... for the alignak daemons run files (pid)


