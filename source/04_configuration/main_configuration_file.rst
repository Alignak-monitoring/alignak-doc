.. _configuration/main:

==============================================
 Main Configuration File (alignak.cfg) Options
==============================================

When creating and/or editing configuration files, keep the following in mind:

    * Lines that start with a '"#"' character are taken to be comments and are not processed
    * Variable names are case-sensitive
    * If you want to configure a process to use a specific module:

        * You must define the module in a **xxx.cfg** file in the **modules** directory
        * You must reference it in the **modules** section for that process, e.g. the **broker.cfg** file



The main configuration file is ``alignak.cfg``.
Below are listed the most common parameters used in the file. For other parameters (not mentionned by default) see :ref:`Main Configuration File Advanced <configuration/main-advanced>`

Default used options
====================

.. _configuration/main#cfg_dir:
.. _configuration/main#cfg_file:

Cfg dir and Cfg files
---------------------
Format :

::

    cfg_dir=<directory_name>
    cfg_file=<file_name>

Those are **statements and not parameters**. The arbiter considers them as order to open other(s) configuration(s) file(s)

For the ``cfg_dir`` one, the arbiter reads recursively the specified directory and **only** reads the files ending with ".cfg". It **does not** consider lines into those files as **statements** anymore (no more ``cfg_dir`` or ``cfg_file`` are considered in the read files).

Thus, the arbiter handles the main configuration files differently than any other files.



.. _configuration/main#retention_update_interval:

Automatic State Retention Update Interval
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


.. _configuration/main#max_service_check_spread:

Maximum Host/Service Check Spread
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


.. _configuration/main#host_check_timeout:
.. _configuration/main#service_check_timeout:

Service/Host Check Timeout
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

.. _configuration/main#timeout_exit_status:

Timeout Exit Status
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


.. _configuration/main#flap_history:

Flap History
------------

Format:

::

  flap_history=<int>

Default:

::

  flap_history=20

This option is used to set the history size of states keep by the scheduler to make the flapping calculation. By default, the value is 20 states kept.

The size in memory is for the scheduler daemon : 4Bytes * flap_history * (nb hosts + nb services). For a big environment, it costs 4 * 20 * (1000+10000) - 900Ko. So you can raise it to higher value if you want. To have more information about flapping, you can read :ref:`this <monitoring_features/flapping>`.


.. _configuration/main#max_plugins_output_length:

Max Plugins Output Length
-------------------------

Format:

::

  max_plugins_output_length=<int>

Default:

::

  max_plugins_output_length=8192

This option is used to set the max size in bytes for the checks plugins output. So if you have some truncated output like for huge disk check when you have a lot of partitions, increase this value.


.. _configuration/main#enable_problem_impacts_states_change:

Enable problem/impacts states change
------------------------------------

Format:

::

  enable_problem_impacts_states_change=<0/1>

Default:

::

  enable_problem_impacts_states_change=0

This option is used to know if we apply or not the state change when a host or service is impacted by a root problem (like the service's host going down or a host's parent being down too). The state will be changed by UNKNONW for a service and UNREACHABLE for a host until their next schedule check. This state change do not count as a attempt, it's just for console so the users know that theses objects got problems and the previous states are not sure.


.. _configuration/main#disable_old_nagios_parameters_whining:

Disable Old Nagios Parameters Whining
-------------------------------------

Format:

::

  disable_old_nagios_parameters_whining=<0/1>

Default:

::

  disable_old_nagios_parameters_whining=0

If 1, disable all notice and warning messages when the Arbiter is checking the configuration.


.. _configuration/main#use_timezone:

Timezone Option
---------------

Format:

::

  use_timezone=<tz from tz database>

Default:

::

  use_timezone=''

This option allows you to override the default timezone that this instance of Alignak runs in. Useful if you have multiple instances of Alignak that need to run from the same server, but have different local times associated with them. If not specified, Alignak will use the system configured timezone.



.. _configuration/main#enable_environment_macros:

Environment Macros Option
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


.. _configuration/main#log_initial_states:

Initial States Logging Option (Not implemented)
-----------------------------------------------

Format:

::

  log_initial_states=<0/1>

Default:

::

  log_initial_states=1

This variable determines whether or not Alignak will force all initial host and service states to be logged, even if they result in an OK state. Initial service and host states are normally only logged when there is a problem on the first check. Enabling this option is useful if you are using an application that scans the log file to determine long-term state statistics for services and hosts.

  * 0 = Don't log initial states
  * 1 = Log initial states


.. _configuration/main#no_event_handlers_during_downtimes:

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



Arbiter daemon part
===================

The following parameters are common to all daemons.

.. _configuration/main#workdir:

Workdir
-------

Format:

::

  workdir=<directory>

Default :

::

  workdir=/var/run/alignak/

This variable specify the working directory of the daemon.
In the arbiter case, if the value is empty, the directory name of lock_file parameter. See below


.. _configuration/main#lock_file:

Arbiter Lock File
------------------

Defined in nagios.cfg file.

Format:

::

  lock_file=<file_name>

Example:

::

  lock_file=/var/lib/alignak/arbiterd.pid

This option specifies the location of the lock file that Alignak **arbiter daemon** should create when it runs as a daemon (when started with the "-d" command line argument). This file contains the process id (PID) number of the running **arbiter** process.


.. _configuration/main#local_log:

Local Log
----------

Format:

::

  local_log=<filename>

Default:

::

  local_log=/var/log/alignak/arbiterd.log'


This variable specifies the log file for the daemon.


.. _configuration/main#log_level:

Log Level
----------

Format:

::

  log_level=[DEBUG,INFO,WARNING,ERROR,CRITICAL]

Default:

::

  log_level=WARNING


This variable specifies which logs will be raised by the arbiter daemon. For others daemons, it can be defined in their local \*d.ini files.


