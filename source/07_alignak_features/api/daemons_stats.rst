.. _alignak_features/daemons_stats:
.. Built from the test_daemons_api.py unit test last run!

==========================
Alignak daemons statistics
==========================

Daemon type: poller
-------------------

A poller daemon statistics example:
 ::

    {
    "alignak": "My Alignak",
    "counters": {
        "broks": 0,
        "events": 0,
        "external-commands": 0,
        "modules": 0,
        "satellites.arbiters": 0,
        "satellites.schedulers": 1,
        "satellites.workers": 2
    },
    "load": 0.8932060826392618,
    "metrics": [],
    "modules": {
        "external": {},
        "internal": {}
    },
    "name": "poller-master",
    "program_start": 1529900993.322783,
    "spare": false,
    "start_time": 1529900993,
    "type": "poller",
    "version": "1.1.0rc7"
}


Daemon type: arbiter
--------------------

A arbiter daemon statistics example:
 ::

    {
    "alignak": "My Alignak",
    "counters": {
        "arbiters": 1,
        "brokers": 1,
        "broks": 0,
        "businessimpactmodulations": 0,
        "checkmodulations": 0,
        "commands": 10,
        "contactgroups": 2,
        "contacts": 2,
        "dispatcher.arbiters": 1,
        "dispatcher.brokers": 1,
        "dispatcher.pollers": 1,
        "dispatcher.reactionners": 1,
        "dispatcher.receivers": 1,
        "dispatcher.schedulers": 1,
        "escalations": 0,
        "external-commands": 0,
        "hostdependencies": 0,
        "hostgroups": 3,
        "hosts": 13,
        "hostsextinfo": 0,
        "macromodulations": 0,
        "modules": 0,
        "notificationways": 2,
        "pollers": 1,
        "reactionners": 1,
        "realms": 1,
        "receivers": 1,
        "resultmodulations": 0,
        "schedulers": 1,
        "servicedependencies": 40,
        "servicegroups": 1,
        "services": 100,
        "servicesextinfo": 0,
        "timeperiods": 4
    },
    "daemons_states": {
        "broker-master": {
            "active": true,
            "alive": true,
            "configuration_sent": true,
            "last_check": 1529900998.7657082,
            "livestate": 0,
            "livestate_output": "broker/broker-master is up and running.",
            "manage_sub_realms": true,
            "max_check_attempts": 3,
            "name": "broker-master",
            "passive": false,
            "polling_interval": 5,
            "reachable": true,
            "realm_name": "All",
            "spare": false,
            "type": "broker",
            "uri": "http://127.0.0.1:7772/"
        },
        "poller-master": {
            "active": true,
            "alive": true,
            "configuration_sent": true,
            "last_check": 1529900998.7582424,
            "livestate": 0,
            "livestate_output": "poller/poller-master is up and running.",
            "manage_sub_realms": true,
            "max_check_attempts": 3,
            "name": "poller-master",
            "passive": false,
            "polling_interval": 5,
            "reachable": true,
            "realm_name": "All",
            "spare": false,
            "type": "poller",
            "uri": "http://127.0.0.1:7771/"
        },
        "reactionner-master": {
            "active": true,
            "alive": true,
            "configuration_sent": true,
            "last_check": 1529900998.74952,
            "livestate": 0,
            "livestate_output": "reactionner/reactionner-master is up and running.",
            "manage_sub_realms": true,
            "max_check_attempts": 3,
            "name": "reactionner-master",
            "passive": false,
            "polling_interval": 5,
            "reachable": true,
            "realm_name": "All",
            "spare": false,
            "type": "reactionner",
            "uri": "http://127.0.0.1:7769/"
        },
        "receiver-master": {
            "active": true,
            "alive": true,
            "configuration_sent": true,
            "last_check": 1529900998.786808,
            "livestate": 0,
            "livestate_output": "receiver/receiver-master is up and running.",
            "manage_sub_realms": true,
            "max_check_attempts": 3,
            "name": "receiver-master",
            "passive": false,
            "polling_interval": 5,
            "reachable": true,
            "realm_name": "All",
            "spare": false,
            "type": "receiver",
            "uri": "http://127.0.0.1:7773/"
        },
        "scheduler-master": {
            "active": true,
            "alive": true,
            "configuration_sent": true,
            "last_check": 1529900998.793909,
            "livestate": 0,
            "livestate_output": "scheduler/scheduler-master is up and running.",
            "manage_sub_realms": true,
            "max_check_attempts": 3,
            "name": "scheduler-master",
            "passive": false,
            "polling_interval": 5,
            "reachable": true,
            "realm_name": "All",
            "spare": false,
            "type": "scheduler",
            "uri": "http://127.0.0.1:7768/"
        }
    },
    "livestate": {
        "daemons": {
            "broker-master": 0,
            "poller-master": 0,
            "reactionner-master": 0,
            "receiver-master": 0,
            "scheduler-master": 0
        },
        "output": "all daemons are up and running.",
        "state": 0,
        "timestamp": 1529901001
    },
    "load": 0.7197991030075386,
    "metrics": [],
    "modules": {
        "external": {},
        "internal": {}
    },
    "name": "arbiter-master",
    "program_start": 1529900989.9218647,
    "spare": false,
    "start_time": 1529900989,
    "type": "arbiter",
    "version": "1.1.0rc7"
}


