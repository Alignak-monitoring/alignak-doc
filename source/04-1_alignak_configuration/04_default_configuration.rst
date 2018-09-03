.. _configuration/default_configuration:

=====================
Default configuration
=====================

The default configuration shipped with Alignak is a quite good start to build your own configuration because it defines helpful stuff to set-up a monitoring configuration from scratch...

.. note: This configuration allows you to run Alignak "out-of-the-box" because it only includes few hosts that are self-checked thanks to some Alignak internal checks commands.

You will find more information on the content of this configuration and how to adapt to your needs in the :ref:`Alignak configuration chapter<configuration/core>` and in the :ref:`next chapter<extending/updating_default>`.


The default shipped example configuration is structured as::

    alignak.ini
    alignak.d/
        daemons.ini
        modules.ini


Main configuration file (alignak.ini)
-------------------------------------
The default file shipped when installing is largely commented to explain more about the configuration variables.::

   ;
   ; This configuration file is the main Alignak configuration entry point. Each Alignak installer
   ; will adapt the content of this file according to the installation process. This will allow
   ; any Alignak extension or third party application to find where the Alignak components and
   ; files are located on the system.
   ;
   ; ---
   ; This version of the file contains variable that are suitable to run a single node Alignak
   ; with all its daemon using the default configuration existing in the repository.
   ;


   ; Declaring script macros
   ; -----
   ; To declare a macro that can be used in the plugins scripts, you must set a variable prefixed
   ; with an underscore (_). All the variables prefixed with _ will be transformed to macros
   ; the leading and trailng underscores will be removed and the variable name will be uppercased.
   ;
   ; A variable _test_macro will become a $TEST_MACRO$
   ; A variable _test_macro_ will become a $TEST_MACRO$
   ;

   ; Main alignak variables.
   ; -----
   ; The variables declared in this DEFAULT section will be inherited in all
   ; the other sections of this file!
   ;
   ; Two main interests for this section:
   ; - define the global Alignak configuration parameters
   ; - define the common parameters to all the Alignak configuration daemons
   ;
   ; -----
   ; NOTE that defining all the parameters in the DEFAULT section is an easy solution but it will make
   ; these parameters available in all the daemons. It is also possible to define only the daemon
   ; specific parameters in the daemon own section
   ;
   [DEFAULT]
   ; --------------------------------------------------------------------
   ; Installation directories
   ; ----------
   ; - _dist_BIN is where the launch scripts are located
   ;   (Standard installation sets to /usr/local/bin)
   ; - _dist_ETC is where we store the configuration files
   ;   (Standard installation sets to /usr/local/etc/alignak)
   ; - _dist_VAR is where the libraries and plugins files are installed
   ;   (Standard installation sets to /usr/local/var/lib/alignak)
   ; - _dist_RUN is the daemons working directory and where pid files are stored
   ;   (Standard installation sets to /usr/local/var/run/alignak)
   ; - _dist_LOG is where we put log files
   ;   (Standard installation sets to /usr/local/var/log/alignak)
   _dist=/usr/local/
   _dist_BIN=%(_dist)s/bin
   _dist_ETC=%(_dist)s/etc/alignak
   _dist_VAR=%(_dist)s/var/lib/alignak
   _dist_RUN=%(_dist)s/var/run/alignak
   _dist_LOG=%(_dist)s/var/log/alignak

   ; Daemons path configuration
   ; ----------
   ; Set as the default installation paths.
   ; If you set relative paths, they are relative to the default working directory.
   workdir=%(_dist_RUN)s
   logdir=%(_dist_LOG)s
   etcdir=%(_dist_ETC)s
   bindir=%(_dist_BIN)s
   ; --------------------------------------------------------------------


   ; --------------------------------------------------------------------
   ; Generic configuration name
   ; ----------
   ; This is the name used for this configuration
   ; The only purpose of this variable is to make it easier to search in the system log
   config_name=Alignak global configuration

   ; Alignak instance name
   ; ----------
   ; This information is useful to get/store alignak global configuration in the Alignak backend
   ; If you share the same backend between several Alignak instances, each instance must have its own
   ; name. If not defined, Alignak will use the master arbiter name as Alignak instance name.
   ; Anyway, it is recommended to make it unique if you run several Alignak instances
   alignak_name=My Alignak
   ; --------------------------------------------------------------------


   ; --------------------------------------------------------------------
   ; Alignak monitoring
   ; ----------
   ; The arbiter daemon can report the overall Alignak status to an external application that
   ; exposes the same services as implemented by the Alignak Web service module.
   ; The Arbiter will report the Alignak status as a passive host check. The Alignak daemons
   ; are considered as some services of an host named with the alignak_name

   ; Even if no reporting is configured, Alignak will raise an event log if log_alignak_checks is set

   ; Default is no reporting - else set the monitor URL
   ;alignak_monitor = http://127.0.0.1:7773/ws
   ; Report every alignak_monitor_period seconds
   ;alignak_monitor_period=60
   ; Set the username and password to use for the authentication
   ; If not set, no authentication will be used
   ;alignak_monitor_username = admin
   ;alignak_monitor_password = admin
   ; --------------------------------------------------------------------


   ; --------------------------------------------------------------------
   ; Alignak log management
   ; ----------
   ; Python logger configuration file
   ; Default is to get this file in the same directory as the alignak.ini
   logger_configuration=./alignak-logger.json
   ; This will set the daemon log file
   ; --------------------------------------------------------------------

   ; --------------------------------------------------------------------
   ; Timezone
   ; ----------
   ; If you need to set a specific timezone to your deamons, update and uncomment this
   ; Useful if you have multiple instances of Alignak that need to run from the same server,
   ; but have different local times associated with them. If not specified, Alignak will use
   ; the system configured timezone.
   ;use_timezone=Europe/Paris
   ; --------------------------------------------------------------------


   ; --------------------------------------------------------------------
   ; Daemons configuration
   ; ----------
   ; Unset for the daemon to be ignored by the arbiter
   ; Use this with many care and only if you really want a running daemon to be ignored!
   ; If you think you need to use this parameter, do not hesitate to contact us;)
   ;active=1

   ; Debugging daemons
   ; If this is set, the daemon log level is set to DEBUG
   ;debug=true

   ; Username and group to run
   ; ----------
   ; If not defined, the current user account will be used instead.
   ; It is recommended to define an alignak:alignak user/group account on your system.
   ; When Alignak is started with system services, it will try to use the root account which
   ; is not a recommended configuration...
   ; Note that this configuration will be ignored if it exists ALIGNAK_USER/ALIGNAK_GROUP
   ; environment variables because they will take precedence over this file configuration
   ;user=alignak
   ;group=alignak

   ; Disabling security means allowing the daemons to run under root account
   ; Set this variable to allow daemons running as root
   ;idontcareaboutsecurity=0

   ; Log file
   ; The daemon log file is configured according to the Python logger but it is
   ; still possible to override this...
   ;log_filename=%(workdir)s/daemon.log
   ; Same for the log_level
   ;log_level=

   ; Include the CherryPy daemon HTTP server log in the daemon log file
   ; This is interesting if you want many many details about the daemons inter-communication
   ;log_cherrypy=1

   ;  Pid file
   ; The daemon will chdir into the workdir directory when launched
   ; and it will create its pid file in this working dir
   ; You can override this location with the pid_filename variable
   ;pid_filename=%(workdir)s/daemon.pid

   ; Realm
   ; Each daemon is concerned by a realm. It will receive an appropriate configuration
   ; according to its realm
   ; The default value is the realm 'All'
   ;realm=All

   ; Advanced realm parameters:
   ; Do not change this paraemter unless you know what you are doing;)
   ; Is the daemon linked to the schedulers of sub-realms or only for its own realm?
   ; The default is that a daemon will also manage the sub realms of its realm. This parameter is
   ; useful if you need to define some daemons dedicated to a specific realm
   ; Make sure to avoid having several daemons of the same type for the same realm ;)
   ;manage_sub_realms=1

   ; Is the daemon connected to the arbiters?
   ; The default is that the daemon will not have a relation with the Alignak arbiter
   ; Handle this parameter with much care!
   ; An arbiter daemon will force-have a relation with the master arbiter
   ; A scheduler will also force-have a relation with the master arbiter
   ; This is only useful for a broker daemon. The master arbiter will push its brok to all
   ; the brokers that manage arbiters
   ;manage_arbiters=0

   ; Daemon high availability mode
   ; Unset (default) this parameter for a normal daemon
   ; Set for a spare daemon. A spare daemon will assume the main daemon role if the
   ; main daemon is not available
   ;spare=0

   ; Set to make the process daemonize itself, else it runs as a foreground process
   ;is_daemon=0

   ; Set to make the arbiter launch the daemon process
   ; If set, the arbiter will launch the corresponding daemon, else it will consider
   ; that this daemon is still started
   ;alignak_launched=1

   ; Set if you want to replace a running daemon. If an existing pid file is found
   ; the new process will try to kill an existing instance before daemonizing itself
   ;do_replace=0

   ; Daemons WS interface
   ; ----------
   ; Network configuration
   ; -----
   ; daemon host is set to 0.0.0.0 to listen on all interfaces,
   ; set 127.0.0.1 for a local loop only listening daemon
   ;host=0.0.0.0
   ; Port the daemon is listening to
   ;port=10000
   ; address is the IP address (or FQDN) used by the other daemons to contact the daemon
   ;address=127.0.0.1
   ; Number of threads the daemon is able to listen to
   ; Increase this number if some connection problems are raised; the more daemons exist in
   ; the configuration the more this pool size must be important
   ;thread_pool_size=32

   ; Daemon availability
   ; -----
   ; Daemon interface uses two different timeouts:
   ; - short for light data and long for heavy data exchanges
   ;short_timeout=3
   ;long_timeout=120

   ; If daemon communication fails max_check_attempts tims, the daemon is considered as dead
   ;max_check_attempts=3

   ; SSL configuration
   ; -----
   ; Configure this part if you are using SSL for communication between the Alignak daemons
   ;use_ssl=0
   ; Paths for certificate files
   ;server_cert=%(etcdir)s/certs/server.crt
   ;server_key=%(etcdir)s/certs/server.key
   ;ca_cert=%(etcdir)s/certs/ca.pem

   ;### Deprecated option - feel free to request for an implementation if needed
   ;hard_ssl_name_check=0
   ;### Deprecated option - feel free to request for an implementation if needed
   ;server_dh=%(etcdir)s/certs/server.pem

   ; Daemons external modules watchdog --
   ; ----------
   ; If a daemon external module has a brok queue higher than this value, it will be
   ; killed and restarted.
   ; Set to 0 to disable this behavior
   ;max_queue_size=0
   ; --------------------------------------------------------------------


   ; --------------------------------------------------------------------
   ; Notifications configuration
   ; ---
   ; Notifications are enabled/disabled
   ;enable_notifications=1

   # After a short_timeout, launched notification scripts are killed
   ;notification_timeout=30
   ; --------------------------------------------------------------------


   ; --------------------------------------------------------------------
   ; Retention configuration
   ; ---
   ; Set this variable to activate the Alignak inner retention module
   ;retain_state_information=true
   ; Alignak will persist its live state in a Json file which name is defined in this variable
   ;state_retention_file=
   ; Number of minutes between 2 retention save, default is 60 minutes
   ; This is only used if retention is enabled
   ; todo: move this parameter to the retention aware modules?
   ; If 0, the retention is disabled, else retention is enabled and the retention period is
   ; defined in the scheduler ticks parameters (see later)
   ;retention_update_interval=60
   ; --------------------------------------------------------------------


   ; --------------------------------------------------------------------
   ; Active checks configuration
   ; ---
   ; Active host/service checks are enabled/disabled
   ;execute_host_checks=1
   ;execute_service_checks=1

   ; Max plugin output in bytes for the plugins launched by the pollers
   ; Change this only if needed to increase for very long output check plugins
   ;max_plugins_output_length=8192

   ; Disabling environment macros for the check plugins is better for performance.
   ; If you really need to use environment variables, set this parameter.
   ;enable_environment_macros=0

   ; After a short_timeout, launched plugins are killed
   ; and the host state is set to a default value (2 for DOWN)
   ; and the service state is set to a default value (2 for CRITICAL)
   ;host_check_timeout=30
   ;service_check_timeout=60
   ;timeout_exit_status=2
   # --------------------------------------------------------------------


   ; --------------------------------------------------------------------
   ; Passive checks configuration
   ; ---
   ; Passive host/service checks are enabled/disabled
   ;accept_passive_host_checks=1
   ;accept_passive_service_checks=1

   ; Does Alignak accept passive check results for unknown hosts?
   ;accept_passive_unknown_check_results=1

   ; As default, Alignak always consider that passive host checks are SOFT states and it manages
   ; the check attempts before raising a HARD state. This Nagios parameter is not managed:
   ;passive_host_checks_are_soft=0

   ; Freshness check
   ; ---
   ; Default is enabled for hosts and services
   ; This all host/services that are passive checks enabled and not active checks
   ; enabled will have their freshness checked
   ;check_host_freshness=1
   ;check_service_freshness=1
   ; How often Alignak is checking for host/service freshness
   ; Default is 60 for hosts and services
   ;host_freshness_check_interval=60
   ;service_freshness_check_interval=60
   ; Extra time for freshness check ...
   ;additional_freshness_latency=15
   ; --------------------------------------------------------------------


   ; --------------------------------------------------------------------
   ; Checks scheduler configuration
   ; ---
   ; Scheduler interval length configuration
   ; Do not change this value unless you are really sure to master what you are doing...
   ;interval_length=60

   ; Number of intervals to spread the very first checks for hosts and services
   ; 5 minutes looks correct indeed...
   ;max_service_check_spread=5
   ;max_host_check_spread=5
   ; --------------------------------------------------------------------


   ; --------------------------------------------------------------------
   ; Flapping detection configuration
   ; ---
   ; Default is enabled
   ;enable_flap_detection=1
   ;
   ; Flapping threshold for hosts and services
   ;low_service_flap_threshold=20
   ;high_service_flap_threshold=30
   ;low_host_flap_threshold=20
   ;high_host_flap_threshold=30
   ;
   ; flap_history is the lengh of history states we keep to look for flapping.
   ; 20 is a correct default value but it can be increased.
   ;flap_history=20
   ; --------------------------------------------------------------------


   ; --------------------------------------------------------------------
   ; Performance data configuration
   ; ---
   ; Performance data management is enabled/disabled
   ;process_performance_data=1
   ; Commands to process the performance data
   ; Old Nagios parameters that are not used by Alignak
   ;host_perfdata_command=
   ;service_perfdata_command=
   ; --------------------------------------------------------------------


   ; --------------------------------------------------------------------
   ; Event handlers configuration
   ; ---
   ; Event handlers are enabled/disabled
   ;enable_event_handlers=1
   ;
   ; By default don't launch event handlers during a downtime period.
   ; Unset to get back the default Nagios behavior and raise event handlers during the downtime periods
   ;no_event_handlers_during_downtimes=1

   ; Global host/service event handlers: short names of defined commands
   ;global_host_event_handler=
   ;global_service_event_handler=
   ;
   ; After a short_timeout, launched event handlers are killed
   ;event_handler_timeout=30
   ; --------------------------------------------------------------------


   ; --------------------------------------------------------------------
   ; External commands configuration
   ; ---
   ; External commands are enabled/disabled
   ; Unset to disable the Alignak external commands processing
   ;check_external_commands=1
   ; --------------------------------------------------------------------


   ; --------------------------------------------------------------------
   ; Impacts configuration
   ; ---
   ; Enable or not the state change on impact detection (like a host going unreachable
   ; if a parent is DOWN for example). It's for services and hosts.
   ; Note: unset this for Nagios old behavior compatibility
   ;enable_problem_impacts_states_change=1
   ; --------------------------------------------------------------------


   ; --------------------------------------------------------------------
   ; Monitoring log configuration
   ; ---
   ; Note that alerts and downtimes are always logged
   ; ---
   ; --------------------------------------------------------------------
   ; Notifications
   ;log_notifications=1

   ; Services retries
   ;log_service_retries=1

   ; Hosts retries
   ;log_host_retries=1

   ; Event handlers
   ;log_event_handlers=1

   ; Flappings
   ;log_flappings=1

   ; Snapshots
   ;log_snapshots=1

   ; External commands
   ;log_external_commands=1

   ; Active checks
   ; Default is not logging this event, because it makes a quite verbose log
   ;log_active_checks=0

   ; Passive checks
   ; Default is not logging this event, because it makes a quite verbose log
   ;log_passive_checks=0

   ; Alignak self checks
   ; Default is not logging this event, because it makes a quite verbose log
   ; Note that whatever this variable value, alerts will always be raised
   ;log_alignak_checks=0

   ; Initial states
   ; Default it not logging this event, because it makes a quite verbose log
   ;log_initial_states=0
   ; --------------------------------------------------------------------


   ; --------------------------------------------------------------------
   ; Arbiter daemons part,
   ; ---
   ; The Arbiter is able to launch the required daemons that are not declared in the configuration
   ; To activate this feature, set this parameter
   ;launch_missing_daemons=0

   ; When the arbiter starts some daemons by itself
   ; This may happen if some hosts are defined in a realm that do not have all
   ; its required daemons defined or if the alignak_launched parameter is set
   ; for a daemon
   ; Daemons startup script location
   ; Default is to use the bin directory of the daemon
   ;daemons_script_location=%(bindir)s
   ; Daemons extra arguments
   ; Define some extra arguments to be provided on the daemon command line
   ;daemons_arguments=
   ; Default is to allocate a port number incrementally starting from the value defined here
   ;daemons_initial_port=10000
   ;

   ; Daemons monitoring
   ; ---
   ; The daemons are polling their satellites every polling_interval seconds
   ;polling_interval=5
   ; After max_check_attempts unsuccessfull connection try, the daemon is declared as dead
   ;max_check_attempts=5

   ; The arbiter is checking the running processes for the daemons every daemons_check_period
   ; seconds. The checking only concerns the daemons that were started by the arbiter itself
   ;daemons_check_period=5
   ; Daemons failure kill all daemons
   ; If a missing daemon is detected, all the arbiter children daemons will be killed and
   ; the arbiter will stop. This will make Alignak stop itself and restart if is configured to
   ; respawn in the system.
   ;daemons_failure_kill=1
   ;
   ; Graceful stop delay
   ; - on stop request, the arbiter will inform the daemons that stopping will happen soon
   ; - after the daemons_stop_timeout period, the arbiter will force kill the daemons
   ; that it launched and inform the other daemons that stopping is now effective
   ;daemons_stop_timeout=5
   ;
   ; Delay after daemons got started by the Arbiter
   ; The arbiter will pause a maximum delay of daemons_start_timeout or 0.5 seconds per
   ; launched daemon
   ; Whatever the value set in this file or internally computed, the arbiter will pause
   ;for a minimum of 1 second
   ;daemons_start_timeout=1
   ;
   ; Delay before dispatching a new configuration after reload
   ; Whatever the value set in this file, the arbiter will pause for a minimum of 1 second
   ;daemons_new_conf_timeout=1
   ;
   ; Delay after the configuration got dispatched to the daemons
   ; The arbiter will pause a maximum delay of daemons_dispatch_timeout or 0.5 seconds
   ; per launched daemon
   ; Whatever the value set in this file or internally computed, the arbiter will pause
   ; for a minimum of 1 second
   ;daemons_dispatch_timeout=5
   ; --------------------------------------------------------------------


   ; --------------------------------------------------------------------
   ; Alignak internal metrics
   ; Export all alignak inner performance metrics to a statsd server.
   ; By default at localhost:8125 (UDP) with the alignak prefix
   ; --------------------------------------------------------------------
   ;statsd_host = localhost
   ;statsd_port = 8125
   ;statsd_prefix = alignak
   ; --------------------------------------------------------------------
   ; When graphite_enabled is set, the Alignak internal metrics are sent
   ; to a graphite/carbon port instead of a StatsD instance.
   ; Contrary to StatsD, Graphite/carbon uses a TCP connection but it
   ; allows to bulk send metrics.
   ; This is more reliable and improved than the StatsD interface that is based upon UDP
   ; Default is not enabled for any interface
   ;statsd_enabled = 0
   ;graphite_enabled = 0
   ; --------------------------------------------------------------------


   ; --------------------------------------------------------------------
   ; Scheduler loop configuration
   ; Those parameters allow to configure the scheduler actions execution
   ; period.
   ; Each parameter is a scheduler recurrent action. On each scheduling
   ; loop turn, the scheduler checks if the time is come to execute
   ; the corresponding work.
   ; Each parameter defines on which loop turn count the action is to be
   ; executed. Considering a loop turn is 1 second, a parameter value set
   ; to 10 will make the corresponding action to be executed every 10
   ; seconds.
   ; --------------------------------------------------------------------
   ; BEWARE: changing some of those parameters may have unexpected
   ; effects! Do not change unless you know what you are doing ;)
   ; Some tips:
   ; - tick_check_freshness, allow to change the freshness check period
   ; - tick_update_retention, allow to change the retention save period
   ; --------------------------------------------------------------------
   ;tick_update_downtimes_and_comments=1
   ;tick_schedule=1
   ; ### Check host/service freshness every 10 seconds
   ;tick_check_freshness=10
   ;tick_consume_results=1
   ;tick_get_new_actions=1
   ;tick_scatter_master_notifications=1
   ;tick_get_new_broks=1
   ;tick_delete_zombie_checks=1
   ;tick_delete_zombie_actions=1
   ;tick_clean_caches=1
   ; ### Retention save every hour
   ;tick_update_retention=3600
   ;tick_check_orphaned=60
   ; ### Notify about scheduler status every 10 seconds
   ;tick_update_program_status=10
   ;tick_check_for_system_time_change=1
   ; ### Internal checks are computed every loop turn
   ;tick_manage_internal_checks=1
   ;tick_clean_queues=1
   ; ### Note that if it set to 0, the scheduler will never try to clean its queues for oversizing
   ;tick_clean_queues=10
   ;tick_update_business_values=60
   ;tick_reset_topology_change_flags=1
   ;tick_check_for_expire_acknowledge=1
   ;tick_send_broks_to_modules=1
   ;tick_get_objects_from_from_queues=1
   ;tick_get_latency_average_percentile=10



   [alignak-configuration]
   ; Alignak monitored system configuration files
   ; Declaring such configuration files is useful if you have some items declared in plain-old
   ; legacy configuration files (eg. Nagios, Shinken, ...)
   ; ---
   ; All the variables starting with 'cfg' are considered as some configuration files and will
   ; be parsed according to the Nagios parsing rules
   ; ---
   ; First configuration file
   ;cfg=%(etcdir)s/alignak.cfg
   ; Second configuration file
   ;cfg2=%(etcdir)s/macros.cfg


