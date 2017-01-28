.. _statistics/configuration:

=====================
Statistics parameters
=====================

The Alignak main configuration file allows to enable and set the Alignak interface to a StatsD daemon. See :ref:`Alignak StatsD configuration<configuration/core#statsd>`_.

As a sum-up, you must set the following parameters if you installed a local StatsD:
::

  statsd_enabled=1
  statsd_host=localhost
  statsd_port=8125
  statsd_prefix=Alignak

