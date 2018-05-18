.. _configuration/logger:

Logger configuration
====================

Default logging configuration
-----------------------------

The Alignak logger configuration is defined thanks to a JSON Python logger configuration file.

Thanks to this implementation all the Python logger features are available with a simple configuration in this file: changing the log format, sending log to several destination, logging to a file, logging through email, http,... For more information, see `the Python logging cookbook <https://docs.python.org/2/howto/logging-cookbook.html>`_. All is not possible, but many scenario are yet ;)

The default shipped configuration file is */usr/local/share/alignak/etc/alignak-logger.json* and it defines a logger for the Alignak daemons and a logger for :ref:`the monitoring events log<alignak_features/monitoring_log>`.

The Alignak daemons log are stored in daily rotated files located in the */usr/local/var/log/alignak* directory. The log file names are prefixed with the daemon name. These files are kept for seven (default) days.

The Alignak monitoring events log is stored in a daily rotated file named *alignak-events-log* in the same directory. As per default, this file is kept for 365 days.

If a problem is raised before the logger configuration is set-up, a default */tmp/alignak.log* is used to log the raised errors.

.. tip:: If you meet some problems when starting an Alignak daemon, think about having a look to this file, it may help understanding the problem!

The default shipped logger configuration is::

   {
       "version": 1,
       "disable_existing_loggers": false,
       "formatters": {
           "alignak": {
               "format": "[%(asctime)s] %(levelname)s: [%(daemon)s.%(name)s] %(message)s",
               "datefmt": "%Y-%m-%d %H:%M:%S"
           },
           "monitoring-log": {
               "format": "[%(my_date)s] %(levelname)s: %(message)s",
               "datefmt": "%Y-%m-%d %H:%M:%S"
           }
       },

       "handlers": {
           "unit_tests": {
               "class": "alignak.log.CollectorHandler",
               "level": "DEBUG",
               "formatter": "alignak"
           },
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
               "backupCount": 7
           },
           "event_log": {
               "class": "logging.handlers.TimedRotatingFileHandler",
               "level": "INFO",
               "formatter": "monitoring-log",
               "filename": "%(logdir)s/alignak-events.log",
               "when": "midnight",
               "interval": 1,
               "backupCount": 365
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
               "handlers": ["console", "event_log"],
               "propagate": "no"
           }
       },

       "root": {
           "level": "ERROR",
           "handlers": []
       }
   }

When this file is loaded by an Alignak daemon, its content is parsed and the `%(logdir)s` and `%(daemon)s` variables are respectively replaced with the log directory configuration parameter and the daemon name.

The monitoring log event date is not the time when the log is emitted to the logger but the time when the event is raised by the originating daemon. The arbiter periodically collects all the events near all its satellites and raises the log with the creation time date.

.. note:: that the formatter used for the monitoring log uses a ``%(my_date)s`` variable which is not a standard logger date.


Specific CherryPy logging
-------------------------

For development purpose it may be interesting to have some CherryPy log for the underlying inter-daemon HTTP communication.

The Alignak main configuration file allows to activate CherryPy logging thanks to the `log_cherrypy` variable. When set, this variable will make the concerned HTTP daemon add some CherryPy log into the daemon log file.

For more specific need, it is possible possible to create a dedicated logger hierarchy configuration where all the Alignak and CherryPy logging behavior is configured. As an example, see the *dev/alignak-logger-cherrypy.json* file in the Alignak repository. This file redefines all the logger, handlers and formatters for Alignak and CherryPy. This will make CherryPy send its log to dedicated files with a specific formatting.

**Note** that the CherryPy access log formating is not easily updatable thanks to the logging formatter :(
