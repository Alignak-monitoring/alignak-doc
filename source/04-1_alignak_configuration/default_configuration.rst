.. _configuration/default_configuration:

=====================
Default configuration
=====================

The default configuration installed with Alignak is a quite good start to build your own configuration because it defines helpful stuff to set-up a configuration

.. note: This configuration allows you to run Alignak "out-of-the-box" because it only includes a *localhost* host that is self-checked and always considered to be in a UP state.

You will find more information on the content of this configuration and how to adapt to your needs in the :ref:`Alignak configuration chapter<alignak_configuration/index>` and in the :ref:`next chapter<extending/updating_default>`.


The default configuration is a folders' hierarchy built as following.

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
        -> init scripts to start, stop, alignak (only on non-Linux distros (FreeBSD))

The scripts that are included in this directory are an easy means to run Alignak in a simple and standard configuration. They may be used, updated, duplicated to make them suit your needs ;)

More information on how to run Alignak :ref:`is available here<howitworks/run_alignak>`.

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
        arbiterd.ini
        brokerd.ini
        pollerd.ini
        reactionned.ini
        receiverd.ini
        schedulerd.ini

Arbiter
~~~~~~~
This part of the configuration is used to describe the whole framework environment: daemons, hosts, services, ...

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
        -> ... for the default monitored objects (by object type)
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


