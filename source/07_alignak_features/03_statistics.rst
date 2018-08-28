.. _alignak_features/statistics:

==================
Alignak Statistics
==================

Getting monitoring statistics from Alignak
------------------------------------------

Alignak daemons have an :ref:`HTTP JSON API <alignak_features/web_service_api>` that allows to get information about the daemons status. Especially, the arbiter daemon has an endpoint providing many useful data to be aware of the global Alignak framework status.

.. note:: This part of the documentation is still to be improved. Many information exist that are not yet enough documented !

Thanks to `collectd <https://collectd.org/>`_, some metrics can be easily collected and provided to a graphite database. Then a smart `Grafana <https://grafana.com/>`_ dashboard allows to have a nice interface to monitor your Alignak instance :)

.. tip:: all the necessary information to implement this feature on your configuration is available in the project repository *contrib/collectd* directory.

Screen captures:

.. image:: /_static/images/grafana-alignak-collectd-1.png

.. image:: /_static/images/grafana-alignak-collectd-2.png


Getting inner metrics from Alignak
----------------------------------

Alignak daemons are logging some internal metrics that may be notified to a StatsD or Graphite server. These metrics are mainly intended to know everything about the internal Alignak depths... most of them are intended to expert eyes but the provided information are a bit explained thanks to smart Grafana dashboards.

To activate the metrics notification to a StatsD or Graphite server, see :ref:`this part of the Alignak configuration <configuration/core#statsd>`.

Some :ref:`snvironment variables <howitworks/environment>` allow to configure the statistics sending.

Grafana
~~~~~~~

Thanks to `collectd <https://collectd.org/>`_, some metrics can be easily collected and provided to a graphite database. Then a smart `Grafana <https://grafana.com/>`_ dashboard allows to have a nice interface to monitor your Alignak instance :)

.. tip:: all the necessary information to implement this feature on your configuration is available in the project repository *contrib/grafana* directory.

Statistics dictionary
~~~~~~~~~~~~~~~~~~~~~

**NOTE**: this list needs to be updated according to some recent modifications in the scheduler daemon statistics. There are much more than the counters listed here under...

Alignak daemons statistics dictionary:

* all daemons:
    - loop-count - current loop index count
    - loop-turn (timer) - duration spent in the loop processing of the daemon
    - run-duration - duration spent since the daemon start
    - sleep-time (timer) - time slept during the current loop

    - hooks:
        - tick (timer)
        - hook_name.module_name (timer)

* arbiter:
    - configuration reading:
        - configuration.loading (timer) - duration spent to load the configuration
        - configuration.spliting (timer) - duration spent to load and split the configuration
        - configuration.available (timer) - duration spent before the configuration is fully available for dispatch

    - configuration dispatch:
        - prepare-dispatch (timer) - duration to check the configuration dispatching
        - dispatch (timer) - duration to dispatch the configuration to the daemons
        - check-dispatch (timer) - duration to check that the configuration is correctly
    dispatched

        - check-alive (timer) - duration to check that Alignak daemons are alive
        - check-status (timer) - duration to get Alignak daemons status

    - hooks:
        - read-configuration (timer): all the modules having a configuration to provide

    - events:
        - events (counter)
        - broks.added (counter)
        - broks.pushed.count (counter)
        - broks.pushed.time (timer)
        - broks.get-initial (timer): get initial broks from the satellites
        - external-commands.added (counter)

    - get-objects-from-queues (timer): time to get objects from our external modules





* scheduler:
    - configured objects count (gauge)
        - configuration.hosts
        - configuration.services
        - configuration.hostgroups
        - configuration.servicegroups
        - configuration.contacts
        - configuration.contactgroups
        - configuration.timeperiods
        - configuration.commands
        - configuration.notificationways
        - configuration.escalations

    - retention objects count (gauge)
        - retention.hosts
        - retention.services

    - scheduler load (gauge) - seems to be buggy :(
        - load.sleep
        - load.average
        - load.load

    - scheduler checks (gauge)
        - checks.total
        - checks.scheduled
        - checks.inpoller
        - checks.zombie
        - actions.notifications

    - first_scheduling (timer) - for the first scheduling on scheduler start

    - push_actions_to_passives_satellites (timer) - duration to push actions to passive satellites

    - get_actions_from_passives_satellites (timer) - duration to get results from passive satellites

    - loop.whole (timer) - for the scheduler complete loop

    - loop.%s (timer) -  for each scheduler recurrent work in the scheduling loop, where %s can be:

        - update_downtimes_and_comments
        - schedule
        - check_freshness
        - consume_results
        - get_new_actions
        - scatter_master_notifications
        - get_new_broks
        - delete_zombie_checks
        - delete_zombie_actions
        - clean_caches
        - update_retention_file
        - check_orphaned
        - get_and_register_update_program_status_brok
        - check_for_system_time_change
        - manage_internal_checks
        - clean_queues
        - update_business_values
        - reset_topology_change_flags
        - check_for_expire_acknowledge
        - send_broks_to_modules
        - get_objects_from_from_queues
        - get_latency_average_percentile

* satellite (poller, reactionner):
    - con-init.scheduler (timer) - scheduler connection duration
    - core.get-new-actions (timer) - duration to get the new actions to execute from the scheduler
    - core.manage-returns (timer) - duration to send back to the scheduler the results of executed actions
    - core.worker-%s.queue-size (gauge) - size of the actions queue for each satellite worker
    - core.wait-ratio (timer) - time waiting for launched actions to finish
    - core.wait-arbiter (timer) - time waiting for arbiter configuration

* arbiter:
    - core.hook.get_objects (timer) - duration spent in the get_objects hook function provided by a module

* receiver:
    - external-commands.pushed (gauge) - number of external commands pushed to schedulers
    - core.get-objects-from-queues (timer) - duration to get the objects from modules queues
    - core.push-external-commands (timer) - duration to push the external commands to the schedulers

* broker:
    - con-init.%s (timer) - for the %s daemon connection duration
    - get-new-broks.%s (timer) - duration to get new broks from other daemons, where %s can be: arbiter, scheduler, poller, reactionner, receiver or broker
    - core.put-to-external-queue (timer) - duration to send broks to external modules
    - core.put-to-external-queue.%s (timer) - duration to send broks to each external module, where %s is the external module alias
    - core.manage-broks (timer) - duration to manage broks with internal modules
    - core.manage-broks.%s (timer) - duration to manage broks with each internal module, where %s is the internal module alias
