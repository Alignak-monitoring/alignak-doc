.. _monitoring_features/services_checks:

===============
Services Checks
===============


Introduction
============

The basic workings of services checks are described here...


When Are Service Checks Performed?
==================================

Services are checked by the Alignak daemon at regular intervals, as defined by the ``check_interval`` and ``retry_interval``
options in your service definitions.



Cached Service Checks
=====================

The performance of on-demand service checks can be significantly improved by implementing the use of cached checks,
which allow Alignak to forgo executing a service check if it determines a relatively recent check result will do instead.
Cached checks will only provide a performance increase if you are making use of :ref:`service dependencies <monitoring_features/dependencies>`.
More information on cached checks can be found :ref:`here <alignak_features/cached_checks>`.


Dependencies and Checks
=======================

You can define :ref:`service execution dependencies <monitoring_features/dependencies>` that prevent Alignak from checking
the status of a service depending on the state of one or more other services. More information on dependencies can
be found :ref:`here <monitoring_features/dependencies>`.


Parallelization of Service Checks
=================================

Scheduled service checks are run in parallel.


Service States
==============

Services that are checked can be in one of four different states:

  * OK
  * WARNING
  * UNKNOWN
  * CRITICAL


Service State Determination
===========================

Service checks are performed by plugins which can return a state of OK, WARNING, UNKNOWN, or CRITICAL. These plugins
states directly translate to service states. For example, a plugin which returns a WARNING state will cause a service to have a WARNING state.

Many plugins exist for Alignak because Alignak is fully compatible with the Nagios / Shinken plugins interface.


Services State Changes
======================

When Alignak checks the status of services, it will be able to detect when a service changes between OK, WARNING, UNKNOWN,
and CRITICAL states and take appropriate action. These state changes result in different state types (HARD or SOFT),
which can trigger :ref:`event handlers <monitoring_features/event_handlers>` to be run and :ref:`notifications <monitoring_features/notifications>`
to be sent out. Service state changes can also trigger on-demand :ref:`host checks <monitoring_features/hosts_checks>`.
Detecting and dealing with state changes is what Alignak is all about.

When services change state too frequently they are considered to be “flapping". Alignak can detect when services
start flapping, and can suppress notifications until flapping stops and the service's state stabilizes.
More information on the flap detection logic can be found :ref:`here <monitoring_features/flapping>`.