Daemons configuration
----------------------
The default file shipped when installing for the daemons configuration (*alignak.d/daemons.ini*) is declaring one instance of each Alignak daemons type. This configuration is suitable for a standard non distributed Alignak configuration.::

   # For each Alignak daemon, this file contains a section with the daemon name. The section
   # identifier is the corresponding daemon name prefixed with the keyword daemon and a dot.
   # This daemon name is usually built with the daemon type (eg. arbiter, poller,...) and the
   # daemon name separated with a dash.
   #
   # The previous rules ensure that Alignak will be able to find all the daemons configuration
   # in this file whatever the number of daemons is existing in the configuration
   #
   # To be easily used as a configuration variable of this file, the daemon name is repeated
   # inside the section in a NAME variable.
   #
   # Each section inherits from the [DEFAULT] section and only defines the specific values
   # inherent to the declared daemon.

   [daemon.arbiter-master]
   type=arbiter
   name=arbiter-master

   ; Network configuration
   ; ---
   ; My listening interface
   ;host=0.0.0.0
   port=7770
   ; My adress for the other daemons
   ;address=127.0.0.1

   ; Modules
   ; ---
   ; Default: None
   ; Interesting modules:
   ; - backend_arbiter: get the monitored objects configuration from the Alignak backend
   ;modules=backend_arbiter


   [daemon.scheduler-master]
   type=scheduler
   name=scheduler-master

   ; Network configuration
   ; ---
   ; My listening interface
   ;host=0.0.0.0
   port=7768
   # My adress for the other daemons
   ;address=127.0.0.1

   ; Modules
   ; ---
   ; Default: None
   ; Interesting modules:
   ; - backend_scheduler: store the system live state in the Alignak backend (retention)
   ;modules=backend_scheduler

   ; Advanced Features:
   ; If set, the scheduler will skip initial broks creation. It will be a little faster to start-up
   ; but no broker module will receive the initial_status broks. Take care about this!
   ;skip_initial_broks=0

   ; Some schedulers can manage more hosts than others
   ; The scheduler weight indicates if the scheduler can manage more hosts than its siblings...
   ;weight=1

   [daemon.poller-master]
   type=poller
   name=poller-master

   ; Network configuration
   ; ---
   ; My listening interface
   ;host=0.0.0.0
   port=7771
   ; My adress for the other daemons
   ;address=127.0.0.1

   ; Modules
   ; ---
   ; Default: None
   ; Interesting modules:
   ; - nrpe-booster, replaces the check_nrpe binary to enhance performance for NRPE checks
   ; - snmp-booster, replace the snmp_get with a bulk polling module
   ;modules=nrpe-booster

   ; Advanced parameters:
   ;manage_sub_realms=1
   ; If set to 0 the min_workers and max_workers values will be configured according to the
   ; system CPU count, This will lead to use as many workers as CPUs count less one; one CPU
   ; is preserved to avoid too much load on the system and let the other daemons do thei job;)
   ; If you set min_workers and max_workers to the same value, you will set the workers count.
   ; Use as much worker as possible for the pollers
   min_workers=0
   max_workers=0
   ;processes_by_worker=256
   ;worker_polling_interval=1

   ; Passive mode
   ; In active mode (default behavior), connections between scheduler and poller are
   ; poller -> scheduler to get checks to launch
   ; poller -> scheduler to report checks results
   ; For DMZ monitoring, set the passive mode for the connections to be from scheduler -> poller.
   ;passive=0

   ; Poller tags
   ; Poller tags are the tag that the poller will manage.
   ; Use None as tag name to manage untagged checks (default)
   ;poller_tags=None

   [daemon.reactionner-master]
   type=reactionner
   name=reactionner-master

   ; Network configuration
   ; ---
   ; My listening interface
   ;host=0.0.0.0
   port=7769
   ; My adress for the other daemons
   ;address=127.0.0.1

   ; Modules
   ; ---
   ; Default: None
   ; Interesting modules:
   ; - none currently
   ;modules

   ; Advanced parameters:
   ;manage_sub_realms=1
   ; If set to 0 the min_workers and max_workers values will be configured with the system CPU count
   ; this to use as many workers as CPUs
   ; If you set min_workers and max_workers to the same value, you will set the workers count.
   ; Use only 1 worker for the reactionner
   min_workers=1
   max_workers=1
   ;processes_by_worker=256
   ;worker_polling_interval=1

   ; Passive mode
   ; In active mode (default behavior), connections between scheduler and reactionner are
   ; reactionner -> scheduler to get checks to launch
   ; reactionner -> scheduler to report checks results
   ; For DMZ monitoring, set the passive mode for the connections to be from scheduler -> reactionner.
   ;passive=0

   ; Reactionner tags
   ; Reactionner tags are the tag that the reactionner will manage.
   ; Use None as tag name to manage untagged actions (default)
   ;reactionner_tags=None

   [daemon.broker-master]
   type=broker
   name=broker-master

   ; Network configuration
   ; ---
   ; My listening interface
   ;host=0.0.0.0
   port=7772
   ; My adress for the other daemons
   ;address=127.0.0.1

   ; Advanced parameters:
   ;manage_sub_realms=1
   ; The broker daemon may have an important message queue size so it is important to alert
   ; if this queue size becomes too huge; it may be caused by a broker module problem!
   max_queue_size=100000

   ; Gets the arbiter broks
   ; There must only be one and only one broker that gets the broks created by the arbiter
   ; Do not set this parameter for all other brokers because it defaults to False.
   manage_arbiters=1

   ; Modules
   ; ---
   ; Default: None
   ; Interesting modules:
   ; - backend_broker, update the live state in the Alignak backend
   ; - logs, collect monitoring logs and send them to the Alignak backend
   ;modules=backend_broker, logs

   [daemon.receiver-master]
   type=receiver
   name=receiver-master

   ; Network configuration
   ; ---
   ; My listening interface
   ;host=0.0.0.0
   port=7773
   ; My adress for the other daemons
   ;address=127.0.0.1

   ; Modules
   ; ---
   ; Default: None
   ; Interesting modules:
   ; - nsca, NSCA protocol server for collecting passive checks
   ; - external-commands, read a nagios commands file to notify external commands
   ; - web-services, expose Web services to get Alignak daemons state and notify external commands
   ;modules=nsca,external-commands,web-services

   ; Advanced parameters:
   ;manage_sub_realms=1

