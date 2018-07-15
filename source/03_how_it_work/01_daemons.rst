.. _howitworks/daemons:

==================================
Daemons and how they work together
==================================

Daemons
=======

Alignak framework is designed to set up easily and smartly a distributed monitoring application for network services and resources.

It is made of 6 daemons which features may be extended thanks to modules. Each daemon type has its own role in the monitoring process.


A picture says a thousand words:

.. figure:: /_static/images/Alignak-architecture-1.png
   :scale: 90 %
   :alt: Alignak daemons architecture

   Alignak framework daemons synthetic view.



Arbiter
-------

The *Arbiter* daemon role:

   * Loading the Alignak own configuration (daemons, behavior, ...)

   * Loading the monitored system objects configuration (hosts, services, contacts, ...), loaded from Nagios legacy configuration files or from the Alignak backend database

   * Dispatching the whole framework configuration to the other daemons

   * Managing daemons connections and monitoring the state of the other daemons

   * Forwarding failed daemons configuration to spare daemons

   * Receiving external commands

   * Collecting the monitoring events log

   * Reporting Alignak state

There can have only one active Arbiter, other arbiters (if they exist in the configuration) are acting as standby spares.


Scheduler
---------

The *Scheduler* daemon role:

    * scheduling the checks to launch

    * determines action to execute (notifications, acknowledges, ...)

    * dispatches the checks and actions to execute to the pollers and reactionners

There can have many schedulers for load-balancing; each scheduler is managing its own hosts list.


Poller
------

The *Poller* runs the :ref:`active checks<monitoring_features/active_checks>` required by the *Scheduler*.

There can have many pollers for load-balancing.


Receiver
--------

The *Receiver* daemon receives the :ref:`passive checks<monitoring_features/passive_checks>` and :ref:`external commands<monitoring_features/external_commands>`.

There can have many receivers for load-balancing.

Broker
------

The *Broker* daemon gets all the broks from the other daemons. It propagates the events to its specialized modules (eg. Alignak backend database storage, ...)

There can have many brokers for load-balancing.


Reactionner
-----------

The *Reactionner* daemon runs the :ref:`event handlers<monitoring_features/event_handlers>` and sends the :ref:`notifications<monitoring_features/notifications>` to the users.

There can have many reactionners for load-balancing.

