.. _configuration/logger:

Logger configuration
====================

Default logging configuration
-----------------------------

The Alignak logger configuration is defined thanks to a JSON Python logger configuration file.

Thanks to this implementation all the Python logger features are available with a simple configuration in this file: changing the log format, sending log to several destination, logging to a file, logging through email, http,... For more information, see `the Python logging cookbook <https://docs.python.org/2/howto/logging-cookbook.html>`_. All is not possible, but many scenario are yet ;)

The default shipped configuration file is */usr/local/etc/alignak/alignak-logger.json* and it defines a logger for the Alignak daemons and a logger for the monitoring events log.

If a problem is raised before the logger configuration took place, a default */tmp/alignak.log* is used to log the raised errors.

The default logger configuration is:
::

    {
        "version": 1,
        "disable_existing_loggers": false,
        "formatters": {
            "alignak": {
                "format": "[%(asctime)s] %(levelname)s: [%(daemon)s.%(name)s] %(message)s"
                ,"datefmt": "%Y-%m-%d %H:%M:%S"
            },
            "monitoring-log": {
                "format": "[%(asctime)s] %(levelname)s: %(message)s"
            ,"datefmt": "%Y-%m-%d %H:%M:%S"
            }
        },

        "handlers": {
            "console": {
                "class": "logging.StreamHandler",
                "level": "DEBUG",
                "formatter": "alignak",
                "stream": "ext://sys.stdout"
            },
            "color_console": {
                "class": "alignak.log.ColorStreamHandler",
                "level": "DEBUG",
                "formatter": "alignak",
                "stream": "ext://sys.stdout"
            },
            "daemons": {
                "class": "logging.handlers.TimedRotatingFileHandler",
                "level": "DEBUG",
                "formatter": "alignak",
                "filename": "%(logdir)s/%(daemon)s.log",
                "when": "midnight",
                "interval": 1,
                "backupCount": 7,
                "encoding": "utf8"
            },
            "monitoring_logs": {
                "class": "logging.handlers.TimedRotatingFileHandler",
                "level": "INFO",
                "formatter": "monitoring-log",
                "filename": "%(logdir)s/monitoring-logs.log",
                "when": "midnight",
                "interval": 1,
                "backupCount": 365,
                "encoding": "utf8"
            }
        },

        "loggers": {
            "alignak": {
                "level": "INFO",
                "handlers": ["color_console", "daemons"],
                "propagate": "no"
            },
            "monitoring-log": {
                "level": "DEBUG",
                "handlers": ["console", "monitoring_logs"],
                "propagate": "no"
            }
        },

        "root": {
            "level": "ERROR",
            "handlers": []
        }
    }

When this file is loaded by an Alignak daemon, its content is parsed and the `%(logdir)s` and `%(daemon)s` variables are respectively replaced with the log directory configuration parameter and the daemon name.


Specific CherryPy logging
-------------------------

For development purpose it may be interesting to have some CherryPy log for the underlying inter-daemon HTTP communication.

The Alignak main configuration file allows to activate CherryPy logging thanks to the `log_cherrypy` variable. When set, this variable will make the concerned HTTP daemon add some CherryPy log into the daemon log file.

For more specific need, it is possible possible to create a dedicated logger hierarchy configuration where all the Alignak and CherryPy logging behavior is configured. As an example, see the *dev/alignak-logger-cherrypy.json* file in the Alignak repository. This file redefines all the logger, handlers and formatters for Alignak and CherryPy. This will make CherryPy send its log to dedicated files with a specific formatting.

**Note** that the CherryPy access log formating is not easily updatable thanks to the logging formatter :(
