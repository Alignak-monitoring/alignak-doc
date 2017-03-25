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

All the command line parameters are optional because default values are used by the daemon when it starts but it is recommended to use, at least, a daemon configuration file with the `-c` option.

Without a configuration file, the daemon will create its pid file in the current working directory.

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


Start / stop example scripts
----------------------------

Some scripts to start/stop Alignak daemons are provided with the Alignak source archive.

Those scripts are located in the *dev* directory of the alignak repository. They are delivered as examples to build your own start/stop scripts if you do not use the one provided with an OS installation package.

They are using the *etc/alignak.ini* file to get the Alignak daemons parameters and start the daemons.

The main `_launch_daemon.sh` script is called to launch a daemon which you provide the name. The `launch_arbiter.sh`, `launch_scheduler.sh`, ... scripts are used to start one instance of the each type of daemon. You can start all the daemons at once with the `launch_all.sh` script.

Stopping the launched daemons is possible thanks to the `stop_*.sh` scripts.

You can then start all daemons (as alignak user) like this::

    $launch_all.sh

Then stop all daemons::

    $stop_all.sh


Restart to load a new configuration::

    $restart_all.sh


As default:

    - each daemon starts in daemonize mode to be detached from the current shell;
    - the working directory of each daemon is the current working directory. As such, each daemon will create its pid file in the current directory

Specifying the `-d` option will start the daemons in debug mode. Then you will get a log file for each daemon in the current working directory.

Specifying the `-c` option will start the daemons with its own configuration file as defined in *alignak.ini*. In this mode, the daemon will change its working directory according to the values defined in its configuration file. Take care about the defined parameters ;)


.. note :: By default, the arbiter starting script uses the monitoring configuration file defined in the *alignak.ini* file. You can use another configuration file if you set the ``ALIGNAKCFG`` shell environment variable.


.. note :: It is also possible to define a second monitoring configuration file that will be used by the Alignak arbiter. If your configuration is defined in two separated files, you can define the second configuration file if you set the ``ALIGNAKSPECIFICCFG`` shell environment variable.


The `_launch_daemon.sh` script has several command line parameters that may be interesting for more specific usage. When calling one of the `launch*.sh` script you can also use those parameters because they will be forwarded to the `launch_daemon.sh` script.

::

    Usage: ./_launch_daemon.sh [-h|--help] [-v|--version] [-d|--debug] [-a|--arbiter] [-n|--no-daemon] [-V|--verify] daemon_name

        -h (--help)        display this message
        -v (--version)     display alignak version
        -d (--debug)       start requested daemon in debug mode
        -c (--config)      start requested daemon without its configuration file
                           Default is to start with the daemon configuration file
                           This option allow to use the default daemon parameters and the pid and
                           log files are stored in the current working directory
        -r (--replace)     do not replace an existing daemon (if valid pid file exists)
        -n (--no-daemon)   start requested daemon in console mode (do not daemonize)
        -a (--arbiter)     start requested daemon in arbiter mode
                           This option adds the monitoring configuration file(s) on the command line
                           This option will raise an error if the the daemon is not an arbiter.
        -V (--verify)      start requested daemon in verify mode (only for the arbiter)
                           This option will raise an error if the the daemon is not an arbiter.



Alignak.ini configuration file
------------------------------

.. note: This part will be moved to the configuration part of this documentation but, as of now, is stays here for a better understanding of the previously described scripts.

The *etc/alignak.ini* configuration aims to define the main information about how Alignak is installed on the current system.

This file will be located by an OS installation package in the Alignak *etc* directory (eg. */etc/alignak/alignak.ini* or */usr/local/etc/alignak/alignak.ini*). This to allow a third party application or alignak extension to locate it easily. Once parsed this file will contain the necessary information about:

    - the alignak installation directories
    - the alignak daemons and their configuration
    - the alignak monitoring configuration file

This file is structured as an Ini file:

