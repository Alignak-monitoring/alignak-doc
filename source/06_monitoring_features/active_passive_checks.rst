.. _monitoring_features/active_passive_checks:

====================
Alignak checks logic
====================

Alignak is capable of monitoring hosts and services in two ways: actively and passively. Active checks are described :ref:`in this chapter<monitoring_features/passive_checks>` and passive checks are described :ref:`in this chapter <monitoring_features/passive_checks>`.

Active checks are the most common method for monitoring hosts and services. Active checks are initiated by the Alignak framework to "poll" a device on a regularly scheduled basis. In most cases you'll use Alignak to monitor your hosts and services with this checking strategy.

Alignak also supports a way to monitor hosts and services passively instead of actively. Passive checks are initiated and performed by external applications/processes and then submitted to Alignak for its processing.

The major difference between active and passive checks is that active checks are initiated and performed by Alignak, while passive checks are performed by external applications.


.. _monitoring_features/active_checks:

Active Checks
=============


How are active checks performed?
--------------------------------

.. image:: /_static/images///official/images/activechecks.png
   :scale: 90 %

Active checks are initiated by the check logic in the Alignak daemon. When Alignak needs to check the status of a host or service it will execute a plugin and pass it information about what needs to be checked. The plugin will then check the operational state of the host or service and report the results back to the Alignak daemon. Alignak will process the results of the host or service check and take appropriate action as necessary (eg. send notifications, run event handlers, etc).


When are active checks executed?
--------------------------------

Active check are executed:

    * At regular intervals, as defined by the ``check_interval`` and ``retry_interval`` options in your host and service definitions
    * On-demand as needed

Regularly scheduled checks occur at intervals equaling either the ``check_interval`` or the ``retry_interval`` in your host or service definitions, depending on which type of state the host or service is in. If a host or service is in a HARD state, it will be actively checked at intervals equal to the ``check_interval`` option. If it is in a SOFT state, it will be checked at intervals equal to the ``retry_interval`` option.

On-demand checks are performed whenever Alignak needs to obtain the latest status information about a particular host or service. For example, when Alignak is determining the :ref:`reachability <monitoring_features/network_reachability>` of an host, it will often perform on-demand checks for parent and child hosts to accurately determine the status of a particular network segment.



.. _monitoring_features/passive_checks:

Passive checks
==============


Using passive checks?
---------------------

Passive checks are useful for monitoring services that are:

    * Asynchronous in essence, they cannot or would not be monitored effectively by polling their status on a regularly scheduled basis
    * Located behind a firewall and cannot be checked actively from the monitoring framework

Some examples of asynchronous services that are commonly monitored passively:

    * "SNMP" traps and security alerts. You never know how many (if any) traps or alerts you'll receive in a given time frame, so it's not possible to just monitor their status every few minutes.
    * Aggregated checks from a host running an agent. Checks may be run at much lower intervals on hosts running an agent.
    * Submitting check results that happen directly within an application without using an intermediate log file (syslog, event log, etc.).
    * Hosts monitored from outside their own network. Passive checks do not need specific firewall rules for active monitoring protocols


How passive checks works?
-------------------------

.. image:: /_static/images///official/images/passivechecks.png
   :scale: 90 %


Here's how passive checks work in more detail:

    * An external application checks the status of an host or service.
    * The external application notifies the result of the check to Alignak with an external command.
    * Alignak gets the external command and places the result of all passive checks into a queue for processing by the Alignak framework.
    * Alignak will execute a poll each second and scan the check result queue.

Each service check result is processed in the same manner - regardless of whether the check was active or passive. Alignak may send out notifications, log alerts, etc. depending on the check result information.

Passive checks are conditioned by another parameter: the freshness of the check. What if an external application does not raise any check since a long time? And what if a passively checked host does not give some news since several hours? Alignak allows to define a freshness threshold to make some decision about what is to be done in this situation.

When the freshness threshold is reached, Alignak sets the host or service in its defined freshness state and runs the appropriate actions according to this new state (eg. notifications, event handlers,...).

