.. _monitoring_features/services_checks:

===============
Services checks
===============


Introduction
------------

The basic workings of services checks are described here...


When are service checks performed?
----------------------------------

Services are checked by the Alignak scheduler daemon at regular intervals, as defined by the ``check_interval`` and ``retry_interval`` options in your service definitions.



Cached service checks
---------------------
**Not available (see #1026)!**

The performance of on-demand service checks can be significantly improved by implementing the use of cached checks, which allow Alignak to omit executing a service check if it determines that a relatively recent check result still exists. Cached checks will only provide a performance increase if you are making use of :ref:`service dependencies <monitoring_features/dependencies>`. More information on cached checks can be found :ref:`here <alignak_features/cached_checks>`.


Dependencies and checks
-----------------------

You can define :ref:`service execution dependencies <monitoring_features/dependencies>` that prevent Alignak from checking the status of a service depending on the state of one or more other services. More information on dependencies can be found :ref:`here <monitoring_features/dependencies>`.


Parallelization of service checks
---------------------------------

Scheduled service checks are run in parallel.


Service states
--------------

Services that are checked can be in one of four different states:

   * OK
   * WARNING
   * UNKNOWN
   * CRITICAL
   * UNREACHABLE

.. note:: the UNREACHABLE state is an Alignak specific feature that does not exist in Nagios. A service is UNREACHABLE when its attached host is DOWN or UNREACHABLE

Service state determination
---------------------------

Service checks are performed by plugins which can return a status code of OK, WARNING, UNKNOWN, or CRITICAL. These plugins exit codes directly translate to service states. For example, a plugin which returns a WARNING state will cause a service to have a WARNING state.


Services state changes
----------------------

When Alignak checks the status of services, it will be able to detect when a service changes between OK, WARNING, UNKNOWN, and CRITICAL states and take appropriate action. These state changes result in different state types (HARD or SOFT), which can trigger :ref:`event handlers <monitoring_features/event_handlers>` to be run and :ref:`notifications <monitoring_features/notifications>` to be sent out. Service state changes can also trigger on-demand :ref:`host checks <monitoring_features/hosts_checks>`. Detecting and dealing with state changes is what Alignak is all about.

Soft (state type is SOFT) states occur when the service checks return a non-OK state and are in the process of being retried. Hard states (state type is HARD) result when the service checks have been checked a specified maximum number of times and the current state is confirmed.

When services change state too frequently they are considered to be “flapping". Alignak can detect when services start flapping, and can suppress notifications until flapping stops and the service's state stabilizes. More information on the flap detection logic can be found :ref:`here <monitoring_features/flapping>`.

