.. _howitworks/daemons:

==================================
Daemons and how they work together
==================================

Daemons
=======

Alignak framework is designed to set up easily and smartly a distributed monitoring application for network services and resources.

It is made of 6 daemons wich features may be extended thanks to modules. Each daemon type has its own role in the monitoring process.


A picture says a thousand words:

.. image:: /_static/images///official/images/alignak-architecture.png
   :scale: 90 %



Arbiter
-------

The *Arbiter* daemon has several features:

    * Loading the configuration:
        - monitoring objects configuration (hosts, services, contacts, ...)
        - other daemons list and configuration (address, port, spare, modules...)

    * Dispatching the whole framework configuration to the other daemons

    * Managing daemons connections and monitoring the state of the other daemons

    * Forwarding failed daemons configuration to spare daemons

There can have only one active Arbiter, other arbiters acting as standby spares.

Scheduler
---------

The *Scheduler* daemon:

    * schedules the checks to run

    * determines action to execute (notifications, acknowledges, ...)

    * dispatches the checks and actions to execute to the pollers and reactionners

It is connected to the other daemons:

    * Arbiter
    * Poller
    * Receiver
    * Broker
    * Reactionner

There can have many schedulers for load-balancing or standby spares.

Poller
------

The *Poller* runs active checks required by the *Scheduler*.

It is connected to the other daemons:

    * Arbiter
    * Scheduler
    * Broker

There can have many pollers for load-balancing or standby spares.

Receiver
--------

The *Receiver* daemon receives the passive checks an external commands

It is connected to the other daemons:

    * Arbiter
    * Scheduler
    * Broker

There can have many receivers for load-balancing or standby spares.

Broker
------

The *Broker* daemon gets all events from scheduler

It is connected to the other daemons:

    * Arbiter
    * Poller
    * Scheduler
    * Receiver
    * Reactionner

There can have many brokers for load-balancing or standby spares.

Reactionner
-----------

The *Reactionner* daemon sends the notifications to the users

It is connected to the other daemons:

    * Arbiter
    * Scheduler
    * Broker

There can have many reactionners for load-balancing or standby spares.