Modules configuration
---------------------
The default file shipped when installing for the modules configuration (*alignak.d/modules.ini*) is not declaring any module. It is only an example file to get used for declaring a new module.::

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
   ;statsd_host = localhost
   ;statsd_port = 8125
   ;statsd_prefix = alignak
   ;statsd_enabled = 0
   ;# --------------------------------------------------------------------
   ;
   ;# Module log level
   ;;log_level=INFO
   ;
   ;# Module specific parameters
   ;option_1=foo
   ;option_2=bar
   ;option_3=foobar


Extra shipped configuration
---------------------------

Sample directory
~~~~~~~~~~~~~~~~

The *etc/alignak/sample* directory contain many samples for the configuration of the different
elements defined in the configuration.

.. note:: Please consider these files are samples and that they will probably not be fully functional out-of-the-box...

.. hint:: Many sample files exist in the Alignak tests suites of the repository. If you are searching for a little help or some inspiration, feel free to have a look into the *tests/cfg* for simple configuration and *tests_integ/cfg* for more complex configurations!

Arbiter directory
~~~~~~~~~~~~~~~~~

This directory contains a default configuration built with legacy configuration files.

This configuration only declare one host which is always considered as UP because it is internaly checked::

    /usr/local/etc/alignak/arbiter
        -> ... for the main monitoring configuration file (alignak.cfg)

    /usr/local/etc/alignak/arbiter/resource.d
        -> ... for the global macros and resources

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
