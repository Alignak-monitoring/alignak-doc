.. _alignak_features/distributed:

======================
Distributed Monitoring
======================


Introduction
============

Alignak can be configured for distributed monitoring of network services and resources. It is designed to make it easy and really smart.


Distributing the monitoring environment allows to offload the overhead (CPU usage, etc.) due to the
performing and receiving service checks from a "central" server onto one or more "distributed" servers.

Most small to medium sized monitorer infrastructure will not have a real need for setting up such an environment.

However, when you want to start monitoring thousands of hosts (and several times that many services) using Alignak, this becomes quite important.


The global architecture
=======================

Alignak's architecture has been designed according to the Unix Way: one tool, one task!

Alignak has an architecture where each part is isolated and connects to the others via standard interfaces. Alignak is based on the a HTTP backend.

This makes building a highly available or distributed monitoring architecture quite easy.

Major innovations of Alignak over monolithic solutions such as Nagios are:

    * splitting the different roles into separate daemons
    * allowing the use of modules to extend and enrich the native features of the Alignak daemons

Alignak core uses **distributed** programming, meaning a daemon will often do remote invocations of
code on other daemons. This means that to ensure maximum compatibility and stability, the core
language, paths and module versions **must** be the same everywhere a daemon is running.


.. _alignak_features/distributed#daemons_roles:

Alignak Daemon roles
====================

    * **Arbiter**:
       The arbiter daemon parses the monitoring configuration, divides it into parts (N schedulers = N parts),
       and distributes the parts to the appropriate other daemons.

       Additionally, it manages the high availability features: if a particular daemon dies, it forwards the
       configuration managed by this failed daemon to the configured spare.

       Finally, it may collect external data (external commands or passive check results) and routes them to the appropriate daemon.

       There can only be one active Arbiter, other arbiters acting as hot standby spares in the architecture.


    * **Scheduler**:
       The scheduler daemon manages the dispatching of checks and actions to the pollers and reactionners daemons.

       It is also responsible for processing the check result queue, analyzing the results, doing correlation and following up
       actions accordingly (if a service is down, ask for a host check).

       It does not launch checks or notifications but it maintains a queue of pending checks and notifications for the other daemons
       (like pollers or reactionners). This allows distributing the checking load equally across many pollers.

       There can be many schedulers for load-balancing or hot standby roles.


    * **Poller**:
       The poller daemon launches check plugins as required by schedulers.
       When the check is finished it returns the result to its scheduler.
       Pollers can be tagged for specialized checks (ex. Windows versus Unix, customer A versus customer B, DMZ).

       There can be many pollers for load-balancing or hot standby spare roles.


    * **Reactionner**:
       The reactionner daemon raises notifications and launches event_handlers.
       It centralizes communication channels with external systems in order to simplify SMTP authorizations
       or RSS feed sources (only one for all hosts/services).

       There can be many reactionners for load-balancing and spare roles


    * **Broker**:
       The broker daemon exports and manages data from schedulers. The management is done exclusively with modules.


    * **Receiver** (optional):
       The receiver daemon receives passive check data and serves as a distributed passive command buffer that will
       be read by the arbiter or scheduler daemon.

       There can be many receivers for load-balancing and hot standby spare roles.

       The receiver uses external modules to accept data from different protocols (NSCA, ...).

This architecture is fully flexible and scalable: the daemons that require the more performance are the pollers and schedulers.
The administrator can add as many of them as he wants.

A picture is worth a thousand words:


.. image:: /_static/images///official/images/alignak-architecture.png
   :scale: 90 %


.. _alignak_features/distributed#load_balancing:

The smart and automatic load balancing
======================================

Alignak is able to cut the user monitoring configuration into parts and to dispatch those parts to the schedulers.
The load balancing is done automatically: the administrator does not need to remember which host is linked with
another one to create packs, Alignak deals with this by itself.

The dispatch is a host-based one: that means that all services of a host will be in the same scheduler as this host.
The major advantage of Alignak is the ability to create independent configurations: an element of a configuration will
not have to call an element of another pack. That means that the administrator does not need to know all the relations
between the elements like parents, hostdependencies or service dependencies.

Alignak is able to manage all the relations and keep together all the related elements into the same packs.

This action is done in two parts:

  * create independent packs of elements
  * paste packs to create N configurations for the N schedulers


Creating independent packs
--------------------------

The splitting action is done by looking at two elements: hosts and services.
Services are linked with their host so they will be in the same pack.

Other relations are taken into account :

  * parent relationship for hosts (like a distant server and its router)
  * hostdependencies
  * servicesdependencies

Alignak looks at all these relations and creates a graph with it; this graph is a relation pack.



.. image:: /_static/images///official/images/pack-creation.png
   :scale: 90 %

The packs aggregations into scheduler configurations
----------------------------------------------------


