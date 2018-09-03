.. _alignak_features/dmz_monitoring:

================
Monitoring a DMZ
================


There are two ways for monitoring a DMZ network:
    * have a poller on the LAN, and launch check from it, so the firewall should allow monitoring traffic (like NRPE, SNMP, etc.)
    * have a poller into the DMZ, so only the Alignak communication should be opened through the firewall

If you can use the first solution, it is the most simple one, use it :)

If you can't (because of security concerns), use the second one and set a poller into the DMZ.

Pollers are "dumb" things. They get their jobs from the schedulers. So if you just have a poller in the DMZ network aside another one in the LAN, some checks for the DMZ hosts will be caught by the LAN one, and some for the LAN hosts will be caught by the DMZ one.


Tag your hosts and pollers for being "in the DMZ"
-------------------------------------------------

Alignak allows to dedicate some checks to a specific poller. We will "tag" checks, so they will be able to run **only** in a specific poller.

This is done with the **poller_tag** parameter that can be applied on the following objects:
    * pollers
    * commands
    * services
    * hosts

It's quite simple: the monitoring objects are tagged, and the pollers are also tagged. When the tags match, the poller and the objects are compatible each others...

There is an implicit inheritance in this order: hosts->services->commands. If a command doesn't have a ``poller_tag``, it will inherit from the service ones. And if this service neither has a ``poller_tag``, it will inherit from those of its host.

You just need to install a poller with the *DMZ* tag in the DMZ and then tag the hosts (or services) that are in the DMZ with the same tag. Tagged hosts/services will have their checks run by the tagged poller. You simply have to open the DMZ poller communication from the LAN to allow scheduler / poller communication.

You are sure that the tagged checks won't be launched from other pollers, because untagged pollers can't get tagged checks.


Configuration part
------------------

You need to tag the poller in its configuration file:

::

    define poller{
        poller_name    poller-DMZ
        address        server-dmz
        port           7771
        poller_tags    DMZ
    }


And then tag some hosts and/or some services.

::

    define host{
        host_name  server-DMZ-1
        [...]
        poller_tag DMZ
        [...]
    }



All checks for the host *server-DMZ-1* will be launched from the poller *poller-dmz*, and only from this poller.