.. _configuration/main#alignak_user:

Arbiter Daemon User
--------------------

Defined in brokerd.ini, brokerd-windows.ini, pollerd.ini, pollerd-windows.ini, reactionnerd.ini, schedulerd.ini and schedulerd-windows.ini.

Format:

::

  alignak_user=username

Default:

::

  alignak_user=<current user>

This is used to set the effective user that the **Arbiter** process (main process) should run as.
After initial program startup, Alignak will drop its effective privileges and run as this user.



.. _configuration/main#alignak_group:

Arbiter Daemon user Group
--------------------------

Defined in brokerd.ini, brokerd-windows.ini, pollerd.ini, pollerd-windows.ini, reactionnerd.ini, schedulerd.ini and schedulerd-windows.ini.

Format:

::

  alignak_group=groupname

Default:

::

  alignak_group=<current group>

This is used to set the effective group of the user used to launch the **arbiter** daemon.


.. _configuration/main#modules_dir:

Modules directory
------------------

Format:

::

  modules_dir=<direname>

Default:

::

  modules_dir=/var/lib/alignak/modules


Path to the modules directory


.. _configuration/main#daemon_enabled:

Daemon Enabled
---------------

Format:

::

  daemon_enabled=[0/1]

Default:

::
  daemon_enabled=1

Set to 0 if you want to make this daemon (arbiter) **NOT** to run


.. _configuration/main#use_ssl:

Use SSL
-------

Format:

::

  use_ssl=[0/1]

Default:

::

  use_ssl=0

Use SSl or not. You have to enable it on other daemons too.


.. _configuration/main#ca_cert:

Ca Cert
--------

Format:

::

  ca_cert=<filename>

Default:

::

  ca_cert=etc/certs/ca.pem

Certification Authority (CA) certificate

.. warning::  Put full paths for certs


.. _configuration/main#server_cert:

Server Cert
------------

Format:

::

  server_cert=<filename>

Default:

::

  server_cert=/etc/certs/server.cert

Server certificate for SSL

.. warning::  Put full paths for certs


.. _configuration/main#server_key:

Server Key
-----------

Format:

::

  server_key=<filename>

Default:

::

  server_key=/etc/certs/server.key

Server key for SSL

.. warning::  Put full paths for certs


.. _configuration/main#hard_ssl_name_check:

Hard SSL Name Check
--------------------

Format:

::

  hard_ssl_name_check=[0/1]


Default:

::

  hard_ssl_name_check=0

Enable SSL name check.


.. _configuration/main#http_backend:

HTTP Backend
-------------

Format:

::

  http_backend=[auto, cherrypy, swsgiref]

Default:

::

  http_backend=auto

Specify which http_backend to use. Auto is better. If cherrypy3 is not available, it will fail back to swsgiref
.. note:: Actually, if you specify something else than cherrypy or auto, it will fall into swsgiref


.. _configuration/main:

===========================
Main advanced configuration
===========================


Tuning and advanced parameters
===============================

.. important::  If you do not know how to change the values of theses parameters, don't touch them :)
   (and ask for help on the mailing list).


Performance data parameters
============================

Performance Data Processor Command Timeout
-------------------------------------------

Format:

::

  perfdata_timeout=<seconds>

Example:

::

  perfdata_timeout=5

This is the maximum number of seconds that Shinken will allow a :ref:`host performance data processor command <configuration/configmain-advanced#host_perfdata_file_processing_command>` or :ref:`service performance data processor command <configuration/configmain-advanced#service_perfdata_file_processing_command>` to be run. If a command exceeds this time limit it will be killed and a warning will be logged.


.. _configuration/main#process_performance_data:

Performance Data Processing Option
-----------------------------------

Format:

::

  process_performance_data=<0/1>

Example:

::

  process_performance_data=1

This value determines whether or not Shinken will process host and service check :ref:`performance data <advanced/perfdata>`.

  * 0 = Don't process performance data
  * 1 = Process performance data (default)

If you want to use tools like PNP, NagiosGrapher or Graphite set it to 1.


.. _configuration/main#host_perfdata_command:
.. _configuration/main#service_perfdata_command:

Host/Service Performance Data Processing Command
-------------------------------------------------

Format:

::

  host_perfdata_command=<configobjects/command>
  service_perfdata_command=<configobjects/command>

Example:

::

  host_perfdata_command=process-host-perfdata
  service_perfdata_command=process-service-perfdata

This option allows you to specify a command to be run after every host/service check to process host/service :ref:`performance data <advanced/perfdata>` that may be returned from the check. The command argument is the short name of a :ref:`command definition <configobjects/command>` that you define in your object configuration file. This command is only executed if the :ref:`Performance Data Processing Option <configuration/configmain-advanced#process_performance_data>` option is enabled globally and if the "process_perf_data" directive in the :ref:`host definition <configobjects/host>` is enabled.


.. _configuration/main#host_perfdata_file:
.. _configuration/main#service_perfdata_file:

Host/Service Performance Data File
-----------------------------------

Format:

::

  host_perfdata_file=<file_name>
  service_perfdata_file=<file_name>

Example:

::

  host_perfdata_file=/var/lib/shinken/host-perfdata.dat
  service_perfdata_file=/var/lib/shinken/service-perfdata.dat

This option allows you to specify a file to which host/service :ref:`performance data <advanced/perfdata>` will be written after every host check. Data will be written to the performance file as specified by the :ref:`Host Performance Data File Template <configuration/configmain-advanced#host_perfdata_file_template>` option or the service one. Performance data is only written to this file if the :ref:`Performance Data Processing Option <configuration/configmain-advanced#process_performance_data>` option is enabled globally and if the "process_perf_data" directive in the :ref:`host definition <configobjects/host>` is enabled.


.. _configuration/main#host_perfdata_file_template:

