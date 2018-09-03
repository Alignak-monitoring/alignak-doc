.. _howitworks/run_alignak:

===============
Running Alignak
===============

.. note:: this documentation is assuming that the Alignak configuration used for running is the default shipped configuration. Thus all examples are using */usr/local/share/alignak* directory... please adapt the example scripts to your own configuration.


.. _run_alignak/services:
.. _run_alignak/services_systemd:

Systemd services
================

If your system is a recent Linux distribution (Debian 7, Ubuntu 16, CentOS 7) using *systemd*, and you installed from the distro packaging, you should have installed some system services that allow starting Alignak daemons with the standard `systemctl` command.

Installing systemd units
------------------------
.. _Installation/services:

The Alignak installation process ships some systemd unit files in the */usr/local/share/alignak/bin* directory. To install the services, if not yet installed), you must::

    # Install the man pages
    sudo cp /usr/local/share/alignak/bin/manpages/manpages/* /usr/share/man/man8

    # Copy the systemd units
    sudo cp /usr/local/share/alignak/bin/systemd/alignak* /etc/systemd/system

    ll /etc/systemd/system
      -rw-r--r--. 1 root root  777 May 24 17:48 /lib/systemd/system/alignak-arbiter@.service
      -rw-r--r--. 1 root root  770 May 24 17:48 /lib/systemd/system/alignak-broker@.service
      -rw-r--r--. 1 root root  770 May 24 17:48 /lib/systemd/system/alignak-poller@.service
      -rw-r--r--. 1 root root  805 May 24 17:48 /lib/systemd/system/alignak-reactionner@.service
      -rw-r--r--. 1 root root  784 May 24 17:48 /lib/systemd/system/alignak-receiver@.service
      -rw-r--r--. 1 root root  791 May 24 17:48 /lib/systemd/system/alignak-scheduler@.service
      -rw-r--r--. 1 root root 1286 May 24 17:48 /lib/systemd/system/alignak.service

    sudo systemctl enable alignak
      Created symlink from /etc/systemd/system/multi-user.target.wants/alignak.service to /usr/lib/systemd/system/alignak.service.

.. note:: more information about the default shipped configuration is available :ref: `on this page <configuration/default_configuration>`.


Once you achieved this tricky part, running Alignak daemons is easy. All you need is to inform the Alignak daemons where they will find the configuration to use and start the `alignak` system service. All this is explained :ref:`in this chapter <run_alignak/services_systemd>`.



Configuring Alignak
-------------------
You need to inform Alignak daemons where they should find the main configuration file. Using the ``ALIGNAK_CONFIGURATION_FILE`` environment variable is the simplest solution.

This variable is configured, as default, in each Alignak service unit file::

   [Service]
   # Environment variables - may be overriden in the /etc/default/alignak
   Environment=ALIGNAK_CONFIGURATION_FILE=/usr/local/share/alignak/etc/alignak.ini
   Environment=ALIGNAK_USER=alignak
   Environment=ALIGNAK_GROUP=alignak
   EnvironmentFile=-/etc/default/alignak

To change its value, you need to create an environment configuration file: */etc/default/alignak*::

   ALIGNAK_CONFIGURATION_FILE=/usr/local/etc/my-alignak.ini
   ALIGNAK_USER=my-alignak
   ALIGNAK_GROUP=my-alignak

.. note:: that the Alignak user/group information are also configurable thanks to this feature. If you did not created the default proposed user account, you must update the default information.

To make Alignak start automatically when the system boots up::

   # Enable Alignak on system start
   sudo systemctl enable alignak.service

And to manage Alignak services::

   # Start Alignak daemons
   sudo systemctl start alignak

   # Stop Alignak daemons
   sudo systemctl stop alignak

