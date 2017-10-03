.. _configuration/core_tips:

=============================================
Main configuration file (alignak.cfg) options
=============================================

When creating and/or editing configuration files, keep the following in mind:

    * Lines that start with a ``#`` character are comments that are not processed

    * Variable names are case-sensitive

    * If you want to configure a process to use a specific module:

        * You must define the module in a **xxx.cfg** file in the **modules** directory
        * You must reference it in the **modules** section for that process, e.g. the **broker.cfg** file

The main configuration file is named ``alignak.cfg``.

Default used options
====================

.. _configuration/core#cfg_dir:
.. _configuration/core#cfg_file:

Cfg dir and Cfg files
---------------------
Format :

::

    cfg_dir=<directory_name>
    cfg_file=<file_name>

Those are **statements and not parameters**. The arbiter considers them as order to open other(s) configuration(s) file(s)

For the ``cfg_dir`` one, the arbiter reads recursively the specified directory and **only** reads the files ending with ".cfg". It **does not** consider lines into those files as **statements** anymore (no more ``cfg_dir`` or ``cfg_file`` are considered in the read files).

Thus, the arbiter handles the main configuration files differently than any other files.



.. _configuration/core#retention_update_interval:

Automatic state retention update interval
-----------------------------------------

Format:

::

  retention_update_interval=<minutes>

Default:

::

  retention_update_interval=60

This setting determines how often (in minutes) that Alignak **scheduler** will automatically save retention data during normal operation.
If you set this value to 0, it will not save retention data at regular intervals, but it will still save retention data before shutting down or restarting.
If you have disabled state retention (with the :ref:`State Retention Option <configuration/main-advanced#retain_state_information>` option), this option has no effect.


.. _configuration/core#max_service_check_spread:

Maximum Host/Service check spread
---------------------------------

Format:

::

  max_service_check_spread=<minutes>
  max_host_check_spread=<minutes>

Default:

::

  max_service_check_spread=30
  max_host_check_spread=30

This option determines the maximum number of minutes from when Alignak starts that all hosts/services (that are scheduled to be regularly checked) are checked. This option will ensure that the initial checks of all hosts/services occur within the timeframe you specify. Default value is 30 (minutes).


.. _configuration/core#host_check_timeout:
.. _configuration/core#service_check_timeout:

Service/Host check timeout
--------------------------

Format:

::

  service_check_timeout=<seconds>
  host_check_timeout=<seconds>

Default:

::

  service_check_timeout=60
  host_check_timeout=30

This is the maximum number of seconds that Alignak will allow service/host checks to run. If checks exceed this limit, they are killed and a CRITICAL state is returned. A timeout error will also be logged.

There is often widespread confusion as to what this option really does. It is meant to be used as a last ditch mechanism to kill off plugins which are misbehaving and not exiting in a timely manner. It should be set to something high (like 60 seconds or more), so that each check normally finishes executing within this time limit. If a check runs longer than this limit, Alignak will kill it off thinking it is a runaway processes.

.. _configuration/core#timeout_exit_status:

Timeout exit status
-------------------

Format:

::

   timeout_exit_status=[0,1,2,3]

Default:

::

   timeout_exit_status=2

State set by Alignak in case of timeout. The value is a state identifier, thus:

    * 0: OK/UP
    * 1: WARNING/UNREACHABLE
    * 2: CRITICAL/DOWN
    * 3: UNKNOWN


.. _configuration/core#flap_history:

Flap history
------------

Format:

::

  flap_history=<int>

Default:

::

  flap_history=20

This option is used to set the history size of states keep by the scheduler to make the flapping calculation. By default, the value is 20 states kept.

The size in memory is for the scheduler daemon : 4Bytes * flap_history * (nb hosts + nb services). For a big environment, it costs 4 * 20 * (1000+10000) - 900Ko. So you can raise it to higher value if you want. To have more information about flapping, you can read :ref:`this <monitoring_features/flapping>`.


.. _configuration/core#max_plugins_output_length:

Maximum plugins output length
-----------------------------

Format:

::

  max_plugins_output_length=<int>

Default:

::

  max_plugins_output_length=8192

This option is used to set the max size in bytes for the checks plugins output. So if you have some truncated output like for huge disk check when you have a lot of partitions, increase this value.


.. _configuration/core#enable_problem_impacts_states_change:

Enable problem/impacts states change
------------------------------------

Format:

::

  enable_problem_impacts_states_change=<0/1>

Default:

