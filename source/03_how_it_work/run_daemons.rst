.. _howitworks/run_alignak:

===============
Running Alignak
===============

With packaging
==============

If you install with packaging (DEB, RPM...), see the package documentation for the best solution to
configure the daemons and to start/stop Alignak.


With sources and pip
====================

Individual start / stop
-----------------------
All the Alignak daemons have a script that can be launched with command line parameters.

All the command line parameters are optional because default values are used by the aemon when it
starts but it is recommended to use, at least, a daemon configuration file with the `-c` option.

Other command line parameters are available, but they are use rarely ;)

For all the daemons (broker, poller, receiver, reactionner, scheduler)::

    $ alignak-broker -h

    usage: alignak-broker [-h] [-v] [-c CONFIG_FILE] [-d] [-r]
                          [--debugfile DEBUG_FILE]

    optional arguments:
      -h, --help            show this help message and exit
      -v, --version         show program's version number and exit
      -c CONFIG_FILE, --config CONFIG_FILE
                            Daemon configuration file
      -d, --daemon          Run as a daemon
      -r, --replace         Replace previous running daemon
      --debugfile DEBUG_FILE
                            File to dump debug logs


The arbiter is slightly different because it needs to receive the monitoring configuration that is to be loaded::

    $ alignak-arbiter -h

    usage: alignak-arbiter [-h] [-v] -a MONITORING_FILES [-V] [-n CONFIG_NAME]
                           [-c CONFIG_FILE] [-d] [-r] [--debugfile DEBUG_FILE]

    optional arguments:
      -h, --help            show this help message and exit
      -v, --version         show program's version number and exit
      -a MONITORING_FILES, --arbiter MONITORING_FILES
                            Monitored configuration file(s),multiple -a can be
                            used, and they will be concatenated.
      -V, --verify-config   Verify config file and exit
      -n CONFIG_NAME, --config-name CONFIG_NAME
                            Use name of arbiter defined in the configuration files
                            (default arbiter-master)
      -c CONFIG_FILE, --config CONFIG_FILE
                            Daemon configuration file
      -d, --daemon          Run as a daemon
      -r, --replace         Replace previous running daemon
      --debugfile DEBUG_FILE
                            File to dump debug logs


With the default installed configuration::

    $ alignak-broker -c /usr/local/etc/alignak/daemons/brokerd.ini
    $ alignak-scheduler -c /usr/local/etc/alignak/daemons/schedulerd.ini
    $ alignak-poller -c /usr/local/etc/alignak/daemons/pollerd.ini
    $ alignak-reactionner -c /usr/local/etc/alignak/daemons/reactionnerd.ini
    $ alignak-receiver -c /usr/local/etc/alignak/daemons/receiverd.ini
    $ alignak-arbiter -c /usr/local/etc/alignak/daemons/arbiterd.ini -a /usr/local/etc/alignak/alignak.cfg


Installed scripts
-----------------
Some scripts to start/stop are provided when installing Alignak with its default configuration.
Those scripts are located in the */usr/local/etc/init.d* or *rc.d* directory. They allow starting
one instance of each Alignak daemon with:

    - its own configuration file as installed in the default configuration (*-c /usr/local/etc/alignak/daemons/*.ini*)
    - in daemon mode (*-d*)
    - for the Arbiter, adding the default monitored configuration (*-a /usr/local/etc/alignak/alignak.cfg*)

You can then start all daemons (as alignak user) like this::

    $ /usr/local/etc/init.d/alignak start (or /usr/local/etc/rc.d/alignak start)

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

Then stop all daemons::

    $ /usr/local/etc/init.d/alignak stop (or /usr/local/etc/rc.d/alignak stop)


Restart to load a new configuration::

    $ /usr/local/etc/init.d/alignak restart (or /usr/local/etc/rc.d/alignak restart)



Log files
=========

When running, the Alignak daemons are logging their activity in log files that can be found in the
*/usr/local/var/log/* directory. Each daemon has its own log file. Log files are kept on the system
for a default period of 7 rotating days.

Each daemon log file configuration is found in the daemon configuration file (/usr/local/etc/alignak/daemons/*.ini*).

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

