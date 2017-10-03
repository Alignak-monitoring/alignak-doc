.. _configuration/core:

==================
Core configuration
==================

The core configuration part describes the Alignak framework infrastructure (which daemons are used and how they are) and main configuration.

The Alignak environment file (*alignak.ini*) aims to contain all the Alignak configuration, except the monitored system tha tmay be configured separately in plain old Cfg files or in the Alignak backend.

If your monitored system configuration is stored in the Alignak backend, the *alignak.ini* file is the only configuration file you will have to manage ;)

Alignak environment file
========================

The Alignak environment file contains the necessary information about:

    - the alignak installation directories
    - the alignak daemons and their configuration
    - the alignak monitored configuration

This file is structured as an Ini file with some sections.

The default shipped file is hugely commented to explain which variable is used for what...

[DEFAULT]
---------
The **DEFAULT** section is the main section of this file. It defines some *global* variables that will be inherited by all the other configuration sections. For more information about this inheritance, check the Python configuration parser.

The variables defined in this section are the main Alignak configuration.

[alignak-configuration]
-----------------------

The **alignak-configuration** section is used to define the monitored system configuration files (eg the plain old Nagios cfg files).
If your system is configured in flat files, you can declare the configuration files in this section: ::

    CFG=my_nagios.cfg
    CFG2=extra_file.cfg

[daemon.daemon-name]
--------------------

The **daemon.*** sections allow to declare all the daemons that are involved in the Alignak framework.

Each daemon has its own section with its specific parameters. Remember that the **DEFAULT** section variables are inherited and, thus, you only need to declare the specific ones in the own daemon section.

[module.module-name]
--------------------

The **module.*** sections allow to declare all the extra modules that are used and their configuration.


Default shipped file
--------------------

The arbiter is the main Alignak daemon. As of it, its configuration part is the most important one.


