.. _configuration/global_variables:

===============================
Alignak configuration variables
===============================

All the variables described in this chapter may be used in the Alignak environment configuration file as defined in :ref:`the following chapter <configuration/core>`.

.. note:: some variables existing in the Alignak environment configuration file are not described in this chapter. This because they are not really useful for configuration or too specific ... despite they are commented and explained in the default shipped configuration file;)


.. note:: When creating and/or editing configuration files, keep the following in mind:

    * Lines that start with a ``#`` or ``;`` character are comments that are not processed

    * Variable names are case-sensitive

Main parameters
===============

.. _configuration/core#cfg:

Legacy configuration files
--------------------------
Format::

    cfg=<file_name>
    cfg2=<file_name>
    cfg_extra_file=<file_name>

The arbiter get all the parameters starting with a `cfg` prefix and considers them as Nagios legacy configuration files.


.. _configuration/core#macros:

Macros
------
Format::

    _macro=<macro value>
    _macro_=<macro value>

The arbiter get all the parameters starting with a `_` prefix and considers them as monitoring macros.


.. _configuration/core#alignak_name:

Algnak instance name
--------------------

**Alignak new!**

Format::

   ; This information is useful to get/store alignak global configuration in the Alignak backend
   ; If you share the same backend between several Alignak instances, each instance must have its own
   ; name. If not defined, Alignak will use the master arbiter name as Alignak instance name.
   ; Anyway, it is recommended to make it unique if you run several Alignak instances
   alignak_name=My Alignak

This variable defines the name of the Alignak instance. This is useful, for instance, when you share an Alignak backend between several Alignak instances to identify the source of the stored information.


.. _configuration/core#logger_configuration:

Daemons logger configuration
----------------------------

**Alignak new!**

Format::

   ; Default is to get this file in the same directory as the alignak.ini
   logger_configuration=./alignak-logger.json

This variable defines the configuration of the daemon Python logger.


.. _configuration/core#alignak_monitoring:

Alignak monitoring
-----------------

**Alignak new!**

Format::

   ; Default is no reporting - else set the monitor URL
   ;alignak_monitor = http://127.0.0.1:7773/ws
   ; Set the username and password to use for the authentication near the WS provider
   ; If not set, no authentication will be used
   ;alignak_monitor_username = admin
   ;alignak_monitor_password = admin


The arbiter daemon can report the overall Alignak status to an external application that exposes the same services as implemented by the Alignak Web service module.
The Arbiter will report the Alignak status as a passive host check. The Alignak daemons are considered as some services of an host named with the instance alignak_name.


Daemon specific
===============

.. _configuration/core#user_group:

Daemons user/group account
--------------------------

Format::

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

These variables define the user/group used by the running alignak daemons.


.. _configuration/core#log:

Daemons log file
----------------

Format::

   ; The daemon log file is configured according to the Python logger but it is
   ; still possible to override this...
   ;log_filename=%(workdir)s/daemon.log
   ; Same for the log_level
   ;log_level=

The daemon log file is configured according to the Python logger but it is still possible to change the file name and log level with these variables.

.. note:: that some command line parameters can also take precedence over these variables!

.. _configuration/core#pid:

Daemons PID file
----------------

Format::

   ;  Pid file
   ; The daemon will chdir into the workdir directory when launched
   ; and it will create its pid file in this working dir
   ; You can override this location with the pid_filename variable
   ;pid_filename=%(workdir)s/daemon.pid

The daemon will create its pid file in its working dir but this can be overriden with this variable.

.. note:: that some command line parameters can also take precedence over these variables!

.. _configuration/core#realm:

Daemons realm
-------------

Format::

   ; Realm
   ; Each daemon is concerned by a realm. It will receive an appropriate configuration
   ; according to its realm
   ; The default value is the realm 'All'
   ;realm=All

As explained :ref:`in this chapter <alignak_features/realms>`, a daemon is involved in a realm. This variable will define the daemon realm.

.. _configuration/core#interface:

Daemons WS interface
--------------------

Format::

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


Arbiter daemon
==============

Daemons launch
--------------

**Alignak new!**

The Arbiter is able to launch the required daemons that are not declared in the configuration.

.. tip:: This may be necessary if some hosts are defined in a realm that do not have all its required daemons defined...

.. tip:: For simple tests, it may be easier to start the arbiter from a shell and set the `alignak_launched` parameter for the other daemons rather than using system services.

To activate this feature, set this parameter.

