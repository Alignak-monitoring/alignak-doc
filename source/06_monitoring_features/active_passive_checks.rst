.. _monitoring_features/active_passive_checks:

====================
Alignak checks logic
====================

Alignak is capable of monitoring hosts and services in two ways: actively and passively. Active checks are described :ref:`in this chapter<monitoring_features/passive_checks>` and passive checks are described :ref:`in this chapter <monitoring_features/passive_checks>`.

Active checks are the most common method for monitoring hosts and services. Active checks are initiated by the Alignak framework to "poll" a device on a regularly scheduled basis. In most cases you'll use Alignak to monitor your hosts and services within this checking strategy.

Alignak also supports a way to monitor hosts and services passively instead of actively. Passive checks are initiated and performed by external applications/processes and then submitted to Alignak for its processing.

The major difference between active and passive checks is that active checks are initiated and performed by Alignak, while passive checks are performed by external applications.


.. _monitoring_features/active_checks:

Active Checks
-------------


How are active checks performed?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. image:: /_static/images///official/images/activechecks.png
   :scale: 90 %

Active checks are initiated by the check logic in the Alignak daemon. When Alignak needs to check the status of a host or service it will execute a plugin and pass it information about what needs to be checked. The plugin will then check the operational state of the host or service and report the results back to the Alignak daemon. Alignak will process the results of the host or service check and take appropriate action as necessary (eg. send notifications, run event handlers, etc).


When are active checks executed?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Active check are executed:

    * At regular intervals, as defined by the ``check_interval`` and ``retry_interval`` options in your host and service definitions
    * On-demand as needed

Regularly scheduled checks occur at intervals equaling either the ``check_interval`` or the ``retry_interval`` in your host or service definitions, depending on which type of state the host or service is in. If a host or service is in a HARD state, it will be actively checked at intervals equal to the ``check_interval`` option. If it is in a SOFT state, it will be checked at intervals equal to the ``retry_interval`` option.

On-demand checks are performed whenever Alignak needs to obtain the latest status information about a particular host or service. For example, when Alignak is determining the :ref:`reachability <monitoring_features/network_reachability>` of an host, it will often perform on-demand checks for parent and child hosts to accurately determine the status of a particular network segment.

.. _monitoring_features/passive_checks:

Passive checks
--------------


Using passive checks?
~~~~~~~~~~~~~~~~~~~~~

Passive checks are useful for monitoring services that are:

    * Asynchronous in nature, they cannot or would not be monitored effectively by polling their status on a regularly scheduled basis
    * Located behind a firewall and cannot be checked actively from the monitoring host

Some examples of asynchronous services that are commonly monitored passively:

    * "SNMP" traps and security alerts. You never know how many (if any) traps or alerts you'll receive in a given time frame, so it's not possible to just monitor their status every few minutes.
    * Aggregated checks from a host running an agent. Checks may be run at much lower intervals on hosts running an agent.
    * Submitting check results that happen directly within an application without using an intermediate log file (syslog, event log, etc.).
    * Hosts monitored from outside their own network. Passive checks do not need to create firewall rules for active monitoring protocols

Passive checks are also used when configuring :ref:`distributed or redundant<alignak_features/distributed>` monitoring installations.


How passive checks works?
~~~~~~~~~~~~~~~~~~~~~~~~~

.. image:: /_static/images///official/images/passivechecks.png
   :scale: 90 %


Here's how passive checks work in more detail:

    * An external application checks the status of an host or service.
    * The external application notifies the result of the check to Alignak with an external command.
    * Alignak gets the external command and places the result of all passive checks into a queue for processing by the Alignak framework.
    * Alignak will execute a each second and scan the check result queue.

Each service check result is processed in the same manner - regardless of whether the check was active or passive. Alignak may send out notifications, log alerts, etc. depending on the check result information.

The processing of active and passive check results is essentially identical. This allows for seamless integration of status information from external applications with Alignak.


Enabling Passive Checks
~~~~~~~~~~~~~~~~~~~~~~~

In order to enable passive checks in Alignak, you'll need to do the following:

  * Set ``accept_passive_service_checks`` directive in the monitoring configuration file.
  * Set the ``passive_checks_enabled`` directive in your host and service definitions.

If you want to disable processing of passive checks on a global basis, set the ``accept_passive_service_checks`` directive to 0.

If you would like to disable passive checks for just a few hosts or services, set the ``passive_checks_enabled`` directive in the host and/or service definitions to 0.


Submitting Passive Check Results to Alignak
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. image:: /_static/images///official/images/nsca.png
   :scale: 90 %


Submitting passive checks to Alignak implies to send an :ref:`external command<monitoring_features/external_commands>` containing the passive check result. The most common solution to submit passive checks are:

    * use a dedicated protocol such as NSCA
    * use an external commands capable module

The :ref:`NSCA collector module <modules/nsca>` collects the passive checks sent by the *send_nsca*  command or from an NSCA agent (eg. Windows NSClient ++).

The external commands capable modules are described in the :ref:`following chapter<monitoring_features/external_commands>`.


Submitting Passive Service Check Results
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

External applications can submit passive service check results to Alignak by notifying a **PROCESS_SERVICE_CHECK_RESULT**
:ref:`external command<monitoring_features/network_reachability>`.

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


Submitting Passive Host Check Results
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

External applications can submit passive host check results to Alignak by notifying a **PROCESS_HOST_CHECK_RESULT**
:ref:`external command<monitoring_features/network_reachability>`.

The format of the command is as follows: ``[<timestamp>]PROCESS_HOST_CHECK_RESULT;<host_name>;<configobjects/host_status>;<plugin_output>``
where:

  * ``timestamp`` is the time in time_t format (seconds since the UNIX epoch) that the host check was performed (or submitted). Please note the single space after the right bracket.
  * ``host_name`` is the short name of the host (as defined in the host definition)
  * ``host_status`` is the status of the host (0=UP, 1=DOWN, 2=UNREACHABLE)
  * ``plugin_output`` is the text output of the host check

.. note :: The ``plugin_output`` can also contain some performance data. To include performance data you simply
           need to include a ``|`` and the perf_data string after the ``plugin_output``.

A host must be defined in Alignak before you can submit passive check results for it! Alignak will ignore all passive check results for undefined hosts unless you set the ``accept_passive_unknown_check_results`` option in the monitoring configuration file.

Once data has been received by the Arbiter process, either directly or through a Receiver daemon, it will forward the check results to the appropriate Scheduler to apply check logic.


Passive Checks and Host States
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Unlike with active host checks, Alignak does not (by default) attempt to determine whether or host is DOWN or UNREACHABLE with passive checks. Rather, Alignak takes the passive check result to be the actual state the host is in and doesn't try to determine the hosts' actual state using the :ref:`reachability logic <monitoring_features/network_reachability>`. This can cause problems if you are submitting passive checks from a remote host or you have a :ref:`distributed monitoring setup <alignak_features/distributed>` where the parent/child host relationships are different.

You can tell Alignak to translate DOWN/UNREACHABLE passive check result states to their "proper" state by using the ``translate_passive_host_checks`` variable.

Passive host checks are normally treated as HARD states, unless the ``passive_host_checks_are_soft`` option is set.
