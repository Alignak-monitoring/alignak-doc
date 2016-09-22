.. _howitworks/load_balancing:

==============
Load balancing
==============

Introduction
============

The load balancing is the ability to share load on multiple servers.

The first thing  to know is that not all Alignak daemons can be managed in load availability.

This ia the list of daemons it's possible:

* *Scheduler* daemon
* *Poller* daemon
* *Broker* daemon
* *Reactionner* daemon
* *Receiver* daemon

Configuration
=============

Introduction
------------

We will introduce the configuration part with an example. We put *Scheduler* daemon in load
balancing mode.

We have the daemons:

* *Arbiter* daemon on *ServerA* with IP 192.168.0.1
* *Scheduler mars* on *ServerB* with IP 192.168.0.2
* *Scheduler venus* on *ServerC* with IP 192.168.0.3

So, it's possible to have more than 2 *Scheduler* daemons, this example is for 2.

First step
----------

We need install Alignak on the 3 servers.

Second step
-----------

We configure the 2 *Scheduler* daemons in *Arbiter* config on *ServerA*.

In folder etc/alignak/arbiter/daemons:

* rename the file *scheduler-master.cfg* in *scheduler-mars.cfg*. In this file, define the IP of *ServerB* like::

    scheduler_name      scheduler-mars
    address             192.168.0.2
    spare               0

* copy this file *scheduler-mars.cfg* to *scheduler-venus.cfg* (the name is not really important). In this file, define the scheduler_name, the IP of ServerC like::

    scheduler_name      scheduler-venus
    address             192.168.0.3
    spare               0


Third step
----------

On *serverA* start *Arbiter* but not *Scheduler*.

On *ServerB* and *ServerC*, start only *Scheduler* daemon.

Conclusion
----------

You run now Alignak with load balancing for Scheduler.
You can do the same for other daemons.
