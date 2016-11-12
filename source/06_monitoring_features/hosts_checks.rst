.. _monitoring_features/host_checks:

============
Hosts checks 
============


Introduction 
============

The basic workings of host checks are described here...


When Are Host Checks Performed? 
===============================

Hosts are checked by the Alignak daemon:

  * At regular intervals, as defined by the ``check_interval`` and ``retry_interval`` options in your host definitions.
  * On-demand when the state of a service associated with the host changes.
  * On-demand as needed as part of the :ref:`host reachability logic<monitoring_features/network_reachability>`.

Regularly scheduled host checks are optional. If you set the check_interval option in your host definition to zero (0),
Alignak will not perform checks of the hosts on a regular basis.
It will, however, still perform on-demand checks of the host as needed for other parts of the monitoring logic.

On-demand checks are made when a service associated with the host changes state because Alignak needs to
know whether the host has also changed state. Services that change state are often an indicator that the host
may have also changed state. As an example, if Alignak detects that the "HTTP" service associated with a host
just changed from a CRITICAL to an OK state, it may indicate that the host just recovered from a reboot
and is now back up and running.

On-demand checks of hosts are also made as part of the :ref:`host reachability logic<monitoring_features/network_reachability>`.
Alignak is designed to detect network outages as quickly as possible, and distinguish between DOWN and UNREACHABLE host states.
These are very different states and can help an admin quickly locate the cause of a network outage.


Cached Host Checks 
==================

The performance of on-demand host checks can be significantly improved by implementing the use of cached checks,
which allow Alignak to forgo executing a host check if it determines a relatively recent check result will do instead.
More information on cached checks can be found :ref:`here <alignak_features/cached_checks>`.


Dependencies and Checks 
=======================

You can define host execution dependencies that prevent Alignak from checking the status of a host depending
on the state of one or more other hosts.
More information on dependencies can be found :ref:`here <monitoring_features/dependencies>`.


Parallelization of Host Checks 
==============================

All checks are run in parallel.


Host States 
===========

Hosts that are checked can be in one of three different states:

  * UP
  * DOWN
  * UNREACHABLE


Host State Determination 
========================

Host checks are performed by plugins, which can return a state of OK (0), WARNING (1), CRITICAL (2) or UNKNOWN (3).
Let's see how Alignak translates these plugin return codes into host states of UP, DOWN, or UNREACHABLE? Lets see...

The table below shows how plugin return codes correspond with preliminary host states. Some post-processing
(described later) is done which may then alter the final host state.


============= ======================
Plugin Result Preliminary Host State
OK            UP                    
WARNING       DOWN*                 
CRITICAL      DOWN
UNKNOWN       DOWN
============= ======================

If the preliminary host state is DOWN, Alignak will attempt to see if the host is really DOWN or if it is
UNREACHABLE. The distinction between DOWN and UNREACHABLE host states is important, as it allows admins to
determine root cause of network outages faster. The :ref:`host reachability logic<monitoring_features/network_reachability>` defines

The following table shows how Alignak makes a final state determination based on the state of the hosts parent(s).
A host's parents are defined in the parents directive in host definition.


====================== ========================================== ================
Preliminary Host State Parent Host State                          Final Host State
DOWN                   At least one parent is UP                  DOWN            
DOWN                   All parents are either DOWN or UNREACHABLE UNREACHABLE     
====================== ========================================== ================

More information on how Alignak distinguishes between DOWN and UNREACHABLE states can be found :ref:`here <monitoring_features/network_reachability>`.


Host State Changes 
==================

As you are probably well aware, hosts don't always stay in one state. Things break, patches get applied, and
servers need to be rebooted. When Alignak checks the status of hosts, it will be able to detect when a host
changes between UP, DOWN, and UNREACHABLE states and take appropriate action.

These state changes result in different state types (HARD or SOFT), which can trigger :ref:`event handlers <monitoring_features/event_handlers>`
to be run and :ref:`notifications <monitoring_features/notifications>`
to be sent out. Detecting and dealing with state changes is what Alignak is all about.

When hosts change state too frequently they are considered to be “flapping". A good example of a flapping host
would be server that keeps spontaneously rebooting as soon as the operating system loads. That's always a fun
scenario to have to deal with. Alignak can detect when hosts start flapping, and can suppress notifications until
flapping stops and the host's state stabilizes. More information on the flap detection logic can be found :ref:`here <monitoring_features/flapping>`.