::

    #
    # Copyright (C) 2015-2016: Alignak team, see AUTHORS.txt file for contributors
    #
    # This file is part of Alignak.
    #
    # Alignak is free software: you can redistribute it and/or modify
    # it under the terms of the GNU Affero General Public License as published by
    # the Free Software Foundation, either version 3 of the License, or
    # (at your option) any later version.
    #
    # Alignak is distributed in the hope that it will be useful,
    # but WITHOUT ANY WARRANTY; without even the implied warranty of
    # MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    # GNU Affero General Public License for more details.
    #
    # You should have received a copy of the GNU Affero General Public License
    # along with Alignak.  If not, see <http://www.gnu.org/licenses/>.
    #

    #
    # This configuration file is the main Alignak configuration entry point. Each Alignak installer
    # will adapt the content of this file according to the installation process. This will allow
    # any Alignak extension or third party application to find where the Alignak components and
    # files are located on the system.
    #
    # ---
    # This version of the file contains variable that are suitable to run a single node Alignak
    # with all its daemon using the default configuration existing in the repository.
    #

    # Main alignak variables:
    # - BIN is where the launch scripts are located
    #   (Debian sets to /usr/bin)
    # - ETC is where we store the configuration files
    #   (Debian sets to /etc/alignak)
    # - VAR is where the libraries and plugins files are installed
    #   (Debian sets to /var/lib/alignak)
    # - RUN is the daemons working directory and where pid files are stored
    #   (Debian sets to /var/run/alignak)
    # - LOG is where we put log files
    #   (Debian sets to /var/log/alignak)
    #
    [DEFAULT]
    BIN=../alignak/bin
    ETC=../etc
    VAR=/tmp
    RUN=/tmp
    LOG=/tmp


    # We define the name of the 2 main Alignak configuration files.
    # There may be 2 configuration files because tools like Centreon generate those...
    [alignak-configuration]
    # Alignak main configuration file
    CFG=%(ETC)s/alignak.cfg
    # Alignak secondary configuration file (none as a default)
    SPECIFICCFG=


    # For each Alignak daemon, this file contains a section with the daemon name. The section
    # identifier is the corresponding daemon name. This daemon name is built with the daemon
    # type (eg. arbiter, poller,...) and the daemon name separated with a dash.
    # This rule ensure that alignak will be able to find all the daemons configuration in this
    # whatever the number of daemons existing in the configuration
    #
    # Each section defines:
    # - the location of the daemon configuration file
    # - the daemon launching script
    # - the location of the daemon pid file
    # - the location of the daemon debug log file (if any is to be used)

    [arbiter-master]
    ### ARBITER PART ###
    PROCESS=alignak-arbiter
    DAEMON=%(BIN)s/alignak_arbiter.py
    CFG=%(ETC)s/daemons/arbiterd.ini
    DEBUGFILE=%(LOG)s/arbiter-debug.log


    [scheduler-master]
    ### SCHEDULER PART ###
    PROCESS=alignak-scheduler
    DAEMON=%(BIN)s/alignak_scheduler.py
    CFG=%(ETC)s/daemons/schedulerd.ini
    DEBUGFILE=%(LOG)s/scheduler-debug.log

    [poller-master]
    ### POLLER PART ###
    PROCESS=alignak-poller
    DAEMON=%(BIN)s/alignak_poller.py
    CFG=%(ETC)s/daemons/pollerd.ini
    DEBUGFILE=%(LOG)s/poller-debug.log

    [reactionner-master]
    ### REACTIONNER PART ###
    PROCESS=alignak-reactionner
    DAEMON=%(BIN)s/alignak_reactionner.py
    CFG=%(ETC)s/daemons/reactionnerd.ini
    DEBUGFILE=%(LOG)s/reactionner-debug.log

    [broker-master]
    ### BROKER PART ###
    PROCESS=alignak-broker
    DAEMON=%(BIN)s/alignak_broker.py
    CFG=%(ETC)s/daemons/brokerd.ini
    DEBUGFILE=%(LOG)s/broker-debug.log

    [receiver-master]
    ### RECEIVER PART ###
    PROCESS=alignak-receiver
    DAEMON=%(BIN)s/alignak_receiver.py
    CFG=%(ETC)s/daemons/receiverd.ini
    DEBUGFILE=%(LOG)s/receiver-debug.log




Alignak processes list
======================

The daemons involved in Alignak are starting several processes in the system. All the processes started have a process title set by Alignak to help the user knowing which is which. Several processes types are present in the system processes list:

    * the main daemon process
        There will always be one process for each Alignak daemon type. The process title is the daemon type (eg. *alignak-arbiter*, *alignak-scheduler*,...)

    * the main daemon forked process.
        Each Alignak daemon forks a new process instance for each daemon instance existing in the configuration. If you defined several schedulers you will get a process for each scheduler instance. Each daemon instance process has a title built with the instance name (eg. *alignak-scheduler scheduler-master*)

    * the external modules processes
        The daemons that have some external modules attached, like the brokers or receivers, launch new processes for their modules. Those processes titles are made of the daemon instance name and the module alias (eg. *alignak-receiver-master module: nsca*)

    * the satellite workers processes
        The satellites daemons that need some worker processes (pollers and reactionners) launch several worker processes to execute their actions (checks or notifications). Those worker processes have a title made of the daemon instance name and the worker label (eg. *alignak-poller-master worker*)


 As an example, here is the processes list of an Alignak "simple" configuration with no spare daemons and no distributed configuration::

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

