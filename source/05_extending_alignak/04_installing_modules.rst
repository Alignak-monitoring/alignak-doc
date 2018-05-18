.. _extending/modules:

===============
Alignak modules
===============

Extending Alignak features is made thanks to daemons modules. Some modules are already packaged in an easy installable package called an **Alignak module package**.

Configuring daemons with modules
--------------------------------

Each Alignak daemon can be configured to load and use modules. In the daemon configuration file, the attribute `modules` may contain a list of the daemon modules.

As a default, no module is installed nor configured but one can edit the daemon configuration file to declare which module is to be used. The `modules` property is a comma separated list of the declared modules alias.

As an example, the receiver can have several attached modules.::

    # Receiver daemon configuration
    define receiver{
        receiver_name           receiver-master

        modules                 nsca, external-commands, web-services
    }



**Note** that the module alias is case sensitive.

For more information about modules, see the `example module repository <https://github.com/Alignak-monitoring/alignak-module-example>`_.


Alignak modules are available on PyPI_. Their name starts with "alignak_module".
To install them, install with pip command::

     pip install alignak_module_nameofmodule

The configuration file of the installed module will be installed in the folder::

    etc/alignak/arbiter/modules

Each module installed on Alignak stores its configuration file in this part of the configuration.


.. _PyPI: https://pypi.python.org/pypi


.. _modules/logs:

Logs
----

This module collects all the monitoring logs and send them to a python logger.

Short story::

    pip install alignak-module-logs

    # Update your broker daemon configuration
    define broker{
        broker_name             broker-master

        modules                 logs
    }


    # Edit the module configuration (etc/alignak/arbiter/modules/mod-logs.cfg)
    define module {
        module_alias            logs
        module_types            logs
        python_name             alignak_module_logs

        # Logger configuration file
        # ---
        # You should define the logger JSON configuration file here or, even better, declare an
        # environment variable 'ALIGNAK_MONITORING_LOGS_CFG' to specify the full path of the
        # logger configuration file.
        # The environment variable will be used in priority to any other configuration in this file
        #logger_configuration    ALIGNAKETC/arbiter/modules/mod-logs-logger.json

        # Default parameters
        # ---
        # If the logger configuration file is not configured or it does not exist the logger is
        # configured with the following default parameters
        # Logger name
        #log_logger_name         monitoring-logs

        # Logger file
        log_dir                 ALIGNAKLOG
        #log_file                monitoring-logs.log

        # Logger file rotation parameters
        #log_rotation_when       midnight
        #log_rotation_interval   1
        #log_rotation_count      7

        # Logger level (accepted log level values=INFO,WARNING,ERROR)
        #log_level               INFO

        # Logger log format
        #log_format              [%(created)i] %(levelname)s: %(message)s

        # Logger date is ISO8601 with timezone
        #log_date                %Y-%m-%d %H:%M:%S %Z
    }

The module default configuration allows to create a monitoring-logs.log file in the default Alignak log directory.
The file is largely commented to help understand and configure this module.

More information is available in the `monitoring logs module repository <https://github.com/Alignak-monitoring-contrib/alignak-module-log>`_.


.. _modules/nsca:

NSCA collector
--------------

Managing passive hosts and services requires that Alignak *listen* to passive checks.

To achieve this, the Receiver daemon needs to be extended with an NSCA module. This module listens on a TCP port for NSCA packets, decode the packets and builds an external command corresponding to the received check: **HOST_PASSIVE_CHECK** or **SERVICE_PASSIVE_CHECK**.

Passive checks are managed by Alignak according :ref:`to this behavior<monitoring_features/passive_checks>`.

Short story::

    pip install alignak-module-nsca

    # Update your receiver daemon configuration
    define receiver{
        receiver_name           receiver-master

        modules                 nsca
    }


    # Edit the NSCA configuration (etc/alignak/arbiter/modules/mod-nsca.cfg)
    define module {
        module_alias             nsca
        python_name              alignak_module_nsca
        ...
        ...

    }

The module default configuration allows to collect non-encrypted NSCA checks for hosts and services.
The file is largely commented to help understand and configure this module.

More information is available in the `NSCA module repository <https://github.com/Alignak-monitoring-contrib/alignak-module-nsca>`_.


.. _modules/named_pipe:

External commands
-----------------

This module allows Alignak framework (like Nagios *and al.*) to reacts to external commands sent to a named pipe file.

