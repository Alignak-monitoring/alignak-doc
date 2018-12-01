.. _alignak_features/inner_modules:

=============
Inner modules
=============


Introduction
------------

Alignak has some inner modules that extend the basic monitoring features without the need to install separate modules. The main goal of this is to have a full featured monitoring application that remains quite simple to set-up ;)

Retention module
----------------

The inner retention module provides a simple retention save / load of the current system live state in Json formated files. This to mimic the Nagios legacy `status.dat` feature...

This module is automatically enabled if your configuration has some values in the `retain_state_information` parameter. The module has its own default configuration but it will use the `state_retention_file` and `state_retention_dir` if it not empty as the directory/file name.

If you set some values in the module configuration they will overload the one defined formely in the main configuration

Module default configuration::

   [module.inner-retention]
   ; The inner retention module is declared to allow parameters configuration when it is activated
   ; in the configuration. To activate, simply set 'enabled' as 1 or set the
   ; retain_state_information Nagios legacy parameter
   name = inner-retention
   type = retention
   python_name = alignak.modules.inner_retention
   definition_order = 1
   enabled = 1

   ; --------------------------------------------------------------------
   ; Retention configuration
   ; ---
   ; If defined in this file, the configuration will overload the default one
   ; built on former configuration loading.
   ; ---

   ; retention_dir overloads main state_retention_dir
   ; Environment variable 'ALIGNAK_RETENTION_DIR' overloads this configuration variable
   ;retention_dir=/var/run/alignak

   ; retention_file overloads main state_retention_file
   ; Environment variable 'ALIGNAK_RETENTION_FILE' overloads this configuration variable
   ;retention_file=
   ; --------------------------------------------------------------------


Metrics module
--------------

The inner metrics module provides a simple performance data management. This to mimic the Nagios legacy performance data commands feature...

This module is automatically enabled if your configuration has some values in the `host_perfdata_file` or `service_perfdata_file` parameters. The module has its own default configuration.

Module default configuration::

    [module.inner-metrics]
    ; The inner metrics module is declared to allow parameters configuration when it is activated
    ; in the configuration. To activate, simply set -enabled' as 1 or declare a value for
    ; host_perfdata_file or service_perfdata_file Nagios legacy parameters
    name = inner-metrics
    type = metrics
    python_name = alignak.modules.inner_metrics
    definition_order = 1
    enabled = 1

    ; --------------------------------------------------------------------
    ; Module internal metrics
    ; Export module metrics to a statsd server.
    ; By default at localhost:8125 (UDP) with the alignak prefix
    ; Default is not enabled
    ; --------------------------------------------------------------------
    ;statsd_host = localhost
    ;statsd_port = 8125
    ;statsd_prefix = alignak
    ;statsd_enabled = 0
    ; --------------------------------------------------------------------
    ;
    ; Module log level
    ;log_level=INFO

    ;
    ; Module specific parameters
    graphite_host=localhost
    graphite_port=2004
    graphite_prefix=alignak

    ; Add this suffix to the hosts/services matrics
    ;graphite_data_source=from_alignak

    ;
    ; Output metrics to a file - specify the output file full path name
    ; Default is disabled
    ;output_file=

    ; Flush to Graphite everay X received metrics
    ; This allows sending metrics to Graphite in bulk mode
    ;metrics_flush_count=64

    ; Do not ignore unknown hosts/services
    ignore_unknown=0

    ; Use a fake service description for the metrics of an host check result
    ; This will group the host metrics in a same directory
    ;host_check=

    ; Send the warning, critical, ... to Graphite
    ; Default is to not send because it creates many similar metrics
    ;send_warning=true
    ;send_critical=true
    ;send_min=true
    ;send_max=true
