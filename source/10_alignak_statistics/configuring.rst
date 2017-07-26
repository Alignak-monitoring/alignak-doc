.. _statistics/configuration:

=====================
Statistics parameters
=====================

The Alignak main configuration file allows to enable and set the Alignak interface to a StatsD daemon. See :ref:`Alignak StatsD configuration <configuration/core#statsd>`.

As a sum-up, you must set the following parameters if you installed a local StatsD:
::

  statsd_enabled=1
  statsd_host=localhost
  statsd_port=8125
  statsd_prefix=Alignak

If `statsd_host` is set to 'None', the metrics will not be sent to the StatsD.

If some environment variables exist the metrics will be logged to a file in append mode:
    'ALIGNAK_STATS_FILE'
        the file name
    'ALIGNAK_STATS_FILE_LINE_FMT'
        defaults to [#date#] #counter# #value# #uom#\n'
    'ALIGNAK_STATS_FILE_DATE_FMT'
        defaults to '%Y-%m-%d %H:%M:%S'
        date is UTC
        if configured as an empty string, the date will be output as a UTC timestamp
