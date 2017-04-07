.. _monitoring_features/volatile_services:

=================
Volatile services
=================


Alignak has the ability to distinguish between “normal" services and "volatile" services. The ``is_volatile`` property of a service allows you to specify whether a specific service is volatile or not (default behavior).

Volatile services are useful for monitoring:

  * Things that automatically reset themselves to an "OK" state each time they are checked
  * Events such as security alerts which require attention every time there is a problem (and not only the first time)


Volatile services differ from “normal" services in three important ways. Each time they are checked when they are in a hard non-OK state, and the check returns a non-OK state (i.e. no state change has occurred):

  * The non-OK service state is logged
  * Contacts are notified about the problem and notification intervals are ignored
  * The :ref:`event handler <monitoring_features/event_handlers>` for the service is run

These events normally only occur for services when they are in a non-OK state and a hard state change has just occurred. In other words, they only happen the first time that a service goes into a non-OK state. If future checks of the service result in the same non-OK state, no hard state change occurs and none of the events mentioned take place again.

.. tip::  If you are only interested in logging, consider using :ref:`stalking <monitoring_features/stalking>` feature instead.

