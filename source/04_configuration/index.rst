.. raw:: LaTeX

    \newpage

.. _configuration/index:

Configuration
=============

.. raw:: LaTeX

    \newpage


The Alignak configuration is splitted in three main parts:

- arbiter configuration,
- daemons configuration,
- external modules configuration.

The arbiter configuration part describes the Alignak framework infrastructure
(which daemons are used and how they are) and the monitoring objects (what is monitored and how it is).

All the configuration is stored by default in the folder */usr/local/etc/alignak/*.
Each main part of the configuration is described in the following chapters:

The default configuration hierarchy is as following::

    /usr/local/etc/
    /usr/local/etc/default
        -> main configuration file used by the init scripts

    /usr/local/etc/init.d
        -> init scripts to start, stop, alignak

    /usr/local/etc/alignak
        -> configuration...
    /usr/local/etc/alignak/certs
        -> ... for SSL connexion
    /usr/local/etc/alignak/daemons
        -> ... for the daemons
    /usr/local/etc/alignak/arbiter_cfg
        -> ... for the main monitoring configuration file (alignak.cfg)
    /usr/local/etc/alignak/arbiter_cfg/resource.d
    /usr/local/etc/alignak/arbiter_cfg/modules
        -> ... for the modules
    /usr/local/etc/alignak/arbiter_cfg/daemons_cfg
        -> ... for the daemons
    /usr/local/etc/alignak/arbiter_cfg/objects
        -> ... for the monitored objects
    /usr/local/etc/alignak/arbiter_cfg/objects/contactgroups
    /usr/local/etc/alignak/arbiter_cfg/objects/services
    /usr/local/etc/alignak/arbiter_cfg/objects/hostgroups
    /usr/local/etc/alignak/arbiter_cfg/objects/contacts
    /usr/local/etc/alignak/arbiter_cfg/objects/realms
    /usr/local/etc/alignak/arbiter_cfg/objects/timeperiods
    /usr/local/etc/alignak/arbiter_cfg/objects/sample
    /usr/local/etc/alignak/arbiter_cfg/objects/sample/services
    /usr/local/etc/alignak/arbiter_cfg/objects/sample/hosts
    /usr/local/etc/alignak/arbiter_cfg/objects/sample/triggers.d
    /usr/local/etc/alignak/arbiter_cfg/objects/commands
    /usr/local/etc/alignak/arbiter_cfg/objects/packs
    /usr/local/etc/alignak/arbiter_cfg/objects/notificationways
    /usr/local/etc/alignak/arbiter_cfg/objects/escalations
    /usr/local/etc/alignak/arbiter_cfg/objects/templates
    /usr/local/etc/alignak/arbiter_cfg/objects/servicegroups
    /usr/local/etc/alignak/arbiter_cfg/objects/hosts
    /usr/local/etc/alignak/arbiter_cfg/objects/dependencies
    /usr/local/etc/alignak/arbiter_cfg/templates
        -> ... for the monitored objects templates
    /usr/local/etc/alignak/arbiter_cfg/packs
        -> ... for the installed monitoring checks packs

    /usr/local/var/log/alignak
        -> ... for the alignak daemons log files

    /usr/local/var/lib/alignak
        -> ... for the alignak libraries

    /usr/local/var/libexec/alignak
        -> ... for the alignak external checks plugins

    /usr/local/var/run/alignak
        -> ... for the alignak daemons run files (pid)


.. toctree::

   arbiter
   daemons
   modules