Alignak daemons
---------------
The target and templating features of systemctl are used to declare all the daemons that need to be started before starting the Arbiter. See the service units installed files in */lib/systemd/system/* for more information and configuration.

.. note:: the *alignak.service* defines the daemons that will be involved in the monitoring configuration. Especially, this file allows to define several instances of each daemon that use each daemon type service template.

.. _run_alignak/services_freebsd:

FreeBSD services
================

The alignak repository contains an rc.d script that allows running Alignak daemons as system services. See the *bin/rc.d* directory in the project repository. the *alignak* file is commented to explain about its installation and usage. This script is able to operate on several alignak daemons instances. Defining which daemon is to be started is made thanks to configuration variables.

The *alignak* file is shipped by the installation process in the */usr/local/share/alignak/bin/rc.d* directory.Its header is commented to explain which configuration variables are available and what they are made for::

   #!/bin/sh

   # Configuration settings for an alignak-daemon instance in /etc/rc.conf:
   # $FreeBSD$
   #
   # PROVIDE: alignak
   # REQUIRE: LOGIN
   # KEYWORD: shutdown
   #
   # alignak_enable (bool):
   #   Default value: "NO"
   #   Flag that determines whether Alignak is enabled.
   #
   # alignak_prefix (string):
   #   Default value: "/usr/local"
   #   Alignak default installation prefix
   #
   # alignak_user (string):
   #   Default value: "alignak"
   #   Alignak default user - if set an ALIGNAK_USER environment variable will be defined
   #   Set a value to override the user configured in the Alignak configuration file
   #   If you are using the FreeBSD daemon, it will use this value to start the Alignak daemon
   #
   # alignak_group (string):
   #   Default value: "alignak"
   #   Alignak default user group - same as the user variable
   #
   # alignak_configuration (string):
   #   Default value: "/usr/local/share/alignak/etc/alignak.ini"
   #   Alignak configuration file name
   #
   # alignak_log_file (string):
   #   Default value: "/tmp/alignak.log"
   #   Alignak default log file name (used for configuration check reporting)
   #
   # alignak_pid_file (string):
   #   Default value: "/tmp/alignak.pid"
   #   Alignak default pid file name (used for configuration check reporting)
   #
   # alignak_daemonize (bool):
   #   Default value: "NO"
   #   Use the daemon FreeBSD utility to start the Alignak daemons
   #
   # alignak_daemon (bool):
   #   Default value: "YES"
   #   Start in daemon mode - each deamon will fork itself to daemonize
   #
   # alignak_replace (bool):
   #   Default value: "YES"
   #   Start in replace mode - replaces an existing daemon if a stale pid file exists
   #
   # alignak_flags (string):
   #   Default value: ""
   #   Extra parameters to be provided to the started script
   #
   # alignak_alignak_name (string):
   #   Default value: ""
   #   Alignak instance name
   #   Default is empty to get this parameter in the configuration file
   #
   # alignak_host (string):
   #   Default value: ""
   #   Interface listened to by the Alignak arbiter.
   #   Default is empty to get this parameter in the configuration file
   #
   # alignak_port (integer):
   #   Default value:
   #   Port listened to by the Alignak arbiter.
   #   Default is empty to get this parameter in the configuration file
   #
   # -------------------------------------------------------------------------------------------------
   # alignak rc.d script is able to operate on several alignak daemons instances
   # Defining which daemons are to be started is made thanks to these configuration variables:
   #
   # alignak_types (string list):
   #   Defines the daemons types to be started
   #   Default is all the daemon types: arbiter scheduler poller broker receiver reactionner
   #
   # alignak_arbiter_instances (string list):
   #   Defines the daemon instances to be started
   #   Default is all only one master instance: arbiter-master
   #
   # alignak_scheduler_instances (string list):
   #   Defines the daemon instances to be started
   #   Default is all only one master instance: scheduler-master
   #
   # alignak_broker_instances (string list):
   #   Defines the daemon instances to be started
   #
   # alignak_poller_instances (string list):
   #   Defines the daemon instances to be started
   #   Default is all only one master instance: poller-master
   #
   # alignak_reactionner_instances (string list):
   #   Defines the daemon instances to be started
   #   Default is all only one master instance: reactionner-master
   #
   # alignak_receiver_instances (string list):
   #   Defines the daemon instances to be started
   #   Default is all only one master instance: receiver-master
   #
   # -------------------------------------------------------------------------------------------------
   # Defining a specific Alignak daemons configuration is quite easy:
   # 1- define the daemons instances list
   # alignak_types="scheduler broker receiver"
   # 2- define each daemon instance for each daemons type
   # alignak_scheduler_instances="scheduler-realm-1 scheduler-realm-2"
   # alignak_broker_instances="broker-realm-1"
   # alignak_receiver_instances="receiver-realm-1 receiver-realm-2"
   # 3- define each daemon instance specific parameters
   # alignak_scheduler_realm_1_flags="-n scheduler-realm-1 -p 10000"
   # alignak_scheduler_realm_2_flags="-n scheduler-realm-2 -p 10001"
   # alignak_broker_realm_1_flags="-n broker-realm-1 -p 10002"
   # alignak_broker_realm_2_flags="-n broker-realm-2 -p 10003"
   # alignak_receiver_realm_1_flags="-n receiver-realm-1 -p 10004"
   # alignak_receiver_realm_2_flags="-n receiver-realm-2 -p 10005"

   # -------------------------------------------------------------------------------------------------
   # The default configuration is to have one instance for each daemon type:
   # alignak_types="broker poller reactionner receiver scheduler arbiter"
   # alignak_arbiter_instances="arbiter-master"
   # alignak_scheduler_instances="scheduler-master"
   # alignak_broker_instances="broker-master"
   # alignak_poller_instances="poller-master"
   # alignak_reactionner_instances="reactionner-master"
   # alignak_receiver_instances="receiver-master"

   # Each daemon instance has its own specific port
   # alignak_arbiter_arbiter_master_port="7770"
   # alignak_scheduler_scheduler_master_port="7768"
   # alignak_broker_broker_master_port="7772"
   # alignak_poller_poller_master_port="7771"
   # alignak_reactionner_reactionner_master_port="7769"
   # alignak_receiver_receiver_master_port="7773"
   # -------------------------------------------------------------------------------------------------

   #
   # -------------------------------------------------------------------------------------------------
   # When types and instances are specified, the non-type specific parameters defined
   # previously (upper) become the default values for the type/instance specific parameters.
   #
   # Example:
   # If no specific "alignak_arbiter_arbiter_master_host" variable is defined then the default
   # "alignak_host" variable value will be used the the arbiter arbiter-master daemon host
   # variable.