Host Performance Data File Template
------------------------------------

Format:

::

  host_perfdata_file_template=<template>

Example:

::

  host_perfdata_file_template=[HOSTPERFDATA]\t$TIMET$\t$HOSTNAME$\t$HOSTEXECUTIONTIME$\t$HOSTOUTPUT$\t$HOSTPERFDATA$

This option determines what (and how) data is written to the :ref:`host performance data file <configuration/configmain-advanced#host_perfdata_file>`. The template may contain :ref:`macros <thebasics/macros>`, special characters (\t for tab, \r for carriage return, \n for newline) and plain text. A newline is automatically added after each write to the performance data file.


.. _configuration/main#service_perfdata_file_template:

Service Performance Data File Template
---------------------------------------

Format:

::

  service_perfdata_file_template=<template>

Example:

::

  service_perfdata_file_template=[SERVICEPERFDATA]\t$TIMET$\t$HOSTNAME$\t$SERVICEDESC$\t$SERVICEEXECUTIONTIME$\t$SERVICELATENCY$\t$SERVICEOUTPUT$\t$SERVICEPERFDATA$

This option determines what (and how) data is written to the :ref:`service performance data file <configuration/configmain-advanced#service_perfdata_file>`. The template may contain :ref:`macros <thebasics/macros>`, special characters (\t for tab, \r for carriage return, \n for newline) and plain text. A newline is automatically added after each write to the performance data file.


.. _configuration/main#host_perfdata_file_mode:
.. _configuration/main#service_perfdata_file_mode:

Host/Service Performance Data File Mode
----------------------------------------

Format:

::

  host_perfdata_file_mode=<mode>
  service_perfdata_file_mode=<mode>

Example:

::

  host_perfdata_file_mode=a
  service_perfdata_file_mode=a

This option determines how the :ref:`host performance data file <configuration/configmain-advanced#host_perfdata_file>` (or the service one) is opened. Unless the file is a named pipe you'll probably want to use the default mode of append.

  * a = Open file in append mode (default)
  * w = Open file in write mode
  * p = Open in non-blocking read/write mode (useful when writing to pipes)


.. _configuration/main#host_perfdata_file_processing_interval:
.. _configuration/main#service_perfdata_file_processing_interval:

Host/Service Performance Data File Processing Interval (Unused)
----------------------------------------------------------------

Format:

::

  host_perfdata_file_processing_interval=<seconds>
  service_perfdata_file_processing_interval=<seconds>

Example:

::

  host_perfdata_file_processing_interval=0
  service_perfdata_file_processing_interval=0

This option allows you to specify the interval (in seconds) at which the :ref:`host performance data file <configuration/configmain-advanced#host_perfdata_file>` (or the service one) is processed using the :ref:`host performance data file processing command <configuration/configmain-advanced#host_perfdata_command>`. A value of 0 indicates that the performance data file should not be processed at regular intervals.


.. _configuration/main#host_perfdata_file_processing_command:
.. _configuration/main#service_perfdata_file_processing_command:

Host/Service Performance Data File Processing Command (Unused)
---------------------------------------------------------------

Format:

::

  host_perfdata_file_processing_command=<configobjects/command>
  service_perfdata_file_processing_command=<configobjects/command>

Example:

::

  host_perfdata_file_processing_command=process-host-perfdata-file
  service_perfdata_file_processing_command=process-service-perfdata-file

This option allows you to specify the command that should be executed to process the :ref:`host performance data file <configuration/configmain-advanced#host_perfdata_file>` (or the service one). The command argument is the short name of a :ref:`command definition <configobjects/command>` that you define in your object configuration file. The interval at which this command is executed is determined by the :ref:`host_perfdata_file_processing_interval <configuration/configmain-advanced#host_perfdata_file_processing_interval>` directive.


Advanced scheduling parameters
===============================


.. _configuration/main#passive_host_checks_are_soft:

Passive Host Checks Are SOFT Option (Not implemented)
------------------------------------------------------

Format:

::

  passive_host_checks_are_soft=<0/1>

Example:

::

  passive_host_checks_are_soft=1

This option determines whether or not Shinken will treat :ref:`passive host checks <thebasics/passivechecks>` as HARD states or SOFT states. By default, a passive host check result will put a host into a :ref:`HARD state type <thebasics/statetypes>`. You can change this behavior by enabling this option.

  * 0 = Passive host checks are HARD (default)
  * 1 = Passive host checks are SOFT



.. _configuration/main#enable_predictive_host_dependency_checks:
.. _configuration/main#enable_predictive_service_dependency_checks:

Predictive Host/Service Dependency Checks Option (Unused)
----------------------------------------------------------

Format:

::

  enable_predictive_host_dependency_checks=<0/1>
  enable_predictive_service_dependency_checks=<0/1>

Example:

::

  enable_predictive_host_dependency_checks=1
  enable_predictive_service_dependency_checks=1

This option determines whether or not Shinken will execute predictive checks of hosts/services that are being depended upon (as defined in :ref:`host/services dependencies <advanced/dependencies>`) for a particular host/service when it changes state. Predictive checks help ensure that the dependency logic is as accurate as possible. More information on how predictive checks work can be found :ref:`here <advanced/dependencychecks>`.

  * 0 = Disable predictive checks
  * 1 = Enable predictive checks (default)


.. _configuration/main#check_for_orphaned_services:
.. _configuration/main#check_for_orphaned_hosts:

Orphaned Host/Service Check Option
-----------------------------------

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

If this option is enabled and Shinken finds that results for a particular check have not come back, it will log an error message and reschedule the check. If you start seeing checks that never seem to get rescheduled, enable this option and see if you notice any log messages about orphaned services.

  * 0 = Don't check for orphaned service checks
  * 1 = Check for orphaned service checks (default)




.. _configuration/main#soft_state_dependencies:

