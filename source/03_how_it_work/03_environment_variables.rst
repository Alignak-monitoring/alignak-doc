.. _howitworks/environment:

=====================
Environment variables
=====================

Alignak uses some environment variables. These variables, if defined, always take precedence over the usual configuration parameters.


Alignak main configuration file
-------------------------------

An environment variable exist to define the Alignak main configuration file:

    ``ALIGNAK_CONFIGURATION_FILE``
        the main ini environment configuration file

When Alignak is started with system services, :ref:`see how to declare this variable <run_alignak/services>`.

Alignak running user/group
--------------------------

Environment variables exist to define the Alignak user/group running account:

    ``ALIGNAK_USER`` and ``ALIGNAK_GROUP``
        the user account used to run Alignak daemons

When Alignak is started with system services, :ref:`see how to declare these variables <run_alignak/services>`.

These variables override the corresponding `user` and `group` :ref:`configuration variables <configuration/core#user_group>`.

Alignak logger configuration
----------------------------

An environment variable exist to define the Alignak logger configuration file:

    ``ALIGNAK_LOGGER_CONFIGURATION``
        the Json formatted logger configuration file

This variable overrides the corresponding `logger_configuration` :ref:`configuration variable <configuration/core#logger_configuration>`.


Alignak internal metrics
------------------------

When the Alignak internal metrics are sent to Graphite, the daemons will send the metrics to Graphite in bulk mode. A flushing happens periodically but the metrics are pushed only if the internal metrics queue contains a minimum of `` ALIGNAK_STATS_FLUSH_COUNT`` metrics to be sent. The default value is 256 and it can be configured thanks to the environment variable.