Configure the ``alignak`` system service in the */etc/rc.conf* file::

   # Simply use the default parameters
   echo 'alignak="YES"' >> /etc/rc.conf
   # And define your own configuration file
   echo 'alignak_configuration="/usr/local/etc/my_alignak_configuration.ini"' >> /etc/rc.conf

As an example, the content of an */etc/rc.conf.d/alignak*::

   #rc_debug="YES"
   # Information in the service script
   rc_info="YES"
   alignak_enable="YES"
   # No /usr/local prefix (eg. /var/log/alignak for the log files)
   alignak_prefix=""
   alignak_config="/usr/local/share/alignak/etc/alignak.ini"
   # Declare 3 schedulers
   alignak_scheduler_instances="scheduler-master scheduler-master-2 scheduler-master-3"
   alignak_scheduler_scheduler_master_port="7768"
   alignak_scheduler_scheduler_master_2="17768"
   alignak_scheduler_scheduler_master_3="27768"
   # Declare 2 receivers
   alignak_receiver_instances="receiver-master receiver-nsca"
   alignak_receiver_receiver_nsca="17773"


.. tip:: rather than updating the */etc/rc.conf* file, you can create an */etc/rc.conf.d/alignak* file for all the configuration variables!

.. tip:: configure ``rc_info=YES`` in the */etc/rc.conf* file to have some information message on the console and in the system log. You can also configure the ``rc_debug=YES`` to have more detailed information about each alignak daemon configuration!

To manage Alignak services::

      # Start Alignak daemons
      sudo service alignak start

      # Stop Alignak daemons
      sudo service alignak stop

      # Check Alignak configuration
      sudo service alignak check
      # Creates a /tmp/alignak/log file with the configuration parsing result



.. _run_alignak/shell:

Shell script
============

Starting each daemon individually is the old plain start method inherited from Shinken and from the very first Alignak version.

Running all the Alignak daemons::

    $ alignak-broker -n broker-master -e /usr/local/etc/alignak/alignak.ini
    $ alignak-scheduler -n scheduler-master -e /usr/local/etc/alignak/alignak.ini
    $ alignak-poller -n poller-master -e /usr/local/etc/alignak/alignak.ini
    $ alignak-reactionner -n reactionner-master -e /usr/local/etc/alignak/alignak.ini
    $ alignak-receiver -n receiver-master -e /usr/local/etc/alignak/alignak.ini

    # And the last, but not the least...
    $ alignak-arbiter -e /usr/local/etc/alignak/alignak.ini

This, because the default shipped configuration file is built in a manner that it considers all the other the daemons are still started when the arbiter starts.

It is possible to start only the arbiter and make it start all the other daemons by itself. Edit the *alignak.ini*  configuration file and set the `alignak_launched` variable to 1. This can be configured for all the daemons or on a per-daemon basis ... see :ref:`core configuration <configuration/core>` for more information.

When the arbiter is started with the `alignak_launched` variable set, it will start / stop the other configured daemons. While it is running the arbiter daemon will check if all the other daemons processes are still running and it will restart them if they exit. As such, running the Alignak framework is only::

    $ alignak-arbiter -e /usr/local/etc/alignak/alignak.ini

Starting a daemon
-----------------

