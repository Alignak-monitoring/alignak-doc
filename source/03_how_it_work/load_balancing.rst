.. _howitworks/load_balancing:

==============
Load balancing
==============

Introduction
============

The load balancing is the ability to share load on multiple servers.

The first thing to know is that not all Alignak daemons can be managed in load availability.

This is the list of daemons for which it is possible:

    * *Scheduler* daemon
    * *Poller* daemon
    * *Broker* daemon
    * *Reactionner* daemon
    * *Receiver* daemon

Only the Arbiter is not concerned with this

Configuration
=============

Introduction
------------

We will introduce the configuration part with an example.

We set the *Scheduler* daemon in load balancing mode.

We will distribute the following daemons on three servers:

* *Arbiter* daemon is on *ServerA* with IP 192.168.0.1
* *Scheduler master* is on *ServerB* with IP 192.168.0.2
* *Scheduler spare* is on *ServerC* with IP 192.168.0.3

**Note**: it is possible to have more than 2 *Scheduler* daemons, this example is for 2.

First step
----------

We need to install Alignak on the 3 servers.

Second step
-----------

We configure the 2 *Scheduler* daemons in the *Arbiter* configuration on *ServerA*.

In the folder *etc/alignak/arbiter/daemons*:

* rename the file *scheduler-master.cfg* in *scheduler-mars.cfg*. In this file, define the IP of *ServerB* like::

    scheduler_name      scheduler-mars
    address             192.168.0.2
    spare               0

* copy this file *scheduler-mars.cfg* to *scheduler-venus.cfg* (the name is not really important). In this file, define the scheduler_name, the IP of *ServerC* like::

    scheduler_name      scheduler-venus
    address             192.168.0.3
    spare               0


Third step
----------

On *ServerB* and *ServerC*, start the *Scheduler* daemons, and only those daemons.

On *ServerA*, start the *Arbiter* daemon.

Conclusion
----------

You run now Alignak with load balancing for Scheduler.
You can do the same for other daemons.
