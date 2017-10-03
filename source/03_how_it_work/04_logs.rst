.. _howitworks/logs:

=========
Log files
=========

When running, the Alignak daemons are logging their activity in log files that can be found (per default) in the
*/usr/local/var/log/* (or */var/log*) directory. Each daemon has its own log file. Log files are kept on the system
for a default period of 7 rotating days.

Each daemon log file configuration is found in the daemon configuration file (/usr/local/etc/alignak/daemons/*.ini*).

In case of problem, make sure that there is no ERROR and/or WARNING logs in the log files.

The log files are the very first information source about Alignak activity. You will find:

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