As an example, starting a daemon from the shell::

   [2018-06-18 14:42:02] INFO: [scheduler-master.alignak.daemon] -----
   [2018-06-18 14:42:02] INFO: [scheduler-master.alignak.daemon] Alignak 1.1.0rc5 - scheduler-master daemon
   [2018-06-18 14:42:02] INFO: [scheduler-master.alignak.daemon] Copyright (c) 2015-2018: Alignak Team
   [2018-06-18 14:42:02] INFO: [scheduler-master.alignak.daemon] License: AGPL
   [2018-06-18 14:42:02] INFO: [scheduler-master.alignak.daemon] -----
   [2018-06-18 14:42:02] INFO: [scheduler-master.alignak.daemon] My pid: 10948
   [2018-06-18 14:42:02] INFO: [scheduler-master.alignak.daemon] Daemon 'scheduler-master' is started with an environment file: /usr/local/share/alignak/etc/alignak.ini
   [2018-06-18 14:42:02] INFO: [scheduler-master.alignak.daemon] Daemon 'scheduler-master' pid file: /usr/local/var/run/alignak/scheduler-master.pid
   [2018-06-18 14:42:02] INFO: [scheduler-master.alignak.daemon] Using working directory: /usr/local/var/run/alignak
   [2018-06-18 14:42:02] INFO: [scheduler-master.alignak.daemon] Daemonizing...
   [2018-06-18 14:42:02] INFO: [scheduler-master.alignak.daemon] Do not close fd: 3
   [2018-06-18 14:42:02] INFO: [scheduler-master.alignak.daemon] We are now fully daemonized :) pid=10948
   [2018-06-18 14:42:02] INFO: [scheduler-master.alignak.daemon] Setting up HTTP daemon (0.0.0.0:7768), 32 threads
   [2018-06-18 14:42:02] INFO: [scheduler-master.alignak.http.daemon] Configured HTTP server on http://0.0.0.0:7768, 32 threads
   [2018-06-18 14:42:02] INFO: [scheduler-master.alignak.daemon] Starting http_daemon thread
   [2018-06-18 14:42:02] INFO: [scheduler-master.alignak.daemon] HTTP daemon thread started
   [2018-06-18 14:42:02] INFO: [scheduler-master.alignak.daemon] Waiting for initial configuration

After a first initialization phase, the daemon stops its execution unitl it receives a configuration sent by the arbiter. Once received, the daemon loads the configuration::

   [2018-06-18 14:42:03] INFO: [scheduler-master.alignak.scheduler] Disabling the scheduling loop...
   [2018-06-18 14:42:03] INFO: [scheduler-master.alignak.http.generic_interface] My Arbiter wants me to wait for a new configuration.
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.daemon] Got initial configuration, waited for: 2.01
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.satellite] Received a new configuration (arbiters / schedulers)
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.satellite] My Alignak instance: My Alignak
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.daemons.schedulerdaemon] Monitored configuration <Config Config_2 - Alignak global configuration (0) /> received at 1529325724. Un-serialized in 0 secs
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.daemons.schedulerdaemon] Scheduler received configuration : <Config Config_2 - Alignak global configuration (0) />
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.daemons.schedulerdaemon] - received PollerLink_1 - poller: poller-master
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.daemons.schedulerdaemon] I got a new pollers satellite: <PollerLink_1 - poller/poller-master, http//127.0.0.1:7771, rid: 0, spare: False, managing:  () />
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.daemons.schedulerdaemon] - received ReactionnerLink_1 - reactionner: reactionner-master
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.daemons.schedulerdaemon] I got a new reactionners satellite: <ReactionnerLink_1 - reactionner/reactionner-master, http//127.0.0.1:7769, rid: 0, spare: False, managing:  () />
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.daemons.schedulerdaemon] - received BrokerLink_1 - broker: broker-master
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.daemons.schedulerdaemon] I got a new brokers satellite: <BrokerLink_1 - broker/broker-master, http//127.0.0.1:7772, rid: 0, spare: False, managing:  () />
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.daemons.schedulerdaemon] Modules configuration: []
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.daemons.schedulerdaemon] I do not have modules
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.daemons.schedulerdaemon] Loading configuration...
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.scheduler] Scheduling loop reset
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.scheduler] loading my configuration (SchedulerLink_1 / Config_2):
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.scheduler] Set my scheduler instance: SchedulerLink_1 - scheduler-master - None
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.daemons.schedulerdaemon] Loaded: <Config Config_2 - Alignak global configuration (0) />
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.scheduler] Retention data loaded: 0.00 seconds
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.daemons.schedulerdaemon] Initializing connection with my satellites:
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.daemons.schedulerdaemon] - : broker/broker-master
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.objects.satellitelink]   get the running identifier for broker broker-master.
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.objects.satellitelink]   -> got the running identifier for broker broker-master: 1529325722.54579368.
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.daemons.schedulerdaemon] - : poller/poller-master
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.objects.satellitelink]   get the running identifier for poller poller-master.
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.objects.satellitelink]   -> got the running identifier for poller poller-master: 1529325722.43028172.
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.daemons.schedulerdaemon] - : reactionner/reactionner-master
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.objects.satellitelink]   get the running identifier for reactionner reactionner-master.
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.objects.satellitelink]   -> got the running identifier for reactionner reactionner-master: 1529325722.78737948.
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.daemons.schedulerdaemon] Loaded: <Config Config_2 - Alignak global configuration (0) />
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.scheduler] Enabling the scheduling loop...
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.daemon] pause duration: 0.50
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.daemon] maximum expected loop duration: 1.00
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.scheduler] Disabling the scheduling loop...
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.daemon] starting main loop: 1529325724.44
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.daemons.schedulerdaemon] First scheduling launched
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.daemons.schedulerdaemon] First scheduling done
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.scheduler] Enabling the scheduling loop...