Soft State Dependencies Option (Not implemented)
-------------------------------------------------

Format:  soft_state_dependencies=<0/1>
Example:  soft_state_dependencies=0

This option determines whether or not Shinken will use soft state information when checking :ref:`host and service dependencies <advanced/dependencies>`. Normally it will only use the latest hard host or service state when checking dependencies. If you want it to use the latest state (regardless of whether its a soft or hard :ref:`state type <thebasics/statetypes>`), enable this option.

  * 0 = Don't use soft state dependencies (default)
  * 1 = Use soft state dependencies


Performance tuning
===================

.. _configuration/main#cached_host_check_horizon:
.. _configuration/main#cached_service_check_horizon:

Cached Host/Service Check Horizon
----------------------------------

Format:

::

  cached_host_check_horizon=<seconds>
  cached_service_check_horizon=<seconds>

Example:

::

   cached_host_check_horizon=15
   cached_service_check_horizon=15

This option determines the maximum amount of time (in seconds) that the state of a previous host check is considered current. Cached host states (from host/service checks that were performed more recently than the time specified by this value) can improve host check performance immensely. Too high of a value for this option may result in (temporarily) inaccurate host/service states, while a low value may result in a performance hit for host/service checks. Use a value of 0 if you want to disable host/service check caching. More information on cached checks can be found :ref:`here <advanced/cachedchecks>`.

.. tip::  Nagios default is 15s, but it's a tweak that make checks less accurate. So Shinken use 0s as a default. If you have performances problems and you can't add a new scheduler or poller, increase this value and start to buy a new server because this won't be magical.


.. _configuration/main#use_large_installation_tweaks:

Large Installation Tweaks Option
---------------------------------

Format:

::

  use_large_installation_tweaks=<0/1>

Example:

::

  use_large_installation_tweaks=0

This option determines whether or not the Shinken daemon will take shortcuts to improve performance. These shortcuts result in the loss of a few features, but larger installations will likely see a lot of benefit from doing so. If you can't add new satellites to manage the load (like new pollers), you can activate it. More information on what optimizations are taken when you enable this option can be found :ref:`here <tuning/largeinstalltweaks>`.

  * 0 = Don't use tweaks (default)
  * 1 = Use tweaks



Flapping parameters
====================

.. _configuration/main#enable_flap_detection:

Flap Detection Option
----------------------

Format:

::

  enable_flap_detection=<0/1>

Example:

::

  enable_flap_detection=1

This option determines whether or not Shinken will try and detect hosts and services that are “flapping". Flapping occurs when a host or service changes between states too frequently, resulting in a barrage of notifications being sent out. When Shinken detects that a host or service is flapping, it will temporarily suppress notifications for that host/service until it stops flapping.

More information on how flap detection and handling works can be found :ref:`here <advanced/flapping>`.

  * 0 = Don't enable flap detection (default)
  * 1 = Enable flap detection


.. _configuration/main#low_host_flap_threshold:
.. _configuration/main#low_service_flap_threshold:

Low Service/Host Flap Threshold
--------------------------------

Format:

::

  low_service_flap_threshold=<percent>
  low_host_flap_threshold=<percent>

Example:

::

  low_service_flap_threshold=25.0
  low_host_flap_threshold=25.0

This option is used to set the low threshold for detection of host/service flapping. For more information on how flap detection and handling works (and how this option affects things) read :ref:`this <advanced/flapping>`.


.. _configuration/main#high_host_flap_threshold:
.. _configuration/main#high_service_flap_threshold:

High Service/Host Flap Threshold
---------------------------------

Format:

::

  high_service_flap_threshold=<percent>
  high_host_flap_threshold=<percent>

Example:

::

  high_service_flap_threshold=50.0
  high_host_flap_threshold=50.0

This option is used to set the high threshold for detection of host/service flapping. For more information on how flap detection and handling works (and how this option affects things) read :ref:`this <advanced/flapping>`.




.. _configuration/main#event_handler_timeout:
.. _configuration/main#notification_timeout:

Various commands Timeouts
--------------------------

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
  ocsp_timeout=5
  ochp_timeout=5

This is the maximum number of seconds that Shinken will allow :ref:`event handlers <advanced/eventhandlers>`, notification, :ref:`obsessive compulsive service processor command <configuration/configmain-advanced#ocsp_command>` or a :ref:`Obsessive Compulsive Host Processor Command <configuration/configmain-advanced#ochp_command>` to be run. If an command exceeds this time limit it will be killed and a warning will be logged.

There is often widespread confusion as to what this option really does. It is meant to be used as a last ditch mechanism to kill off commands which are misbehaving and not exiting in a timely manner. It should be set to something high (like 60 seconds or more for notification, less for oc*p commands), so that each event handler command normally finishes executing within this time limit. If an event handler runs longer than this limit, Shinken will kill it off thinking it is a runaway processes.


Old Obsess Over commands
=========================

.. _configuration/main#obsess_over_services:

Obsess Over Services Option
----------------------------

Format:

::

  obsess_over_services=<0/1>

Example:

::

  obsess_over_services=1

This value determines whether or not Shinken will “obsess" over service checks results and run the :ref:`obsessive compulsive service processor command <configuration/configmain-advanced#ocsp_command>` you define. I know _ funny name, but it was all I could think of. This option is useful for performing :ref:`distributed monitoring <advanced/distributed>`. If you're not doing distributed monitoring, don't enable this option.

  * 0 = Don't obsess over services (default)
  * 1 = Obsess over services


.. _configuration/main#ocsp_command:

Obsessive Compulsive Service Processor Command
-----------------------------------------------

Format:

::

  ocsp_command=<configobjects/command>

Example:

::

  ocsp_command=obsessive_service_handler

