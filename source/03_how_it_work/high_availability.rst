.. _howitworks/high_availability:

=================
High availability
=================

Introduction
============

Alignak can be used in a high availability environment.

The daemons can be run in 2 different servers / locations.

First instance in normal mode, the second in spare mode.

Configuration
=============

Introduction
------------

We will introduce the configuration part with an example.

We set the *Scheduler* daemon in high availability mode.

We will distribute the following daemons on three servers:

* *Arbiter* daemon is on *ServerA* with IP 192.168.0.1
* *Scheduler master* is on *ServerB* with IP 192.168.0.2
* *Scheduler spare* is on *ServerC* with IP 192.168.0.3

First step 
----------

We need to install Alignak on the 3 servers.

Second step
-----------

We configure the 2 *Scheduler* daemons in the *Arbiter* configuration on *ServerA*.

In the folder *etc/alignak/arbiter/daemons*:

* modify the file *scheduler-master.cfg*. In this file, define the IP of *ServerB* like::

    scheduler_name      scheduler-master
    address             192.168.0.2
    spare               0

* copy this file *scheduler-master.cfg* to *scheduler-slave.cfg* (the name is not really important). In this file, define the scheduler_name, the IP of *ServerC* and activate the spare like::

    scheduler_name      scheduler-slave
    address             192.168.0.3
    spare               1


Third step
----------

On *ServerB* and *ServerC*, start the *Scheduler* daemons, and only those daemons.

On *ServerA*, start the *Arbiter* daemon.

Test
----

To test if it works, it's so simple, stop *Scheduler* daemon on *ServerB*.

The *Scheduler* on ServerC will exit from its waiting state and continue the job of the *Scheduler* of *ServerB*.

If you restart the *Scheduler* daemon on the *ServerB*, the *Scheduler* on *ServerC* will enter its waiting state.

Conclusion
----------

You run now Alignak with high availability for Scheduler.

You can do the same for other daemons.

Like you see, it's simple to define Alignak in high availability mode.

Important
=========

Be careful, if you configure *Arbiter* in high availability, you need to have the same configuration
files (folder *etc/alignak/arbiter*) on both *Arbiter* servers (master and slave).