::

  enable_problem_impacts_states_change=0

This option is used to know if we apply or not the state change when a host or service is impacted by a root problem (like the service's host going down or a host's parent being down too). The state will be changed by UNKNONW for a service and UNREACHABLE for a host until their next schedule check. This state change do not count as a attempt, it's just for console so the users know that theses objects got problems and the previous states are not sure.


.. _configuration/core#disable_old_nagios_parameters_whining:

Disable old nagios parameters whining
-------------------------------------

Format:

::

  disable_old_nagios_parameters_whining=<0/1>

Default:

::

  disable_old_nagios_parameters_whining=0

If 1, disable all notice and warning messages when the Arbiter is checking the configuration.


.. _configuration/core#use_timezone:

Timezone option
---------------

Format:

::

  use_timezone=<tz from tz database>

Default:

::

  use_timezone=''

This option allows you to override the default timezone that this instance of Alignak runs in. Useful if you have multiple instances of Alignak that need to run from the same server, but have different local times associated with them. If not specified, Alignak will use the system configured timezone.



.. _configuration/core#enable_environment_macros:

Environment macros option
-------------------------

Format:

::

  enable_environment_macros=<0/1>

Default:

::

  enable_environment_macros=1

This option determines whether or not the Alignak daemon will make all standard :ref:`macros <monitoring_features/macros>` available as environment variables to your check, notification, event hander, etc. commands. In large installations this can be problematic because it takes additional CPU to compute the values of all macros and make them available to the environment. It also costs an increased network communication between schedulers and pollers.

  * 0 = Don't make macros available as environment variables
  * 1 = Make macros available as environment variables


.. _configuration/core#log_initial_states:

Initial states logging option
-----------------------------

Format:

::

  log_initial_states=<0/1>

Default:

::

  log_initial_states=1

This variable determines whether or not Alignak will force all initial host and service states to be logged, even if they result in an OK state. Initial service and host states are normally only logged when there is a problem on the first check. Enabling this option is useful if you are using an application that scans the log file to determine long-term state statistics for services and hosts.

  * 0 = Don't log initial states
  * 1 = Log initial states


.. _configuration/core#log_notifications:

Notification logging option
---------------------------

Format:

::

  log_notifications=<0/1>

Example:

::

  log_notifications=1

