.. _howitworks/run_alignak:

===============
Running Alignak
===============

If you installed Alignak with a distro packaging (DEB, RPM...), see the package documentation for the best solution to
configure the daemons and to start/stop Alignak.

*Note*: Almost surely, the distro packaging will propose to set Alignak daemons as system services... anyway the information in the next section are still of interest for the reader even if Alignak is not installed from the source code ;)


Starting Alignak framework - daemons individual start
=====================================================

Starting each daemon individually is the old plain start method inherited from Shinken and from the very first Alignak version. It is still the method that is assumed with the default installed configuration.

Running all the Alignak daemons requires these commands:
::

    $ alignak-broker -n broker-master -e /usr/local/etc/alignak/alignak.ini
    $ alignak-scheduler -n scheduler-master -e /usr/local/etc/alignak/alignak.ini
    $ alignak-poller -n poller-master -e /usr/local/etc/alignak/alignak.ini
    $ alignak-reactionner -n reactionner-master -e /usr/local/etc/alignak/alignak.ini
    $ alignak-receiver -n receiver-master -e /usr/local/etc/alignak/alignak.ini

    # And the last, but not the least...
    $ alignak-arbiter -e /usr/local/etc/alignak/alignak.ini

This, because the default shipped configuration file is built in a manner that considers that the daemons are still started when the arbiter starts.

It is possible to start only the arbiter and make it start all the other daemons by itself. Edit the *alignak.ini*  configuration file and set the `alignak_launched` variable to 1. This can be configured for all the daemons or on a per-daemon basis ... see :ref:`core configuration <configuration/core>` for more information.

When the arbiter is started with the `alignak_launched` variable set, it will start / stop the other configured daemons. While it is running the arbiter daemon will check if all the other daemons processes are still runnig and it will restart them if they exit. As such, running the Alignak framework is only:
::

    $ alignak-arbiter -e /usr/local/etc/alignak/alignak.ini


Daemons command line parameters
===============================
All the Alignak daemons have a startup script that can be launched with command line parameters. These scripts have been installed by the Python installation process (or the distro packaging).

All the Alignak daemons need to be started with high privileges (root or sudo) that they will downgrade to a configured user/group account. The user they will use will need to have some permissions on the daemon working directory. See :ref:`core configuration <configuration/core>` for more information.

The only necessary configuration to provide to the daemons when they get started is:

    - the daemon name for the daemon to be able to find out its configuration
    - the *alignak.ini* file installed by the setup process.

Where to find the *alignak.ini* file:

   - in the */usr/local/etc/alignak* (or */etc/alignak*) directory

Except for the environment file and the daemon name, all other command line parameters are optional because default values are used by the daemon when it starts.

The daemon will get its configuration parameters from the *alignak.ini* environment file in the section named as *[daemon.daemon-name]*.
The daemon will also use some default values if they are not defined:

    - it will create its pid (*daemon-name.pid*) and log (*daemon-name.log*) file in the current working directory.
    - It will also use a default port to listen to the other daemons (arbiter: 7770, scheduler: 7768, broker: 7772, poller: 7771, reactionner: 7769, receiver: 7773).

For all the daemons (broker, poller, receiver, reactionner, scheduler)::

    $ alignak-broker -h
    usage: alignak-broker [-h] [-v] -n DAEMON_NAME [-c CONFIG_FILE] [-d] [-r]
                          [-f DEBUG_FILE] [-o HOST] [-p PORT] [-l LOG_FILENAME]
                          [-i PID_FILENAME] -e ENV_FILE

    optional arguments:
      -h, --help            show this help message and exit
      -v, --version         show program's version number and exit
      -n DAEMON_NAME, --name DAEMON_NAME
                            Daemon unique name. Must be unique for the same daemon
                            type.
      -c CONFIG_FILE, --config CONFIG_FILE
                            Daemon configuration file. Deprecated parameter, do
                            not use it anymore!
      -d, --daemon          Run as a daemon. Fork the launched process and
                            daemonize.
      -r, --replace         Replace previous running daemon if any pid file is
                            found.
      -f DEBUG_FILE, --debugfile DEBUG_FILE
                            File to dump debug logs. Not of any interest, will be
                            deprecated!
      -o HOST, --host HOST  Host interface used by the daemon. Default is 0.0.0.0
                            (all interfaces).
      -p PORT, --port PORT  Port used by the daemon. Default is set according to
                            the daemon type.
      -l LOG_FILENAME, --log_file LOG_FILENAME
                            File used for the daemon log. Set as empty to disable
                            log file.
      -i PID_FILENAME, --pid_file PID_FILENAME
                            File used to store the daemon pid
      -e ENV_FILE, --environment ENV_FILE
                            Alignak global environment file. This file defines all
                            the daemons of this Alignak instance and their
                            configuration. Each daemon configuration is defined in
                            a specifc section of this file.


