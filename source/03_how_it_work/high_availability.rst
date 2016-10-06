.. _howitworks/high_availability:

=================
High availability
=================

Introduction
============

Alignak can be used in a high availability environment.

The daemons can be run in 2 differents servers / locations.

First instance in normal mode, the second in spare mode.

Configuration
=============

Introduction
------------

We will introduce the configuration part with an example. We put *Scheduler* daemon in high
availability mode.

We have the daemons:

* *Arbiter* daemon on *ServerA* with IP 192.168.0.1
* *Scheduler master* on *ServerB* with IP 192.168.0.2
* *Scheduler spare* on *ServerC* with IP 192.168.0.3

First step 
----------

We need install Alignak on the 3 servers.

Second step
-----------

We configure the 2 *Scheduler* daemons in *Arbiter* config on *ServerA*.

In folder etc/alignak/arbiter/daemons:

* modify the file *scheduler-master.cfg*. In this file, define the IP of *ServerB* like::

    address             192.168.0.2

* copy this file *scheduler-master.cfg* to *scheduler-slave.cfg* (the name is not really important). In this file, define the scheduler_name, the IP of ServerC and activate the spare like::

    scheduler_name      scheduler-slave
    address             192.168.0.3
    spare               1


Third step
----------

If you have *Scheduler* daemon un on ServerA, you can stop it, we don't need it.

Start the *Arbiter* daemon on *ServerA*.

Fouth step
----------

On *ServerB* and *ServerC*, start only *Scheduler* daemon.

Test
----

To test if it works, it's so simple, stop *Scheduler* daemon on *ServerB*.

The *Scheduler* on ServerC will
go out of waiting state and continue the job of *Scheduler* of *ServerB*.

If you restart *Scheduler* daemon on *ServerB*, the *Scheduler* on *ServerC* will return in
waiting state.

Conclusion
----------

You run now Alignak with high availability for Scheduler.

You can do the same for other daemons.

Like you see, it's simple to define Alignak in high availibility mode.

Important
=========

Be careful, in case you configure *Arbiter* in high availability, you need to have same configuration
files (folder etc/alignak/arbiter) in both *Arbiter* servers (master and slave).
