.. _howitworks/run_alignak:

===============
Running Alignak
===============

With packaging
==============

If you install with packaging (DEB, RPM...), see the package documentation for the best soltuion to start/stop Alignak.


With sources and pip
====================

You can start all daemons (as alignak user) like this::

    /usr/local/etc/init.d/alignak start

    Starting scheduler:
       ...done.
    Starting poller:
       ...done.
    Starting reactionner:
       ...done.
    Starting broker:
       ...done.
    Starting receiver:
       ...done.
    Starting arbiter:
       ...done.

You can stop all daemons (as alignak user) like this::

    /usr/local/etc/init.d/alignak stop

    Stopping scheduler
       ...done.
    Stopping poller
       ...done.
    Stopping reactionner
       ...done.
    Stopping broker
       ...done.
    Stopping receiver
       ...done.
    Stopping arbiter
       ...done.


Restart to load a new configuration::

    Restarting scheduler
       ...done.
    Restarting poller
       ...done.
    Restarting reactionner
       ...done.
    Restarting broker
       ...done.
    Restarting receiver
       ...done.
    Restarting arbiter
    Doing config check
       ...done.
       ...done.

You can also start each daemon individually.

For Broker::

    alignak-broker -c /usr/local/etc/alignak/daemons/brokerd.ini

For Scheduler::

    alignak-scheduler -c /usr/local/etc/alignak/daemons/schedulerd.ini

For Poller::

    alignak-poller -c /usr/local/etc/alignak/daemons/pollerd.ini

for Reactionner::

    alignak-reactionner -c /usr/local/etc/alignak/daemons/reactionnerd.ini

For Receiver::

    alignak-receiver -c /usr/local/etc/alignak/daemons/receiverd.ini


For Arbiter (be carefull, this daemon does not start like other)::

    alignak-arbiter -c /usr/local/etc/alignak/alignak.cfg



Log files
=========

When running, the Alignak daemons are logging their activity in log files that can be found in the
*/usr/local/var/log/* directory. Each daemon has its own log file. Log files are kept on the system
for a period of 6 rotating days.

In case of problem, make sure that there is no ERROR and/or WARNING logs in the log files.

The log files are the number one information source about Alignak activity. You will find:

    * HOST ALERT information
    * SERVICE ALERT information
    * ...

to keep you informed about your system state.

As an example, the *schedulerd.log* file some few minutes after start::

    [1474548490] INFO: [Alignak] Loading configuration.
    [1474548490] INFO: [Alignak] New configuration loaded
    [1474548490] INFO: [Alignak] [scheduler-master] First scheduling launched
    [1474548490] INFO: [Alignak] [scheduler-master] First scheduling done
    [1474548490] INFO: [Alignak] A new broker just connected : broker-master
    [1474548490] INFO: [Alignak] [scheduler-master] Created 38 initial Broks for broker broker-master
    [1474548530] HOST ALERT: host_snmp;DOWN;SOFT;1;Alarm timeout
    [1474548581] SERVICE ALERT: host_snmp;Disks;CRITICAL;SOFT;1;CRITICAL : (>95%) Cached memory: 100%used(189MB/189MB) Physical memory: 95%used(1892MB/2000MB) Shared memory: 100%used(23MB/23MB)
    [1474548602] HOST ALERT: host_snmp;DOWN;SOFT;1;Alarm timeout
    [1474548614] SERVICE ALERT: host_snmp;Memory;WARNING;SOFT;1;Ram : 85%, Swap : 54% : > 80, 80 ; WARNING
    [1474548637] HOST ALERT: host_snmp;DOWN;SOFT;1;Alarm timeout
    [1474548662] SERVICE ALERT: host_snmp;NetworkUsage;UNKNOWN;SOFT;1;ERROR : Unknown interface eth\d+
    [1474548683] HOST ALERT: host_snmp;DOWN;SOFT;1;Alarm timeout
    [1474548700] SERVICE ALERT: host_snmp;Disks;CRITICAL;SOFT;2;CRITICAL : (>95%) Cached memory: 100%used(193MB/193MB) Physical memory: 96%used(1921MB/2000MB) Shared memory: 100%used(23MB/23MB)
    [1474548722] HOST ALERT: host_snmp;DOWN;SOFT;1;Alarm timeout
    [1474548734] SERVICE ALERT: host_snmp;Memory;WARNING;SOFT;2;Ram : 86%, Swap : 54% : > 80, 80 ; WARNING
    [1474548757] HOST ALERT: host_snmp;DOWN;SOFT;1;Alarm timeout
    [1474548783] SERVICE ALERT: host_snmp;NetworkUsage;UNKNOWN;SOFT;2;ERROR : Unknown interface eth\d+
    [1474548805] HOST ALERT: host_snmp;DOWN;SOFT;1;Alarm timeout
    [1474548819] SERVICE ALERT: host_snmp;Disks;CRITICAL;HARD;3;CRITICAL : (>95%) Cached memory: 100%used(193MB/193MB) Physical memory: 96%used(1930MB/2000MB) Shared memory: 100%used(23MB/23MB)
    [1474548829] HOST ALERT: host_snmp;DOWN;HARD;2;Alarm timeout
    [1474548829] HOST NOTIFICATION: admin;host_snmp;DOWN;notify-host-by-email;Alarm timeout
    [1474548854] SERVICE ALERT: host_snmp;Memory;WARNING;HARD;3;Ram : 86%, Swap : 54% : > 80, 80 ; WARNING
    [1474548902] SERVICE ALERT: host_snmp;NetworkUsage;UNKNOWN;HARD;3;ERROR : Unknown interface eth\d+