Then, the daemon start its background loop::

   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.daemon] Daemon scheduler-master is living: loop #1 ;)

   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.http.scheduler_interface] A new broker just connected : broker-master
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.scheduler] Filling initial broks for: broker-master (7478fa0a-4549-4bfe-9522-7683fe1e36e5)
   [2018-06-18 14:42:04] INFO: [scheduler-master.alignak.scheduler] Created 7 initial broks for broker-master

On stop request, the daemon runs its ending phase::

   [2018-06-18 14:44:35] INFO: [scheduler-master.alignak.daemon] received a signal: SIGINT
   [2018-06-18 14:44:35] INFO: [scheduler-master.alignak.daemon] request to stop the daemon
   [2018-06-18 14:44:35] INFO: [scheduler-master.alignak.daemon] Someone asked us to stop now
   [2018-06-18 14:44:35] INFO: [scheduler-master.alignak.scheduler] Retention data saved: 0.00 seconds
   [2018-06-18 14:44:35] INFO: [scheduler-master.alignak.daemon] Stopping scheduler-master...
   [2018-06-18 14:44:35] INFO: [scheduler-master.alignak.daemon] Shutting down synchronization manager...
   [2018-06-18 14:44:35] INFO: [scheduler-master.alignak.daemon] received a signal: SIGINT
   [2018-06-18 14:44:35] INFO: [scheduler-master.alignak.daemon] request to stop the daemon
   [2018-06-18 14:44:35] INFO: [scheduler-master.alignak.daemon] Shutting down modules manager...
   [2018-06-18 14:44:35] INFO: [scheduler-master.alignak.modulesmanager] Shutting down modules...
   [2018-06-18 14:44:35] INFO: [scheduler-master.alignak.daemon] Shutting down HTTP daemon...
   [2018-06-18 14:44:40] INFO: [scheduler-master.alignak.daemon] Checking HTTP thread...
   [2018-06-18 14:44:40] INFO: [scheduler-master.alignak.daemon] Stopped scheduler-master.



Daemons command line parameters
-------------------------------
All the Alignak daemons have a startup script that can be launched with command line parameters. These scripts have been installed by the Python installation process (or the distro packaging).

All the Alignak daemons need to be started with high privileges (root or sudo) that they will downgrade to a configured user/group account. The user they will use will need to have some permissions on the daemon working directory. See :ref:`core configuration <configuration/core>` for more information.

The only necessary configuration to provide to the daemons when they get started is:

    - the daemon name for the daemon to be able to find out its configuration (`-n`)
    - the *alignak.ini* file installed by the setup process (`-e`).

Where to find the *alignak.ini* file:

   - in the */usr/local/etc/alignak* (or */etc/alignak*) directory

Except for the environment file and the daemon name, all other command line parameters are optional because default values are used by the daemon when it starts.

The daemon will get its configuration parameters from the *alignak.ini* environment file in the section named as *[daemon.daemon-name]*. The daemon will also use some default values if they are not defined:

    - it will create its pid (*daemon-name.pid*) and log (*daemon-name.log*) file in the current working directory.
    - it will also use a default port to listen to the other daemons (arbiter: 7770, scheduler: 7768, broker: 7772, poller: 7771, reactionner: 7769, receiver: 7773).

For all the daemons (broker, poller, receiver, reactionner, scheduler)::

   $ alignak-broker -h
      usage: alignak-broker [-h] -n DAEMON_NAME [-c CONFIG_FILE] [-d] [-r] [-vv]
                            [-v] [-o HOST] [-p PORT] [-l LOG_FILENAME]
                            [-i PID_FILENAME] -e ENV_FILE

      Alignak daemon launching

      optional arguments:
        -h, --help            show this help message and exit
        -n DAEMON_NAME, --name DAEMON_NAME
                              Daemon unique name. Must be unique for the same daemon
                              type.
        -c CONFIG_FILE, --config CONFIG_FILE
                              Daemon configuration file. Deprecated parameter, do
                              not use it anymore!
        -d, --daemon          Run as a daemon. Fork the launched process and
                              daemonize.
        -r, --replace         Replace previous running daemon if any pid file is
                              found.
        -vv, --debug          Set log level to debug mode (DEBUG)
        -v, --verbose         Set log level to verbose mode (INFO)
        -o HOST, --host HOST  Host interface used by the daemon. Default is 0.0.0.0
                              (all interfaces).
        -p PORT, --port PORT  Port used by the daemon. Default is set according to
                              the daemon type.
        -l LOG_FILENAME, --log_file LOG_FILENAME
                              File used for the daemon log. Set as empty to disable
                              log file.
        -i PID_FILENAME, --pid_file PID_FILENAME
                              File used to store the daemon pid
        -e ENV_FILE, --environment ENV_FILE
                              Alignak global environment file. This file defines all
                              the daemons of this Alignak instance and their
                              configuration. Each daemon configuration is defined in
                              a specifc section of this file.

      And that's it!



