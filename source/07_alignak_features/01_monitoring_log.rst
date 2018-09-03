.. _alignak_features/monitoring_log:

==============
Monitoring log
==============


The monitoring log is what Alignak is made for!


This log contains all the monitoring events that Alignak is able to raise:

   * active host/service checks
   * passive host/service checks
   * alerts
   * notifications
   * acknowledgements
   * downtimes
   * comments

As soon as one of this event is raised by Alignak, it is stored locally by the originating daemon. The arbiter periodically collects all the events near all its satellites and raises the log with the collected data: creation date, log level and message.


As an example::

    [2018-04-22 08:52:49] INFO: TIMEPERIOD TRANSITION: 24x7;-1;1
    [2018-04-22 08:52:49] INFO: TIMEPERIOD TRANSITION: ipm_fdj_hours;-1;1
    [2018-04-22 08:52:50] INFO: RETENTION SAVE: scheduler-master scheduler
    [2018-04-22 08:59:59] WARNING: SERVICE ALERT: es3;Memory;WARNING;HARD;3;Memory WARNING - 89.5% (15373434880 kB) used
    [2018-04-22 08:59:59] WARNING: SERVICE NOTIFICATION: ipm-fdj;es3;Memory;WARNING;notify-service-by-email-html;Memory WARNING - 89.5% (15373434880 kB) used
    [2018-04-22 08:59:59] WARNING: SERVICE NOTIFICATION: Bee-notifier;es3;Memory;WARNING;notify-service-by-email-html;Memory WARNING - 89.5% (15373434880 kB) used
    [2018-04-22 08:59:59] WARNING: SERVICE NOTIFICATION: Bee-notifier;es3;Memory;WARNING;notify-service-to-Bee;Memory WARNING - 89.5% (15373434880 kB) used
    [2018-04-22 09:00:41] WARNING: CONFIGURATION RELOAD
    [2018-04-22 09:01:03] INFO: TIMEPERIOD TRANSITION: ipm_fdj_hours;-1;1
    [2018-04-22 09:01:03] INFO: TIMEPERIOD TRANSITION: 24x7;-1;1
    [2018-04-22 09:01:05] INFO: RETENTION SAVE: scheduler-master scheduler
    [2018-04-22 09:01:10] INFO: RETENTION LOAD: scheduler-master scheduler
    ...
    ...
    ...
    [2018-04-22 16:38:51] INFO: EXTERNAL COMMAND: [1524400607] ACKNOWLEDGE_SVC_PROBLEM;rsync;Up-to-date;2;1;1;admin;Acknowledge requested from WebUI
    [2018-04-22 16:38:51] INFO: SERVICE ACKNOWLEDGE ALERT: rsync;Up-to-date;STARTED; Service problem has been acknowledged
    [2018-04-22 16:38:51] INFO: EXTERNAL COMMAND: [1524400614] ACKNOWLEDGE_SVC_PROBLEM;mysql_slave;Up-to-date;2;1;1;admin;Acknowledge requested from WebUI
    [2018-04-22 16:38:51] INFO: SERVICE ACKNOWLEDGE ALERT: mysql_slave;Up-to-date;STARTED; Service problem has been acknowledged
    [2018-04-22 16:38:51] INFO: EXTERNAL COMMAND: [1524400624] ACKNOWLEDGE_SVC_PROBLEM;es1;Up-to-date;2;1;1;admin;Acknowledge requested from WebUI
    [2018-04-22 16:38:51] INFO: SERVICE ACKNOWLEDGE ALERT: es1;Up-to-date;STARTED; Service problem has been acknowledged
    [2018-04-22 16:38:52] INFO: SERVICE NOTIFICATION: Bee-notifier;mysql_slave;Up-to-date;ACKNOWLEDGEMENT (CRITICAL);notify-service-by-email-html;CHECKPKGAUDIT CRITICAL - found 2 vulnerable(s) pkg(s) in : mysql_slave
    [2018-04-22 16:38:52] INFO: SERVICE NOTIFICATION: Bee-notifier;mysql_slave;Up-to-date;ACKNOWLEDGEMENT (CRITICAL);notify-service-to-Bee;CHECKPKGAUDIT CRITICAL - found 2 vulnerable(s) pkg(s) in : mysql_slave
    [2018-04-22 16:38:52] INFO: SERVICE NOTIFICATION: ipm-fdj;mysql_slave;Up-to-date;ACKNOWLEDGEMENT (CRITICAL);notify-service-by-email-html;CHECKPKGAUDIT CRITICAL - found 2 vulnerable(s) pkg(s) in : mysql_slave
    [2018-04-22 16:38:52] INFO: SERVICE NOTIFICATION: Bee-notifier;rsync;Up-to-date;ACKNOWLEDGEMENT (CRITICAL);notify-service-by-email-html;CHECKPKGAUDIT CRITICAL - found 2 vulnerable(s) pkg(s) in : rsync
    [2018-04-22 16:38:52] INFO: SERVICE NOTIFICATION: Bee-notifier;rsync;Up-to-date;ACKNOWLEDGEMENT (CRITICAL);notify-service-to-Bee;CHECKPKGAUDIT CRITICAL - found 2 vulnerable(s) pkg(s) in : rsync
    [2018-04-22 16:38:52] INFO: SERVICE NOTIFICATION: ipm-fdj;rsync;Up-to-date;ACKNOWLEDGEMENT (CRITICAL);notify-service-by-email-html;CHECKPKGAUDIT CRITICAL - found 2 vulnerable(s) pkg(s) in : rsync
    [2018-04-22 16:38:52] INFO: SERVICE NOTIFICATION: Bee-notifier;es1;Up-to-date;ACKNOWLEDGEMENT (CRITICAL);notify-service-by-email-html;CHECKPKGAUDIT CRITICAL - found 2 vulnerable(s) pkg(s) in : es1
    [2018-04-22 16:38:52] INFO: SERVICE NOTIFICATION: Bee-notifier;es1;Up-to-date;ACKNOWLEDGEMENT (CRITICAL);notify-service-to-Bee;CHECKPKGAUDIT CRITICAL - found 2 vulnerable(s) pkg(s) in : es1
    [2018-04-22 16:38:52] INFO: SERVICE NOTIFICATION: ipm-fdj;es1;Up-to-date;ACKNOWLEDGEMENT (CRITICAL);notify-service-by-email-html;CHECKPKGAUDIT CRITICAL - found 2 vulnerable(s) pkg(s) in : es1