The arbiter is slightly different because it manages some extra parameters::

    $ alignak-arbiter -h
    usage: alignak-arbiter [-h] [-v] [-a MONITORING_FILES] [-V] [-k ALIGNAK_NAME]
                           [-n DAEMON_NAME] [-c CONFIG_FILE] [-d] [-r]
                           [-f DEBUG_FILE] [-o HOST] [-p PORT] [-l LOG_FILENAME]
                           [-i PID_FILENAME] -e ENV_FILE

    optional arguments:
      -h, --help            show this help message and exit
      -v, --version         show program's version number and exit
      -a MONITORING_FILES, --arbiter MONITORING_FILES
                            Monitored configuration file(s). This option is still
                            available but is is preferable to declare the
                            monitored objects files in the alignak-configuration
                            section of the environment file specified with the -e
                            option.Multiple -a can be used, and they will be
                            concatenated to make a global configuration file.
      -V, --verify-config   Verify the configuration file(s) and exit
      -k ALIGNAK_NAME, --alignak-name ALIGNAK_NAME
                            Set the name of the Alignak instance. If not set, the
                            arbiter name will be used in place. Note that if an
                            alignak_name variable is defined in the configuration,
                            it will overwrite this parameter.For a spare arbiter,
                            this parameter must contain its name!
      -n DAEMON_NAME, --name DAEMON_NAME
                            Daemon unique name. Must be unique for the same daemon
                            type.
      -c CONFIG_FILE, --config CONFIG_FILE
                            Daemon configuration file. Deprecated parameter, do
                            not use it anymore!
      -d, --daemon          Run as a daemon. Fork the launched process and
                            daemonize.
      -r, --replace         Replace previous running daemon if any pid file is
                            found.
      -f DEBUG_FILE, --debugfile DEBUG_FILE
                            File to dump debug logs. Not of any interest, will be
                            deprecated!
      -o HOST, --host HOST  Host interface used by the daemon. Default is 0.0.0.0
                            (all interfaces).
      -p PORT, --port PORT  Port used by the daemon. Default is set according to
                            the daemon type.
      -l LOG_FILENAME, --log_file LOG_FILENAME
                            File used for the daemon log. Set as empty to disable
                            log file.
      -i PID_FILENAME, --pid_file PID_FILENAME
                            File used to store the daemon pid
      -e ENV_FILE, --environment ENV_FILE
                            Alignak global environment file. This file defines all
                            the daemons of this Alignak instance and their
                            configuration. Each daemon configuration is defined in
                            a specifc section of this file.


As a sump up:

    All daemons:
        '-n', "--name": Set the name of the daemon to pick in the configuration files.
        This allows an arbiter to find its own configuration in the whole Alignak configuration
        Using this parameter is mandatory for all the daemons except for the arbiter
        (defaults to arbiter-master). If several arbiters are existing in the
        configuration this will allow to determine which one is the master/spare.
        The spare arbiter must be launched with this parameter!

        '-e', '--environment': Alignak environment file - the most important and mandatory
        parameter to define the name of the alignak.ini configuration file

        '-c', '--config': Daemon configuration file (ini file) - deprecated! This parameter is still managed to alert about its deprecation and to maintain compatibility with former daemon startup scripts.
        '-d', '--daemon': Run as a daemon
        '-r', '--replace': Replace previous running daemon
        '-f', '--debugfile': File to dump debug logs.

        These parameters allow to override the one defined in the Alignak configuration file:
            '-o', '--host': interface the daemon will listen to
            '-p', '--port': port the daemon will listen to

            '-l', '--log_file': set the daemon log file name
            '-i', '--pid_file': set the daemon pid file name

    Arbiter only:
            "-a", "--arbiter": Monitored configuration file(s),
            (multiple -a can be used, and they will be concatenated to make a global configuration
            file) - Note that this parameter is not necessary anymore
            "-V", "--verify-config": Verify configuration file(s) and exit


Arbiter daemon exit codes
-------------------------

The arbiter dameon has some process exit code. Their meaning is:

    - 0: everything ok. Arbiter requested to stop and stopped as expected
    - 1: provided configuration parsing error detected and the arbiter stopped
    - 2: some necessary files declared in the configuration are missing
    - 3: an error was raised during the daemon initialization/fork
    - 4: running daemons connection problems when checking daemon communication or dispatching the configuration



Alignak processes list
======================

