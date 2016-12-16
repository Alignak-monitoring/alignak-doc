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


.. note :: By default, the arbiter starting script uses */usr/local/etc/alignak/alignak.cfg* as a monitoring configuration file. You can use another configuration file if you set the ``ALIGNAKCFG`` shell environment variable.


.. note :: It is also possible to define a second monitoring configuration file that will be used by the Alignak arbiter. If your configuration is defined in two separated files, you can define the second configuration file if you set the ``ALIGNAKSPECIFICCFG`` shell environment variable.


Alignak processes list
======================

The daemons involved in Alignak are strating several processes in the system. All the processes started have a process title set by Alignak to help the user knowing which is which. Several processes types are present in the system processes list:

    * the main daemon process
        There will alwys be one process for each Alignak daemon type. The process title is the daemon type (eg. *alignak-arbiter*, *alignak-scheduler*,...)

    * the main daemon forked process.
        Each Alignak daemon forks a new process instance for each daemon instance existing in the configuration. If you defined several schedulers you will get a process for each scheduler instance. Each daemon instance process has a title built with the instance name (eg. *alignak-scheduler scheduler-master*)

    * the external modules processes
        The daemons that have some external modules attached, like the brokers or receivers, launch new processes for their modules. Those processes titles are made of the daemon instance name and the module alias (eg. *alignak-receiver-master module: nsca*)

    * the satelitte workers processes
        The satellites daemons that need some worker processes (pollers and reactionners) launch several worker processes to execute their actions (checks or notifications). Those worker processes have a title made of the daemon instance name and the worker label (eg. *alignak-poller-master worker*)


 As an exemple, here is the processes list of an Alignak "simple" configuration with no spare daemons and no distributedd configuration::

    alignak   5850  0.7  1.0 867048 43148 ?        Sl   10:54   0:00 alignak-scheduler scheduler-master
    alignak   5851  0.0  0.9 208644 37076 ?        S    10:54   0:00 alignak-scheduler
    alignak   5907  0.4  1.0 865080 42516 ?        Sl   10:54   0:00 alignak-poller poller-master
    alignak   5908  0.0  0.9 495000 37964 ?        Sl   10:54   0:00 alignak-poller
    alignak   5968  0.4  1.0 864756 42456 ?        Sl   10:54   0:00 alignak-reactionner reactionner-master
    alignak   5973  0.0  0.9 421272 38044 ?        Sl   10:54   0:00 alignak-reactionner
    alignak   6078  1.2  1.1 867732 45072 ?        Sl   10:55   0:00 alignak-broker broker-master
    alignak   6079  0.1  0.9 495276 40048 ?        Sl   10:55   0:00 alignak-broker
    alignak   6153  0.4  1.0 864576 42036 ?        Sl   10:55   0:00 alignak-receiver receiver-master
    alignak   6154  0.0  0.9 347940 37736 ?        Sl   10:55   0:00 alignak-receiver
    alignak   6216  1.6  1.1 867588 44528 ?        Sl   10:55   0:00 alignak-arbiter arbiter-master
    alignak   6217  0.0  0.9 211000 39376 ?        S    10:55   0:00 alignak-arbiter
    alignak   6230  0.0  0.9 864184 40452 ?        S    10:55   0:00 alignak-poller-master worker
    alignak   6240  0.0  1.0 864320 40960 ?        S    10:55   0:00 alignak-receiver-master module: nsca
    alignak   6250  0.2  1.0 866748 43228 ?        S    10:55   0:00 alignak-broker-master module: backend_broker
    alignak   6260  0.2  1.0 866748 43072 ?        S    10:55   0:00 alignak-broker-master module: logs
    alignak   6271  0.0  1.0 864196 40592 ?        S    10:55   0:00 alignak-poller-master worker
    alignak   6279  0.0  1.0 864188 40544 ?        S    10:55   0:00 alignak-reactionner-master worker


Log files
=========

When running, the Alignak daemons are logging their activity in log files that can be found in the
*/usr/local/var/log/* directory. Each daemon has its own log file. Log files are kept on the system
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