Daemon type: reactionner
------------------------

A reactionner daemon statistics example:
 ::

    {
    "alignak": "My Alignak",
    "counters": {
        "broks": 0,
        "events": 0,
        "external-commands": 0,
        "modules": 0,
        "satellites.arbiters": 0,
        "satellites.schedulers": 1,
        "satellites.workers": 1
    },
    "load": 0.8942220659216491,
    "metrics": [],
    "modules": {
        "external": {},
        "internal": {}
    },
    "name": "reactionner-master",
    "program_start": 1529900992.9968927,
    "spare": false,
    "start_time": 1529900992,
    "type": "reactionner",
    "version": "1.1.0rc7"
}


Daemon type: broker
-------------------

A broker daemon statistics example:
 ::

    {
    "alignak": "My Alignak",
    "counters": {
        "broks-arbiter": 0,
        "broks-external": 0,
        "broks-internal": 0,
        "external-commands": 0,
        "modules": 0,
        "satellites.arbiters": 1,
        "satellites.pollers": 1,
        "satellites.reactionners": 1,
        "satellites.receivers": 1,
        "satellites.schedulers": 1
    },
    "load": 0.8902093877229311,
    "metrics": [],
    "modules": {
        "external": {},
        "internal": {}
    },
    "name": "broker-master",
    "program_start": 1529900993.3720996,
    "spare": false,
    "start_time": 1529900993,
    "type": "broker",
    "version": "1.1.0rc7"
}


Daemon type: receiver
---------------------

A receiver daemon statistics example:
 ::

    {
    "alignak": "My Alignak",
    "counters": {
        "broks": 0,
        "events": 0,
        "external-commands": 0,
        "external-commands-unprocessed": 0,
        "modules": 0,
        "satellites.arbiters": 0,
        "satellites.schedulers": 1,
        "satellites.workers": 0
    },
    "load": 0.9424491016464911,
    "metrics": [],
    "modules": {
        "external": {},
        "internal": {}
    },
    "name": "receiver-master",
    "program_start": 1529900993.143938,
    "spare": false,
    "start_time": 1529900993,
    "type": "receiver",
    "version": "1.1.0rc7"
}


Daemon type: scheduler
----------------------

A scheduler daemon statistics example:
 ::

    {
    "alignak": "My Alignak",
    "counters": {
        "actions.count": 0,
        "actions.in_poller": 1,
        "actions.scheduled": 110,
        "actions.zombie": 0,
        "brokers": 1,
        "checks.count": 111,
        "checks.in_poller": 1,
        "checks.scheduled": 110,
        "checks.zombie": 0,
        "external-commands": 0,
        "modules": 0,
        "pollers": 1,
        "reactionners": 1,
        "receivers": 0,
        "satellites.arbiters": 0,
        "satellites.schedulers": 0
    },
    "latency": {
        "avg": 0.0,
        "max": 0.0,
        "min": 0.0
    },
    "livesynthesis": {
        "hosts_acknowledged": 0,
        "hosts_down_hard": 0,
        "hosts_down_soft": 0,
        "hosts_flapping": 0,
        "hosts_in_downtime": 0,
        "hosts_not_monitored": 0,
        "hosts_total": 13,
        "hosts_unreachable_hard": 0,
        "hosts_unreachable_soft": 0,
        "hosts_up_hard": 13,
        "hosts_up_soft": 0,
        "services_acknowledged": 0,
        "services_critical_hard": 0,
        "services_critical_soft": 0,
        "services_flapping": 0,
        "services_in_downtime": 0,
        "services_not_monitored": 0,
        "services_ok_hard": 100,
        "services_ok_soft": 0,
        "services_total": 100,
        "services_unknown_hard": 0,
        "services_unknown_soft": 0,
        "services_unreachable_hard": 0,
        "services_unreachable_soft": 0,
        "services_warning_hard": 0,
        "services_warning_soft": 0
    },
    "load": 0.8919040414318031,
    "metrics": [],
    "modules": {
        "external": {},
        "internal": {}
    },
    "monitored_objects": {
        "arbiters": 0,
        "brokers": 0,
        "businessimpactmodulations": 0,
        "checkmodulations": 0,
        "commands": 10,
        "contactgroups": 2,
        "contacts": 2,
        "escalations": 0,
        "hostdependencies": 0,
        "hostescalations": 0,
        "hostgroups": 3,
        "hosts": 13,
        "hostsextinfo": 0,
        "macromodulations": 0,
        "modules": 0,
        "notificationways": 2,
        "pollers": 0,
        "reactionners": 0,
        "realms": 0,
        "receivers": 0,
        "resultmodulations": 0,
        "schedulers": 0,
        "servicedependencies": 0,
        "serviceescalations": 0,
        "servicegroups": 1,
        "services": 100,
        "servicesextinfo": 0,
        "timeperiods": 4
    },
    "name": "scheduler-master",
    "program_start": 1529900993.3263967,
    "spare": false,
    "start_time": 1529900993,
    "type": "scheduler",
    "version": "1.1.0rc7"
}