When all relation packs are created, the Arbiter aggregates them into N configurations if the administrator
has defined N active schedulers (no spare ones). Packs are aggregated into configurations (it's like "Big packs").
The dispatch looks at the weight property of schedulers: the higher weight a scheduler has, the more packs it will have.

The configurations sending to satellites
----------------------------------------

Once all the packs are created, the Arbiter sends them to the N active Schedulers.
A Scheduler can start processing checks once it has received and loaded it's configuration without having to wait
for all schedulers to be ready.

For larger configurations, having more than one Scheduler (even on a single server) is highly recommended, as they will
load their configurations (new or updated) faster.

The Arbiter also creates configurations for satellites (pollers, reactionners and brokers) with links to the schedulers they are
related with so they know where to get their jobs.

After having dispatched the configuration, the Arbiter is continuously monitoring the availability of all the satellites.


.. _alignak_features/distributed#high_availability:

High availability
=================


Nobody is perfect. A server can crash, an application too!

That's why administrators have spares: they replace failing elements and reassign them.

All Alignak daemons can have their spare daemon. As the Arbiter is continuously monitoring the availability of all
the satellites, if a daemon dies, the Arbiter sends its configuration to a spare daemon, as defined by the administrator.
All satellites are informed about this modification so they can get their jobs from the new element and do not try to reach the dead one.

If a node was lost due to a network interruption and it comes back up, the Arbiter will notice its resurrection and it
will require that it drops its current configuration to provide a new one.

The availability parameters can be modified from the default settings when using larger configurations as
the Schedulers or Brokers can become busy and delay their availability responses. The timers are aggressive by default
for smaller installations.


.. _alignak_features/distributed#external_commands:

External commands dispatching
=============================

The administrator needs to send orders to their monitoring framework.

In the Alignak way of thinking, the orders are received by one daemon (the receiver) that will then dispatch them to the others.

External commands can be divided into two types :

   * global commands that impact the whole monitoring framework
   * element specific commands that target one element (host, service, contact).

For each command, Alignak determines if it is global or not. For a global command, it just sends orders to all schedulers.
For element specific ones, it searches which scheduler manages the element referred by the command (host/service)
and sends the order to this specific scheduler. When the orders are received by the schedulers they just need to apply them.


.. _alignak_features/distributed#poller_tag:

Different types of Pollers: poller_tag
======================================

The current Alignak architecture is useful for someone that uses the same type of poller for checks.
But it can be useful to have different types of pollers, like GNU/Linux ones and Windows ones.

This is useful when the user needs to have hosts in the same scheduler (like with dependencies) but needs some
hosts or services to be checked by specific pollers (see usage cases below).

These checks can in fact be tagged on 3 levels :

  * Host
  * Service
  * Command

The parameter to tag a command, host or service, is `poller_tag`. If a check uses a "tagged" or "untagged"
command in a untagged host/service, it takes the poller_tag of this host/service.

In a "untagged" host/service, it's the command tag that is taken into account.

There is an implicit inheritance in this order: hosts->services->commands.
If a command doesn't have a `poller_tag`, it will inherit the one from the service.
And if this service neither has one, it will inherit from its host.

The pollers can be tagged with several tags. If they are tagged, they will only get and execute checks that
are tagged with the same tags as theirs, and not the untagged ones, unless they defined the tag "None".


This feature is useful in two cases:

   * separated pollers for different kind of checks: GNU/Linux and Windows
   * DMZ hosts checking

In the first case, it can be useful to have a Windows host in a domain with a poller daemon running under a domain account.
If this poller launches WMI queries, the user can have an easy Windows monitoring.

The second case is a classic one: when you have a DMZ network, you need to have a dedicated poller in the DMZ,
which returns results to a scheduler in the main LAN. With this configuration , you can still have dependencies between DMZ
hosts and LAN hosts, and still be sure that checks are done in a DMZ-only poller.


.. _alignak_features/distributed#reactionner_tag:

Different types of Reactionners: reactionner_tag
================================================

Like for the pollers, reactionners can also have 'tags'. So you can tag your host/service or commands with `reactionner_tag`.
If a notification or an event handler uses a "tagged" or "untagged" command in a untagged host/service, it
takes the `reactionner_tag` of this host/service. In a "untaged" host/service, it's the command tag that is taken into account.

The reactionners can be tagged with several tags. If they are tagged, they will only take commands that are tagged,
not the untagged ones, unless they defined the tag "None".

Like for the poller case, it's mainly useful for DMZ/LAN or GNU/Linux/Windows cases.


.. _alignak_features/distributed#realms:

Advanced architectures: Realms
==============================

Alignak architecture allows the administrator to have a unique point of administration with numerous
schedulers, pollers, reactionners, brokers and receivers.

Hosts are dispatched with their own services to schedulers. The satellites daemons (pollers/reactionners/brokers)
get and execute jobs from their schedulers. Everyone is happy! Or almost everyone...

Think about an administrator who has a distributed architecture around the world.
With the current Alignak architecture the administrator can have a couple scheduler/poller daemons in Europe
and another one set in Asia, but he cannot "tag" hosts in Asia to be checked by the asian scheduler.
Trying to check an asian server with an european scheduler can be very sub-optimal, read very sloooow.

The hosts are dispatched to all schedulers and satellites so the administrator cannot be sure that asian hosts
will be checked by the asian monitoring servers.

Alignak provides a way to manage different geographic or organizational sites.

We will use a generic term for this site management, **Realms**.


Realms in few words
-------------------

A realm is a pool of resources (scheduler, poller, reactionner, broker and receiver) that hosts or hostgroups
can be attached to.

The following rule apply:

   * A host can be attached to **one and only one** realm.

   * A hostgroup can be attached to **one and only one** realm; all the hosts in the group will be *de facto* in the same realm.

   * All "dependencies" or parents of an host must be in the same realm as the related host

   * A realm can be set as the default one, and all *"unrealmed"* hosts will be attached to the default realm.

   * If no default realm exists in the configuration, Alignak will create one

   * In a realm, pollers, reactionners and brokers will only get jobs from schedulers of the same realm.


Realms are not poller_tags!
---------------------------

Make sure to undestand when to use realms and when to use poller_tags.

For some cases poller_tag functionality could also be done using Realms.
The question you need to ask yourself: is a poller_tag "enough", or do you need to fully segregate the
scheduler level and use Realms. In realms, schedulers do not communicate with schedulers from other Realms.

If you just need a poller in a DMZ network, use poller_tag.

If you need a scheduler/poller in a customer LAN, use realms.


Sub realms
----------

A realm can contain another realm. It does not change anything for schedulers: they are only responsible for
hosts of their realm not the ones of the sub realms.

The realm tree is useful for satellites like reactionners or brokers: they can get jobs from the schedulers of
their realm, but also from schedulers of sub realms.

Pollers can also get jobs from sub realms, but it's less useful so it's disabled by default.

**Warning:** having more than one broker with a scheduler is not a good idea. The jobs for brokers can be taken by only one broker!

 For the Arbiter it does not change anything: there is still only one Arbiter and one configuration whatever realms you have.


Example of realm usage
----------------------

Let's take a look at two distributed environnements.
In the first case the administrator wants totally distinct daemons.
In the second one he just wants the schedulers/pollers to be distincts, but still have one place to send
notifications (reactionners) and one place for database export (broker).

Distincts realms :


.. image:: /_static/images///official/images/alignak-architecture-isolated-realms.png
   :scale: 90 %


More common usage, the global realm with reactionner/broker, and sub realms with schedulers/pollers :


.. image:: /_static/images///official/images/alignak-architecture-global-realm.png
   :scale: 90 %


Realms configuration
--------------------

Here is the configuration for the shared architecture:

::

   ; One main default realm with 3 sub-realms
   define realm {
      realm_name       All
      realm_members    Europe,US,Asia
      default          1
   }

   ; The Europe realm with a sub-realm
   define realm{
      realm_name       Europe
      realm_members    Paris
   }


An now the satellites:

::

   ; A scheduler that will only manage Paris hosts
   define scheduler{
      scheduler_name       scheduler_Paris
      realm                Paris
   }

   ; A reactionner for all the schedulers (All realm + sub-realms)
   define reactionner{
      reactionner_name     reactionner-master
      realm                All
   }

And in host/hostgroup definition:

::

   ; A server in the Paris realm
   define host{
      host_name            server-paris
      realm                Paris
      [...]
   }

   ; All the hosts in this group will be in the realm Europe
   define hostgroups{
      hostgroup_name       linux-servers
      alias			       Linux Servers
      members			   srv1,srv2
      realm                Europe
   }


Multi levels brokers
~~~~~~~~~~~~~~~~~~~~

In the previous samples, if you put numerous brokers into the realm, each scheduler will have only one
broker at the same time. It is also impossible to have a common Broker in All, and one brokers in each sub-realms.

You can activate multi-brokers features with a realm parameter, the broker_complete_links option (0 by default).

You will have to enable this option in ALL your realms! For example:

::

   define realm{
      realm_name       Europe
      broker_complete_links  1
   }

This will enable the fact that each scehduler will be linked with each brokers.
This will make it possible to have dedicated brokers in a same realm; each one for its specific stuff.

It will also make it possible to have a common Broker in "All", and one broker in each of its sub-realms (Europe, US and Asia).

Of course the sub-brokers will only see the data from their realms, and the sub-realms (like Paris for Europe for example).
