.. _extending/updating_default:

==============================
Updating default configuration
==============================

What does it include ?
======================

The default configuration installed with Alignak defines:

    * daemons configuration for: one arbiter, one scheduler, one broker, one reactionner, one poller and one receiver

    * some templates:
        * a `generic-contact` template that contains the main common contact parameters
        * a `generic-host` template that contains the main common host parameters
        * a `generic-service` template that contains the main common service parameters
        for hosts, services and contacts

    * one host (``localhost``) which is always UP

    * no services

    * two contacts: ``guest`` and ``admin``

This configuration is fully functionnal but it almost does nothing ... except saying that ``localhost`` is UP without even checking if it is true :)

What is important at the moment is to check that the existing configuration is valid and that Alignak is able to use it. For checking the configuration, run::

    alignak-arbiter -V -a /usr/local/etc/alignak/alignak.cfg

where ``alignak.cfg`` is the configuration entry point.

The Alignak Arbiter will parse the configuration and will inform about its validity::

    [1474542500] INFO: [Alignak] Alignak 0.2
    [1474542500] INFO: [Alignak] Copyright (c) 2015-2015:
    [1474542500] INFO: [Alignak] Alignak Team
    [1474542500] INFO: [Alignak] License: AGPL
    [1474542500] INFO: [Alignak] Loading configuration
    [1474542500] INFO: [Alignak] [config] opening '/usr/local/etc/alignak/alignak.cfg' configuration file
    [1474542500] INFO: [Alignak] Processing object config file '/usr/local/etc/alignak/arbiter/objects/realms/all.cfg'
    [1474542500] INFO: [Alignak] Processing object config file '/usr/local/etc/alignak/arbiter/objects/commands/check_host_alive.cfg'
    [1474542500] INFO: [Alignak] Processing object config file '/usr/local/etc/alignak/arbiter/objects/commands/detailled-service-by-email.cfg'
    [1474542500] INFO: [Alignak] Processing object config file '/usr/local/etc/alignak/arbiter/objects/commands/notify-service-by-email.cfg'
    [1474542500] INFO: [Alignak] Processing object config file '/usr/local/etc/alignak/arbiter/objects/commands/detailled-host-by-email.cfg'
    [1474542500] INFO: [Alignak] Processing object config file '/usr/local/etc/alignak/arbiter/objects/commands/notify-host-by-email.cfg'
    [1474542500] INFO: [Alignak] Processing object config file '/usr/local/etc/alignak/arbiter/objects/commands/check_ping.cfg'
    [1474542500] INFO: [Alignak] Processing object config file '/usr/local/etc/alignak/arbiter/objects/timeperiods/none.cfg'
    [1474542500] INFO: [Alignak] Processing object config file '/usr/local/etc/alignak/arbiter/objects/timeperiods/workhours.cfg'
    [1474542500] INFO: [Alignak] Processing object config file '/usr/local/etc/alignak/arbiter/objects/timeperiods/us-holidays.cfg'
    [1474542500] INFO: [Alignak] Processing object config file '/usr/local/etc/alignak/arbiter/objects/timeperiods/24x7.cfg'
    [1474542500] INFO: [Alignak] Processing object config file '/usr/local/etc/alignak/arbiter/objects/escalations/sample.cfg'
    [1474542500] INFO: [Alignak] Processing object config file '/usr/local/etc/alignak/arbiter/objects/dependencies/sample.cfg'
    [1474542500] INFO: [Alignak] Processing object config file '/usr/local/etc/alignak/arbiter/templates/business-impacts.cfg'
    [1474542500] INFO: [Alignak] Processing object config file '/usr/local/etc/alignak/arbiter/templates/time_templates.cfg'
    [1474542500] INFO: [Alignak] Processing object config file '/usr/local/etc/alignak/arbiter/templates/generic-contact.cfg'
    [1474542500] INFO: [Alignak] Processing object config file '/usr/local/etc/alignak/arbiter/templates/generic-host.cfg'
    [1474542500] INFO: [Alignak] Processing object config file '/usr/local/etc/alignak/arbiter/templates/generic-service.cfg'
    [1474542500] INFO: [Alignak] Processing object config file '/usr/local/etc/alignak/arbiter/packs/readme.cfg'
    [1474542500] INFO: [Alignak] Processing object config file '/usr/local/etc/alignak/arbiter/packs/resource.d/readme.cfg'
    [1474542500] INFO: [Alignak] Processing object config file '/usr/local/etc/alignak/arbiter/objects/notificationways/email.cfg'
    [1474542500] INFO: [Alignak] Processing object config file '/usr/local/etc/alignak/arbiter/objects/notificationways/detailled-email.cfg'
    [1474542500] INFO: [Alignak] Processing object config file '/usr/local/etc/alignak/arbiter/objects/servicegroups/sample.cfg'
    [1474542500] INFO: [Alignak] Processing object config file '/usr/local/etc/alignak/arbiter/objects/hostgroups/linux.cfg'
    [1474542500] INFO: [Alignak] Processing object config file '/usr/local/etc/alignak/arbiter/objects/contactgroups/admins.cfg'
    [1474542500] INFO: [Alignak] Processing object config file '/usr/local/etc/alignak/arbiter/objects/contactgroups/users.cfg'
    [1474542500] INFO: [Alignak] Processing object config file '/usr/local/etc/alignak/arbiter/objects/hosts/localhost.cfg'
    [1474542500] INFO: [Alignak] Processing object config file '/usr/local/etc/alignak/arbiter/objects/services/services.cfg'
    [1474542500] INFO: [Alignak] Processing object config file '/usr/local/etc/alignak/arbiter/objects/contacts/guest.cfg'
    [1474542500] INFO: [Alignak] Processing object config file '/usr/local/etc/alignak/arbiter/objects/contacts/admin.cfg'
    [1474542500] INFO: [Alignak] Processing object config file '/usr/local/etc/alignak/arbiter/daemons/scheduler-master.cfg'
    [1474542500] INFO: [Alignak] Processing object config file '/usr/local/etc/alignak/arbiter/daemons/receiver-master.cfg'
    [1474542500] INFO: [Alignak] Processing object config file '/usr/local/etc/alignak/arbiter/daemons/poller-master.cfg'
    [1474542500] INFO: [Alignak] Processing object config file '/usr/local/etc/alignak/arbiter/daemons/broker-master.cfg'
    [1474542500] INFO: [Alignak] Processing object config file '/usr/local/etc/alignak/arbiter/daemons/arbiter-master.cfg'
    [1474542500] INFO: [Alignak] Processing object config file '/usr/local/etc/alignak/arbiter/daemons/reactionner-master.cfg'
    [1474542500] INFO: [Alignak] Processing object config file '/usr/local/etc/alignak/arbiter/modules/sample.cfg'
    [1474542500] INFO: [Alignak] Processing object config file '/usr/local/etc/alignak/arbiter/resource.d/paths.cfg'
    [1474542500] INFO: [Alignak] Processing object config file '/usr/local/etc/alignak/arbiter/packs/resource.d/readme.cfg'
    [1474542500] INFO: [Alignak] I am the master Arbiter: arbiter-master
    [1474542500] INFO: [Alignak] My own modules:
    [1474542500] INFO: [Alignak] I correctly loaded the modules: []
    [1474542500] INFO: [Alignak] All: (in/potential) (schedulers:1) (pollers:1/1) (reactionners:1/1) (brokers:1/1) (receivers:1/1)
    [1474542500] WARNING: [Alignak] The following parameter(s) are not currently managed.
    [1474542500] INFO: [Alignak] enable_predictive_service_dependency_checks
    [1474542500] INFO: [Alignak] host_perfdata_file_processing_interval
    [1474542500] INFO: [Alignak] use_embedded_perl_implicitly
    [1474542500] INFO: [Alignak] use_regexp_matching: If you go some host or service definition like prod*, it will surely failed from now, sorry.
    [1474542500] INFO: [Alignak] service_perfdata_file_processing_command
    [1474542500] INFO: [Alignak] use_true_regexp_matching
    [1474542500] INFO: [Alignak] enable_embedded_perl: It will surely never be managed, but it should not be useful with poller performances.
    [1474542500] INFO: [Alignak] enable_predictive_host_dependency_checks
    [1474542500] INFO: [Alignak] service_perfdata_file_processing_interval
    [1474542500] INFO: [Alignak] host_perfdata_file_processing_command
    [1474542500] INFO: [Alignak] passive_host_checks_are_soft
    [1474542500] INFO: [Alignak] date_format
    [1474542500] INFO: [Alignak] translate_passive_host_checks
    [1474542500] INFO: [Alignak] auto_rescheduling_interval
    [1474542500] INFO: [Alignak] soft_state_dependencies
    [1474542500] INFO: [Alignak] auto_reschedule_checks
    [1474542500] INFO: [Alignak] auto_rescheduling_window
    [1474542500] WARNING: [Alignak] Unmanaged configuration statement, do you really need it?Ask for it on the developer mailing list https://lists.sourceforge.net/lists/listinfo/alignak-devel or submit a pull request on the Alignak github
    [1474542500] INFO: [Alignak] Running pre-flight check on configuration data...
    [1474542500] INFO: [Alignak] Checking global parameters...
    [1474542500] INFO: [Alignak] Checking hosts...
    [1474542500] INFO: [Alignak] 	Checked 1 hosts
    [1474542500] INFO: [Alignak] Checking hostgroups...
    [1474542500] INFO: [Alignak] 	Checked 1 hostgroups
    [1474542500] INFO: [Alignak] Checking contacts...
    [1474542500] INFO: [Alignak] 	Checked 2 contacts
    [1474542500] INFO: [Alignak] Checking contactgroups...
    [1474542500] INFO: [Alignak] 	Checked 2 contactgroups
    [1474542500] INFO: [Alignak] Checking notificationways...
    [1474542500] INFO: [Alignak] 	Checked 2 notificationways
    [1474542500] INFO: [Alignak] Checking escalations...
    [1474542500] INFO: [Alignak] 	Checked 0 escalations
    [1474542500] INFO: [Alignak] Checking services...
    [1474542500] INFO: [Alignak] 	Checked 0 services
    [1474542500] INFO: [Alignak] Checking servicegroups...
    [1474542500] INFO: [Alignak] 	Checked 0 servicegroups
    [1474542500] INFO: [Alignak] Checking timeperiods...
    [1474542500] INFO: [Alignak] 	Checked 4 timeperiods
    [1474542500] INFO: [Alignak] Checking commands...
    [1474542500] INFO: [Alignak] 	Checked 9 commands
    [1474542500] INFO: [Alignak] Checking hostsextinfo...
    [1474542500] INFO: [Alignak] 	Checked 0 hostsextinfo
    [1474542500] INFO: [Alignak] Checking servicesextinfo...
    [1474542500] INFO: [Alignak] 	Checked 0 servicesextinfo
    [1474542500] INFO: [Alignak] Checking checkmodulations...
    [1474542500] INFO: [Alignak] 	Checked 0 checkmodulations
    [1474542500] INFO: [Alignak] Checking macromodulations...
    [1474542500] INFO: [Alignak] 	Checked 0 macromodulations
    [1474542500] INFO: [Alignak] Checking realms...
    [1474542500] INFO: [Alignak] 	Checked 1 realms
    [1474542500] INFO: [Alignak] Checking servicedependencies...
    [1474542500] INFO: [Alignak] 	Checked 0 servicedependencies
    [1474542500] INFO: [Alignak] Checking hostdependencies...
    [1474542500] INFO: [Alignak] 	Checked 0 hostdependencies
    [1474542500] INFO: [Alignak] Checking arbiters...
    [1474542500] INFO: [Alignak] 	Checked 1 arbiters
    [1474542500] INFO: [Alignak] Checking schedulers...
    [1474542500] INFO: [Alignak] 	Checked 1 schedulers
    [1474542500] INFO: [Alignak] Checking reactionners...
    [1474542500] INFO: [Alignak] 	Checked 1 reactionners
    [1474542500] INFO: [Alignak] Checking pollers...
    [1474542500] INFO: [Alignak] 	Checked 1 pollers
    [1474542500] INFO: [Alignak] Checking brokers...
    [1474542500] INFO: [Alignak] 	Checked 1 brokers
    [1474542500] INFO: [Alignak] Checking receivers...
    [1474542500] INFO: [Alignak] 	Checked 1 receivers
    [1474542500] INFO: [Alignak] Checking resultmodulations...
    [1474542500] INFO: [Alignak] 	Checked 0 resultmodulations
    [1474542500] INFO: [Alignak] Checking businessimpactmodulations...
    [1474542500] INFO: [Alignak] 	Checked 0 businessimpactmodulations
    [1474542500] INFO: [Alignak] Cutting the hosts and services into parts
    [1474542500] INFO: [Alignak] Creating packs for realms
    [1474542500] INFO: [Alignak] Number of hosts in the realm All: 1 (distributed in 1 linked packs)
    [1474542500] INFO: [Alignak] Number of Contacts : 2
    [1474542500] INFO: [Alignak] Number of Hosts : 1
    [1474542500] INFO: [Alignak] Number of Services : 0
    [1474542500] INFO: [Alignak] Number of Commands : 9
    [1474542500] INFO: [Alignak] Total number of hosts in all realms: 1
    [1474542500] INFO: [Alignak] Things look okay - No serious problems were detected during the pre-flight check

Declaring new objects
=====================

Declaring new objects in the monitoring configuration follow the rules as they are defined for a Nagios flat-files configuration as they are defined on the `Nagios objects Definition <https://assets.nagios.com/downloads/nagioscore/docs/nagioscore/3/en/objectdefinitions.html>`_

The objects defined use the same properties as the one defined in Shinken and `documented here <http://shinken.readthedocs.io/en/latest/03_configuration/configobject.html>`_.

.. warning:: *** TO BE COMPLETED/IMPROVED ***

A new contact
-------------
To declare a new contact, you can create a new file in the *alignak/arbiter/objects/contacts* directory
::

    define contact{
        use                 generic-contact
        contact_name        new_contact
        email               guest@localhost
        password            password
        can_submit_commands 0
    }



A new host
----------
To declare a new host, you can create a new file in the *alignak/arbiter/objects/hosts* directory
::

    define host{
        use                 generic-host
        host_name           new_host
        address             127.0.0.1
    }