The arbiter is slightly different because it manages some extra parameters::

   $ alignak-arbiter -h
      usage: alignak-arbiter [-h] [-a LEGACY_CFG_FILES] [-V] [-k ALIGNAK_NAME]
                             [-n DAEMON_NAME] [-c CONFIG_FILE] [-d] [-r] [-vv] [-v]
                             [-o HOST] [-p PORT] [-l LOG_FILENAME] [-i PID_FILENAME]
                             -e ENV_FILE

      Alignak daemon launching

      optional arguments:
        -h, --help            show this help message and exit
        -a LEGACY_CFG_FILES, --arbiter LEGACY_CFG_FILES
                              Legacy configuration file(s). This option is still
                              available but is is preferable to declare the Nagios-
                              like objects files in the alignak-configuration
                              section of the environment file specified with the -e
                              option.Multiple -a can be used to include several
                              configuration files.
        -V, --verify-config   Verify the configuration file(s) and exit
        -k ALIGNAK_NAME, --alignak-name ALIGNAK_NAME
                              Set the name of the Alignak instance. If not set, the
                              arbiter name will be used in place. Note that if an
                              alignak_name variable is defined in the configuration,
                              it will overwrite this parameter.For a spare arbiter,
                              this parameter must contain its name!
        -n DAEMON_NAME, --name DAEMON_NAME
                              Daemon unique name. Must be unique for the same daemon
                              type.
        -c CONFIG_FILE, --config CONFIG_FILE
                              Daemon configuration file. Deprecated parameter, do
                              not use it anymore!
        -d, --daemon          Run as a daemon. Fork the launched process and
                              daemonize.
        -r, --replace         Replace previous running daemon if any pid file is
                              found.
        -vv, --debug          Set log level to debug mode (DEBUG)
        -v, --verbose         Set log level to verbose mode (INFO)
        -o HOST, --host HOST  Host interface used by the daemon. Default is 0.0.0.0
                              (all interfaces).
        -p PORT, --port PORT  Port used by the daemon. Default is set according to
                              the daemon type.
        -l LOG_FILENAME, --log_file LOG_FILENAME
                              File used for the daemon log. Set as empty to disable
                              log file.
        -i PID_FILENAME, --pid_file PID_FILENAME
                              File used to store the daemon pid
        -e ENV_FILE, --environment ENV_FILE
                              Alignak global environment file. This file defines all
                              the daemons of this Alignak instance and their
                              configuration. Each daemon configuration is defined in
                              a specifc section of this file.

      And that's it!

As a sump up:

   All daemons:
      **'-n', "--name":**

      Set the name of the daemon to pick in the configuration files.

      This allows the daemon to find its own configuration in the whole Alignak configuration
      Using this parameter is mandatory for all the daemons except for the arbiter (defaults to arbiter-master). If several arbiters are existing in the configuration this will allow to determine which one is the master/spare. The spare arbiter must be launched with this parameter!

      **'-e', '--environment':**

      Alignak environment file - the most important and mandatory parameter to define the name of the alignak.ini configuration file

      **'-c', '--config':**

      Old daemon configuration file (ini file) - deprecated! This parameter is still managed to alert about its deprecation and to maintain compatibility with former daemon startup scripts.

      **'-v', '--verbose':**

      Set the daemon log to level INFO

      **'-vv', '--debug':**

      Set the daemon log to level DEBUG

      **'-d', '--daemon':**

      Run as a daemon. The launched process will fork itself to run as a system daemon

      **'-r', '--replace':**

      Replace previous running daemon if it exists. Read the PID file end kills the corresponding process

      **'-o', '--host':** interface the daemon will listen to
      **'-p', '--port':** port the daemon will listen to
      **'-l', '--log_file':** set the daemon log file name
      **'-i', '--pid_file':** set the daemon pid file name

      These parameters allow to override the one defined in the Alignak configuration file

   Arbiter only:
      **"-a", "--arbiter":** Legacy configuration file(s),

      (multiple -a can be used, and they will be concatenated to make a global configuration file)

      Note that this parameter is not necessary anymore because the Nagios legacy configuration files may be defined in the alignak.ini configuration file

      **"-V", "--verify-config":** Verify configuration file(s) and exit

      This is very useful to check the configuration file after some modificationsand before starting Alignak.


Arbiter daemon exit codes
-------------------------

The arbiter dameon has some process exit code. Their meaning is:

   - 0: everything ok. Arbiter requested to stop and stopped as expected
   - 1: provided configuration parsing error detected and the arbiter stopped
   - 2: some necessary files declared in the configuration are missing
   - 3: an error was raised during the daemon initialization/fork
   - 4: running daemons connection problems when checking daemon communication or dispatching the configuration
   - 99: the provided environment configuration file is not available


