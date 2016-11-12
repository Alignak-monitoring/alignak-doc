.. _alignak_features/dmz_monitoring:

================
Monitoring a DMZ
================


There is two ways for monitoring a DMZ network:
  * got a poller on the LAN, and launch check from it, so the firewall should allow monitoring traffic (like nrpe, snmp, etc)
  * got a poller on the DMZ, so only the Alignak communication should be opened through the firewall

If you can take the first, use it :)

If you can't because your security manager is not happy about it, you should put a poller in the DMZ.
So look at the page :ref:`distributed shinken <alignak_features/distributed>` first, because you
will need a distributed architecture.

Pollers are "dumb" things. They get their jobs from the schedulers (of their realm, if you don't
know what is it from now, it's not important). So if you just have a poller in the DMZ network
aside another one in the LAN, some checks for the dmz will only be catched by the LAN one,
and some for the lan will be catched by the DMZ one. It's not a good thing of course :)


Tag your hosts and pollers for being "in the DMZ"
=================================================

So we will need to "tag" checks, so they will be able to run **only** in the DMZ poller, or the lan one.

This tag is done with the **poller_tag** parameter. It can be applied on the following objects:
 * pollers
 * commands
 * services
 * hosts

It's quite simple: the objects have `tags`, and the pollers also have `tags`. You've got an
implicit inheritance in this order: hosts->services->commands.
If a command doesn't have a `poller_tag`, it will inherit the one from the service.
And if this service neither has one, it will inherit from its host.

You just need to install a poller with the *DMZ* tag in the DMZ and then add it to all hosts
(or services) that are in the DMZ. They will be managed by this poller and you just need to open
the port to this poller from the LAN. Your network admins will be happier :)


Configuration part
==================

So you need to declare in your poller configuration file:

::

    define poller{
        poller_name    poller-DMZ
        address        server-dmz
        port           7771
        poller_tags    DMZ
    }


And "tag" some hosts and/or some services.

::

    define host{
        host_name  server-DMZ-1
        [...]
        poller_tag DMZ
        [...]
    }


And that's all :)

All checks for the host *server-DMZ-1* will be launched from the poller *poller-dmz*, and only
from this poller (unless there is another poller in the DMZ with the same tag).

You are sure that those checks won't be launched from the pollers within the LAN,
because untagged pollers can't take tagged checks.