::

    #
    # This configuration file is the main Alignak configuration entry point. Each Alignak installer
    # will adapt the content of this file according to the installation process. This will allow
    # any Alignak extension or third party application to find where the Alignak components and
    # files are located on the system.
    #
    # ---
    # This version of the file contains variable that are suitable to run a single node Alignak
    # with all its daemon using the default configuration existing in the repository.
    #

    # Main alignak variables:
    # - _dist_BIN is where the launch scripts are located
    #   (Debian sets to /usr/bin)
    # - _dist_ETC is where we store the configuration files
    #   (Debian sets to /etc/alignak)
    # - _dist_VAR is where the libraries and plugins files are installed
    #   (Debian sets to /var/lib/alignak)
    # - _dist_RUN is the daemons working directory and where pid files are stored
    #   (Debian sets to /var/run/alignak)
    # - _dist_LOG is where we put log files
    #   (Debian sets to /var/log/alignak)
    #
    # The variables declared in this section will be inherited in all the other sections of this file!
    #
    [DEFAULT]
    _dist=/usr/local/
    _dist_BIN=%(_dist)s/bin
    _dist_ETC=%(_dist)s/etc/alignak
    _dist_VAR=%(_dist)s/var/lib/alignak
    _dist_RUN=%(_dist)s/var/run/alignak
    _dist_LOG=%(_dist)s/var/log/alignak

    #-- Generic configuration name
    config_name=Alignak global configuration

    #-- Username and group to run (defaults to current user)
    # If not defined, the current user account will be used instead. It is recommended
    # to define an alignak:alignak user/group account on your system.
    user=alignak
    group=alignak

    # Disabling security means allowing the daemons to run under root account
    # set to 1 to allow daemons running as root
    idontcareaboutsecurity=0


    #-- Path Configuration
    # paths variables values, if not absolute paths, they are relative to workdir.
    # using default values for following config variables value:
    workdir=%(_dist_RUN)s
    logdir=%(_dist_LOG)s
    etcdir=%(_dist_ETC)s

    ; Set to 0 for the daemon to be ignored by the arbiter
    ;active=0

    #-- Set to 0 to make the daemon run foreground (else daemonize mode)
    ;is_daemon=1

    #-- Set to 1 to make the arbiter launch the daemon process
    ;alignak_launched=1

    #-- Set to 1 if you want to replace a running daemon
    do_replace=1

    #-- SSL configuration --
    use_ssl=0
    # Paths for certificates files
    ;server_cert=%(etcdir)s/certs/server.crt
    ;server_key=%(etcdir)s/certs/server.key
    ;ca_cert=%(etcdir)s/certs/ca.pem

    ### Deprecated option - feel free to request for an implementation if needed
    ;hard_ssl_name_check=0
    ### Deprecated option - feel free to request for an implementation if needed
    ;server_dh=%(etcdir)s/certs/server.pem

    ##-- Realm
    ## Default value is the realm All
    realm=All

    #-- Daemon high availability mode
    # 1 for a spare daemon, 0 for the main daemon
    spare=0

    ; Daemon interface uses two different timeouts:
    ; - short for light data and long for heavy data exchanges
    #timeout=3          ; Short timeout
    #data_timeout=120   ; Long timeout

    #max_check_attempts=3   ; If ping fails N or more, then the node is set as dead

    #-- Debugging daemons
    ;debug=true
    ;debug_file=%(LOG)s/%(NAME)s-debug.log

    #-- Network configuration
    ; host is set to 0.0.0.0 to listen on all interfaces, set 127.0.0.1 for a local host
    ;host=0.0.0.0
    ; address is the IP address (or FQDN) used by the other daemons to contact the daemon
    ;address=127.0.0.1
    ; Port the daemon is listening to
    ;port=10000
    ; Number of threads the daemon is able to listen to
    ; Increase this number if some connection problems are raised; the more daemons exist the
    ;more this poll must be important
    ;thread_pool_size=30

    #-- pid file
    # The daemon will chdir into the workdir directory when launched
    # and it will create its pid file in this working dir
    # You can override this location with the pid_file variable
    ;pidfile=%(workdir)s/daemon.pid

    #-- Local log management --
    ; Python logger configuration
    logger_configuration=%(etcdir)s/alignak-logger.json

    ; Include the CherryPy daemon HTTP server log in the daemon log file
    ;log_cherrypy=1

    ## Advanced parameters:
    # Is the daemon linked to the schedulers of sub-realms or only for its own realm?
    # The default is that a daemon will also manage the sub realms of its realm. This parameter is
    # useful if you need to define some daemons dedicated to a specific realm
    # Make sure to avoid having several daemons of the same type for the same realm ;)
    ;manage_sub_realms=1

    # Is the daemon connected to the arbiters?
    # The default is that the daemon will have a relation with the Alignak arbiter
    # Handle this parameter with much care!
    # todo: more information about this is a must-have!
    ;manage_arbiters=0


    #-- External modules watchdog --
    # If a module got a brok queue() higher than this value, it will be
    # killed and restarted. Set to 0 to disable this behavior
    max_queue_size=0

    # --------------------------------------------
    # We also define the global Alignak parameters in this default section. As of it, all the
    # daemons will get those parameters made available
    # --------------------------------------------
    # Alignak instance name
    # This information is useful to get/store alignak global configuration in the Alignak backend
    # If you share the same backend between several Alignak instances, each instance must have its own
    # name. The default is to use the master arbiter name as Alignak instance name.
    # Else, you can uncomment this declaration and define your own Alignak instance name in this property
    # alignak_name=my_alignak
    alignak_name=My Alignak


    # --------------------------------------------------------------------
    # Notifications configuration
    # ---
    # Notifications are enabled/disabled
    enable_notifications=1

    # After a timeout, launched plugins are killed
    notification_timeout=30
    # --------------------------------------------------------------------


    # --------------------------------------------------------------------
    # Retention configuration
    # ---
    # Number of minutes between 2 retention save, default is 60 minutes
    # This is only used if retention is enabled
    # todo: move this parameter to the retention aware modules?
    retention_update_interval=60
    # --------------------------------------------------------------------


    # --------------------------------------------------------------------
    # Active checks configuration
    # ---
    # Active host/service checks are enabled/disabled
    execute_host_checks=1
    execute_service_checks=1

    # Max plugin output for the plugins launched by the pollers, in bytes
    #max_plugins_output_length=8192
    max_plugins_output_length=65536

    # After a timeout, launched plugins are killed
    # and the host state is set to a default value (2 for DOWN)
    # and the service state is set to a default value (2 for CRITICAL)
    #host_check_timeout=30
    #service_check_timeout=60
    #timeout_exit_status=2
    # --------------------------------------------------------------------


    # --------------------------------------------------------------------
    # Passive checks configuration
    # ---
    # Passive host/service checks are enabled/disabled
    accept_passive_host_checks=1
    accept_passive_service_checks=1

    # As default, passive host checks are HARD states
    #passive_host_checks_are_soft=0

    # Freshness check
    # Default is enabled for hosts and services
    #check_host_freshness=1
    #check_service_freshness=1
    # Default is 60 for hosts and services
    #host_freshness_check_interval=60
    host_freshness_check_interval=1200
    #service_freshness_check_interval=60
    service_freshness_check_interval=1800
    # Extra time for freshness check ...
    #additional_freshness_latency=15
    # --------------------------------------------------------------------


    # --------------------------------------------------------------------
    # Checks scheduler configuration
    # ---
    # Interval length and re-scheduling configuration
    # Do not change those values unless you are really sure to master what you are doing...
    # todo: confirm the real interest of those configuration parameters!
    #interval_length=60
    #auto_reschedule_checks=1
    #auto_rescheduling_interval=1
    #auto_rescheduling_window=180

    # Number of interval to spread the first checks for hosts and services
    # Default is 30
    #max_service_check_spread=30
    max_service_check_spread=5
    # Default is 30
    #max_host_check_spread=30
    max_host_check_spread=5
    # --------------------------------------------------------------------


    # --------------------------------------------------------------------
    # Flapping detection configuration
    # ---
    # Default is enabled
    #enable_flap_detection=1

    # Flapping threshold for hosts and services
    #low_service_flap_threshold=20
    #high_service_flap_threshold=30
    #low_host_flap_threshold=20
    #high_host_flap_threshold=30

    # flap_history is the lengh of history states we keep to look for flapping.
    # 20 by default, can be useful to increase it. Each flap_history increases cost:
    #    flap_history cost = 4Bytes * flap_history * (nb hosts + nb services)
    # Example: 4 * 20 * (1000+10000) ~ 900Ko for a quite big conf. So, go for it!
    #flap_history=20
    # --------------------------------------------------------------------


    # --------------------------------------------------------------------
    # Performance data configuration
    # ---
    # Performance data management is enabled/disabled
    #process_performance_data=1
    # Commands for performance data
    #host_perfdata_command=
    #service_perfdata_command=
    # --------------------------------------------------------------------


    # --------------------------------------------------------------------
    # Event handlers configuration
    # ---
    # Event handlers are enabled/disabled
    #enable_event_handlers=1

    # By default don't launch even handlers during downtime. Put 0 to
    # get back the default nagios behavior
    no_event_handlers_during_downtimes=1

    # Global host/service event handlers
    #global_host_event_handler=
    #global_service_event_handler=

    # After a timeout, launched plugins are killed
    #event_handler_timeout=30
    # --------------------------------------------------------------------


    # --------------------------------------------------------------------
    # External commands configuration
    # ---
    # External commands are enabled/disabled
    # check_external_commands=1
    # --------------------------------------------------------------------


    # --------------------------------------------------------------------
    # Impacts configuration
    # ---
    # Enable or not the state change on impact detection (like a host going unreachable
    # if a parent is DOWN for example). It's for services and hosts.
    # Note: defaults to 0 for Nagios old behavior compatibility
    #enable_problem_impacts_states_change=0
    enable_problem_impacts_states_change=1


    # if 1, disable all notice and warning messages at
    # configuration checking when arbiter checks the configuration.
    # Default is to log the notices and warnings
    #disable_old_nagios_parameters_whining=0
    disable_old_nagios_parameters_whining=1


    # --------------------------------------------------------------------
    # Environment macros configuration
    # ---
    # Disabling environment macros is good for performance. If you really need it, enable it.
    #enable_environment_macros=1
    enable_environment_macros=0


    # --------------------------------------------------------------------
    # Monitoring log configuration
    # ---
    # Note that alerts and downtimes are always logged
    # ---
    # --------------------------------------------------------------------
    # Notifications
    log_notifications=1

    # Services retries
    log_service_retries=1

    # Hosts retries
    log_host_retries=1

    # Event handlers
    log_event_handlers=1

    # Flappings
    log_flappings=1

    # Snapshots
    log_snapshots=1

    # External commands
    log_external_commands=1

    # Active checks
    log_active_checks=0

    # Passive checks
    log_passive_checks=0

    # Initial states
    log_initial_states=0
    # --------------------------------------------------------------------


    # --------------------------------------------------------------------
    # Timezone
    # --------------------------------------------------------------------
    # If you need to set a specific timezone to your deamons, update and uncomment this
    #use_timezone=Europe/Paris
    # --------------------------------------------------------------------


    # --------------------------------------------------------------------
    # Arbiter daemons part, when the arbiter starts some daemons by itself
    # This may happen if some hosts are defined in a realm that do not
    # have all its daemons defined or if the alignak_launched parameter is set for a daemon
    # --------------------------------------------------------------------
    # Daemons extra arguments
    #daemons_arguments=
    # Daemons log file
    daemons_log_folder=%(logdir)s
    # Default is to allocate a port number incrementally starting from the value defined here
    daemons_initial_port=7800

    # The arbiter is polling its satellites every polling_interval seconds
    polling_interval=5
    # The arbiter is checking the running daemons every daemons_check_period seconds
    # - the checking only concerns the daemons that got started by the arbiter
    daemons_check_period=5
    # Graceful stop delay
    # - beyond this period, the arbiter will force kill the daemons that it launched
    daemons_stop_timeout=30
    # Delay after daemons got started by the Arbiter
    ;daemons_start_timeout=0
    # --------------------------------------------------------------------


    # --------------------------------------------------------------------
    # Alignak internal metrics
    # Export all alignak inner performance metrics to a statsd server.
    # By default at localhost:8125 (UDP) with the alignak prefix
    # Default is not enabled
    # --------------------------------------------------------------------
    statsd_host = localhost
    statsd_port = 8125
    statsd_prefix = alignak
    statsd_enabled = 0
    # --------------------------------------------------------------------


    # --------------------------------------------------------------------
    # Scheduler loop configuration
    # Those parameters allow to configure the scheduler actions execution
    # period.
    # Each parameter is a scheduler recurrent action. On each scheduling
    # loop turn, the scheduler checks if the time is come to execute
    # the corresponding work.
    # Each parameter defines on which loop turn count the action is to be
    # executed. Considering a loop turn is 1 second, a parameter value set
    # to 10 will make the corresponding action to be executed every 10
    # seconds.
    # --------------------------------------------------------------------
    # BEWARE: changing some of those parameters may have unexpected
    # effects! Do not change unless you know what you are doing ;)
    # Some tips:
    # tick_check_freshness, allow to change the freshness check period
    # tick_update_retention, allow to change the retention save period
    # --------------------------------------------------------------------
    ;tick_update_downtimes_and_comments=1
    ;tick_schedule=1
    ;tick_check_freshness=10
    ;tick_consume_results=1
    ;tick_get_new_actions=1
    ;tick_scatter_master_notifications=1
    ;tick_get_new_broks=1
    ;tick_delete_zombie_checks=1
    ;tick_delete_zombie_actions=1
    ;tick_clean_caches=1
    ;tick_update_retention=3600
    tick_update_retention=1800
    ;tick_check_orphaned=60
    tick_update_program_status=10
    ;tick_check_for_system_time_change=1
    ;tick_manage_internal_checks=1
    ;tick_clean_queues=1
    ; ### Note that if it set to 0, the scheduler will never try to clean its queues for oversizing
    tick_clean_queues=10
    ;tick_update_business_values=60
    ;tick_reset_topology_change_flags=1
    ;tick_check_for_expire_acknowledge=1
    ;tick_send_broks_to_modules=1
    ;tick_get_objects_from_from_queues=1
    ;tick_get_latency_average_percentile=10



    # We define the name of the 2 main Alignak configuration files.
    # There may be 2 configuration files because tools like Centreon generate those...
    [alignak-configuration]
    # Alignak main configuration file
    # Declaring this file is useful only if you have some items declared in old legacy Cfg files
    ;CFG=%(etcdir)s/alignak.cfg
    # Alignak secondary configuration file (none as a default)
    ;CFG2=


    # For each Alignak daemon, this file contains a section with the daemon name. The section
    # identifier is the corresponding daemon name prefixed with the keyword daemon and a dot.
    # This daemon name is usually built with the daemon type (eg. arbiter, poller,...) and the
    # daemon name separated with a dash.
    #
    # The previous rules ensure that Alignak will be able to find all the daemons configuration
    # in this file whatever the number of daemons existing in the configuration
    #
    # To be easily used as a configuration variable of this file, the daemon name is repeated
    # inside the section in a NAME variable.
    #
    # Each section inherits from the [DEFAULT] section and only defines the specific values
    # inherent to the declared daemon.

    [daemon.arbiter-master]
    type=arbiter
    name=arbiter-master

    #-- Network configuration
    # My listening interface
    ;host=0.0.0.0
    port=7770
    # My adress for the other daemons
    ;address=127.0.0.1

    ## Modules
    # Default: None
    ## Interesting modules:
    # - backend_arbiter     = get the monitored objects configuration from the Alignak backend
    ;modules=backend_arbiter


    [daemon.scheduler-master]
    type=scheduler
    name=scheduler-master

    #-- Launched by Alignak or yet launched
    ;alignak_launched=False

    #-- Network configuration
    # My listening interface
    ;host=0.0.0.0
    port=7768
    # My adress for the other daemons
    ;address=127.0.0.1

    ## Modules
    # Default: None
    # Interesting modules that can be used:
    # - backend_scheduler   = store the live state in the Alignak backend (retention)
    ;modules=backend_scheduler

    ## Advanced Features:
    # Skip initial broks creation. Boot fast, but some broker modules won't
    # work with it! (like livestatus for example)
    skip_initial_broks=0

    # Some schedulers can manage more hosts than others
    weight=1

    # In NATted environments, you declare each satellite ip[:port] as seen by
    # *this* scheduler (if port not set, the port declared by satellite itself
    # is used)
    ;satellitemap=poller-1=1.2.3.4:7771, reactionner-1=1.2.3.5:7769, ...

    # Does it accept passive check results for unknown hosts?
    accept_passive_unknown_check_results=1

    [daemon.poller-master]
    type=poller
    name=poller-master

    #-- Network configuration
    ;address=127.0.0.1
    port=7771

    ## Modules
    # Default: None
    ## Interesting modules:
    # - nrpe-booster        = Replaces the check_nrpe binary to enhance performance for NRPE checks
    # - snmp-booster        = Snmp bulk polling module
    ;modules=nrpe-booster

    ## Advanced parameters:
    manage_sub_realms=1        ; Does it take jobs from schedulers of sub-Realms?
    min_workers=0              ; Starts with N processes (0 = 1 per CPU)
    max_workers=0              ; No more than N processes (0 = 1 per CPU)
    processes_by_worker=256    ; Each worker manages N checks
    worker_polling_interval=1  ; Get jobs from schedulers each N seconds

    ## Passive mode
    # In active mode (default behavior), connections are poller -> scheduler to report checks results
    # For DMZ monitoring, set to 1 for the connections to be from scheduler -> poller.
    #passive=0

    ## Poller tags
    # Poller tags are the tag that the poller will manage. Use None as tag name to manage
    # untagged checks (default)
    #poller_tags=None

    [daemon.reactionner-master]
    type=reactionner
    name=reactionner-master

    #-- Network configuration
    ;address=127.0.0.1
    port=7769

    ## Modules
    # Default: None
    # Interesting modules that can be used:
    # - nothing currently
    ;modules

    ## Advanced parameters:
    manage_sub_realms=1        ; Does it take jobs from schedulers of sub-Realms?
    min_workers=0              ; Starts with N processes (0 = 1 per CPU)
    max_workers=1              ; No more than N processes (0 = 1 per CPU)
    processes_by_worker=256    ; Each worker manages N checks
    worker_polling_interval=1  ; Get jobs from schedulers each N seconds

    ## Passive mode
    # In active mode (default behavior), connections are poller -> scheduler to report checks results
    # For DMZ monitoring, set to 1 for the connections to be from scheduler -> poller.
    #passive=0

    ## Reactionner tags
    # Reactionner tags are the tag that the reactionner will manage. Use None as tag name to manage
    # untagged checks (default)
    #reactionner_tags=None

    [daemon.broker-master]
    type=broker
    name=broker-master

    #-- Network configuration
    ;address=127.0.0.1
    port=7772

    #-- External modules watchdog --
    # The broker daemon has a huge queue size.
    max_queue_size=100000

    ## Advanced parameters:
    # Does it receive for schedulers of sub-Realms or only for its realm?
    manage_sub_realms=1

    # Does it manage arbiters?
    manage_arbiters=1

    ## Modules
    # Default: None
    # Interesting modules that can be used:
    # - backend_broker      = update the live state in the Alignak backend
    # - logs                = collect monitoring logs and send them to a Python logger
    ;modules=backend_broker, logs

    [daemon.receiver-master]
    type=receiver
    name=receiver-master

    #-- Network configuration
    ;address=127.0.0.1
    port=7773

    ## Modules
    # Default: None
    # Interesting modules that can be used:
    # - nsca                = NSCA protocol server for collecting passive checks
    # - external-commands   = read a nagios commands file to notify external commands
    # - web-services        = expose Web services to get Alignak daemons state and
    #                         notify external commands
    ;modules=nsca,external-commands,web-services

    ## Advanced parameters:
    # Does it receive for schedulers of sub-Realms or only for its realm?
    manage_sub_realms=1

    # Does it manage arbiters?
    manage_arbiters=1
    # Does it accept passive check results for unknown hosts?
    accept_passive_unknown_check_results=1

    # For each Alignak module, this file contains a section with the module configuration.
    ;[module.example]
    ;# --------------------------------------------------------------------
    ;# The module inherits from the global configuration defined in the
    ;# DEFAULT section
    ;# only specific module configuration may be set here
    ;# --------------------------------------------------------------------
    ;name=Example
    ;type=type1,type2
    ;python_name=alignak_module_example
    ;
    ;# --------------------------------------------------------------------
    ;# Module internal metrics
    ;# Export module metrics to a statsd server.
    ;# By default at localhost:8125 (UDP) with the alignak prefix
    ;# Default is not enabled
    ;# --------------------------------------------------------------------
    ;;statsd_host = localhost
    ;;statsd_port = 8125
    ;;statsd_prefix = alignak
    ;;statsd_enabled = 0
    ;# --------------------------------------------------------------------
    ;
    ;# Module log level
    ;;log_level=INFO
    ;
    ;# Module specific parameters
    ;option_1=foo
    ;option_2=bar
    ;option_3=foobar