.. _run_alignak/ps:

Alignak processes list
======================

The daemons involved in Alignak are starting several processes in the system. All the processes started have a process title set by Alignak to help the user know which is which. Several processes types are present in the system processes list:

    * the main daemon process
        There will always be one process for each Alignak daemon type. The process title is built with the daemon type and the daemon name (eg. *alignak-arbiter arbiter-master*, *alignak-scheduler scheduler-other*,...)

    * the main daemon forked process.
        Each Alignak daemon forks a new process instance for each daemon instance existing in the configuration. If you defined several schedulers you will get a process for each scheduler instance. Each daemon instance process has a title built with the instance name (eg. *alignak-scheduler scheduler-master*)

    * the external modules processes
        The daemons that have some external modules attached, like brokers or receivers, launch new processes for their modules. These processes titles are made of the daemon instance name and the module alias (eg. *alignak-receiver-master module: nsca*)

    * the satellite workers processes
        The satellites daemons that need some worker processes (pollers and reactionners) launch several worker processes to execute their actions (checks, event handlers or notifications). These worker processes have a title made of the daemon instance name and the worker label (eg. *alignak-poller-master worker*)


Each daemon is also starting some threads for its HTTP interface.

As an example, the processes list of an Alignak configuration with one instance of each daemon started in daemonized mode::

   11921 alignak   20   0  983360  46752   5004 S  0,4  2,3   0:01.96  `- alignak-receiver receiver-master                                                     1
   11923 alignak   20   0  171564  39836   3588 S  0,0  1,9   0:00.00      `- alignak-receiver receiver-master                                             11921
   11924 alignak   20   0  984632  52236   5460 S  0,7  2,5   0:03.90  `- alignak-arbiter arbiter-master                                                       1
   11927 alignak   20   0  171636  39156   2860 S  0,0  1,9   0:00.00      `- alignak-arbiter arbiter-master                                               11924
   11925 alignak   20   0  984212  49528   5040 S  1,2  2,4   0:04.95  `- alignak-scheduler scheduler-master                                                   1
   11931 alignak   20   0  171588  39368   2956 S  0,0  1,9   0:00.00      `- alignak-scheduler scheduler-master                                           11925
   11932 alignak   20   0  983768  49152   5196 S  1,7  2,4   0:07.44  `- alignak-broker broker-master                                                         1
   11933 alignak   20   0  171576  39296   3016 S  0,0  1,9   0:00.00      `- alignak-broker broker-master                                                 11932
   11935 alignak   20   0  983640  49160   5076 S  0,9  2,4   0:03.67  `- alignak-poller poller-master                                                         1
   11938 alignak   20   0  171568  39748   3504 S  0,0  1,9   0:00.00      `- alignak-poller poller-master                                                 11935
   12152 alignak   20   0  983384  47100   3128 S  0,0  2,3   0:00.06      `- alignak-poller-master worker fork_1                                          11935
   11939 alignak   20   0  983636  49248   4996 S  0,9  2,4   0:03.78  `- alignak-reactionner reactionner-master                                               1
   11975 alignak   20   0  171564  39748   3512 S  0,0  1,9   0:00.00      `- alignak-reactionner reactionner-master                                       11939
   12153 alignak   20   0  983380  47572   3444 S  0,0  2,3   0:00.06      `- alignak-reactionner-master worker fork_1                                     11939

.. note:: the parent PI (PPID) of the main process of each daemon is 1!

