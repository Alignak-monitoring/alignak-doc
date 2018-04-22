.. _alignak_features/monitoring_log:

==============
Monitoring log
==============


Introduction
------------

The monitoring log is what Alignak is made for!


This log contains all the monitoring events that Alignak is able to raise:

    * active host/service checks
    * passive host/service checks
    * alerts
    * notifications
    * acknowledgements
    * downtimes

As soon as one of this event is raised by Alignak, it is sent to the monitoring log. Thanks to the Alignak configuration (see the :ref:`logger configuration <configuration/logger>`), the raised events may be logged to a file, sent by email, pushed to a Web service,...

As an example:
::

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


The monitoring log file(s) can be easily parsed thanks to parsing tools like Logstash... see the project repo in the *contrib* directory for more information about this.