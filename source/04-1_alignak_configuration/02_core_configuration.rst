.. _configuration/core:

==================
Core configuration
==================

The core configuration part describes the Alignak framework infrastructure (which daemons are used and how they are) and the main configuration.

The Alignak environment file (*alignak.ini*) aims to contain all the Alignak configuration, except the monitored system that may be configured separately in Nagios legacy configuration (cfg) files or in the Alignak backend.

If your monitored system configuration is stored in the Alignak backend, the *alignak.ini* file is (almost) the only configuration file you will have to manage ;)

The Alignak environment file contains the necessary information about:

    - the Alignak installation directories
    - the Alignak daemons and their configuration
    - the Alignak monitored configuration
    - the Alignak macros

This file is structured as a common Ini file with some sections containig variables.

All the Alignak configuration variables are commented  in the default shipped configuration file and are described in :ref:`the following chapter <configuration/global_variables>`

When an Alignak daemon loads its configuration, it will try to open the file which name is provided on the command line with the `-e` / `--environment` parameter. If it exists an *alignak.d* sub-directory in the same directory as the environment file, all the **.ini* files located in this sub-directory will also be loaded and parsed.

When updating the configuration with a new daemon or new parameters, it is cleaner/easier to add a new file in the *alignak.d* rather than modifying a unique configuration file;)

The default shipped example configuration is structured as::

    alignak.ini
    alignak.d/
        daemons.ini
        modules.ini

.. warning:: all the default shipped files may be updated when you will update Alignak. As such, all the modifications you did in these files will be lost!

Configuration sections
----------------------

[DEFAULT]
~~~~~~~~~

The **DEFAULT** section is the main section of the configuration. It defines *global* variables that will be inherited by all the other configuration sections.

The default configuration defines all the Alignak configuration variable in this section. It is a choice to make all the variables available, if needed, in any daemon that loads the *alignak.ini* file.

.. note :: For more information about this inheritance process, check the `Python configuration parser <https://wiki.python.org/moin/ConfigParser>`_ behavior.

[alignak-configuration]
~~~~~~~~~~~~~~~~~~~~~~~

The **alignak-configuration** section is used to define the :ref:`global variables and macros <configuration/global_variables>`, and the monitored system configuration files (eg. the Nagios legacy cfg files).

Thanks to the section inheritance process, all the configuration variables are made available in this section and they may be overloaded if needed. Only the Alignak Arbiter gets this section variables to build its monitoring configuration.

**Global variables**

To change a default configured variable::

  ; Disable notifications
  enable_notifications=0

Alignak uses most of the Nagios common configuration variables that may be defined in this section.

.. note:: the variables defined in some legacy configuration files take precedence over the one defined in this configuration file.


**Nagios legacy configuration**

If your monitored system is configured with legacy configuration files, you can declare the main configuration entry point file in this section: ::

    cfg=my_nagios.cfg
    cfg2=an_extra_file.cfg

All the `cfg` prefixed variables will be considered as some Nagios legacy configuration files. The Alignak arbiter will open and parse these files to build the monitored system configuration.


**Macros**

Some plugins used to check the system hosts/services often need global variables known as macros. This section is the rigth place to declare such variables::

    _my_macro_=1
    _nrpe_plugins_dir_=/usr/local/libexec/nagios/

The variables prefixed with an `_` (underscore) will be considered as macros. The leading and trailing underscores will be removed and the variable name will be uppercased. `_my_macro_` will be usable as `$MY_MACRO$`.

[daemon.daemon-name]
~~~~~~~~~~~~~~~~~~~~

The **daemon.*** sections allow to declare all the daemons that are involved in the Alignak configuration.

Each daemon must have its own section with its specific parameters.

.. note :: Remember that the **DEFAULT** section variables are inherited in all the other sections. Thus, you only need to declare the daemon specific variables (eg. listening port) in each daemon section.

All the daemons have a common set of configuration variables which are explained in this table:

===================================== ======= =========================== ============================================================
Variable name                         Type    Default                     Short description
===================================== ======= =========================== ============================================================
type                                  string                              Daemon type (arbiter, scheduler, poller, broker, reactionner, receive, poller)
name                                  string                              Daemon unique name

user                                  string                              Daemon user account username
group                                 string                              Daemon user account group