The daemons involved in Alignak are starting several processes in the system. All the processes started have a process title set by Alignak to help the user knowing which is which. Several processes types are present in the system processes list:

    * the main daemon process
        There will always be one process for each Alignak daemon type. The process title is built with the daemon type and the daemon name (eg. *alignak-arbiter arbiter-master*, *alignak-scheduler scheduler-other*,...)

    * the main daemon forked process.
        Each Alignak daemon forks a new process instance for each daemon instance existing in the configuration. If you defined several schedulers you will get a process for each scheduler instance. Each daemon instance process has a title built with the instance name (eg. *alignak-scheduler scheduler-master*)

    * the external modules processes
        The daemons that have some external modules attached, like brokers or receivers, launch new processes for their modules. Those processes titles are made of the daemon instance name and the module alias (eg. *alignak-receiver-master module: nsca*)

    * the satellite workers processes
        The satellites daemons that need some worker processes (pollers and reactionners) launch several worker processes to execute their actions (checks or notifications). Those worker processes have a title made of the daemon instance name and the worker label (eg. *alignak-poller-master worker*)


 As an example, here is the processes list of an Alignak configuration with several daemons of each type and some modules attached to some of the deamons::

    $ ps -aux | grep alignak-
    alignak   3432 10.2  0.5 1063940 64728 pts/2   Sl+  13:57   0:02 alignak-arbiter arbiter-master
    alignak   3441  0.0  0.3 265972 44132 pts/2    S+   13:57   0:00 alignak-arbiter arbiter-master

    alignak   3510  5.7  0.4 1061692 60000 pts/2   Sl+  13:57   0:01 alignak-receiver receiver-master
    alignak   3608  0.1  0.3 397196 44904 pts/2    Sl+  13:57   0:00 alignak-receiver receiver-master
    alignak   3505  5.6  0.4 1061664 59920 pts/2   Sl+  13:57   0:01 alignak-receiver receiver-master2
    alignak   3596  0.0  0.3 397044 44904 pts/2    Sl+  13:57   0:00 alignak-receiver receiver-master2
    alignak   3768  0.4  0.4 1062540 50072 pts/2   S+   13:57   0:00 alignak-receiver-master module: web-services
    alignak   3784  0.2  0.4 1062540 50068 pts/2   S+   13:57   0:00 alignak-receiver-master2 module: web-services

    alignak   3513  6.1  0.4 1061428 59420 pts/2   Sl+  13:57   0:01 alignak-reactionner reactionner-master
    alignak   3633  0.0  0.3 265676 44096 pts/2    S+   13:57   0:00 alignak-reactionner reactionner-master
    alignak   3720  0.0  0.3 1061004 47280 pts/2   S+   13:57   0:00 alignak-reactionner-master worker fork_1
    alignak   3721  0.0  0.3 1061016 47296 pts/2   S+   13:57   0:00 alignak-reactionner-master worker fork_2
    alignak   3722  0.0  0.3 1061164 47304 pts/2   S+   13:57   0:00 alignak-reactionner-master worker fork_3

    alignak   3520  5.7  0.4 1061416 59300 pts/2   Sl+  13:57   0:01 alignak-poller poller-master
    alignak   3619  0.0  0.3 265676 44128 pts/2    S+   13:57   0:00 alignak-poller poller-master
    alignak   3756  0.0  0.3 1061004 47480 pts/2   S+   13:57   0:00 alignak-poller-master worker fork_1
    alignak   3757  0.0  0.3 1061016 47812 pts/2   S+   13:57   0:00 alignak-poller-master worker fork_2
    alignak   3758  0.0  0.3 1061028 47500 pts/2   S+   13:57   0:00 alignak-poller-master worker fork_3
    alignak   3527  6.1  0.4 1061424 59320 pts/2   Sl+  13:57   0:01 alignak-poller poller-other
    alignak   3672  0.0  0.3 265676 44128 pts/2    S+   13:57   0:00 alignak-poller poller-other
    alignak   3737  0.0  0.3 1061004 47580 pts/2   S+   13:57   0:00 alignak-poller-other worker fork_1
    alignak   3738  0.0  0.3 1061016 47984 pts/2   S+   13:57   0:00 alignak-poller-other worker fork_2
    alignak   3739  0.0  0.3 1061028 47800 pts/2   S+   13:57   0:00 alignak-poller-other worker fork_3

    alignak   3549  6.2  0.5 1062340 61128 pts/2   Sl+  13:57   0:01 alignak-scheduler scheduler-master
    alignak   3684  0.0  0.3 266364 44380 pts/2    S+   13:57   0:00 alignak-scheduler scheduler-master
    alignak   3542  6.3  0.5 1062472 62944 pts/2   Sl+  13:57   0:01 alignak-scheduler scheduler-master2
    alignak   3660  0.0  0.3 266364 44400 pts/2    S+   13:57   0:00 alignak-scheduler scheduler-master2
    alignak   3556  6.2  0.5 1062340 61384 pts/2   Sl+  13:57   0:01 alignak-scheduler scheduler-other
    alignak   3708  0.0  0.3 266364 44396 pts/2    S+   13:57   0:00 alignak-scheduler scheduler-other

    alignak   3690  0.4  0.3 618216 45064 pts/2    Sl+  13:57   0:00 alignak-broker broker-master
    alignak   3538  7.5  0.4 1062252 60076 pts/2   Sl+  13:57   0:01 alignak-broker broker-master
    alignak   3764  0.5  0.4 1062320 50300 pts/2   S+   13:57   0:00 alignak-broker-master module: backend_broker
    alignak   3786  0.1  0.4 1062060 49568 pts/2   S+   13:57   0:00 alignak-broker-master module: logs
    alignak   3530  6.5  0.4 1061668 59836 pts/2   Sl+  13:57   0:01 alignak-broker broker-other
    alignak   3632  0.2  0.3 617960 44540 pts/2    Sl+  13:57   0:00 alignak-broker broker-other
    alignak   3729  0.4  0.4 1061808 49176 pts/2   S+   13:57   0:00 alignak-broker-other module: backend_broker