Format::

   ;launch_missing_daemons=0

When the arbiter starts some daemons by itself, some extra parameters are useful.

Format::

   ; Daemons startup script location
   ; Default is to use the bin directory of the daemon
   ;daemons_script_location=%(bindir)s
   ; Daemons extra arguments
   ; Define some extra arguments to be provided on the daemon command line
   ;daemons_arguments=
   ; Default is to allocate a port number incrementally starting from the value defined here
   ;daemons_initial_port=10000
   ;

Satellites polling
------------------

**Alignak new!**

The arbiter is polling its satellites every `polling_interval` seconds. After `max_check_attempts` unsuccessfull connection try, the daemon is declared as dead and an error log is raised.

Format::

   ; Daemons monitoring
   ; ---
   ; The daemons are polling their satellites every polling_interval seconds
   ;polling_interval=5
   ; After max_check_attempts unsuccessfull connection try, the daemon is declared as dead
   ;max_check_attempts=5

The arbiter is checking the satellites that it launched every `daemons_check_period` seconds. If `daemons_failure_kill` is set, and a missing process is detected, it will stop all the other self-launched daemons and stop itself.

Format::

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
   ;daemons_stop_timeout=15
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



Alignak metrics
===============

.. _configuration/core#statsd:

See :ref:`this chapter <monitoring_features/notifications>`.

**Alignak new!**

These parameters allow to configure how Alignak will export its inner performance metrics to a StatsD/Graphite server.

When graphite_enabled is set, the Alignak internal metrics are sent to a graphite/carbon port (`statsd_host:statsd_port`) instead of a StatsD instance (if `statsd_enabled` is set). Contrary to StatsD, Graphite/carbon uses a TCP connection but it allows to bulk send metrics. This is more reliable and improved than the StatsD interface that is based upon UDP