This option allows you to specify a command to be run after every service check, which can be useful in :ref:`distributed monitoring <advanced/distributed>`. This command is executed after any :ref:`event handler <advanced/eventhandlers>` or :ref:`notification <thebasics/notifications>` commands. The command argument is the short name of a :ref:`command definition <configobjects/command>` that you define in your object configuration file.

It's used nearly only for the old school distributed architecture. If you use it, please look at new architecture capabilities that are far efficient than the old one. More information on distributed monitoring can be found :ref:`here <advanced/distributed>`. This command is only executed if the :ref:`Obsess Over Services Option <configuration/configmain-advanced#obsess_over_services>` option is enabled globally and if the "obsess_over_service" directive in the :ref:`service definition <configobjects/service>` is enabled.


.. _configuration/main#obsess_over_hosts:

Obsess Over Hosts Option
-------------------------

Format:

::

  obsess_over_hosts=<0/1>

Example:

::

  obsess_over_hosts=1

This value determines whether or not Shinken will “obsess" over host checks results and run the :ref:`obsessive compulsive host processor command <configuration/configmain-advanced#ochp_command>` you define. Same like the service one but for hosts :)

  * 0 = Don't obsess over hosts (default)
  * 1 = Obsess over hosts


.. _configuration/main#ochp_command:

Obsessive Compulsive Host Processor Command
--------------------------------------------

Format:

::

  ochp_command=<configobjects/command>

Example:

::

  ochp_command=obsessive_host_handler

This option allows you to specify a command to be run after every host check, which can be useful in :ref:`distributed monitoring <advanced/distributed>`. This command is executed after any :ref:`event handler <advanced/eventhandlers>` or :ref:`notification <thebasics/notifications>` commands. The command argument is the short name of a :ref:`command definition <configobjects/command>` that you define in your object configuration file.

This command is only executed if the :ref:`Obsess Over Hosts Option <configuration/configmain-advanced#obsess_over_hosts>` option is enabled globally and if the "obsess_over_host" directive in the :ref:`host definition <configobjects/host>` is enabled.


Freshness check
================

.. _configuration/main#check_service_freshness:
.. _configuration/main#check_host_freshness:

Host/Service Freshness Checking Option
---------------------------------------

Format:

::

  check_service_freshness=<0/1>
  check_host_freshness=<0/1>

Example:

::

  check_service_freshness=0
  check_host_freshness=0

This option determines whether or not Shinken will periodically check the “freshness" of host/service checks. Enabling this option is useful for helping to ensure that :ref:`passive service checks <thebasics/passivechecks>` are received in a timely manner. More information on freshness checking can be found :ref:`here <advanced/freshness>`.

  * 0 = Don't check host/service freshness
  * 1 = Check host/service freshness (default)


.. _configuration/main#service_freshness_check_interval:
.. _configuration/main#host_freshness_check_interval:

Host/Service Freshness Check Interval
--------------------------------------

Format:

::

  service_freshness_check_interval=<seconds>
  host_freshness_check_interval=<seconds>

Example:

::

  service_freshness_check_interval=60
  host_freshness_check_interval=60

This setting determines how often (in seconds) Shinken will periodically check the “freshness" of host/service check results. If you have disabled host/service freshness checking (with the :ref:`check_service_freshness <configuration/configmain-advanced#check_service_freshness>` option), this option has no effect. More information on freshness checking can be found :ref:`here <advanced/freshness>`.


.. _configuration/main#additional_freshness_latency:

Additional Freshness Threshold Latency Option (Not implemented)
----------------------------------------------------------------

Format:

::

  additional_freshness_latency=<#>

Example:

::

  additional_freshness_latency=15

This option determines the number of seconds Shinken will add to any host or services freshness threshold it automatically calculates (e.g. those not specified explicitly by the user). More information on freshness checking can be found :ref:`here <advanced/freshness>`.



.. _configuration/main#human_timestamp_log:

Human format for log timestamp
-------------------------------

Say if the timespam should be a unixtime (default) or a human read one.

Format

::

  human_timestamp_log=[0/1]

Example

::

  human_timestamp_log=0


This directive is used to specify if the timespam before the log entry should be in unixtime (like [1302874960]) which is the default, or a human readable one (like [Fri Apr 15 15:43:19 2011]).

Beware : if you set the human format, some automatic parsing log tools won't work!


.. _configuration/main#resource_file:

Resource File
--------------

Defined in shinken.cfg file.

Format

::

   resource_file=<file_name>

Example:

::

  resource_file=/etc/shinken/resource.cfg

This is used to specify an optional resource file that can contain "$USERn$" :ref:`Understanding Macros and How They Work <thebasics/macros>` definitions. "$USERn$" macros are useful for storing usernames, passwords, and items commonly used in command definitions (like directory paths).
A classical variable used is $USER1$, used to store the plugins path, "/usr/lib/nagios/plugins" on a classic installation.


.. _configuration/main#triggers_dir:

Triggers directory
-------------------

Format

::

  triggers_dir=<directory>

Example:

::

   triggers_dir=triggers.d

Used to specify the :ref:`trigger <advanced/triggers>` directory. It will open the directory and look recursively for .trig files.



.. _configuration/main#idontcareaboutsecurity:

Bypass security checks for the Arbiter daemon
----------------------------------------------

Defined in brokerd.ini, brokerd-windows.ini, pollerd.ini, pollerd-windows.ini, reactionnerd.ini, schedulerd.ini and schedulerd-windows.ini.

Format:

::

  idontcareaboutsecurity=<0/1>

Example:

::

  idontcareaboutsecurity=0

This option determines whether or not Shinken will allow the Arbiter daemon to run under the root account.
If this option is disabled, Shinken will bailout if the :ref:`nagios_user <configuration/configmain#shinken_user>` or the :ref:`nagios_group <configuration/configmain#shinken_group>` is configured with the root account.