.. note:: The monitoring log file(s) can be easily parsed thanks to parsing tools like Logstash... see the project repo in the *contrib* directory for more information about this.

Events dictionary
-----------------

Several types of events may be present in the log:

   - informational events
   - warning and error events


Warning and error events are raised when received commands are not correctly parsed::

   ERROR: Malformed command: command
   ERROR: Command 'command' is not recognized, sorry
   ERROR: Arguments are not correct for the command: command
   WARNING: command: this command is not implemented!


Some information events are raised::

   INFO: RESTART: output
   INFO: RELOAD: output
   INFO: CONFIGURATION RELOAD: duration
   INFO: RETENTION LOAD: scheduler
   INFO: RETENTION SAVE: scheduler
   INFO: TIMEPERIOD TRANSITION: tp;from;to

The received external commands are logged (if ``log_external_commands`` is set)::

   INFO: EXTERNAL COMMAND: [timestamp] command

Initial states are logged on restart or configuration reload (if ``log_initial_state`` is set)::

   INFO: CURRENT HOST STATE: host;state;state_type;current_attempt;output
   INFO: CURRENT SERVICE STATE: host;service;state;state_type;current_attempt;output

Active checks (if ``log_active_checks`` is set)::

   INFO: ACTIVE HOST CHECK: host;status;output;long_output;perf_data
   INFO: ACTIVE SERVICE CHECK: host;service;status;output;long_output;perf_data

Passive checks (if ``log_passive_checks`` is set)::

   INFO: PASSIVE HOST CHECK: host;status;output;long_output;perf_data
   INFO: PASSIVE SERVICE CHECK: host;service;status;output;long_output;perf_data

Comments::

   INFO: HOST COMMENT: host;author;comment
   INFO: SERVICE COMMENT: host;service;author;comment
   WARNING: DEL_HOST_COMMENT: comment id: xxxxxxx does not exist and cannot be deleted.
   WARNING: DEL_SVC_COMMENT: comment id: xxxxxxx does not exist and cannot be deleted.

Alerts (always logged)::

   level: HOST COMMENT: host;state;state_type;current_attempt;output
   level: SERVICE ALERT: host;service;state;state_type;current_attempt;output
   level: SERVICE FLAPPING ALERT: host;service;STARTED; Service appears to have started flapping (ratio% change >= threshold% threshold)
   level: SERVICE FLAPPING ALERT: host;service;STOPPED; Service appears to have stopped flapping (ratio% change >= threshold% threshold)

Acknowledges (always logged)::

   info: HOST ACKNOWLEDGE ALERT: host;STARTED; Host problem has been acknowledged
   info: HOST ACKNOWLEDGE ALERT: host;EXPIRED; Host problem acknowledge expired
   info: SERVICE ACKNOWLEDGE ALERT: host;service;STARTED; Service problem has been acknowledged
   info: SERVICE ACKNOWLEDGE ALERT: host;service;EXPIRED; Service problem acknowledge expired

Event handlers (if ``log_event_handlers`` is set)::

   level: HOST EVENT HANDLER: host;state;state_type;current_attempt;command
   level: SERVICE EVENT HANDLER: host;service;state;state_type;current_attempt;command

Snapshots (if ``log_snapshots`` is set)::

   level: HOST SNAPSHOT: host;state;state_type;current_attempt;command
   level: SERVICE SNAPSHOT: host;service;state;state_type;current_attempt;command

Notifications (if ``log_notifications`` is set)::

   level: HOST NOTIFICATION: host;state;command;output
   level: SERVICE NOTIFICATION: host;service;state;command;output

Downtimes (always logged)::

   INFO: HOST DOWNTIME ALERT: host;STARTED; Host has entered a period of scheduled downtime
   INFO: HOST DOWNTIME ALERT: host;STOPPED; Host has exited from a period of scheduled downtime
   INFO: HOST DOWNTIME ALERT: host;CANCELLED; Scheduled downtime for host has been cancelled.

   INFO: SERVICE DOWNTIME ALERT: host;service;STARTED; Service has entered a period of scheduled downtime
   INFO: SERVICE DOWNTIME ALERT: host;service;STOPPED; Service has exited from a period of scheduled downtime
   INFO: SERVICE DOWNTIME ALERT: host;service;CANCELLED; Scheduled downtime for service has been cancelled.

   INFO: CONTACT DOWNTIME ALERT: contact;STARTED; Contact has entered a period of scheduled downtime
   INFO: CONTACT DOWNTIME ALERT: contact;STOPPED; Contact has exited from a period of scheduled downtime
   INFO: CONTACT DOWNTIME ALERT: contact;CANCELLED; Scheduled downtime for contact has been cancelled.

   WARNING: DEL_CONTACT_DOWNTIME: downtime id: xxxxxxx does not exist and cannot be deleted.
   WARNING: DEL_HOST_DOWNTIME: downtime id: xxxxxxx does not exist and cannot be deleted.
   WARNING: DEL_SVC_DOWNTIME: downtime id: xxxxxxx does not exist and cannot be deleted.