If some environment variables exist the Alignak internal metrics will be logged to a file in append mode:

    ``ALIGNAK_STATS_FILE``
        the file name

    ``ALIGNAK_STATS_FILE_LINE_FMT``
        defaults to [#date#] #counter# #value# #uom#\n'

    ``ALIGNAK_STATS_FILE_DATE_FMT``
        defaults to '%Y-%m-%d %H:%M:%S'
        date is UTC
        if configured as an empty string, the date will be output as a UTC timestamp

.. warning:: storing the internal metrics to a file is really verbose! Use this feature with much caution and only for developement or tests purpose.

When the Alignak :ref:`inner metrics module <alignak_features/inner_modules>`_ is enabled, some more environment variables may be used to configure the module. The value in these variables takes precedence on the Alignak configuration of the module:

    ``ALIGNAK_HOSTS_STATS_FILE``
        writes the metrics in append mode in the file which full path name is defined in the variable


Alignak events log
------------------

The Alignak arbiter stores the most recent monitoring events (eg. alerts, notifications, ...) to be able to provide them on the *alignak_log* HTTP endpoint. The default events stored count is 100. You can change this value thanks to the ``ALIGNAK_EVENTS_LOG_COUNT`` environment variable.



Log system health
-----------------

Defining the ``ALIGNAK_SYSTEM_MONITORING`` environment variable will make Alignak add some log in the arbiter daemon log to inform about the system CPU, memory and disk consumption.

On each activity loop end, if the report period is happening, the arbiter gets the current cpu, memory and disk information from the OS and dumps them to the information log. The dump is formatted as a Nagios plugin output with performance data.

When this variable is defined, the default report period is set to 5. As such, each 5 loop turn, there is a report in the information log. If this variable contains an integer value, this value will define the report period in seconds.

 ::

   # Define environment variable
   setenv ALIGNAK_SYSTEM_MONITORING 5


   [2017-09-19 15:54:36 CEST] INFO: [alignak.scheduler] Scheduler scheduler-master cpu|'cpu_count'=4 'cpu_1_percent'=42.20% 'cpu_2_percent'=38.40% 'cpu_3_percent'=35.40% 'cpu_4_percent'=48.10% 'cpu_1_user_percent'=37.90% 'cpu_1_nice_percent'=0.00% 'cpu_1_system_percent'=4.20% 'cpu_1_idle_percent'=57.80% 'cpu_1_irq_percent'=0.00% 'cpu_2_user_percent'=31.80% 'cpu_2_nice_percent'=0.00% 'cpu_2_system_percent'=6.10% 'cpu_2_idle_percent'=61.60% 'cpu_2_irq_percent'=0.50% 'cpu_3_user_percent'=31.00% 'cpu_3_nice_percent'=0.00% 'cpu_3_system_percent'=4.20% 'cpu_3_idle_percent'=64.60% 'cpu_3_irq_percent'=0.20% 'cpu_4_user_percent'=38.90% 'cpu_4_nice_percent'=0.00% 'cpu_4_system_percent'=9.20% 'cpu_4_idle_percent'=51.90% 'cpu_4_irq_percent'=0.00%
   [2017-09-19 15:54:36 CEST] INFO: [alignak.scheduler] Scheduler scheduler-master disks|'disk_/_total'=952725065728B 'disk_/_used'=93761236992B 'disk_/_free'=858963828736B 'disk_/_percent_used'=9.80%
   [2017-09-19 15:54:36 CEST] INFO: [alignak.scheduler] Scheduler scheduler-master memory|'swap_total'=2621424B 'swap_used'=33514B 'swap_free'=2587910B 'swap_used_percent'=1.30% 'swap_sin'=2687B 'swap_sout'=12851708B
   [2017-09-19 15:54:41 CEST] INFO: [alignak.scheduler] Scheduler scheduler-master cpu|'cpu_count'=4 'cpu_1_percent'=34.00% 'cpu_2_percent'=37.40% 'cpu_3_percent'=36.10% 'cpu_4_percent'=25.10% 'cpu_1_user_percent'=26.90% 'cpu_1_nice_percent'=0.00% 'cpu_1_system_percent'=7.00% 'cpu_1_idle_percent'=66.00% 'cpu_1_irq_percent'=0.00% 'cpu_2_user_percent'=30.10% 'cpu_2_nice_percent'=0.00% 'cpu_2_system_percent'=7.20% 'cpu_2_idle_percent'=62.60% 'cpu_2_irq_percent'=0.20% 'cpu_3_user_percent'=30.40% 'cpu_3_nice_percent'=0.00% 'cpu_3_system_percent'=5.60% 'cpu_3_idle_percent'=63.90% 'cpu_3_irq_percent'=0.20% 'cpu_4_user_percent'=19.20% 'cpu_4_nice_percent'=0.00% 'cpu_4_system_percent'=5.80% 'cpu_4_idle_percent'=74.90% 'cpu_4_irq_percent'=0.20%
   [2017-09-19 15:54:41 CEST] INFO: [alignak.scheduler] Scheduler scheduler-master disks|'disk_/_total'=952725061632B 'disk_/_used'=93761646592B 'disk_/_free'=858963415040B 'disk_/_percent_used'=9.80%
   [2017-09-19 15:54:41 CEST] INFO: [alignak.scheduler] Scheduler scheduler-master memory|'swap_total'=2621424B 'swap_used'=33514B 'swap_free'=2587910B 'swap_used_percent'=1.30% 'swap_sin'=2687B 'swap_sout'=12851710B
   [2017-09-19 15:54:46 CEST] INFO: [alignak.scheduler] Scheduler scheduler-master cpu|'cpu_count'=4 'cpu_1_percent'=28.70% 'cpu_2_percent'=24.60% 'cpu_3_percent'=36.40% 'cpu_4_percent'=41.00% 'cpu_1_user_percent'=21.20% 'cpu_1_nice_percent'=0.00% 'cpu_1_system_percent'=7.50% 'cpu_1_idle_percent'=71.30% 'cpu_1_irq_percent'=0.00% 'cpu_2_user_percent'=17.70% 'cpu_2_nice_percent'=0.00% 'cpu_2_system_percent'=6.80% 'cpu_2_idle_percent'=75.40% 'cpu_2_irq_percent'=0.20% 'cpu_3_user_percent'=27.90% 'cpu_3_nice_percent'=0.00% 'cpu_3_system_percent'=8.20% 'cpu_3_idle_percent'=63.60% 'cpu_3_irq_percent'=0.30% 'cpu_4_user_percent'=33.60% 'cpu_4_nice_percent'=0.00% 'cpu_4_system_percent'=7.10% 'cpu_4_idle_percent'=59.00% 'cpu_4_irq_percent'=0.30%
   [2017-09-19 15:54:46 CEST] INFO: [alignak.scheduler] Scheduler scheduler-master disks|'disk_/_total'=952725045248B 'disk_/_used'=93762039808B 'disk_/_free'=858963005440B 'disk_/_percent_used'=9.80%
   [2017-09-19 15:54:46 CEST] INFO: [alignak.scheduler] Scheduler scheduler-master memory|'swap_total'=2621424B 'swap_used'=33514B 'swap_free'=2587910B 'swap_used_percent'=1.30% 'swap_sin'=2687B 'swap_sout'=12851716B


.. note :: this feature allows to have some information about the system load with a running Alignak scheduler.

Log daemon health
-----------------

Defining the ``ALIGNAK_DAEMON_MONITORING`` environment variable will make each Alignak daemon add some debug log to inform about its own CPU and memory consumption.

On each activity loop end, if the report period is happening, the daemon gets its current cpu and memory information from the OS and dumps these information formatted as a Nagios plugin output with performance data.

When this environment variable is defined, the default report period is set to 10. As such, each 10 loop turn (eg. 10 seconds), there is a report in the information log. If this variable contains an integer value, this value will define the report period in loop count. As such, defining ``ALIGNAK_DAEMON_MONITORING`` with ``5`` will make a log each 5 loop turn.

Log Scheduling loop
-------------------

Defining the ``ALIGNAK_LOG_LOOP`` environment variable will make Alignak add some log in the scheduler daemons log files to inform about the checks that are scheduled.

As an example::

    # Define environment variable
    export ALIGNAK_LOG_LOOP=1

    # Start Alignak daemons

    # Tail scheduler log files
    [2017-05-27 07:32:49 CEST] INFO: [alignak.scheduler] --- 64
    [2017-05-27 07:32:49 CEST] INFO: [alignak.scheduler] Items (loop): broks: 0, notifications: 0, checks: 0, internal checks: 0, event handlers: 0, external commands: 0
    [2017-05-27 07:32:49 CEST] INFO: [alignak.scheduler] Items (total): broks: 52, notifications: 0, checks: 13, internal checks: 0, event handlers: 0, external commands: 0
    [2017-05-27 07:32:49 CEST] INFO: [alignak.scheduler] Actions 'eventhandler/total': launched: 0, timeout: 0, executed: 0
    [2017-05-27 07:32:49 CEST] INFO: [alignak.scheduler] Results 'eventhandler/total': total: 0,
    [2017-05-27 07:32:49 CEST] INFO: [alignak.scheduler] Actions 'eventhandler/loop': launched: 0, timeout: 0, executed: 0
    [2017-05-27 07:32:49 CEST] INFO: [alignak.scheduler] Results 'eventhandler/loop': total: 0,
    [2017-05-27 07:32:49 CEST] INFO: [alignak.scheduler] Actions 'notification/total': launched: 0, timeout: 0, executed: 0
    [2017-05-27 07:32:49 CEST] INFO: [alignak.scheduler] Results 'notification/total': total: 0,
    [2017-05-27 07:32:49 CEST] INFO: [alignak.scheduler] Actions 'notification/loop': launched: 0, timeout: 0, executed: 0
    [2017-05-27 07:32:49 CEST] INFO: [alignak.scheduler] Results 'notification/loop': total: 0,
    [2017-05-27 07:32:49 CEST] INFO: [alignak.scheduler] Actions 'check/total': launched: 2, timeout: 0, executed: 2
    [2017-05-27 07:32:49 CEST] INFO: [alignak.scheduler] Results 'check/total': total: 4, done: 4,
    [2017-05-27 07:32:49 CEST] INFO: [alignak.scheduler] Actions 'check/loop': launched: 0, timeout: 0, executed: 0
    [2017-05-27 07:32:49 CEST] INFO: [alignak.scheduler] Results 'check/loop': total: 2, done: 2,
    [2017-05-27 07:32:49 CEST] INFO: [alignak.scheduler] Checks (loop): total: 12 (scheduled: 11, launched: 0, in poller: 0, timeout: 0, done: 0, zombies: 0)
    [2017-05-27 07:32:50 CEST] INFO: [alignak.scheduler] Elapsed time, current loop: 0.00, from start: 63.20 (64 loops)
    [2017-05-27 07:32:50 CEST] INFO: [alignak.scheduler] Check average (loop) = 0 checks results, 0.00 checks/s
    [2017-05-27 07:32:50 CEST] INFO: [alignak.scheduler] Check average (total) = 13 checks results, 0.21 checks/s
    [2017-05-27 07:32:50 CEST] INFO: [alignak.scheduler] +++ 64
    [2017-05-27 07:32:50 CEST] INFO: [alignak.scheduler] --- 65
    [2017-05-27 07:32:50 CEST] INFO: [alignak.scheduler] Items (loop): broks: 0, notifications: 0, checks: 0, internal checks: 0, event handlers: 0, external commands: 0
    [2017-05-27 07:32:50 CEST] INFO: [alignak.scheduler] Items (total): broks: 52, notifications: 0, checks: 13, internal checks: 0, event handlers: 0, external commands: 0
    [2017-05-27 07:32:50 CEST] INFO: [alignak.scheduler] Actions 'eventhandler/total': launched: 0, timeout: 0, executed: 0
    [2017-05-27 07:32:50 CEST] INFO: [alignak.scheduler] Results 'eventhandler/total': total: 0,
    [2017-05-27 07:32:50 CEST] INFO: [alignak.scheduler] Actions 'eventhandler/loop': launched: 0, timeout: 0, executed: 0
    [2017-05-27 07:32:50 CEST] INFO: [alignak.scheduler] Results 'eventhandler/loop': total: 0,
    [2017-05-27 07:32:50 CEST] INFO: [alignak.scheduler] Actions 'notification/total': launched: 0, timeout: 0, executed: 0
    [2017-05-27 07:32:50 CEST] INFO: [alignak.scheduler] Results 'notification/total': total: 0,
    [2017-05-27 07:32:50 CEST] INFO: [alignak.scheduler] Actions 'notification/loop': launched: 0, timeout: 0, executed: 0
    [2017-05-27 07:32:50 CEST] INFO: [alignak.scheduler] Results 'notification/loop': total: 0,
    [2017-05-27 07:32:50 CEST] INFO: [alignak.scheduler] Actions 'check/total': launched: 2, timeout: 0, executed: 2
    [2017-05-27 07:32:50 CEST] INFO: [alignak.scheduler] Results 'check/total': total: 4, done: 4,
    [2017-05-27 07:32:50 CEST] INFO: [alignak.scheduler] Actions 'check/loop': launched: 0, timeout: 0, executed: 0
    [2017-05-27 07:32:50 CEST] INFO: [alignak.scheduler] Results 'check/loop': total: 2, done: 2,
    [2017-05-27 07:32:50 CEST] INFO: [alignak.scheduler] Checks (loop): total: 12 (scheduled: 11, launched: 0, in poller: 0, timeout: 0, done: 0, zombies: 0)
    [2017-05-27 07:32:51 CEST] INFO: [alignak.scheduler] Elapsed time, current loop: 0.01, from start: 64.21 (65 loops)
    [2017-05-27 07:32:51 CEST] INFO: [alignak.scheduler] Check average (loop) = 0 checks results, 0.00 checks/s
    [2017-05-27 07:32:51 CEST] INFO: [alignak.scheduler] Check average (total) = 13 checks results, 0.20 checks/s
    [2017-05-27 07:32:51 CEST] INFO: [alignak.scheduler] +++ 65


Log Alignak daemons loop
------------------------

Defining the ``ALIGNAK_LOG_ACTIVITY`` environment variable will make Alignak daemons periodically log an information log as a keep alive. The integer value of this variable defines the period count. Each period count, an information log is raised. Per default, the daemons will make a log more or less every hour (3600 loop turns).

 ::

      ==> /usr/local/var/log/alignak/receiver-master.log <==
      [2018-06-16 17:16:37] INFO: [receiver-master.alignak.daemon] Daemon receiver-master is living: loop #18001 ;)

      ==> /usr/local/var/log/alignak/scheduler-master.log <==
      [2018-06-16 17:16:37] INFO: [scheduler-master.alignak.daemon] Daemon scheduler-master is living: loop #18001 ;)

      ==> /usr/local/var/log/alignak/poller-master.log <==
      [2018-06-16 17:16:37] INFO: [poller-master.alignak.daemon] Daemon poller-master is living: loop #18001 ;)

      ==> /usr/local/var/log/alignak/broker-master.log <==
      [2018-06-16 17:16:38] INFO: [broker-master.alignak.daemon] Daemon broker-master is living: loop #18001 ;)

      ==> /usr/local/var/log/alignak/arbiter-master.log <==
      [2018-06-16 17:16:42] INFO: [arbiter-master.alignak.daemon] Daemon arbiter-master is living: loop #18001 ;)

      ==> /usr/local/var/log/alignak/reactionner-master.log <==
      [2018-06-16 17:26:37] INFO: [reactionner-master.alignak.daemon] Daemon reactionner-master is living: loop #18601 ;)

      ==> /usr/local/var/log/alignak/receiver-master.log <==
      [2018-06-16 17:26:37] INFO: [receiver-master.alignak.daemon] Daemon receiver-master is living: loop #18601 ;)

      ==> /usr/local/var/log/alignak/poller-master.log <==
      [2018-06-16 17:26:38] INFO: [poller-master.alignak.daemon] Daemon poller-master is living: loop #18601 ;)

      ==> /usr/local/var/log/alignak/scheduler-master.log <==
      [2018-06-16 17:26:38] INFO: [scheduler-master.alignak.daemon] Daemon scheduler-master is living: loop #18601 ;)

      ==> /usr/local/var/log/alignak/broker-master.log <==
      [2018-06-16 17:26:38] INFO: [broker-master.alignak.daemon] Daemon broker-master is living: loop #18601 ;)


Log Alignak actions
-------------------

Defining the ``ALIGNAK_LOG_ACTIONS`` environment variable will make Alignak add some information in its daemons log files to inform about the commands that are launched for the checks and the notifications. This is very useful to help setting-up the checks because the launched checks and their results are available as INFO log in the Alignak daemons log files;)