Some environment variables exist to log the metrics to a file in append mode:
    'ALIGNAK_STATS_FILE'
        the file name
    'ALIGNAK_STATS_FILE_LINE_FMT'
        defaults to [#date#] #counter# #value# #uom#\n'
    'ALIGNAK_STATS_FILE_DATE_FMT'
        defaults to '%Y-%m-%d %H:%M:%S'
        date is UTC
        if configured as an empty string, the date will be output as a UTC timestamp

If a file is enough for you, set the `statsd_host` 'None' and the metrics will not be sent to the StatsD/Graphite.


Format::

   ; Default is not enabled for any interface
   ;statsd_enabled = 0
   ;graphite_enabled = 0

Configure the StatsD/Graphite address and port::

   ;statsd_host = localhost
   ;statsd_port = 8125

This prefix will be prepended to all the metrics to make them more easily found in Graphite::
   ;statsd_prefix = alignak



Notifications configuration
===========================

See :ref:`this chapter <monitoring_features/notifications>`.

Format::

   ; Notifications are enabled/disabled
   ;enable_notifications=1

   # After a short_timeout, launched notification scripts are killed
   ;notification_timeout=30


Event handlers configuration
============================

See :ref:`this chapter <monitoring_features/event_handlers>`.

Format::

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


Monitoring log configuration
============================

All the monitoring events are logged to a file as defined in the :ref:`Alginak logger configuration <configuration/logger>` according to these configuration variables.

.. note:: alerts and downtimes are always logged. There is no specific variable for this event categories.

Format::

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
   ; Default it not logging this event, because it makes a quite verbose log
   ;log_active_checks=0

   ; Passive checks
   ; Default it not logging this event, because it makes a quite verbose log
   ;log_passive_checks=0

   ; Initial states
   ; Default it not logging this event, because it makes a quite verbose log
   ;log_initial_states=0




Nagios legacy
=============

.. _configuration/core#retention_update_interval:

Automatic state retention update interval
-----------------------------------------

Format::

  retention_update_interval=<minutes>

Default::

  retention_update_interval=60

This setting determines how often (in minutes) that Alignak **scheduler** will automatically save retention data during normal operation.
If you set this value to 0, it will not save retention data at regular intervals, but it will still save retention data before shutting down or restarting.


.. _configuration/core#max_service_check_spread:

Maximum Host/Service check spread
---------------------------------

Format::

  max_service_check_spread=<minutes>
  max_host_check_spread=<minutes>

Default::

  max_service_check_spread=30
  max_host_check_spread=30

This option determines the maximum number of minutes from when Alignak starts that all hosts/services (that are scheduled to be regularly checked) are checked. This option will ensure that the initial checks of all hosts/services occur within the timeframe you specify. Default value is 30 (minutes).


.. _configuration/core#host_check_timeout:
.. _configuration/core#service_check_timeout:

Service/Host check timeout
--------------------------

Format::

  service_check_timeout=<seconds>
  host_check_timeout=<seconds>

Default::

  service_check_timeout=60
  host_check_timeout=30

This is the maximum number of seconds that Alignak will allow service/host checks to run. If checks exceed this limit, they are killed and a CRITICAL state is returned. A timeout error will also be logged.

There is often widespread confusion as to what this option really does. It is meant to be used as a last ditch mechanism to kill off plugins which are misbehaving and not exiting in a timely manner. It should be set to something high (like 60 seconds or more), so that each check normally finishes executing within this time limit. If a check runs longer than this limit, Alignak will kill it off thinking it is a runaway processes.

.. _configuration/core#timeout_exit_status:

Timeout exit status
-------------------

Format::

   timeout_exit_status=[0,1,2,3]

Default::

   timeout_exit_status=2

State set by Alignak in case of timeout. The value is a state identifier, thus:

    * 0: OK/UP
    * 1: WARNING/UNREACHABLE
    * 2: CRITICAL/DOWN
    * 3: UNKNOWN


.. _configuration/core#flap_history:

Flap history
------------

Format::

  flap_history=<int>

Default::

  flap_history=20

This option is used to set the history size of states keep by the scheduler to make the flapping calculation. By default, the value is 20 states kept.

The size in memory is for the scheduler daemon : 4Bytes * flap_history * (nb hosts + nb services). For a big environment, it costs 4 * 20 * (1000+10000) - 900Ko. So you can raise it to higher value if you want. To have more information about flapping, you can read :ref:`this <monitoring_features/flapping>`.


.. _configuration/core#max_plugins_output_length:

Maximum plugins output length
-----------------------------

Format::

  max_plugins_output_length=<int>

Default::

  max_plugins_output_length=8192

This option is used to set the max size in bytes for the checks plugins output. So if you have some truncated output like for huge disk check when you have a lot of partitions, increase this value.


.. _configuration/core#enable_problem_impacts_states_change:

Enable problem/impacts states change
------------------------------------

Format::

  enable_problem_impacts_states_change=<0/1>

Default::

  enable_problem_impacts_states_change=0

This option is used to know if we apply or not the state change when a host or service is impacted by a root problem (like the service's host going down or a host's parent being down too). The state will be changed by UNKNONW for a service and UNREACHABLE for a host until their next schedule check. This state change do not count as a attempt, it's just for console so the users know that theses objects got problems and the previous states are not sure.


.. _configuration/core#disable_old_nagios_parameters_whining:

Disable old nagios parameters whining
-------------------------------------

Format::

  disable_old_nagios_parameters_whining=<0/1>

Default::

  disable_old_nagios_parameters_whining=0

If 1, disable all notice and warning messages when the Arbiter is checking the configuration.


.. _configuration/core#use_timezone:

Timezone option
---------------

Format::

  use_timezone=<tz from tz database>

Default::

  use_timezone=''

This option allows you to override the default timezone that this instance of Alignak runs in. Useful if you have multiple instances of Alignak that need to run from the same server, but have different local times associated with them. If not specified, Alignak will use the system configured timezone.



.. _configuration/core#enable_environment_macros:

Environment macros option
-------------------------

Format::

  enable_environment_macros=<0/1>

Default::

  enable_environment_macros=1

This option determines whether or not the Alignak daemon will make all standard :ref:`macros <monitoring_features/macros>` available as environment variables to your check, notification, event hander, etc. commands. In large installations this can be problematic because it takes additional CPU to compute the values of all macros and make them available to the environment. It also costs an increased network communication between schedulers and pollers.

  * 0 = Don't make macros available as environment variables
  * 1 = Make macros available as environment variables


.. _configuration/core#monitoring_log:
.. _configuration/core#log_initial_states:

Initial states logging option
-----------------------------

Format::

  log_initial_states=<0/1>

Default::

  log_initial_states=1

This variable determines whether or not Alignak will force all initial host and service states to be logged, even if they result in an OK state. Initial service and host states are normally only logged when there is a problem on the first check. Enabling this option is useful if you are using an application that scans the log file to determine long-term state statistics for services and hosts.

  * 0 = Don't log initial states
  * 1 = Log initial states


.. _configuration/core#log_notifications:

Notification logging option
---------------------------

Format::

  log_notifications=<0/1>

Example::

  log_notifications=1

This variable determines whether or not notification messages are logged. If you have a lot of contacts or regular service failures your log file will grow (let say some Mo by day for a huge configuration, so it's quite OK for nearly every one to log them). Use this option to keep contact notifications from being logged.

  * 0 = Don't log notifications
  * 1 = Log notifications


.. _configuration/core#log_service_retries:
.. _configuration/core#log_host_retries:

Service/Host check retry logging option
---------------------------------------

Format::

  log_service_retries=<0/1>
  log_host_retries=<0/1>

Example::

  log_service_retries=0
  log_host_retries=0

This variable determines whether or not service/host check retries are logged. Service check retries occur when a service check results in a non-OK state, but you have configured Alignak to retry the service more than once before responding to the error. Services in this situation are considered to be in "soft" states. Logging service check retries is mostly useful when attempting to debug Alignak or test out service/host :ref:`event handlers <monitoring_features/event_handlers>`.

  * 0 = Don't log service/host check retries (default)
  * 1 = Log service/host check retries


.. _configuration/core#log_event_handlers:

Event handlers logging option
-----------------------------

Format::

  log_event_handlers=<0/1>

Example::

  log_event_handlers=1

This variable determines whether or not service and host :ref:`event handlers <monitoring_features/event_handlers>` are logged. Event handlers are optional commands that can be run whenever a service or hosts changes state. Logging event handlers is most useful when debugging Alignak or first trying out your event handler scripts.

  * 0 = Don't log event handlers
  * 1 = Log event handlers




.. _configuration/core#log_external_commands:

External commands logging option
--------------------------------

Format::

  log_external_commands=<0/1>

Example::

  log_external_commands=1

This variable determines whether or not Alignak will log :ref:`external commands <monitoring_features/external_commands>` that it receives.

  * 0 = Don't log external commands
  * 1 = Log external commands (default)


.. _configuration/core#log_passive_checks:

Passive checks logging option
-----------------------------

Format::

  log_passive_checks=<0/1>

Example::

  log_passive_checks=1

This variable determines whether or not Alignak will log :ref:`passive host and service checks <monitoring_features/passive_checks>` that it receives.

  * 0 = Don't log passive checks
  * 1 = Log passive checks (default)


.. _configuration/core#log_active_checks:

Active checks logging option
----------------------------

Format::

  log_active_checks=<0/1>

Example::

  log_active_checks=1

This variable determines whether or not Alignak will log :ref:`active host and service checks <monitoring_features/active_checks>` that it runs.

  * 0 = Don't log active checks (default)
  * 1 = Log active checks


.. _configuration/core#log_flappings:

Host/Service flapping logging option
------------------------------------

Format::

  log_flappings=<0/1>

Example::

  log_flappings=1

This variable determines whether or not Alignak will log  :ref:`host/service flapping <monitoring_features/flapping>` it detects.

  * 0 = Don't log snapshots
  * 1 = Log snapshots (default)


.. _configuration/core#log_snapshots:

Snapshots logging option
------------------------

Format::

  log_snapshots=<0/1>

Example::

  log_snapshots=1

This variable determines whether or not Alignak will log the snapshots it built.

  * 0 = Don't log snapshots
  * 1 = Log snapshots (default)


.. _configuration/core#no_event_handlers_during_downtimes:

Event Handler during downtimes
------------------------------

Format::

  no_event_handlers_during_downtimes=<0/1>

Default::

  no_event_handlers_during_downtimes=0

This option determines whether or not Alignak will run :ref:`event handlers <monitoring_features/event_handlers>` when the host or service is in a scheduled downtime.

  * 0 = Launch event handlers (Nagios behavior)
  * 1 = Don't launch event handlers



Performance data parameters
===========================

.. _configuration/core#process_performance_data:

Performance data processing option
----------------------------------

Format::

  process_performance_data=<0/1>

Example::

  process_performance_data=1

This value determines whether or not Alignak will process host and service check performance data.

  * 0 = Don't process performance data
  * 1 = Process performance data (default)

If you want to use tools like PNP, NagiosGrapher or Graphite set it to 1.


.. _configuration/core#perfdata_timeout:

Performance data processor command timeout
------------------------------------------

Format::

  perfdata_timeout=<seconds>

Example::

  perfdata_timeout=5

This is the maximum number of seconds that Alignak will allow a host performance data processor command or service performance data processor command to run. If a command exceeds this time limit it will be killed and a warning will be logged.


.. _configuration/core#host_perfdata_command:
.. _configuration/core#service_perfdata_command:

Host/Service performance data processing command
------------------------------------------------

Format::

  host_perfdata_command=<monitoring_objects/command>
  service_perfdata_command=<monitoring_objects/command>

Example::

  host_perfdata_command=process-host-perfdata
  service_perfdata_command=process-service-perfdata

This option allows you to specify a command to be run after every host/service check to process host/service performance data that may be returned from the check. The command argument is the short name of a command definition that you define in your object configuration file. This command is only executed if the :ref:`Performance Data Processing Option <configuration/core#process_performance_data>` option is enabled globally and if the ``process_perf_data`` directive in the host definition is enabled.


Advanced scheduling parameters
==============================


.. _configuration/core#passive_host_checks_are_soft:

Passive host checks are SOFT option
-----------------------------------

Format::

  passive_host_checks_are_soft=<0/1>

Example::

  passive_host_checks_are_soft=1

This option determines whether or not Alignak will treat :ref:`passive host checks <monitoring_features/passive_checks>` as HARD states or SOFT states. As a default, a passive host check result will put a host into a HARD state type. You can change this behavior by enabling this option.

  * 0 = Passive host checks are HARD (default)
  * 1 = Passive host checks are SOFT

.. warning:: This option is not yet implemented.


.. _configuration/core#enable_predictive_host_dependency_checks:
.. _configuration/core#enable_predictive_service_dependency_checks:

Predictive Host/Service dependency checks option
------------------------------------------------

Format::

  enable_predictive_host_dependency_checks=<0/1>
  enable_predictive_service_dependency_checks=<0/1>

Example::

  enable_predictive_host_dependency_checks=1
  enable_predictive_service_dependency_checks=1

This option determines whether or not Alignak will execute predictive checks of hosts/services that are being depended upon (as defined in :ref:`host/services dependencies <monitoring_features/dependencies>`) for a particular host/service when it changes state. Predictive checks help ensure that the dependency logic is as accurate as possible.

  * 0 = Disable predictive checks
  * 1 = Enable predictive checks (default)

.. warning:: This option is not yet implemented.

.. _configuration/core#check_for_orphaned_services:
.. _configuration/core#check_for_orphaned_hosts:

Orphaned Host/Service check option
----------------------------------

Format::

  check_for_orphaned_services=<0/1>
  check_for_orphaned_hosts=<0/1>

Example::

  check_for_orphaned_services=1
  check_for_orphaned_hosts=1

This option allows you to enable or disable checks for orphaned service/host checks. Orphaned checks are checks which have been launched to pollers but have not had any results reported in a long time.

Since no results have come back in for it, it is not rescheduled in the event queue. This can cause checks to stop being executed. Normally it is very rare for this to happen - it might happen if an external user or process killed off the process that was being used to execute a check.

If this option is enabled and Alignak finds that results for a particular check have not come back, it will log an error message and reschedule the check. If you start seeing checks that never seem to get rescheduled, enable this option and see if you notice any log messages about orphaned services.

  * 0 = Don't check for orphaned service checks
  * 1 = Check for orphaned service checks (default)

.. warning:: This option is not yet implemented.



.. _configuration/core#soft_state_dependencies:

Soft state dependencies option
------------------------------

Format:  soft_state_dependencies=<0/1>
Example:  soft_state_dependencies=0

This option determines whether or not Alignak will use soft state information when checking :ref:`host and service dependencies <monitoring_features/dependencies>`. Normally it will only use the latest hard host or service state when checking dependencies. If you want it to use the latest state (regardless of whether its a soft or hard :ref:`state type <monitoring_features/statetypes>`), enable this option.

  * 0 = Don't use soft state dependencies (default)
  * 1 = Use soft state dependencies


.. warning:: This option is not yet implemented.


Performance tuning
===================

.. _configuration/core#cached_host_check_horizon:
.. _configuration/core#cached_service_check_horizon:

Cached Host/Service check horizon
---------------------------------

Format::

  cached_host_check_horizon=<seconds>
  cached_service_check_horizon=<seconds>

Example::

   cached_host_check_horizon=15
   cached_service_check_horizon=15

This option determines the maximum amount of time (in seconds) that the state of a previous host check is considered current. Cached host states (from host/service checks that were performed more recently than the time specified by this value) can improve host check performance immensely. Too high of a value for this option may result in (temporarily) inaccurate host/service states, while a low value may result in a performance hit for host/service checks. Use a value of 0 if you want to disable host/service check caching. More information on cached checks can be found :ref:`here <alignak_features/cached_checks>`.

.. tip::  Nagios default is 15s, but it's a tweak that make checks less accurate. So Alignak uses 0s as a default. If you have performance problems and you can't add a new scheduler or poller, increase this value and start to buy a new server because this won't be magical ;).

.. warning:: This option is not yet implemented.


.. _configuration/core#use_large_installation_tweaks:

Large installation tweaks option
--------------------------------

Format::

  use_large_installation_tweaks=<0/1>

Example::

  use_large_installation_tweaks=0

This option determines whether or not the Alignak daemon will take shortcuts to improve performance. These shortcuts result in the loss of a few features, but larger installations will likely see a lot of benefit from doing so. If you can't add new satellites to manage the load (like new pollers), you can activate it.

  * 0 = Don't use tweaks (default)
  * 1 = Use tweaks



Flapping parameters
===================

.. _configuration/core#enable_flap_detection:

Flap detection option
---------------------

Format::

  enable_flap_detection=<0/1>

Example::

  enable_flap_detection=1

This option determines whether or not Alignak will try and detect hosts and services that are “flapping". Flapping occurs when a host or service changes between states too frequently, resulting in a barrage of notifications being sent out. When Alignak detects that a host or service is flapping, it will temporarily suppress notifications for that host/service until it stops flapping.

More information on how flap detection and handling works can be found :ref:`here <monitoring_features/flapping>`.

  * 0 = Don't enable flap detection (default)
  * 1 = Enable flap detection


.. _configuration/core#low_host_flap_threshold:
.. _configuration/core#low_service_flap_threshold:

Low Service/Host flap threshold
-------------------------------

Format::

  low_service_flap_threshold=<percent>
  low_host_flap_threshold=<percent>

Example::

  low_service_flap_threshold=25.0
  low_host_flap_threshold=25.0

This option is used to set the low threshold for detection of host/service flapping. For more information on how flap detection and handling works (and how this option affects things) read :ref:`this <monitoring_features/flapping>`.


.. _configuration/core#high_host_flap_threshold:
.. _configuration/core#high_service_flap_threshold:

High Service/Host flap threshold
--------------------------------

Format::

  high_service_flap_threshold=<percent>
  high_host_flap_threshold=<percent>

Example::

  high_service_flap_threshold=50.0
  high_host_flap_threshold=50.0

This option is used to set the high threshold for detection of host/service flapping. For more information on how flap detection and handling works (and how this option affects things) read :ref:`this <monitoring_features/flapping>`.




.. _configuration/core#event_handler_timeout:
.. _configuration/core#notification_timeout:

Various commands timeouts
-------------------------

Format::

  event_handler_timeout=<seconds>  # default: 30s
  notification_timeout=<seconds>   # default: 30s

Example::

  event_handler_timeout=60
  notification_timeout=60

This is the maximum number of seconds that Alignak will allow :ref:`event handlers <monitoring_features/event_handlers>`, :ref:`notifications <monitoring_features/notifications>` to be run. If an command exceeds this time limit it will be killed and a warning will be logged.

There is often widespread confusion as to what this option really does. It is meant to be used as a last ditch mechanism to kill off commands which are misbehaving and not exiting in a timely manner. It should be set to something high (like 60 seconds or more for notification), so that each event handler command normally finishes executing within this time limit. If an event handler runs longer than this limit, Alignak will kill it off thinking it is a runaway processes.


Freshness check
===============

.. _configuration/core#check_service_freshness:
.. _configuration/core#check_host_freshness:

Host/Service freshness checking option
--------------------------------------

Format::

  check_service_freshness=<0/1>
  check_host_freshness=<0/1>

Example::

  check_service_freshness=0
  check_host_freshness=0

This option determines whether or not Alignak will periodically check the “freshness" of host/service checks. Enabling this option is useful for helping to ensure that :ref:`passive service checks <monitoring_features/passive_checks>` are received in a timely manner.

  * 0 = Don't check host/service freshness
  * 1 = Check host/service freshness (default)


.. _configuration/core#service_freshness_check_interval:
.. _configuration/core#host_freshness_check_interval:

Host/Service freshness check interval
-------------------------------------

Format::

  service_freshness_check_interval=<seconds>
  host_freshness_check_interval=<seconds>

Example::

  service_freshness_check_interval=60
  host_freshness_check_interval=60

This setting determines how often (in seconds) Alignak will periodically check the “freshness" of host/service check results. If you have disabled host/service freshness checking (with the ``check_service_freshness`` option), this option has no effect.


.. _configuration/core#additional_freshness_latency:

Additional freshness threshold latency option
---------------------------------------------

Format::

  additional_freshness_latency=<#>

Example::

  additional_freshness_latency=15

This option determines the number of seconds Alignak will add to any host or services freshness threshold it automatically calculates (e.g. those not specified explicitly by the user).



.. _configuration/core#enable_notifications:

Notifications option
--------------------

Format::

  enable_notifications=<0/1>

Example::

  enable_notifications=1

This option determines whether or not Alignak will send out :ref:`notifications <monitoring_features/notifications>`. If this option is disabled, Alignak will not send out notifications for any host or service.

Values are as follows:
  * 0 = Disable notifications
  * 1 = Enable notifications (default)


.. _configuration/core#check_external_commands:

External command check option
-----------------------------

Format::

  check_external_commands=<0/1>

Example::

  check_external_commands=1

This option determines whether or not Alignak will execute the external commands that it receives. More information on external commands can be found :ref:`here <monitoring_features/external_commands>`.

  * 0 = Don't check external commands
  * 1 = Check external commands (default)


Scheduling parameters
=====================

.. _configuration/core#execute_service_checks:

Service/Host check execution option
-----------------------------------

Format::

  execute_service_checks=<0/1>
  execute_host_checks=<0/1>

Example::

  execute_service_checks=1
  execute_host_checks=1

This option determines whether or not Alignak will execute :ref:`active host/service checks <monitoring_features/active_checks>`. If this option is disabled, Alignak will not execute any active host/service checks.

  * 0 = Don't execute service checks
  * 1 = Execute service checks (default)


.. _configuration/core#accept_passive_service_checks:

Passive Host/Service check acceptance option
--------------------------------------------

Format::

  accept_passive_service_checks=<0/1>
  accept_passive_host_checks=<0/1>

Example::

  accept_passive_service_checks=1
  accept_passive_host_checks=1

This option determines whether or not Alignak will accept :ref:`passive host/service checks <monitoring_features/passive_checks>`. If this option is disabled, Alignak will not accept any passive host/service checks.

  * 0 = Don't accept passive service/host checks
  * 1 = Accept passive service/host checks (default)


.. _configuration/core#enable_event_handlers:

Event handlers option
---------------------

Format::

  enable_event_handlers=<0/1>

Example::

  enable_event_handlers=1

This option determines whether or not Alignak will run :ref:`event handlers <monitoring_features/event_handlers>`.

  * 0 = Disable event handlers
  * 1 = Enable event handlers (default)




.. _configuration/core#global_host_event_handler:
.. _configuration/core#global_service_event_handler:

Global Host/Service event handlers option
-----------------------------------------

Format::

  global_host_event_handler=<monitoring_objects/command>
  global_service_event_handler=<monitoring_objects/command>

Example::

  global_host_event_handler=log-host-event-to-db
  global_service_event_handler=log-service-event-to-db

This option allows you to specify a host event handler command that is to be run for every host state change. The global event handler is executed immediately prior to the event handler that you have optionally specified in each host definition. The command argument is the short name of a command that you define in your commands definition. The maximum amount of time that this command can run is controlled by the :ref:`Event Handler Timeout <configuration/core#event_handler_timeout>` option. More information on event handlers can be found :ref:`here <monitoring_features/event_handlers>`.

Such commands should not be so useful with the new Alignak distributed architecture. If you use it, look if you can avoid it because such commands will kill your performance!



.. _configuration/core#interval_length:

Timing interval length
----------------------

Format::

  interval_length=<seconds>

Example::

  interval_length=60

This is the number of seconds per “unit interval" used for timing in the scheduling queue, re-notifications, etc. "Units intervals" are used in the object configuration file to determine how often to run a service check, how often to re-notify a contact, etc.

The default value for this is set to 60, which means that a "unit value" of 1 in the object configuration file will mean 60 seconds (1 minute).

.. tip::  Changing this option is not a good thing with Alignak. It's not designed to be a hard real time monitoring system...



Naming and macros parameters
============================

.. _configuration/core#illegal_object_name_chars:

Illegal object name characters
------------------------------

Format::

  illegal_object_name_chars=<chars...>

Example::

  illegal_object_name_chars=`-!$%^&*"|'<>?,()=

This option allows you to specify illegal characters that cannot be used in host names, service descriptions, or names of other object types. Alignak will allow you to use most characters in object definitions, but we recommend not using the characters shown in the example above because it may give you problems in the web interface, notification commands, etc.


.. _configuration/core#illegal_macro_output_chars:

Illegal macro output characters
-------------------------------

Format::

  illegal_macro_output_chars=<chars...>

Example::

  illegal_macro_output_chars=`-$^&"|'<>

This option allows you to specify illegal characters that should be stripped from :ref:`macros <monitoring_features/macros>` before being used in notifications, event handlers, and other commands. This DOES NOT affect macros used in service or host check commands. You can choose to not strip out the characters shown in the example above, but we recommend you do not do this. Some of these characters are interpreted by the shell (i.e. the backtick) and can lead to security problems. The following macros are stripped of the characters you specify:

  * "$HOSTOUTPUT$"
  * "$HOSTPERFDATA$"
  * "$HOSTACKAUTHOR$"
  * "$HOSTACKCOMMENT$"
  * "$SERVICEOUTPUT$"
  * "$SERVICEPERFDATA$"
  * "$SERVICEACKAUTHOR$"
  * "$SERVICEACKCOMMENT$"


.. _configuration/core#env_variables_prefix:

Environment variables prefix
----------------------------

Format::

  env_variables_prefix=<prefix>

Example::

  env_variables_prefix=NAGIOS_

This option allows you to specify the prefix that is prepended to the Alignak macros when they are propagated to the executed plugins shell environement. The default prefix is ``ALIGNAK_`` and this variable to specify an alternate prefix. Indeed, some existing scripts may use the default Nagios / Shinken ``NAGIOS_`` prefix... so feel free to declare this legacy prefix here;)


.. _configuration/core#admin_email:

Administrator email address
---------------------------

Format::

  admin_email=<email_address>

Example::

  admin_email=root@localhost.localdomain

This is the email address for the administrator of the local machine (i.e. the one that Alignak is running on). This value can be used in notification commands by using the "$ADMINEMAIL$" :ref:`macro <monitoring_features/macros>`.


.. _configuration/core#admin_pager:

Administrator pager (unused)
----------------------------

Format::

  admin_pager=<pager_number_or_pager_email_gateway>

Example::

  admin_pager=pageroot@localhost.localdomain

This is the pager number (or pager email gateway) for the administrator of the local machine (i.e. the one that Alignak is running on). The pager number/address can be used in notification commands by using the $ADMINPAGER$ :ref:`macro <monitoring_features/macros>`.


Scheduler loop configuration
============================

**Alignak new!**

These parameters allow to configure the scheduler actions execution period.
Each parameter is a scheduler recurrent action. On each scheduling loop turn, the scheduler checks if the time is come to execute the corresponding work.

Each parameter defines on which loop turn count the action is to be executed. Considering a loop turn is 1 second, a parameter value set to 10 will make the corresponding action to be executed every 10 seconds.

.. note:: changing some of those parameters may have unexpected effects! Do not change unless you know what you are doing ;)

.. tip::    Some tips:
    - tick_check_freshness, allow to change the freshness check period
    - tick_update_retention, allow to change the retention save period

.. _configuration/core#scheduler:

Default values
--------------

 ::

      tick_update_downtimes_and_comments=1
      tick_schedule=1
      ### Check host/service freshness every 10 seconds
      tick_check_freshness=10
      tick_consume_results=1
      tick_get_new_actions=1
      tick_scatter_master_notifications=1
      tick_get_new_broks=1
      tick_delete_zombie_checks=1
      tick_delete_zombie_actions=1
      tick_clean_caches=1
      ### Retention save every hour
      tick_update_retention=3600
      tick_check_orphaned=60
      ### Notify about scheduler status every 10 seconds
      tick_update_program_status=10
      tick_check_for_system_time_change=1
      ### Internal checks are computed every loop turn
      tick_manage_internal_checks=1
      tick_clean_queues=1
      ### Note that if it set to 0, the scheduler will never try to clean its queues for oversizing
      tick_clean_queues=10
      tick_update_business_values=60
      tick_reset_topology_change_flags=1
      tick_check_for_expire_acknowledge=1
      tick_send_broks_to_modules=1
      tick_get_objects_from_from_queues=1
      tick_get_latency_average_percentile=10