host                                  string  0.0.0.0                     listening interface
address                               string  127.0.0.1                   FQDN or ip address used by the other daemons
port                                  integer                             HTTP port of the daemon WS interface
spare                                 boolean 0                           set if the daemon is a spare
debug                                 boolean 0                           set to activate debug log level
active                                boolean 1                           unset to disable the daemon in the configuration
modules                               string                              modules name list separated by comma
use_ssl                               boolean 0                           use SSL for communications with this daemons
realm                                 string  All                         the realm the daemon is attached to
manage_sub_realms                     boolean 0                           manage its realm only (0) and the sub realms (1)
server_cert                           string  %(etcdir)s/certs/server.crt
server_key                            string  %(etcdir)s/certs/server.key
ca_cert                               string  %(etcdir)s/certs/ca.pem
===================================== ======= =========================== ============================================================

Each daemon is listening on an `host`:`port` interface where it exposes its Web Service API. It may be accessible for the other daemons on the same port but with an other `address`.

Each daemon will change its credentials to run as user / group as specified in its parameters. If non is specified it will use the current logged in user.

Each daemon is attached to a `realm` (defaults to *All*) and it may be involved only in its realm (default behavior) or in its realm and all the sub-realms (`manage_sub_realms=1`). When using a multi-realms environment, make sure to **avoid overlapping realms/daemons** because it may have some unexpected behavor!


**Poller / reactionner daemons specific parameters:**

===================================== ======= =========== ============================================================
Variable name                         Type    Default     Short description
===================================== ======= =========== ============================================================
min_workers                           integer 0           The minimum workers launched by the daemon
max_workers                           integer 0           The maximum workers launched by the daemon
processes_by_worker                   integer 256         The processes that may be started by a worker process.
worker_polling_interval               integer 1           The daemon will check its workers on this polling interval
passive                               boolean 0           Set to 1 to use the daemon passive mode
===================================== ======= =========== ============================================================

The minimum and maximum workers launched by the daemon allow to configure the number of processes that will be used to execute the delegated actions. If set to 0, the poller/reactionner daemon will use N-1 workers if your system has N CPUs. The poller defaults to 0 (use as many workers as possible) whereas the reactionner defaults to 1 (use only one worker).

In active mode, the poller/reactionner is connecting to its scheduler to get its actions to execute and to report the execution results. The passive mode allows to make the scheduler push its actions and get the results from the poller/reactionner satellites. This mode is interesting to control the network flow from the scheduler to poller/reactionner on a remote site...


**Scheduler daemons specific parameters (advanced configuration parameters):**

===================================== ======= =========== ============================================================
Variable name                         Type    Default     Short description
===================================== ======= =========== ============================================================
skip_initial_broks                    boolean 0           The scheduler will not require the initial initialization broks
weight                                integer 1           Set the scheduler weight in the dispatching process
accept_passive_unknown_check_results  boolean 0           set 1 to allow passive check for unknown hosts
===================================== ======= =========== ============================================================

.. note :: Those are advanced configuration parameters. Feel free to request for more information about them if needed. This will mean that you already have an idea of what it is about ;)


**Broker daemons specific parameters:**

===================================== ======= =========== ============================================================
Variable name                         Type    Default     Short description
===================================== ======= =========== ============================================================
max_queue_size                        integer 100000      Limit the broker modules queue size if it becomes too important
manage_arbiters                       boolean 1           Set this to get the arbiter created broks
===================================== ======= =========== ============================================================

There must only be one and only one broker that gets the broks created by the arbiter (`manage_arbiters`). o not set this parameter for all other brokers because it defaults to False.

The `max_queue_size` parameter is managed by all the daemons with a default value set to 0, which means: do not care about the queue size. For a broker, it is important to manage the queue size limitation!

.. note :: Those are advanced configuration parameters. Feel free to request for more information about them if needed. This will mean that you already have an idea of what it is about ;)


[module.module-name]
~~~~~~~~~~~~~~~~~~~~

The **module.*** sections allow to declare all the extra modules that are used and their configuration.

Each module must have its own section with its specific parameters.

.. note :: Remember that the **DEFAULT** section variables are inherited in all the other sections. Thus, you only need to declare the module specific variables in each module section.

All the modules have a common set of configuration variables which are explained in this table:

===================================== ======= =========================== ============================================================
Variable name                         Type    Default                     Short description
===================================== ======= =========================== ============================================================
type                                  string                              Module type (retention, metrics, ...)
name                                  string                              Module unique name
python_name                           string  0.0.0.0                     Python libray to be loaded for the module
===================================== ======= =========================== ============================================================

The module type is only an informative field. Except for some specific case, this is not considered by Alignak

.. note :: Contact the development team for more about the module type if needed!.