The processing of active and passive check results is essentially identical. This allows for seamless integration of status information from external applications with Alignak.

.. note :: When the freshness threshold is reached, Nagios will run the ``check_command``. Alignak do not implement such a behavior!It simply makes the host/service go to its defined ``freshness_state`` and executes the according actions if any...


More about passive checks
-------------------------

Enabling passive checks and freshness threshold
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In order to enable passive checks in Alignak, you'll need to do the following:

  * Set the global ``accept_passive_service_checks`` (default=1) directive in the monitoring configuration file.
  * Set the ``passive_checks_enabled`` directive in your host and service definitions.

If you want to disable processing of passive checks on a global basis, set the ``accept_passive_service_checks`` directive to 0.

If you would like to disable passive checks for just a few hosts or services, set the ``passive_checks_enabled`` directive in the host and/or service definitions to 0.

The freshness threshold management is set with those parameters:

  * Set the global ``check_host_freshness`` (default=1) directive in the monitoring configuration file.
  * Set the global ``check_service_freshness`` (default=1) directive in the monitoring configuration file.

  * Set the global ``host_freshness_check_interval`` (default=3600) directive in the monitoring configuration file.
  * Set the global ``service_freshness_check_interval`` (default=3600) directive in the monitoring configuration file.

  * Set the global ``additional_freshness_latency`` (default=15) directive in the monitoring configuration file.

.. note :: The additional freshness latency is an extra duration (in seconds) added to the freshness threshold.

For each host/service, you can set the following parameters:
  * Set the ``check_freshness`` (default=0) directive in your host and service definitions.
  * Set the ``freshness_threshold`` (default=3600) directive in your host and service definitions.



Submitting passive check results to Alignak
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. image:: /_static/images///official/images/nsca.png
   :scale: 90 %


Submitting passive checks to Alignak implies to send an :ref:`external command <monitoring_features/external_commands>` containing the passive check result. The most common solution to submit passive checks are:

    * use a dedicated protocol such as NSCA
    * use an external commands capable module

The :ref:`NSCA collector module <modules/nsca>` collects the passive checks sent by the *send_nsca*  command or from an NSCA agent (eg. Windows NSClient ++).

The external commands capable modules are described in the :ref:`following chapter<monitoring_features/external_commands>`.


External applications can submit passive service check results to Alignak by notifying a **PROCESS_SERVICE_CHECK_RESULT** external command.

The format of the command is as follows: ``[<timestamp>] PROCESS_SERVICE_CHECK_RESULT;<host_name>;<svc_description>;<return_code>;<plugin_output>``
where:

   * ``timestamp`` is the time in time_t format (seconds since the UNIX epoch) that the service check was performed (or submitted).
   * ``host_name`` is the short name of the host associated with the service in the service definition
   * ``svc_description`` is the description of the service as specified in the service definition
   * ``return_code`` is the return code of the check (0=OK, 1=WARNING, 2=CRITICAL, 3=UNKNOWN)
   * ``plugin_output`` is the text output of the service check (i.e. the plugin output)

.. note :: The ``plugin_output`` can also contain some performance data. To include performance data you simply
           need to include a ``|`` and the perf_data string after the ``plugin_output``.

A service must be defined in Alignak before Alignak will accept passive check results for it! Alignak will ignore all check results for undefined services unless you set the ``accept_passive_unknown_check_results`` option in the monitoring configuration file.

Once data has been received by Alignak, the check results will be forwarded to the appropriate Scheduler which will apply :ref:`the check logic<monitoring_features/checks_results>`.


External applications can submit passive host check results to Alignak by notifying a **PROCESS_HOST_CHECK_RESULT** external command.

The format of the command is as follows: ``[<timestamp>]PROCESS_HOST_CHECK_RESULT;<host_name>;<configobjects/host_status>;<plugin_output>``
where:

  * ``timestamp`` is the time in time_t format (seconds since the UNIX epoch) that the host check was performed (or submitted). Please note the single space after the right bracket.
  * ``host_name`` is the short name of the host (as defined in the host definition)
  * ``host_status`` is the status of the host (0=UP, 1=DOWN, 2=UNREACHABLE)
  * ``plugin_output`` is the text output of the host check