Thanks to this module the receiver daemon periodically reads the content of a configured file and builds an external command with the information read from this file. This also allows Alignak to :ref:`receive passive checks<monitoring_features/passive_checks>`.

**Note** that the Arbiter is able to manage the external commands by itself and that it is not necessary to use an external module...

Short story::

    pip install alignak-module-external-commands

    # Update your receiver daemon configuration
    define receiver{
        receiver_name           receiver-master

        modules                 external-commands
    }


    # Edit the external commands module configuration (etc/alignak/arbiter/modules/mod-external-commands.cfg)
    define module {
        module_alias            external-commands
        module_types            external-commands
        python_name             alignak_module_external_commands

        # Default file path is /tmp/alignak.cmd
        file_path               /tmp/alignak.cmd
    }

The module default configuration gets commands from a */tmp/alignak.cmd* file.

More information is available in the `external commands module repository <https://github.com/Alignak-monitoring-contrib/alignak-module-external-commands>`_.


.. _modules/web_services:

Web services
------------

This module exposes Web services to get information about the Alignak framework and to notify external commands from a third-party application.

**Note** that the Arbiter is able to manage the external commands by itself and that it is not necessary to use an external module...


This also allows Alignak to :ref:`receive passive checks<monitoring_features/passive_checks>`.

Short story::

    pip install alignak-module-web-services

    # Update your receiver daemon configuration
    define receiver{
        receiver_name           receiver-master

        modules                 web-services
    }


    # Edit the web services module configuration (etc/alignak/arbiter/modules/mod-web-services.cfg)
    define module {
        module_alias            web-services
        module_types            web-services
        python_name             alignak_module_ws

        #-- Alignak configuration
        # Alignak main arbiter interface
        #alignak_host            127.0.0.1
        #alignak_port            7770

        # Alignak polling period
        #alignak_polling_period  1

        # Alignak daemons status refresh period
        #alignak_daemons_polling_period  10

        #-- Network configuration
        # Interface the modules listens to
        host                    0.0.0.0
        # Do not comment the port parameter (see Alignak #504)
        port                    8888

        #-- SSL configuration --
        use_ssl                 0
        #ca_cert                 /usr/local/etc/alignak/certs/ca.pem
        #server_cert             /usr/local/etc/alignak/certs/server.cert
        #server_key              /usr/local/etc/alignak/certs/server.key
        #server_dh               /usr/local/etc/alignak/certs/server.pem
        #hard_ssl_name_check     0
    }

The module default configuration tries to get information from a local Alignak arbiter and listens
to all network interfaces on port 8888.

More information is available in the `web services module repository <https://github.com/Alignak-monitoring-contrib/alignak-module-web-services>`_.


.. _modules/backend:

Alignak backend
---------------

The Alignak backend module(s) implements several features for several Alignak daemons:

    - loads the configuration for the Arbiter
    - updates the monitored objects live state for the Broker
    - state retention of the live state for the Scheduler

Installing this module will, in fact, install the three modules.

**Note**: this module implies that you already installed the Alignak backend.

Short story::

    pip install alignak-module-backend

    # Update your arbiter daemon configuration
    define arbiter{
        arbiter_name            arbiter-master

        modules                 backend_arbiter
    }


    # Edit the backend arbiter module configuration (etc/alignak/arbiter/modules/mod-alignak_backend_arbiter.cfg)
    define module {
        module_alias            backend_arbiter
        python_name             alignak_module_backend.arbiter
        ...
        ...

    }

    # Update your broker daemon configuration
    define broker{
        broker_name             broker-master

        modules                 backend_broker
    }


    # Edit the backend broker module configuration (etc/alignak/arbiter/modules/mod-alignak_backend_broker.cfg)
    define module {
        module_alias            backend_broker
        python_name             alignak_module_backend.broker
        ...
        ...

    }

    # Update your arbiter scheduler configuration
    define arbiter{
        scheduler_name          scheduler-master

        modules                 backend_scheduler
    }


    # Edit the backend scheduler module configuration (etc/alignak/arbiter/modules/mod-alignak_backend_scheduler.cfg)
    define module {
        module_alias            backend_scheduler
        python_name             alignak_module_backend.scheduler
        ...
        ...

    }

The modules default configuration needs to be updated with your backend connection and login information.
The files are largely commented to help understand and configure this module.

More information is available in the `backend modules repository <https://github.com/Alignak-monitoring-contrib/alignak-module-backend>`_.

