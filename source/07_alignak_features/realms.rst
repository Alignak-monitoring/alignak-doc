.. _alignak_features/realms:

======
Realms
======


Introduction
------------

Alignak architecture allows the administrator to have a unique point of administration with numerous schedulers, pollers, reactionners, brokers and receivers.

Hosts are dispatched with their own services to schedulers. The satellites daemons (pollers/reactionners/brokers) get and execute jobs from their schedulers. Everyone is happy! Or almost everyone...

Think about an administrator who has a distributed architecture around the world. With the current Alignak architecture the administrator can have a couple scheduler/poller daemons in Europe and another one set in Asia, but he cannot "tag" hosts in Asia to be checked by the asian scheduler. Trying to check an asian server with an european scheduler can be very sub-optimal, read very sloooow.

The hosts are dispatched to all schedulers and satellites so the administrator cannot be sure that asian hosts will be checked by the asian monitoring servers.

Alignak provides a way to manage different geographic or organizational sites.

We will use a generic term for this site management, **Realms**.


Realms in few words
-------------------

A realm is a pool of resources (scheduler, poller, reactionner, broker and receiver) that hosts or hostgroups can be attached to.

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

For some cases ``poller_tag`` functionality could also be done using Realms. The question you need to ask yourself: is a poller_tag "enough", or do you need to fully segregate the scheduler level and use Realms. In realms, schedulers do not communicate with schedulers from other Realms.

If you just need a poller in a DMZ network, use poller_tag.

If you need a scheduler/poller in a customer LAN, use realms.


Sub realms
----------

A realm can contain another realm. It does not change anything for schedulers: they are only responsible for hosts of their realm not the ones of the sub realms.

The realm tree is useful for satellites like reactionners or brokers: they can get jobs from the schedulers of their realm, but also from schedulers of sub realms.

Pollers can also get jobs from sub realms, but it's less useful so it's disabled by default.

.. Warning:: having more than one broker with a scheduler is not a good idea. The jobs for brokers can be taken by only one broker!

For the Arbiter it does not change anything: there is still only one Arbiter and one configuration whatever realms you have.


Example of realm usage
----------------------

Let's take a look at two distributed environnements.

In the first case the administrator wants totally distinct daemons.

In the second one he just wants the schedulers/pollers to be distincts, but still have one place to send notifications (reactionners) and one place for database export (broker).

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

This will enable the fact that each scehduler will be linked with each brokers. This will make it possible to have dedicated brokers in a same realm; each one for its specific stuff.

It will also make it possible to have a common Broker in "All", and one broker in each of its sub-realms (Europe, US and Asia).

Of course the sub-brokers will only see the data from their realms, and the sub-realms (like Paris for Europe for example).