The Shinken daemons do not need root right. Without a good reason do not run thems under this account!
  * 0 = Be a responsible administrator
  * 1 = Make crazy your security manager


.. _configuration/main#enable_notifications:

Notifications Option
---------------------

Format:

::

  enable_notifications=<0/1>

Example:

::

  enable_notifications=1

This option determines whether or not Shinken will send out :ref:`notifications <thebasics/notifications>`. If this option is disabled, Shinken will not send out notifications for any host or service.

Values are as follows:
  * 0 = Disable notifications
  * 1 = Enable notifications (default)


.. _configuration/main#log_rotation_method:

Log Rotation Method (Not fully implemented)
--------------------------------------------

Format:

::

  log_rotation_method=<n/h/d/w/m>

Example:

::

  log_rotation_method=d

This is the rotation method that you would like Shinken to use for your log file on the **broker server**. Values are as follows:

  * n = None (don't rotate the log - this is the default)
  * h = Hourly (rotate the log at the top of each hour)
  * d = Daily (rotate the log at midnight each day)
  * w = Weekly (rotate the log at midnight on Saturday)
  * m = Monthly (rotate the log at midnight on the last day of the month)

.. tip::  From now, only the d (Daily) parameter is managed.


.. _configuration/main#check_external_commands:

External Command Check Option
------------------------------

Format:

::

  check_external_commands=<0/1>

Example:

::

  check_external_commands=1

This option determines whether or not Shinken will check the :ref:`External Command File <configuration/configmain-advanced#command_file>` for commands that should be executed with the **arbiter daemon**. More information on external commands can be found :ref:`here <advanced/extcommands>`.

  * 0 = Don't check external commands (default)
  * 1 = Check external commands (default)

.. note::  FIX ME : Find the real default value


.. _configuration/main#command_file:

External Command File
----------------------

Defined in nagios.cfg file.

Format:

::

  command_file=<file_name>

Example:

::

  command_file=/var/lib/shinken/rw/nagios.cmd

This is the file that Shinken will check for external commands to process with the **arbiter daemon**. The :ref:`command CGI <thebasics/cgis#cmd_cgi>` writes commands to this file. The external command file is implemented as a named pipe (FIFO), which is created when Nagios starts and removed when it shuts down. More information on external commands can be found :ref:`here <advanced/extcommands>`.

.. todo: where is thebasics/cgis#cmd-cgi (thebasics-cgis#thebasics-cgis-cmd_cgi-)?

.. tip::  This external command file is not managed under Windows system. Please use others way to send commands like the LiveStatus module for example.



.. _configuration/main#retain_state_information:

State Retention Option (Not implemented)
-----------------------------------------

Format:

::

  retain_state_information=<0/1>

Example:

::

  retain_state_information=1

This option determines whether or not Shinken will retain state information for hosts and services between program restarts. If you enable this option, you should supply a value for the :ref:`State Retention File <configuration/configmain-advanced#state_retention_file>` variable. When enabled, Shinken will save all state information for hosts and service before it shuts down (or restarts) and will read in previously saved state information when it starts up again.
  * 0 = Don't retain state information
  * 1 = Retain state information (default)

.. note::  Idea to approve : Mark it as Unused : `Related topic`_. A Shinken module replace it.


.. _configuration/main#state_retention_file:

State Retention File
---------------------

Format:

::

  state_retention_file=<file_name>

Example:

::

  state_retention_file=/var/lib/shinken/retention.dat

This is the file that Shinken **scheduler daemons** will use for storing status, downtime, and comment information before they shuts down. When Shinken is restarted it will use the information stored in this file for setting the initial states of services and hosts before it starts monitoring anything. In order to make Shinken retain state information between program restarts, you must enable the :ref:`State Retention Option <configuration/configmain-advanced#retain_state_information>` option.

.. important::  The file format is not the same between Shinken and Nagios! The retention.dat generated with Nagios will not load into Shinken.




Scheduling parameters
======================

.. _configuration/main#execute_service_checks:

Service/Host Check Execution Option
------------------------------------

Format:

::

  execute_service_checks=<0/1>
  execute_host_checks=<0/1>

Example:

::

  execute_service_checks=1
  execute_host_checks=1

This option determines whether or not Shinken will execute service/host checks. Do not change this option unless you use a old school distributed architecture. And even if you do this, please change your architecture with a cool new one far more efficient.

  * 0 = Don't execute service checks
  * 1 = Execute service checks (default)


.. _configuration/main#accept_passive_service_checks:

Passive Service/Host Check Acceptance Option
---------------------------------------------

Format:

::

  accept_passive_service_checks=<0/1>
  accept_passive_host_checks=<0/1>

Example:

::

  accept_passive_service_checks=1
  accept_passive_host_checks=1

This option determines whether or not Shinken will accept :ref:`passive service/host checks <thebasics/passivechecks>`. If this option is disabled, Nagios will not accept any passive service/host checks.

  * 0 = Don't accept passive service/host checks
  * 1 = Accept passive service/host checks (default)


.. _configuration/main#enable_event_handlers:

Event Handler Option
---------------------

Format:

::

  enable_event_handlers=<0/1>

Example:

::

  enable_event_handlers=1

This option determines whether or not Shinken will run :ref:`event handlers <advanced/eventhandlers>`.

  * 0 = Disable event handlers
  * 1 = Enable event handlers (default)




.. _configuration/main#use_syslog:

Syslog Logging Option
----------------------

Format:

::

  use_syslog=<0/1>

Example:

::

  use_syslog=1

This variable determines whether messages are logged to the syslog facility on your local host. Values are as follows:

  * 0 = Don't use syslog facility
  * 1 = Use syslog facility

.. tip::  This is a Unix Os only option.


.. _configuration/main#log_notifications:

Notification Logging Option
----------------------------

Format:

::

  log_notifications=<0/1>

Example:

::

  log_notifications=1

This variable determines whether or not notification messages are logged. If you have a lot of contacts or regular service failures your log file will grow (let say some Mo by day for a huge configuration, so it's quite OK for nearly every one to log them). Use this option to keep contact notifications from being logged.

  * 0 = Don't log notifications
  * 1 = Log notifications


.. _configuration/main#log_service_retries:
.. _configuration/main#log_host_retries:

Service/Host Check Retry Logging Option (Not implemented)
----------------------------------------------------------

Format:

::

  log_service_retries=<0/1>
  log_host_retries=<0/1>

Example:

::

  log_service_retries=0
  log_host_retries=0

This variable determines whether or not service/host check retries are logged. Service check retries occur when a service check results in a non-OK state, but you have configured Shinken to retry the service more than once before responding to the error. Services in this situation are considered to be in "soft" states. Logging service check retries is mostly useful when attempting to debug Shinken or test out service/host :ref:`event handlers <advanced/eventhandlers>`.

  * 0 = Don't log service/host check retries (default)
  * 1 = Log service/host check retries


.. _configuration/main#log_event_handlers:

Event Handler Logging Option
-----------------------------

Format:

::

  log_event_handlers=<0/1>

Example:

::

  log_event_handlers=1

This variable determines whether or not service and host :ref:`event handlers <advanced/eventhandlers>` are logged. Event handlers are optional commands that can be run whenever a service or hosts changes state. Logging event handlers is most useful when debugging Shinken or first trying out your event handler scripts.

  * 0 = Don't log event handlers
  * 1 = Log event handlers




.. _configuration/main#log_external_commands:

External Command Logging Option
--------------------------------

Format:

::

  log_external_commands=<0/1>

Example:

::

  log_external_commands=1

This variable determines whether or not Shinken will log :ref:`external commands <advanced/extcommands>` that it receives.

  * 0 = Don't log external commands
  * 1 = Log external commands (default)


.. _configuration/main#log_passive_checks:

Passive Check Logging Option (Not implemented)
-----------------------------------------------

Format:

::

  log_passive_checks=<0/1>

Example:

::

  log_passive_checks=1

This variable determines whether or not Shinken will log :ref:`passive host and service checks <thebasics/passivechecks>` that it receives from the :ref:`external command file <configuration/configmain-advanced#command_file>`.

  * 0 = Don't log passive checks
  * 1 = Log passive checks (default)


.. _configuration/main#global_host_event_handler:
.. _configuration/main#global_service_event_handler:

Global Host/Service Event Handler Option (Not implemented)
-----------------------------------------------------------

Format:

::

  global_host_event_handler=<configobjects/command>
  global_service_event_handler=<configobjects/command>

Example:

::

  global_host_event_handler=log-host-event-to-db
  global_service_event_handler=log-service-event-to-db

This option allows you to specify a host event handler command that is to be run for every host state change. The global event handler is executed immediately prior to the event handler that you have optionally specified in each host definition. The command argument is the short name of a command that you define in your :ref:`Object Configuration Overview <configuration/configobject>`. The maximum amount of time that this command can run is controlled by the :ref:`Event Handler Timeout <configuration/configmain-advanced#event_handler_timeout>` option. More information on event handlers can be found :ref:`here <advanced/eventhandlers>`.

Such commands should not be so useful with the new Shinken distributed architecture. If you use it, look if you can avoid it because such commands will kill your performances.



.. _configuration/main#interval_length:

Timing Interval Length
-----------------------

Format:

::

  interval_length=<seconds>

Example:

::

  interval_length=60

This is the number of seconds per “unit interval" used for timing in the scheduling queue, re-notifications, etc. "Units intervals" are used in the object configuration file to determine how often to run a service check, how often to re-notify a contact, etc.

The default value for this is set to 60, which means that a "unit value" of 1 in the object configuration file will mean 60 seconds (1 minute).

.. tip::  Set this option top 1 is not a good thing with Shinken. It's not design to be a hard real time (<5seconds) monitoring system. Nearly no one need such hard real time (maybe only the Nuclear center or a market place like the London Exchange...).



Old CGI related parameter
==========================

If you are using the old CGI from Nagios, please migrate to a new WebUI. For historical perspective you can find information on the :ref:`specific CGI parameters <integration/specific-cgi-parameters>`.


Unused parameters
==================

The below parameters are inherited from Nagios but are not used in Shinken. You can defined them but if you don't it will be the same :)

They are listed on another page :ref:`unused Nagios parameters <advanced/unused-nagios-parameters>`.



All the others :)
==================


.. _configuration/main#date_format:

Date Format (Not implemented)
------------------------------

Format:

::

  date_format=<option>

Example:

::

  date_format=us

This option allows you to specify what kind of date/time format Shinken should use in date/time :ref:`macros <thebasics/macros>`. Possible options (along with example output) include:

============== =================== ===================
Option         Output Format       Sample Output
us             MM/DD/YYYY HH:MM:SS 06/30/2002 03:15:00
euro           DD/MM/YYYY HH:MM:SS 30/06/2002 03:15:00
iso8601        YYYY-MM-DD HH:MM:SS 2002-06-30 03:15:00
strict-iso8601 YYYY-MM-DDTHH:MM:SS 2002-06-30T03:15:00
============== =================== ===================




.. _configuration/main#illegal_object_name_chars:

Illegal Object Name Characters
-------------------------------

Format:

::

  illegal_object_name_chars=<chars...>

Example:

::

  illegal_object_name_chars=`-!$%^&*"|'<>?,()=

This option allows you to specify illegal characters that cannot be used in host names, service descriptions, or names of other object types. Shinken will allow you to use most characters in object definitions, but I recommend not using the characters shown in the example above. Doing may give you problems in the web interface, notification commands, etc.


.. _configuration/main#illegal_macro_output_chars:

Illegal Macro Output Characters
--------------------------------

Format:

::

  illegal_macro_output_chars=<chars...>

Example:

::

  illegal_macro_output_chars=`-$^&"|'<>

This option allows you to specify illegal characters that should be stripped from :ref:`macros <thebasics/macros>` before being used in notifications, event handlers, and other commands. This DOES NOT affect macros used in service or host check commands. You can choose to not strip out the characters shown in the example above, but I recommend you do not do this. Some of these characters are interpreted by the shell (i.e. the backtick) and can lead to security problems. The following macros are stripped of the characters you specify:

  * "$HOSTOUTPUT$"
  * "$HOSTPERFDATA$"
  * "$HOSTACKAUTHOR$"
  * "$HOSTACKCOMMENT$"
  * "$SERVICEOUTPUT$"
  * "$SERVICEPERFDATA$"
  * "$SERVICEACKAUTHOR$"
  * "$SERVICEACKCOMMENT$"


.. _configuration/main#use_regexp_matching:

Regular Expression Matching Option (Not implemented)
-----------------------------------------------------

Format:

::

  use_regexp_matching=<0/1>

Example:

::

  use_regexp_matching=0

This option determines whether or not various directives in your :ref:`Object Configuration Overview <configuration/configobject>` will be processed as regular expressions. More information on how this works can be found :ref:`here <advanced/objecttricks>`.

  * 0 = Don't use regular expression matching (default)
  * 1 = Use regular expression matching


.. _configuration/main#use_true_regexp_matching:

True Regular Expression Matching Option (Not implemented)
----------------------------------------------------------

Format:

::

  use_true_regexp_matching=<0/1>

Example:

::

  use_true_regexp_matching=0

If you've enabled regular expression matching of various object directives using the :ref:`Regular Expression Matching Option <configuration/configmain-advanced#use_regexp_matching>` option, this option will determine when object directives are treated as regular expressions. If this option is disabled (the default), directives will only be treated as regular expressions if they contain \*, ?, +, or \.. If this option is enabled, all appropriate directives will be treated as regular expression _ be careful when enabling this! More information on how this works can be found :ref:`here <advanced/objecttricks>`.

  * 0 = Don't use true regular expression matching (default)
  * 1 = Use true regular expression matching


.. _configuration/main#admin_email:

Administrator Email Address (unused)
-------------------------------------

Format:

::

  admin_email=<email_address>

Example:

::

  admin_email=root@localhost.localdomain

This is the email address for the administrator of the local machine (i.e. the one that Shinken is running on). This value can be used in notification commands by using the "$ADMINEMAIL$" :ref:`macro <thebasics/macros>`.


.. _configuration/main#admin_pager:

Administrator Pager (unused)
-----------------------------

Format:

::

  admin_pager=<pager_number_or_pager_email_gateway>

Example:

::

  admin_pager=pageroot@localhost.localdomain

This is the pager number (or pager email gateway) for the administrator of the local machine (i.e. the one that Shinken is running on). The pager number/address can be used in notification commands by using the $ADMINPAGER$ :ref:`macro <thebasics/macros>`.


Shinken.io api_key
-----------------------------

Format:

::

  api_key=<api_key>

Example:

::

  api_key=AZERTYUIOP

This is the api_key/scret to exchange with shinken.io and especially the kernel.shinken.io service that will print your shinken metrics. To enable it you must fill the api_key and secret parameters. You must register to http://shinken.io and look at your profile http://shinken.io/~ for your api_key and your secret.


Shinken.io secret
-----------------------------

Format:

::

  secret=<secret>

Example:

::

  secret=QSDFGHJ

This is the api_key/scret to exchange with shinken.io and especially the kernel.shinken.io service that will print your shinken metrics. To enable it you must fill the api_key and secret parameters. You must register to http://shinken.io and look at your profile http://shinken.io/~ for your api_key and your secret.

.. _configuration/main#statsd:

Statsd host
-----------------------------

Format:

::

  statsd_host=<host or ip>

Example:

::

  statsd_host=localhost

Configure your local statsd daemon address.



Statsd port
-----------------------------

Format:

::

  statsd_port=<int>

Example:

::

  statsd_port=8125

Configure your local statsd daemon port. Notice that the port is in UDP


Statsd prefix
-----------------------------

Format:

::

  statsd_prefix=<string>

Example:

::

  statsd_prefix=shinken

The prefix to add before all your stats so you will find them easily in graphite


Statsd enabled (or not)
-----------------------------

Format:

::

  statsd_enabled=<0/1>

Example:

::

  statsd_enabled=0

Enable or not the statsd communication. By default it's disabled.


Statsd metrics polling interval
-------------------------------

Format:

::

  statsd_interval=<int>

Example:

::

  statsd_interval=5

Metrics such as internal queues length (checks, broks), number of elements in
the configuration, latency and so on...may also be exposed via statsd at the
interval specified in this parameter.


Statsd metric name pattern
-----------------------------

Format:

::

  statsd_pattern=<string>

Example:

::

  statsd_interval=shinken.{name}.{metric}

Allows to customize metric names using a pattern string. Each metric has a base name which may be enriched using placeholders under the python `format` python string notation. The available placeholders are `service` (the service type), `metric` (the metric name) and `name` (the service name). Note that this parameter is mutually exclusive with `statsd_prefix` and has precedence if both are defined.


Statsd metrics filter
-----------------------------

Format:

::

  statsd_types=<string>

Example:

::

  statsd_types=system,queue,object,perf

Allows to filter the metrics to send to statsd. Each metric is attached a type, and only the metrics holding the specifed types will be sent. See the metrics complete descriptions to see the available types.


.. _Related topic: http://www.shinken-monitoring.org/forum/index.php/topic,21.0.html
