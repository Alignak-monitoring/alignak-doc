.. _statistics/statistics:

==================
Alignak Statistics
==================

**NOTE**: this list needs to be updated according to some recent modifications in the scheduler daemon statistics. There are much more thant the counters listed hereunder...

Alignak daemons statistics dictionary:

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

* all daemons:
    - core.hook.%s (timer) - duration spent in each hook function provided by a module

* arbiter:
    - core.hook.get_objects (timer) - duration spent in the get_objects hook function provided by a module
    - core.check-alive (timer) - duration to check that Alignak daemons are alive
    - core.check-dispatch (timer) - duration to check that the configuration is correctly dispatched
    - core.dispatch (timer) - duration to dispatch the configuration to the daemons
    - core.check-bad-dispatch (timer) - duration to confirm that the configuration is correctly dispatched
    - core.push-external-commands (timer) - duration to push the external commands to the schedulers

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