If this variable is set to 'WARNING', the logs will be at the WARNING level, else INFO.

As an example::

    # Define environment variable
    setenv ALIGNAK_LOG_ACTIONS 1
    # Or
    export ALIGNAK_LOG_ACTIONS='WARNING'

    # Start Alignak daemons

    # Tail log files
    ==> /usr/local/var/log/alignak/pollerd.log <==
    [2017-04-26 16:23:57 UTC] INFO: [alignak.action] Launch command: /usr/local/libexec/nagios/check_nrpe -H 93.93.47.81 -t 10 -u -n -c check_zombie_procs
    [2017-04-26 16:23:57 UTC] INFO: [alignak.action] Check for /usr/local/libexec/nagios/check_nrpe -H 93.93.47.81 -t 10 -u -n -c check_zombie_procs exited with return code 0
    [2017-04-26 16:23:57 UTC] INFO: [alignak.action] Check result for /usr/local/libexec/nagios/check_nrpe -H 93.93.47.81 -t 10 -u -n -c check_zombie_procs: 0, PROCS OK: 0 processes with STATE = Z
    [2017-04-26 16:23:57 UTC] INFO: [alignak.action] Performance data for /usr/local/libexec/nagios/check_nrpe -H 93.93.47.81 -t 10 -u -n -c check_zombie_procs: procs=0;5;10;0;


Log Alignak checks results
--------------------------

Defining the ``ALIGNAK_LOG_CHECKS`` environment variable will make Alignak add some information in its daemons log files to log the check results. This is also very useful to help understanding why some check results are not ok.

According to the check plugin exit code, a log will be emitted with a certain level: 'info', 'warning', 'error', or 'critical'. As an example, this will add a warning log for a plugin with an exit code of 1, an error log for 2, and a critical log for any value greater than or equal to 3.

Log Alignak alerts and notifications
------------------------------------

Defining the ``ALIGNAK_LOG_ALERTS`` ``ALIGNAK_LOG_NOTIFICATIONS`` environment variables will make Alignak add some information in its daemons log files to inform about the alerts and notifications that are raised for the monitored hosts and services.

If these variables are set to 'WARNING', the logs will be at the WARNING level, else INFO.

Disable internal commands
-------------------------

Defining the ``ALIGNAK_MANAGE_INTERNAL`` environment variable to a value different of ``1`` will make Alignak ignore the internal commands execution. This is to be used with much caution because it will disable the business rules computation and disable the business correlator. But it may be interesting if you do not use this feature because it will reduce the scheduler load and improve performance...