Arbiter, main role
------------------

The arbiter is the main Alignak daemon. As of it, its configuration part is the most important one.

This part of the configuration is stored by default in the folder */usr/local/etc/alignak/arbiter/* and it contains:

    * the configuration of the daemons and their modules
    * the configuration of the monitored objects
    * the configuration of monitoring resources

Each sub-part is stored by default in its own sub-directory:

    * *arbiter/daemons/* for the daemons cfg files. Each daemon has its own cfg file.
    * *arbiter/modules/* for the modules configuration
    * *arbiter/templates/* for the objects templates
    * *arbiter/packs/* for the checks packs
    * *arbiter/objects/* for the objects. Described in  :ref:`the monitored objects dedicated chapter <monitoring_configuration/index>`.
    * *arbiter/resource.d/* for the resources and global macros

Arbiter configuration
---------------------

Definition Format
~~~~~~~~~~~~~~~~~

The configuration is defined in this style::

    define arbiter {
        name    value
        name    value
    }

==================================== ======= ======= ======== ============================================================
Variable name                        Type    default Required Short description
==================================== ======= ======= ======== ============================================================
arbiter_name                         string          yes      Name of arbiter
host_name                            string          yes      hostname
address                              string          yes      fully qualified domain name or ip address
port                                 integer  7770   no       port of the daemon
spare                                boolean  0      no       set 1 if it's a spare
modules                              string          no       modules list separated by comma
use_ssl                              boolean  0      no       use SSL for communications
hard_ssl_name_check                  boolean  0      no       set 1 if require a valid certificate
timeout                              integer  3      no       number of seconds to block the arbiter waiting for an answer
data_timeout                         integer  120    no       seconds to wait when sending data to another daemon
max_check_attempts                   integer  3      no       number of time before node declared failed
check_interval                       integer  60     no       seconds to wait before issuing a new check
accept_passive_unknown_check_results integer  0      no       set 1 to allow passive check for unknown host
==================================== ======= ======= ======== ============================================================

Example Definition:
~~~~~~~~~~~~~~~~~~~

::

  define arbiter{
      arbiter_name       Main-arbiter
      address            node1.mydomain
      host_name          node1
      port               7770
      spare              0
      modules            module1,module2
  }


Variable Descriptions
~~~~~~~~~~~~~~~~~~~~~

arbiter_name
  This variable is used to identify the *short name* of the arbiter with which the data will be associated with.

host_name
  This variable is used by the arbiters daemons to define which 'arbiter' object they are: all theses daemons on different servers use the same configuration, so the only difference is their server name. This value must be equal to the name of the server (like with the hostname command). If none is defined, the arbiter daemon will put the name of the server where it's launched, but this will not be tolerated with more than one arbiter (because each daemons will think it's the master).

address
  This directive is used to define the address from where the main arbiter can reach this arbiter (that can be itself). This can be a fully qualified domain name or an IP address.

port
  This directive is used to define the TCP port used by the daemon. The default value is *7770*.

spare
  This variable is used to define if the daemon matching this arbiter definition is a spare one or not. The default value is *0* (non-spare).

modules
  This variable is used to define all the modules that the arbiter daemon matching this definition will load.

use_ssl
  This variable is used to allow inter-daemons communication in SSL mode (HTTPS)

hard_ssl_name_check
  This variable is used to force the checking of the SSL certificate

timeout
  This variable defines how much time the arbiter will wait for the response of an inter-process ping (default is 3 seconds). This operation is non blocking.

data_timeout
  Data send timeout. When sending data to another process. 120 seconds by default.

max_check_attempts
  If ping fails N or more, then the node is considered dead. 3 attempts by default.

check_interval
  Ping node every N seconds. 60 seconds by default.

accept_passive_unknown_check_results
  If this is enabled, the arbiter will accept passive check results for unconfigured hosts and will generate unknown host/service check result broks.


Scheduler configuration
-----------------------

Definition Format
~~~~~~~~~~~~~~~~~

The configuration is defined in this style::

    define scheduler {
        name    value
        name    value
    }

==================================== ======= ======= ======== =============================================================
Variable name                        Type    default Required Short description
==================================== ======= ======= ======== =============================================================
scheduler_name                       string          yes      Name of scheduler
address                              string          yes      fully qualified domain name or ip address
port                                 integer  7768   no       port of the daemon
spare                                boolean  0      no       set 1 if it's a spare
weight                               integer  1      no       some schedulers can manage more hosts than other
timeout                              integer  3      no       number of seconds to block the arbiter waiting for an answer
data_timeout                         integer  120    no       seconds to wait when sending data to another daemon
max_check_attempts                   integer  3      no       number of time before node declared failed
check_interval                       integer  60     no       seconds to wait before issuing a new check
modules                              string          no       modules list separated by comma
realm                                string   All    no       it's for multi-datacenter
skip_initial_broks                   boolean  0      no       set to 1 to skip initial broks creation
satellitemap                         string          no       define other daemons separated by comma, format: name=ip:port
use_ssl                              boolean  0      no       use SSL for communications
hard_ssl_name_check                  boolean  0      no       set 1 if require a valid certificate
accept_passive_unknown_check_results integer  0      no       set 1 to allow passive check for unknown host
==================================== ======= ======= ======== =============================================================


Example Definition:
~~~~~~~~~~~~~~~~~~~

::

  define scheduler{
      scheduler_name         Europe-scheduler
      address                node1.mydomain
      port                   7770
      spare                  0
      realm                  Europe

      # Optional parameters
      spare                  0   ; 1 = is a spare, 0 = is not a spare
      weight                 1   ; Some schedulers can manage more hosts than others
      timeout                3   ; Ping timeout
      data_timeout           120 ; Data send timeout
      max_check_attempts     3   ; If ping fails N or more, then the node is dead
      check_interval         60  ; Ping node every minutes
      modules                PickleRetention

      # Skip initial broks creation for faster boot time. Experimental feature
      # which is not stable.
      skip_initial_broks    0

      # In NATted environments, you declare each satellite ip[:port] as seen by
      # *this* scheduler (if port not set, the port declared by satellite itself
      # is used)
      satellitemap          poller-1=1.2.3.4:1772, reactionner-1=1.2.3.5:1773, ...
  }


Variable Descriptions
~~~~~~~~~~~~~~~~~~~~~


== TODO UPDATE THIS PART ==

scheduler_name
  This variable is used to identify the *short name* of the scheduler which the data is associated with.

address
  This directive is used to define the address from where the main arbiter can reach this scheduler. This can be a fully qualified domain name or a IP address.

port
  This directive is used to define the TCP port used bu the daemon. The default value is *7768*.

spare
  This variable is used to define if the scheduler must be managed as a spare one (will take the conf only if a master failed). The default value is *0* (master).

realm
  This variable is used to define the realm where the scheduler will be put. If none is selected, it will be assigned to the default one.

modules
  This variable is used to define all modules that the scheduler will load.

accept_passive_unknown_check_results
  If this is enabled, the scheduler will accept passive check results for unconfigured hosts and will generate unknown host/service check result broks.


Broker configuration
--------------------

Definition Format
~~~~~~~~~~~~~~~~~

The configuration is defined in this style::

    define broker {
        name    value
        name    value
    }

==================================== ======= ======= ======== =============================================================
Variable name                        Type    default Required Short description
==================================== ======= ======= ======== =============================================================
broker_name                          string          yes      Name of broker
address                              string          yes      fully qualified domain name or ip address
port                                 integer  7772   no       port of the daemon
spare                                boolean  0      no       set 1 if it's a spare
manage_arbiters                      boolean  1      no       set 1 to take data from Arbiter
manage_sub_realms                    boolean  1      no       set 1 to take jobs from schedulers of sub-realms
timeout                              integer  3      no       number of seconds to block the arbiter waiting for an answer
data_timeout                         integer  120    no       seconds to wait when sending data to another daemon
max_check_attempts                   integer  3      no       number of time before node declared failed
check_interval                       integer  60     no       seconds to wait before issuing a new check
modules                              string          no       modules list separated by comma
use_ssl                              boolean  0      no       use SSL for communications
hard_ssl_name_check                  boolean  0      no       set 1 if require a valid certificate
realm                                string   All    no       it's for multi-datacenter
==================================== ======= ======= ======== =============================================================


Example Definition:
~~~~~~~~~~~~~~~~~~~


::

  define broker{
      broker_name        broker-1
      address            node1.mydomain
      port               7772
      spare              0
      realm              All
      ## Optional
      manage_arbiters     1
      manage_sub_realms   1
      timeout             3   ; Ping timeout
      data_timeout        120 ; Data send timeout
      max_check_attempts  3   ; If ping fails N or more, then the node is dead
      check_interval      60  ; Ping node every minutes  	       manage_sub_realms  1
      modules             livestatus,simple-log,webui
  }


Variable Descriptions
~~~~~~~~~~~~~~~~~~~~~

== TODO UPDATE THIS PART ==

broker_name
  This variable is used to identify the *short name* of the broker which the data is associated with.

address
  This directive is used to define the address from where the main arbiter can reach this broker. This can be a fully qualified domain name or a IP address.

port
  This directive is used to define the TCP port used bu the daemon. The default value is *7772*.

spare
  This variable is used to define if the broker must be managed as a spare one (will take the conf only if a master failed). The default value is *0* (master).

realm
  This variable is used to define the realm where the broker will be put. If none is selected, it will be assigned to the default one.

manage_arbiters
  Take data from Arbiter. There should be only one broker for the arbiter.

manage_sub_realms
  This variable is used to define if the broker will take jobs from scheduler from the sub-realms of it's realm. The default value is *1*.

modules
  This variable is used to define all modules that the broker will load. The main goal of the Broker is to give status to theses modules.


Poller configuration
--------------------

Definition Format
~~~~~~~~~~~~~~~~~

The configuration is defined in this style::

    define poller {
        name    value
        name    value
    }

==================================== ======= ======= ======== =============================================================
Variable name                        Type    default Required Short description
==================================== ======= ======= ======== =============================================================
poller_name                          string          yes      Name of poller
address                              string          yes      fully qualified domain name or ip address
port                                 integer  7771   no       port of the daemon
spare                                boolean  0      no       set 1 if it's a spare
manage_sub_realms                    boolean  0      no       set 1 to take jobs from schedulers of sub-realms
min_workers                          integer  0      no       starts with N processes (0 = 1 per CPU)
max_workers                          integer  0      no       no more than N processes (0 = 1 per CPU)
processes_by_worker                  integer  256    no       each worker manages N checks
polling_interval                     integer  1      no       get jobs from schedulers each N seconds
timeout                              integer  3      no       number of seconds to block the arbiter waiting for an answer
data_timeout                         integer  120    no       seconds to wait when sending data to another daemon
max_check_attempts                   integer  3      no       number of time before node declared failed
check_interval                       integer  60     no       seconds to wait before issuing a new check
modules                              string          no       modules list separated by comma
passive                              boolean  0      no       set 1 to inverse the connections, so scheduler -> poller
poller_tags                          string   None   no       tags separated by comma. Use None to manage untagged checks
use_ssl                              boolean  0      no       use SSL for communications
hard_ssl_name_check                  boolean  0      no       set 1 if require a valid certificate
realm                                string   All    no       it's for multi-datacenter
==================================== ======= ======= ======== =============================================================


Example Definition:
~~~~~~~~~~~~~~~~~~~


::

  define poller{
      poller_name          Europe-poller
      address              node1.mydomain
      port                 7771
      spare                0

      # Optional parameters
      manage_sub_realms    0
      poller_tags          DMZ, Another-DMZ
      modules              module1,module2
      realm                Europe
      min_workers          0    ; Starts with N processes (0 = 1 per CPU)
      max_workers          0    ; No more than N processes (0 = 1 per CPU)
      processes_by_worker  256  ; Each worker manages N checks
      polling_interval     1    ; Get jobs from schedulers each N seconds
  }


Variable Descriptions
~~~~~~~~~~~~~~~~~~~~~

== TODO UPDATE THIS PART ==

poller_name
  This variable is used to identify the *short name* of the poller which the data is associated with.

address
  This directive is used to define the address from where the main arbiter can reach this poller. This can be a fully qualified domain name or a IP address.

port
  This directive is used to define the TCP port used bu the daemon. The default value is *7771*.

spare
  This variable is used to define if the poller must be managed as a spare one (will take the conf only if a master failed). The default value is *0* (master).

realm
  This variable is used to define the realm where the poller will be put. If none is selected, it will be assigned to the default one.

manage_sub_realms
  This variable is used to define if the poller will take jobs from scheduler from the sub-realms of it's realm. The default value is *0*.

poller_tags
  This variable is used to define the checks the poller can take. If no poller_tags is defined, poller will take all untagged checks. If at least one tag is defined, it will take only the checks that are also tagged like it.
  By default, there is no poller_tag, so poller can take all untagged checks (default).

modules
  This variable is used to define all modules that the scheduler will load.


Reactionner configuration
-------------------------

Definition Format
~~~~~~~~~~~~~~~~~

The configuration is defined in this style::

    define reactionner {
        name    value
        name    value
    }

==================================== ======= ======= ======== =============================================================
Variable name                        Type    default Required Short description
==================================== ======= ======= ======== =============================================================
reactionner_name                     string          yes      Name of reactionner
address                              string          yes      fully qualified domain name or ip address
port                                 integer  7769   no       port of the daemon
spare                                boolean  0      no       set 1 if it's a spare
manage_sub_realms                    boolean  0      no       set 1 to take jobs from schedulers of sub-realms
min_workers                          integer  1      no       starts with N processes (0 = 1 per CPU)
max_workers                          integer  15     no       no more than N processes (0 = 1 per CPU)
polling_interval                     integer  1      no       get jobs from schedulers each N seconds
timeout                              integer  3      no       number of seconds to block the arbiter waiting for an answer
data_timeout                         integer  120    no       seconds to wait when sending data to another daemon
max_check_attempts                   integer  3      no       number of time before node declared failed
check_interval                       integer  60     no       seconds to wait before issuing a new check
modules                              string          no       modules list separated by comma
reactionner_tags                     string   None   no       tags separated by comma. Use None to manage untagged handlers
use_ssl                              boolean  0      no       use SSL for communications
hard_ssl_name_check                  boolean  0      no       set 1 if require a valid certificate
realm                                string   All    no       it's for multi-datacenter
==================================== ======= ======= ======== =============================================================


Example Definition:
~~~~~~~~~~~~~~~~~~~

::

  define reactionner{
      reactionner_name      Main-reactionner
      address               node1.mydomain
      port                  7769
      spare                 0
      realm                 All

      # Optional parameters
      manage_sub_realms     0   ; Does it take jobs from schedulers of sub-Realms?
      min_workers           1   ; Starts with N processes (0 = 1 per CPU)
      max_workers           15  ; No more than N processes (0 = 1 per CPU)
      polling_interval      1   ; Get jobs from schedulers each 1 second
      timeout               3   ; Ping timeout
      data_timeout          120 ; Data send timeout
      max_check_attempts    3   ; If ping fails N or more, then the node is dead
      check_interval        60  ; Ping node every minutes
      reactionner_tags      tag1
      modules               module1,module2
  }


Variable Descriptions
~~~~~~~~~~~~~~~~~~~~~

== TODO UPDATE THIS PART ==

reactionner_name
  This variable is used to identify the *short name* of the reactionner which the data is associated with.

address
  This directive is used to define the address from where the main arbiter can reach this reactionner. This can be a fully qualified domain name or a IP address.

port
  This directive is used to define the TCP port used bu the daemon. The default value is *7772*.

spare
  This variable is used to define if the reactionner must be managed as a spare one (will take the conf only if a master failed). The default value is *0* (master).

realm
  This variable is used to define the realm where the reactionner will be put. If none is selected, it will be assigned to the default one.

manage_sub_realms
  This variable is used to define if the poller will take jobs from scheduler from the sub-realms of it's realm. The default value is *1*.

modules
  This variable is used to define all modules that the reactionner will load.

reactionner_tags
  This variable is used to define the checks the reactionner can take. If no reactionner_tags is defined, reactionner  will take all untagged notifications and event handlers. If at least one tag is defined, it will take only the checks that are also tagged like it.

By default, there is no reactionner_tag, so reactionner can take all untagged notification/event handlers (default).

Reaceiver configuration
-----------------------

Definition Format
~~~~~~~~~~~~~~~~~

The configuration is defined in this style::

    define receiver {
        name    value
        name    value
    }

==================================== ======= ======= ======== =============================================================
Variable name                        Type    default Required Short description
==================================== ======= ======= ======== =============================================================
receiver_name                        string          yes      Name of receiver
address                              string          yes      fully qualified domain name or ip address
port                                 integer  7773   no       port of the daemon
spare                                boolean  0      no       set 1 if it's a spare
timeout                              integer  3      no       number of seconds to block the arbiter waiting for an answer
data_timeout                         integer  120    no       seconds to wait when sending data to another daemon
max_check_attempts                   integer  3      no       number of time before node declared failed
check_interval                       integer  60     no       seconds to wait before issuing a new check
modules                              string          no       modules list separated by comma
use_ssl                              boolean  0      no       use SSL for communications
hard_ssl_name_check                  boolean  0      no       set 1 if require a valid certificate
direct_routing                       boolean  0      no       set 1 to allow scheduler to send command instead me
realm                                string   All    no       it's for multi-datacenter
==================================== ======= ======= ======== =============================================================

Example Definition:
~~~~~~~~~~~~~~~~~~~

== TODO UPDATE THIS PART ==


Variable Descriptions
~~~~~~~~~~~~~~~~~~~~~

== TODO UPDATE THIS PART ==

Ini files
=========

To run a daemon, it must start with a ini file.
This ini file defines options like IP address, port, SSL...

The format is the same for all the daemons and is defined in this style::

    [daemon]
    variable=value
    variable=value

When Alignak is installed, some variables in those configuration files are modified to match at 
best with your system. The changed variables are:

    - workdir
    - logdir
    - pidfile
    - user
    - group
    - ca_cert
    - server_cert
    - server_key

Their values are set according to the current platform (Linux, BSD, ...) and configuration defined
in the default alignak configuration.

Arbiter
-------

==================================== ======= ============================== ============================================================
Variable name                        Type    default                        Short description
==================================== ======= ============================== ============================================================
workdir                              string  /var/run/alignak               daemon will chdir into this directory when launched
logdir                               string  /var/log/alignak               daemon will write logs into this directory
etcdir                               string  /etc/alignak                   daemon will get configuration into this directory
pidfile                              string  /var/run/schedulerd.pid        PID file of the daemon
port                                 integer 7768                           port used by the daemon
host                                 string  0.0.0.0                        Set IP daemon listen
user                                 string  alignak                        username to run daemon
group                                string  alignak                        group to run daemon
idontcareaboutsecurity               boolean 0                              set 1 to disable security in daemon (not recommended)
modulesdir                           string  modules                        define modules directory installation
daemon_enabled                       boolean 1                              set to 0 if want this daemon not run
use_ssl                              boolean 0                              set 1 to use SSL for communications
ca_cert                              string  /etc/alignak/certs/ca.pem      full path for certificate
server_cert                          string  /etc/alignak/certs/server.cert full path for certificate
server_key                           string  /etc/alignak/certs/server.key  full path for certificate key
hard_ssl_name_check                  boolean 0                              set 1 if require a valid certificate
use_local_log                        boolean 1                              set to 1 to ease troubleshooting
local_log                            string  /var/log/schedulerd.log        file to write logs
log_level                            string  WARNING                        log level in DEBUG,INFO,WARNING,ERROR,CRITICAL
==================================== ======= ============================== ============================================================

Scheduler
---------

==================================== ======= ============================== ============================================================
Variable name                        Type    default                        Short description
==================================== ======= ============================== ============================================================
workdir                              string  /var/run/alignak               daemon will chdir into this directory when launched
logdir                               string  /var/log/alignak               daemon will write logs into this directory
pidfile                              string  /var/run/schedulerd.pid        PID file of the daemon
port                                 integer 7768                           port used by the daemon
host                                 string  0.0.0.0                        Set IP daemon listen
user                                 string  alignak                        username to run daemon
group                                string  alignak                        group to run daemon
idontcareaboutsecurity               boolean 0                              set 1 to disable security in daemon (not recommended)
modulesdir                           string  modules                        define modules directory installation
daemon_enabled                       boolean 1                              set to 0 if want this daemon not run
use_ssl                              boolean 0                              set 1 to use SSL for communications
ca_cert                              string  /etc/alignak/certs/ca.pem      full path for certificate
server_cert                          string  /etc/alignak/certs/server.cert full path for certificate
server_key                           string  /etc/alignak/certs/server.key  full path for certificate key
hard_ssl_name_check                  boolean 0                              set 1 if require a valid certificate
use_local_log                        boolean 1                              set to 1 to ease troubleshooting
local_log                            string  /var/log/schedulerd.log        file to write logs
log_level                            string  WARNING                        log level in DEBUG,INFO,WARNING,ERROR,CRITICAL
==================================== ======= ============================== ============================================================

Poller
------

==================================== ======= ============================== ============================================================
Variable name                        Type    default                        Short description
==================================== ======= ============================== ============================================================
user                                 string  alignak                        username to run daemon
group                                string  alignak                        group to run daemon
daemon_enabled                       boolean 1                              set to 0 if want this daemon not run
workdir                              string  /var/run/alignak               daemon will chdir into this directory when launched
logdir                               string  /var/log/alignak               daemon will write logs into this directory
pidfile                              string  /var/run/pollerd.pid           PID file of the daemon
host                                 string  0.0.0.0                        Set IP daemon listen
port                                 integer 7771                           port used by the daemon
idontcareaboutsecurity               boolean 0                              set 1 to disable security in daemon (not recommended)
use_ssl                              boolean 0                              set 1 to use SSL for communications
ca_cert                              string  /etc/alignak/certs/ca.pem      full path for certificate
server_cert                          string  /etc/alignak/certs/server.cert full path for certificate
server_key                           string  /etc/alignak/certs/server.key  full path for certificate key
hard_ssl_name_check                  boolean 0                              set 1 if require a valid certificate
use_local_log                        boolean 1                              set to 1 to ease troubleshooting
local_log                            string  /var/log/pollerd.log           file to write logs
log_level                            string  WARNING                        log level in DEBUG,INFO,WARNING,ERROR,CRITICAL
==================================== ======= ============================== ============================================================

Receiver
--------

==================================== ======= ============================== ============================================================
Variable name                        Type    default                        Short description
==================================== ======= ============================== ============================================================
workdir                              string  /var/run/alignak               daemon will chdir into this directory when launched
logdir                               string  /var/log/alignak               daemon will write logs into this directory
pidfile                              string  /var/run/receiverd.pid         PID file of the daemon
port                                 integer 7773                           port used by the daemon
host                                 string  0.0.0.0                        Set IP daemon listen
user                                 string  alignak                        username to run daemon
group                                string  alignak                        group to run daemon
idontcareaboutsecurity               boolean 0                              set 1 to disable security in daemon (not recommended)
daemon_enabled                       boolean 1                              set to 0 if want this daemon not run
use_ssl                              boolean 0                              set 1 to use SSL for communications
ca_cert                              string  /etc/alignak/certs/ca.pem      full path for certificate
server_cert                          string  /etc/alignak/certs/server.cert full path for certificate
server_key                           string  /etc/alignak/certs/server.key  full path for certificate key
hard_ssl_name_check                  boolean 0                              set 1 if require a valid certificate
use_local_log                        boolean 1                              set to 1 to ease troubleshooting
local_log                            string  /var/log/receiverd.log         file to write logs
log_level                            string  WARNING                        log level in DEBUG,INFO,WARNING,ERROR,CRITICAL
==================================== ======= ============================== ============================================================

Broker
------

==================================== ======= ============================== ============================================================
Variable name                        Type    default                        Short description
==================================== ======= ============================== ============================================================
workdir                              string  /var/run/alignak               daemon will chdir into this directory when launched
logdir                               string  /var/log/alignak               daemon will write logs into this directory
pidfile                              string  /var/run/brokerd.pid           PID file of the daemon
user                                 string  alignak                        username to run daemon
group                                string  alignak                        group to run daemon
host                                 string  0.0.0.0                        Set IP daemon listen
port                                 integer 7772                           port used by the daemon
idontcareaboutsecurity               boolean 0                              set 1 to disable security in daemon (not recommended)
daemon_enabled                       boolean 1                              set to 0 if want this daemon not run
use_ssl                              boolean 0                              set 1 to use SSL for communications
ca_cert                              string  /etc/alignak/certs/ca.pem      full path for certificate
server_cert                          string  /etc/alignak/certs/server.cert full path for certificate
server_key                           string  /etc/alignak/certs/server.key  full path for certificate key
hard_ssl_name_check                  boolean 0                              set 1 if require a valid certificate
use_local_log                        boolean 1                              set to 1 to ease troubleshooting
local_log                            string  /var/log/brokerd.log           file to write logs
log_level                            string  WARNING                        log level in DEBUG,INFO,WARNING,ERROR,CRITICAL
max_queue_size                       integer 100000                         restart an external module if queue to high. 0 to disable
==================================== ======= ============================== ============================================================

Reactionner
-----------

==================================== ======= ============================== ============================================================
Variable name                        Type    default                        Short description
==================================== ======= ============================== ============================================================
workdir                              string  /var/run/alignak               daemon will chdir into this directory when launched
logdir                               string  /var/log/alignak               daemon will write logs into this directory
pidfile                              string  /var/run/reactionnerd.pid      PID file of the daemon
port                                 integer 7769                           port used by the daemon
host                                 string  0.0.0.0                        Set IP daemon listen
user                                 string  alignak                        username to run daemon
group                                string  alignak                        group to run daemon
idontcareaboutsecurity               boolean 0                              set 1 to disable security in daemon (not recommended)
daemon_enabled                       boolean 1                              set to 0 if want this daemon not run
use_ssl                              boolean 0                              set 1 to use SSL for communications
ca_cert                              string  /etc/alignak/certs/ca.pem      full path for certificate
server_cert                          string  /etc/alignak/certs/server.cert full path for certificate
server_key                           string  /etc/alignak/certs/server.key  full path for certificate key
hard_ssl_name_check                  boolean 0                              set 1 if require a valid certificate
use_local_log                        boolean 1                              set to 1 to ease troubleshooting
local_log                            string  /var/log/reactionnerd.log      file to write logs
log_level                            string  WARNING                        log level in DEBUG,INFO,WARNING,ERROR,CRITICAL
==================================== ======= ============================== ============================================================