This variable determines whether or not notification messages are logged. If you have a lot of contacts or regular service failures your log file will grow (let say some Mo by day for a huge configuration, so it's quite OK for nearly every one to log them). Use this option to keep contact notifications from being logged.

  * 0 = Don't log notifications
  * 1 = Log notifications


.. _configuration/core#log_service_retries:
.. _configuration/core#log_host_retries:

Service/Host check retry logging option
---------------------------------------

Format:

::

  log_service_retries=<0/1>
  log_host_retries=<0/1>

Example:

::

  log_service_retries=0
  log_host_retries=0

This variable determines whether or not service/host check retries are logged. Service check retries occur when a service check results in a non-OK state, but you have configured Alignak to retry the service more than once before responding to the error. Services in this situation are considered to be in "soft" states. Logging service check retries is mostly useful when attempting to debug Alignak or test out service/host :ref:`event handlers <monitoring_features/event_handlers>`.

  * 0 = Don't log service/host check retries (default)
  * 1 = Log service/host check retries


.. _configuration/core#log_event_handlers:

Event handlers logging option
-----------------------------

Format:

::

  log_event_handlers=<0/1>

Example:

::

  log_event_handlers=1

This variable determines whether or not service and host :ref:`event handlers <monitoring_features/eventhandlers>` are logged. Event handlers are optional commands that can be run whenever a service or hosts changes state. Logging event handlers is most useful when debugging Alignak or first trying out your event handler scripts.

  * 0 = Don't log event handlers
  * 1 = Log event handlers




.. _configuration/core#log_external_commands:

External commands logging option
--------------------------------

Format:

::

  log_external_commands=<0/1>

Example:

::

  log_external_commands=1

This variable determines whether or not Alignak will log :ref:`external commands <monitoring_features/external_commands>` that it receives.

  * 0 = Don't log external commands
  * 1 = Log external commands (default)


.. _configuration/core#log_passive_checks:

Passive checks logging option
-----------------------------

Format:

::

  log_passive_checks=<0/1>

Example:

::

  log_passive_checks=1

This variable determines whether or not Alignak will log :ref:`passive host and service checks <monitoring_features/passive_checks>` that it receives.

  * 0 = Don't log passive checks
  * 1 = Log passive checks (default)


.. _configuration/core#log_active_checks:

Active checks logging option
----------------------------

Format:

::

  log_active_checks=<0/1>

Example:

::

  log_active_checks=1

This variable determines whether or not Alignak will log :ref:`active host and service checks <monitoring_features/active_checks>` that it runs.

  * 0 = Don't log active checks (default)
  * 1 = Log active checks


.. _configuration/core#log_flappings:

Host/Service flapping logging option
------------------------------------

Format:

::

  log_flappings=<0/1>

Example:

::

  log_flappings=1

This variable determines whether or not Alignak will log  :ref:`host/service flapping <monitoring_features/flapping>` it detects.

  * 0 = Don't log snapshots
  * 1 = Log snapshots (default)


.. _configuration/core#log_snapshots:

Snapshots logging option
------------------------

Format:

::

  log_snapshots=<0/1>

Example:

::

  log_snapshots=1

This variable determines whether or not Alignak will log the snapshots it built.

  * 0 = Don't log snapshots
  * 1 = Log snapshots (default)


.. _configuration/core#no_event_handlers_during_downtimes:

Event Handler during downtimes
------------------------------

Format:

::

  no_event_handlers_during_downtimes=<0/1>

Default:

::

  no_event_handlers_during_downtimes=0

This option determines whether or not Alignak will run :ref:`event handlers <monitoring_features/event_handlers>` when the host or service is in a scheduled downtime.

  * 0 = Launch event handlers (Nagios behavior)
  * 1 = Don't launch event handlers



Performance data parameters
===========================

.. _configuration/core#process_performance_data:

Performance data processing option
----------------------------------

Format:

::

  process_performance_data=<0/1>

Example:

::

  process_performance_data=1

This value determines whether or not Alignak will process host and service check :ref:`performance data <advanced/perfdata>`.

  * 0 = Don't process performance data
  * 1 = Process performance data (default)

If you want to use tools like PNP, NagiosGrapher or Graphite set it to 1.


.. _configuration/core#perfdata_timeout:

Performance data processor command timeout
------------------------------------------

Format:

::

  perfdata_timeout=<seconds>

Example:

::

  perfdata_timeout=5

This is the maximum number of seconds that Alignak will allow a host performance data processor command or service performance data processor command to run. If a command exceeds this time limit it will be killed and a warning will be logged.


.. _configuration/core#host_perfdata_command:
.. _configuration/core#service_perfdata_command:

Host/Service performance data processing command
------------------------------------------------

Format:

::

  host_perfdata_command=<configobjects/command>
  service_perfdata_command=<configobjects/command>

Example:

::

  host_perfdata_command=process-host-perfdata
  service_perfdata_command=process-service-perfdata

This option allows you to specify a command to be run after every host/service check to process host/service :ref:`performance data <advanced/perfdata>` that may be returned from the check. The command argument is the short name of a command definition that you define in your object configuration file. This command is only executed if the :ref:`Performance Data Processing Option <configuration/core#process_performance_data>` option is enabled globally and if the ``process_perf_data`` directive in the host definition is enabled.


Advanced scheduling parameters
==============================


.. _configuration/core#passive_host_checks_are_soft:

Passive host checks are SOFT option
-----------------------------------

Format:

::

  passive_host_checks_are_soft=<0/1>

Example:

::

  passive_host_checks_are_soft=1

This option determines whether or not Alignak will treat :ref:`passive host checks <monitoring_features/passive_checks>` as HARD states or SOFT states. As a default, a passive host check result will put a host into a HARD state type. You can change this behavior by enabling this option.

  * 0 = Passive host checks are HARD (default)
  * 1 = Passive host checks are SOFT

.. warning:: This option is not yet implemented.


.. _configuration/core#enable_predictive_host_dependency_checks:
.. _configuration/core#enable_predictive_service_dependency_checks:

Predictive Host/Service dependency checks option
------------------------------------------------

Format:

::

  enable_predictive_host_dependency_checks=<0/1>
  enable_predictive_service_dependency_checks=<0/1>

Example:

::

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

Format:

::

  check_for_orphaned_services=<0/1>
  check_for_orphaned_hosts=<0/1>

Example:

::

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

This option determines whether or not Alignak will use soft state information when checking :ref:`host and service dependencies <advanced/dependencies>`. Normally it will only use the latest hard host or service state when checking dependencies. If you want it to use the latest state (regardless of whether its a soft or hard :ref:`state type <thebasics/statetypes>`), enable this option.

  * 0 = Don't use soft state dependencies (default)
  * 1 = Use soft state dependencies


.. warning:: This option is not yet implemented.


Performance tuning
===================

.. _configuration/core#cached_host_check_horizon:
.. _configuration/core#cached_service_check_horizon:

Cached Host/Service check horizon
---------------------------------

Format:

::

  cached_host_check_horizon=<seconds>
  cached_service_check_horizon=<seconds>

Example:

::

   cached_host_check_horizon=15
   cached_service_check_horizon=15

This option determines the maximum amount of time (in seconds) that the state of a previous host check is considered current. Cached host states (from host/service checks that were performed more recently than the time specified by this value) can improve host check performance immensely. Too high of a value for this option may result in (temporarily) inaccurate host/service states, while a low value may result in a performance hit for host/service checks. Use a value of 0 if you want to disable host/service check caching. More information on cached checks can be found :ref:`here <alignak_features/cached_checks>`.

.. tip::  Nagios default is 15s, but it's a tweak that make checks less accurate. So Alignak uses 0s as a default. If you have performance problems and you can't add a new scheduler or poller, increase this value and start to buy a new server because this won't be magical ;).

.. warning:: This option is not yet implemented.


.. _configuration/core#use_large_installation_tweaks:

Large installation tweaks option
--------------------------------

Format:

::

  use_large_installation_tweaks=<0/1>

Example:

::

  use_large_installation_tweaks=0

This option determines whether or not the Alignak daemon will take shortcuts to improve performance. These shortcuts result in the loss of a few features, but larger installations will likely see a lot of benefit from doing so. If you can't add new satellites to manage the load (like new pollers), you can activate it.

  * 0 = Don't use tweaks (default)
  * 1 = Use tweaks



Flapping parameters
===================

.. _configuration/core#enable_flap_detection:

Flap detection option
---------------------

Format:

::

  enable_flap_detection=<0/1>

Example:

::

  enable_flap_detection=1

This option determines whether or not Alignak will try and detect hosts and services that are “flapping". Flapping occurs when a host or service changes between states too frequently, resulting in a barrage of notifications being sent out. When Alignak detects that a host or service is flapping, it will temporarily suppress notifications for that host/service until it stops flapping.

More information on how flap detection and handling works can be found :ref:`here <monitoring_features/flapping>`.

  * 0 = Don't enable flap detection (default)
  * 1 = Enable flap detection


.. _configuration/core#low_host_flap_threshold:
.. _configuration/core#low_service_flap_threshold:

Low Service/Host flap threshold
-------------------------------

Format:

::

  low_service_flap_threshold=<percent>
  low_host_flap_threshold=<percent>

Example:

::

  low_service_flap_threshold=25.0
  low_host_flap_threshold=25.0

This option is used to set the low threshold for detection of host/service flapping. For more information on how flap detection and handling works (and how this option affects things) read :ref:`this <monitoring_features/flapping>`.


.. _configuration/core#high_host_flap_threshold:
.. _configuration/core#high_service_flap_threshold:

High Service/Host flap threshold
--------------------------------

Format:

::

  high_service_flap_threshold=<percent>
  high_host_flap_threshold=<percent>

Example:

::

  high_service_flap_threshold=50.0
  high_host_flap_threshold=50.0

This option is used to set the high threshold for detection of host/service flapping. For more information on how flap detection and handling works (and how this option affects things) read :ref:`this <monitoring_features/flapping>`.




.. _configuration/core#event_handler_timeout:
.. _configuration/core#notification_timeout:

Various commands timeouts
-------------------------

Format:

::

  event_handler_timeout=<seconds>  # default: 30s
  notification_timeout=<seconds>   # default: 30s
  ocsp_timeout=<seconds>           # default: 15s
  ochp_timeout=<seconds>           # default: 15s

Example:

::

  event_handler_timeout=60
  notification_timeout=60

This is the maximum number of seconds that Alignak will allow :ref:`event handlers <monitoring/eventhandlers>`, :ref:`notifications <monitoring/notifications>` to be run. If an command exceeds this time limit it will be killed and a warning will be logged.

There is often widespread confusion as to what this option really does. It is meant to be used as a last ditch mechanism to kill off commands which are misbehaving and not exiting in a timely manner. It should be set to something high (like 60 seconds or more for notification), so that each event handler command normally finishes executing within this time limit. If an event handler runs longer than this limit, Alignak will kill it off thinking it is a runaway processes.


Freshness check
===============

.. _configuration/core#check_service_freshness:
.. _configuration/core#check_host_freshness:

Host/Service freshness checking option
--------------------------------------

Format:

::

  check_service_freshness=<0/1>
  check_host_freshness=<0/1>

Example:

::

  check_service_freshness=0
  check_host_freshness=0

This option determines whether or not Alignak will periodically check the “freshness" of host/service checks. Enabling this option is useful for helping to ensure that :ref:`passive service checks <monitoring/passivechecks>` are received in a timely manner. More information on freshness checking can be found :ref:`here <alignak_features/freshness>`.

  * 0 = Don't check host/service freshness
  * 1 = Check host/service freshness (default)


.. _configuration/core#service_freshness_check_interval:
.. _configuration/core#host_freshness_check_interval:

Host/Service freshness check interval
-------------------------------------

Format:

::

  service_freshness_check_interval=<seconds>
  host_freshness_check_interval=<seconds>

Example:

::

  service_freshness_check_interval=60
  host_freshness_check_interval=60

This setting determines how often (in seconds) Alignak will periodically check the “freshness" of host/service check results. If you have disabled host/service freshness checking (with the ``check_service_freshness`` option), this option has no effect. More information on freshness checking can be found :ref:`here <alignak_features/freshness>`.


.. _configuration/core#additional_freshness_latency:

Additional freshness threshold latency option
---------------------------------------------

Format:

::

  additional_freshness_latency=<#>

Example:

::

  additional_freshness_latency=15

This option determines the number of seconds Alignak will add to any host or services freshness threshold it automatically calculates (e.g. those not specified explicitly by the user). More information on freshness checking can be found :ref:`here <alignak_features/freshness>`.



.. _configuration/core#triggers_dir:

Triggers directory
------------------

Format

::

  triggers_dir=<directory>

Example:

::

   triggers_dir=triggers.d

Used to specify the :ref:`trigger <alignak_features/triggers>` directory. It will open the directory and look recursively for .trig files.



.. _configuration/core#enable_notifications:

Notifications option
--------------------

Format:

::

  enable_notifications=<0/1>

Example:

::

  enable_notifications=1

This option determines whether or not Alignak will send out :ref:`notifications <monitoring_features/notifications>`. If this option is disabled, Alignak will not send out notifications for any host or service.

Values are as follows:
  * 0 = Disable notifications
  * 1 = Enable notifications (default)


.. _configuration/core#check_external_commands:

External command check option
-----------------------------

Format:

::

  check_external_commands=<0/1>

Example:

::

  check_external_commands=1

This option determines whether or not Alignak will execute the external commands that it receives. More information on external commands can be found :ref:`here <monitoring_features/extcommands>`.

  * 0 = Don't check external commands (default)
  * 1 = Check external commands (default)


Scheduling parameters
=====================

.. _configuration/core#execute_service_checks:

Service/Host check execution option
-----------------------------------

Format:

::

  execute_service_checks=<0/1>
  execute_host_checks=<0/1>

Example:

::

  execute_service_checks=1
  execute_host_checks=1

This option determines whether or not Alignak will execute :ref:`active host/service checks <monitoring_features/active_checks>`. If this option is disabled, Alignak will not execute any active host/service checks.

  * 0 = Don't execute service checks
  * 1 = Execute service checks (default)


.. _configuration/core#accept_passive_service_checks:

Passive Host/Service check acceptance option
--------------------------------------------

Format:

::

  accept_passive_service_checks=<0/1>
  accept_passive_host_checks=<0/1>

Example:

::

  accept_passive_service_checks=1
  accept_passive_host_checks=1

This option determines whether or not Alignak will accept :ref:`passive host/service checks <monitoring_features/passive_checks>`. If this option is disabled, Alignak will not accept any passive host/service checks.

  * 0 = Don't accept passive service/host checks
  * 1 = Accept passive service/host checks (default)


.. _configuration/core#enable_event_handlers:

Event handlers option
---------------------

Format:

::

  enable_event_handlers=<0/1>

Example:

::

  enable_event_handlers=1

This option determines whether or not Alignak will run :ref:`event handlers <monitoring_features/eventhandlers>`.

  * 0 = Disable event handlers
  * 1 = Enable event handlers (default)




.. _configuration/core#global_host_event_handler:
.. _configuration/core#global_service_event_handler:

Global Host/Service event handlers option
-----------------------------------------

Format:

::

  global_host_event_handler=<configobjects/command>
  global_service_event_handler=<configobjects/command>

Example:

::

  global_host_event_handler=log-host-event-to-db
  global_service_event_handler=log-service-event-to-db

This option allows you to specify a host event handler command that is to be run for every host state change. The global event handler is executed immediately prior to the event handler that you have optionally specified in each host definition. The command argument is the short name of a command that you define in your commands definition. The maximum amount of time that this command can run is controlled by the :ref:`Event Handler Timeout <configuration/core#event_handler_timeout>` option. More information on event handlers can be found :ref:`here <monitoring_features/event_handlers>`.

Such commands should not be so useful with the new Alignak distributed architecture. If you use it, look if you can avoid it because such commands will kill your performance!



.. _configuration/core#interval_length:

Timing interval length
----------------------

Format:

::

  interval_length=<seconds>

Example:

::

  interval_length=60

This is the number of seconds per “unit interval" used for timing in the scheduling queue, re-notifications, etc. "Units intervals" are used in the object configuration file to determine how often to run a service check, how often to re-notify a contact, etc.

The default value for this is set to 60, which means that a "unit value" of 1 in the object configuration file will mean 60 seconds (1 minute).

.. tip::  Changing this option is not a good thing with Alignak. It's not designed to be a hard real time monitoring system...



Naming and macros parameters
============================

.. _configuration/core#illegal_object_name_chars:

Illegal object name characters
------------------------------

Format:

::

  illegal_object_name_chars=<chars...>

Example:

::

  illegal_object_name_chars=`-!$%^&*"|'<>?,()=

This option allows you to specify illegal characters that cannot be used in host names, service descriptions, or names of other object types. Alignak will allow you to use most characters in object definitions, but we recommend not using the characters shown in the example above because it may give you problems in the web interface, notification commands, etc.


.. _configuration/core#illegal_macro_output_chars:

Illegal macro output characters
-------------------------------

Format:

::

  illegal_macro_output_chars=<chars...>

Example:

::

  illegal_macro_output_chars=`-$^&"|'<>

This option allows you to specify illegal characters that should be stripped from :ref:`macros <monitoring_features/macros>` before being used in notifications, event handlers, and other commands. This DOES NOT affect macros used in service or host check commands. You can choose to not strip out the characters shown in the example above, but I recommend you do not do this. Some of these characters are interpreted by the shell (i.e. the backtick) and can lead to security problems. The following macros are stripped of the characters you specify:

  * "$HOSTOUTPUT$"
  * "$HOSTPERFDATA$"
  * "$HOSTACKAUTHOR$"
  * "$HOSTACKCOMMENT$"
  * "$SERVICEOUTPUT$"
  * "$SERVICEPERFDATA$"
  * "$SERVICEACKAUTHOR$"
  * "$SERVICEACKCOMMENT$"


.. _configuration/core#admin_email:

Administrator email address
---------------------------

Format:

::

  admin_email=<email_address>

Example:

::

  admin_email=root@localhost.localdomain

This is the email address for the administrator of the local machine (i.e. the one that Alignak is running on). This value can be used in notification commands by using the "$ADMINEMAIL$" :ref:`macro <monitoring_features/macros>`.


.. _configuration/core#admin_pager:

Administrator pager (unused)
----------------------------

Format:

::

  admin_pager=<pager_number_or_pager_email_gateway>

Example:

::

  admin_pager=pageroot@localhost.localdomain

This is the pager number (or pager email gateway) for the administrator of the local machine (i.e. the one that Alignak is running on). The pager number/address can be used in notification commands by using the $ADMINPAGER$ :ref:`macro <monitoring_features/macros>`.


Statistics parameters
=====================

.. _configuration/core#statsd:

Statsd enabled
--------------

Format:

::

  statsd_enabled=<0/1>

Example:

::

  statsd_enabled=0

Enable or not the statsd communication. By default it's disabled.


Statsd host
-----------

Format:

::

  statsd_host=<host or ip>

Example:

::

  statsd_host=localhost

Configure your local statsd daemon address.



Statsd port
-----------

Format:

::

  statsd_port=<int>

Example:

::

  statsd_port=8125

Configure your local statsd daemon port. Notice that the port is in UDP


Statsd prefix
-------------

Format:

::

  statsd_prefix=<string>

Example:

::

  statsd_prefix=Alignak

The prefix to add before all your stats so you will find them easily in graphite

