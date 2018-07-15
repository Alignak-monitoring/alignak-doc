.. _monitoring_features/hosts_checks:

============
Hosts checks 
============


Introduction 
------------

The basic workings of host checks are described here...


When are host checks performed?
-------------------------------

Hosts are checked by the Alignak daemon:

  * At regular intervals, as defined by the ``check_interval`` and ``retry_interval`` options in your host definitions.
  * On-demand when the state of a service associated with the host changes.
  * On-demand as needed as part of the :ref:`host reachability logic<monitoring_features/network_reachability>`.

Regularly scheduled host checks are optional. If you set the ``check_interval`` option in your host definition to zero (0), Alignak will not perform any periodical check of the host. It will, however, still perform on-demand checks of the host as needed for other parts of the monitoring logic.

On-demand checks are made when a service associated with the host changes state because Alignak needs to know whether the host has also changed state. Services that change state are often an indicator that the host may have also changed state. As an example, if Alignak detects that the "HTTP" service associated with a host just changed from a CRITICAL to an OK state, it may indicate that the host just recovered from a reboot and is now back up and running.

On-demand checks of hosts are also made as part of the :ref:`host reachability logic<monitoring_features/network_reachability>`. Alignak is designed to detect network outages as quickly as possible, and distinguish between DOWN and UNREACHABLE host states. These are very different states and can help an admin to quickly locate the cause of a network outage.


Cached host checks
------------------

**Not available (see #1026)!**

The performance of on-demand host checks can be significantly improved by implementing cached checks, which allow Alignak to omit executing an host check if it determines that a relatively recent check result still exists.
More information on cached checks can be found :ref:`here <alignak_features/cached_checks>`.


Dependencies and checks
-----------------------

You can define host execution dependencies that prevent Alignak from checking the status of a host depending
on the state of one or more other hosts.
More information on dependencies can be found :ref:`here <monitoring_features/dependencies>`.


Parallelization of host checks
------------------------------

All checks are run in parallel.


Host states
-----------

Hosts that are checked can be in one of three different states:

    * UP
    * DOWN
    * UNREACHABLE


Host State Determination 
------------------------

Host checks are performed by plugins. The ``check_command`` parameter of an host defines the plugin that will be used to check the host state. The plugin can have an exit code of OK (0), WARNING (1), CRITICAL (2) or UNKNOWN (3).

The table below shows how plugin return codes correspond with preliminary host states. Some post-processing (described later) is done which may then alter the final host state.

============= ======================
Plugin result Preliminary host state
OK            UP                    
WARNING       DOWN*                 
CRITICAL      DOWN
UNKNOWN       DOWN
============= ======================

If the preliminary host state is DOWN, Alignak will attempt to see if the host is really DOWN or if it is UNREACHABLE. The distinction between DOWN and UNREACHABLE host states is important, as it allows admins to determine root cause of network outages faster. The :ref:`host reachability logic<monitoring_features/network_reachability>` describe this distinction.

The following table shows how Alignak makes a final state determination based on the state of the hosts parent(s). A host's parents are defined in the parents directive in host definition.


====================== ========================================== ================
Preliminary host state Parent host state                          Final host state
DOWN                   At least one parent is UP                  DOWN            
DOWN                   All parents are either DOWN or UNREACHABLE UNREACHABLE     
====================== ========================================== ================

More information on how Alignak distinguishes between DOWN and UNREACHABLE states can be found :ref:`here <monitoring_features/network_reachability>`.


Host with no check command
--------------------------

If no ``check_command`` is defined for an host it means that no host check will be performed by Alignak. The host will always keep its ``initial_state`` as defined in its configuration. If none is defined in the configuration then the host will always be considered as UP by Alignak.

.. note:: that even a :ref:`passive check<monitoring_features/passive_checks>` received for such an host will not change its state. If you want to have an host that is never checked and always UP you must defined a ``check_command`` with one of the Alignak internal check commands.


Internal hosts check command
----------------------------

Alignak allows to define a ``check_command`` that do not require executing a plugin to make the host state change. For more information see the :ref:`internal checks documentation<alignak_features/internal_checks>`.


Host state changes
------------------

As you are probably well aware, hosts don't always stay in one state. Things break, patches get applied, and servers need to be rebooted. When Alignak checks the status of hosts, it will be able to detect when a host changes between UP, DOWN, and UNREACHABLE states and take appropriate action.

These state changes result in different state types (HARD or SOFT), which can trigger :ref:`event handlers <monitoring_features/event_handlers>` to be run, alerts to be raised and :ref:`notifications <monitoring_features/notifications>` to be sent out. Detecting and dealing with state changes is what Alignak is all about.

Soft (state type is SOFT) states occur when host checks return a non-OK (non-UP) state and are in the process of being retried. Hard states (state type is HARD) result when host checks have been checked a specified maximum number of times and the current state is confirmed.

When hosts change state too frequently they are considered to be “flapping". A good example of a flapping host would be server that keeps spontaneously rebooting as soon as the operating system loads. That's always a fun scenario to have to deal with. Alignak can detect when hosts start flapping, and can suppress notifications until flapping stops and the host's state stabilizes. More information on the flap detection logic can be found :ref:`here <monitoring_features/flapping>`.