.. note :: The ``plugin_output`` can also contain some performance data. To include performance data you simply
           need to include a ``|`` and the `perf_data` string after the ``plugin_output``.

A host must be defined in Alignak before you can submit passive check results for it! Alignak will ignore all passive check results for undefined hosts unless you set the ``accept_passive_unknown_check_results`` option in the monitoring configuration file.

Once data has been received by Alignak, the check results will be forwarded to the appropriate Scheduler which will apply :ref:`the check logic<monitoring_features/checks_results>`.


Passive Checks and Host States
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Unlike with active host checks, Alignak does not attempt to determine whether an host is DOWN or UNREACHABLE with passive checks. Rather, Alignak takes the passive check result to be the actual state the host is in and doesn't try to determine the hosts' actual state using the :ref:`reachability logic <monitoring_features/network_reachability>`.

You can tell Alignak to translate DOWN/UNREACHABLE passive check result states to their "proper" state by using the ``translate_passive_host_checks`` variable.

Passive host checks are normally treated as HARD states, unless the ``passive_host_checks_are_soft`` option is set.


.. _monitoring_features/checks_results:

Checks results
==============

.. note :: This chapter may seem quite *esoteric* for some of the readers but it uses an algorithm-like style to describe what's Alignak doing when it gets a check result. This may help understanding the framework behavior ;)

what does Alignak do when it gets a check result? Here are the steps of the check result processing:

    * if check status is not 0 and some dependencies exist, wait the result of dependent checks

    * get the check data: execution time, output, ...

    * modulate the check status if some check modulation is defined

    * set real item state according to plugin check status and impacts management

    * manage the check status, if all dependencies are down, set item as unreachable

    * manage the new state:

        - to UP/OK from UP/OK/PENDING:

            unacknowledge former problem

            if state type SOFT and not last state PENDING
                if max attempts and SOFT state
                    HARD state
                else
                    SOFT state
            else
                state type HARD
                attempt 1

        - to UP/OK from WARNING/CRITICAL/UNKNOWN/UNREACHABLE/DOWN (other states)

            unacknowledge former problem

            if state type SOFT
                if no dependents
                    attempt++

                raise alert
                raise event handler

                state type HARD ********
                attempt 1

            else if state type HARD

                raise alert
                remove in progress notifications
                create RECOVERY notification
                attempt 1
                I am no more a problem

        - to UP/OK for a **volatile host/service**

                state type HARD
                attempt 1
                raise alert
                check for flexible downtime
                remove in progress notifications
                create PROBLEM notification
                raise event handler
                set myself as a problem

        - to not UP/OK from OK/UP

            if max attempts
                state type HARD
                raise alert
                check for flexible downtime
                remove in progress notifications
                create PROBLEM notification
                raise event handler
                set myself as a problem
            else
                state type SOFT
                attempt 1
                raise alert
                raise event handler

        - to not UP/OK from non OK/UP

            if state type SOFT
                if no dependents
                    attempt++

                if last state not state
                    unacknowledged if not sticky

                if max attempts
                    state type HARD
                    raise alert
                    raise event handler
                    check for flexible downtime
                    remove in progress notifications
                    create PROBLEM notification
                    set myself as a problem
                else
                    raise alert
                    raise event handler

                attempt 1
            else
                if last state not state
                    if not unreachable check
                        unacknowledged if not sticky
                        raise alert
                        remove in progress notifications
                        create PROBLEM notification
                        raise event handler
                        NO ******************: check for flexible downtime

                else if in scheduled downtime during last check
                    remove in progress notifications
                    create PROBLEM notification

                set myself as a problem
                remove in progress notifications
                create PROBLEM notification

    * update last hard state change if hard state changed
    * update event/problem counter
    * execute triggers
    * obsessive processor (?)
    * performance data commands
    * execute snapshots