As an example, here is the processes list of an Alignak configuration with several daemons of each type and some modules attached to some of the deamons::

    $ ps -aux | grep alignak-
    alignak   3432 10.2  0.5 1063940 64728 pts/2   Sl+  13:57   0:02 alignak-arbiter arbiter-master
    alignak   3441  0.0  0.3 265972 44132 pts/2    S+   13:57   0:00 alignak-arbiter arbiter-master

    alignak   3510  5.7  0.4 1061692 60000 pts/2   Sl+  13:57   0:01 alignak-receiver receiver-master
    alignak   3608  0.1  0.3 397196 44904 pts/2    Sl+  13:57   0:00 alignak-receiver receiver-master
    alignak   3505  5.6  0.4 1061664 59920 pts/2   Sl+  13:57   0:01 alignak-receiver receiver-master2
    alignak   3596  0.0  0.3 397044 44904 pts/2    Sl+  13:57   0:00 alignak-receiver receiver-master2
    alignak   3768  0.4  0.4 1062540 50072 pts/2   S+   13:57   0:00 alignak-receiver-master module: web-services
    alignak   3784  0.2  0.4 1062540 50068 pts/2   S+   13:57   0:00 alignak-receiver-master2 module: web-services

    alignak   3513  6.1  0.4 1061428 59420 pts/2   Sl+  13:57   0:01 alignak-reactionner reactionner-master
    alignak   3633  0.0  0.3 265676 44096 pts/2    S+   13:57   0:00 alignak-reactionner reactionner-master
    alignak   3720  0.0  0.3 1061004 47280 pts/2   S+   13:57   0:00 alignak-reactionner-master worker fork_1
    alignak   3721  0.0  0.3 1061016 47296 pts/2   S+   13:57   0:00 alignak-reactionner-master worker fork_2
    alignak   3722  0.0  0.3 1061164 47304 pts/2   S+   13:57   0:00 alignak-reactionner-master worker fork_3

    alignak   3520  5.7  0.4 1061416 59300 pts/2   Sl+  13:57   0:01 alignak-poller poller-master
    alignak   3619  0.0  0.3 265676 44128 pts/2    S+   13:57   0:00 alignak-poller poller-master
    alignak   3756  0.0  0.3 1061004 47480 pts/2   S+   13:57   0:00 alignak-poller-master worker fork_1
    alignak   3757  0.0  0.3 1061016 47812 pts/2   S+   13:57   0:00 alignak-poller-master worker fork_2
    alignak   3758  0.0  0.3 1061028 47500 pts/2   S+   13:57   0:00 alignak-poller-master worker fork_3
    alignak   3527  6.1  0.4 1061424 59320 pts/2   Sl+  13:57   0:01 alignak-poller poller-other
    alignak   3672  0.0  0.3 265676 44128 pts/2    S+   13:57   0:00 alignak-poller poller-other
    alignak   3737  0.0  0.3 1061004 47580 pts/2   S+   13:57   0:00 alignak-poller-other worker fork_1
    alignak   3738  0.0  0.3 1061016 47984 pts/2   S+   13:57   0:00 alignak-poller-other worker fork_2
    alignak   3739  0.0  0.3 1061028 47800 pts/2   S+   13:57   0:00 alignak-poller-other worker fork_3

    alignak   3549  6.2  0.5 1062340 61128 pts/2   Sl+  13:57   0:01 alignak-scheduler scheduler-master
    alignak   3684  0.0  0.3 266364 44380 pts/2    S+   13:57   0:00 alignak-scheduler scheduler-master
    alignak   3542  6.3  0.5 1062472 62944 pts/2   Sl+  13:57   0:01 alignak-scheduler scheduler-master2
    alignak   3660  0.0  0.3 266364 44400 pts/2    S+   13:57   0:00 alignak-scheduler scheduler-master2
    alignak   3556  6.2  0.5 1062340 61384 pts/2   Sl+  13:57   0:01 alignak-scheduler scheduler-other
    alignak   3708  0.0  0.3 266364 44396 pts/2    S+   13:57   0:00 alignak-scheduler scheduler-other

    alignak   3690  0.4  0.3 618216 45064 pts/2    Sl+  13:57   0:00 alignak-broker broker-master
    alignak   3538  7.5  0.4 1062252 60076 pts/2   Sl+  13:57   0:01 alignak-broker broker-master
    alignak   3764  0.5  0.4 1062320 50300 pts/2   S+   13:57   0:00 alignak-broker-master module: backend_broker
    alignak   3786  0.1  0.4 1062060 49568 pts/2   S+   13:57   0:00 alignak-broker-master module: logs
    alignak   3530  6.5  0.4 1061668 59836 pts/2   Sl+  13:57   0:01 alignak-broker broker-other
    alignak   3632  0.2  0.3 617960 44540 pts/2    Sl+  13:57   0:00 alignak-broker broker-other
    alignak   3729  0.4  0.4 1061808 49176 pts/2   S+   13:57   0:00 alignak-broker-other module: backend_broker


.. _run_alignak/signals:

Alignak system signals
======================

The Alignak daemons listen some system signals:

    * SIGHUP
        configuration reload

    * SIGKILL
        daemon forced stop

    * SIGTERM
        daemon stop

    * SIGUSR1
         Alignak environment dump. The daemon receiving the SIGUSR1 signal will dump its loaded environment to a file in the system temporary files directory. the file name is formated as ``dump-env-%s-%s-%d.ini`` with the daemon type, daemon name and a timestamp.
      .. note:: that all the daemons should write a file with the same content;)

    * SIGUSR2
         The scheduler daemon receiving the SIGUSR2 signal will dump its monitored objects to a file in the system temporary files directory. The file name is formated as ``dump-cfg-scheduler-%s-%d.ini`` with the daemon name and a timestamp.

         The scheduler daemon will dump its inner objects (checks, actions) to a file in the system temporary files directory. The file name is formated as ``dump-obj-scheduler-%s-%d.json`` file with the daemon name and a timestamp.
      .. note:: that the scheduler daemons are the only concerned daemons
