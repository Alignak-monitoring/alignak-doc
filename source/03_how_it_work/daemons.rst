.. _howitworks/daemons:

===================================
Daemons and how they works together
===================================

Daemons
=======

Alignak is composed by 6 daemons


Arbiter
-------

The *Arbiter* daemon has many features:

* Load the configuration. In this word configuration, we have:
  * objects configuration
  * daemons list and configuration of them (address, port, spare, modules...)
* Manage connections with other daemons
* Dispatch the configuration to other daemons

Scheduler
---------

The *Scheduler* is used to manage check to run, period when need to run, acknowledge, downtimes...

It is connected to:

* Arbiter
* Poller
* Receiver
* Broker
* Reactionner

Poller
------

The *Poller* run active checks ordered by *Scheduler*.

It is connected to:

* Arbiter
* Scheduler
* Broker

Receiver
--------

The *Receiver* receive the passive checks

It is connected to:

* Arbiter
* Scheduler
* Broker

Broker
------

The *Broker* get all events from scheduler

It is connected to:

* Arbiter
* Poller
* Scheduler
* Receiver
* Reactionner


Reactionner
-----------

The *Reactionner* send notifications to users

It is connected to:

* Arbiter
* Scheduler
* Broker
