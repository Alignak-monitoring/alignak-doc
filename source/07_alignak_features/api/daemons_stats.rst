.. _alignak_features/daemons_stats:
.. Built from the test_daemons_api.py unit test last run!

==========================
Alignak daemons statistics
==========================

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
        "satellites.arbiters": 1,
        "satellites.schedulers": 1,
        "satellites.workers": 0
    },
    "load": 0.8907917912480041,
    "metrics": [],
    "modules": {
        "external": {},
        "internal": {}
    },
    "name": "receiver-master",
    "program_start": 1534237165.25945,
    "running_id": "1534237165.49084404",
    "spare": false,
    "start_time": 1534237165,
    "type": "receiver",
    "version": "2.0.0rc2"
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
        "hostgroups": 5,
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
            "last_check": 1534237171.968572,
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
            "last_check": 1534237171.9596844,
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
            "last_check": 1534237171.9512877,
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
            "last_check": 1534237171.9824684,
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
            "last_check": 1534237171.9889188,
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
        "timestamp": 1534237174
    },
    "load": 0.6959144899451211,
    "metrics": [],
    "modules": {
        "external": {},
        "internal": {}
    },
    "name": "arbiter-master",
    "program_start": 1534237163.3206577,
    "running_id": "1534237163.58716598",
    "spare": false,
    "start_time": 1534237163,
    "type": "arbiter",
    "version": "2.0.0rc2"
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
    "load": 0.8278832937496876,
    "metrics": [],
    "modules": {
        "external": {},
        "internal": {}
    },
    "name": "reactionner-master",
    "program_start": 1534237165.3795972,
    "running_id": "1534237165.85748172",
    "spare": false,
    "start_time": 1534237165,
    "type": "reactionner",
    "version": "2.0.0rc2"
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
    "load": 0.8269539405320478,
    "metrics": [],
    "modules": {
        "external": {},
        "internal": {}
    },
    "name": "broker-master",
    "program_start": 1534237165.6222887,
    "running_id": "1534237165.31324091",
    "spare": false,
    "start_time": 1534237165,
    "type": "broker",
    "version": "2.0.0rc2"
}


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
    "load": 0.8269462984091107,
    "metrics": [],
    "modules": {
        "external": {},
        "internal": {}
    },
    "name": "poller-master",
    "program_start": 1534237165.425305,
    "running_id": "1534237165.67645452",
    "spare": false,
    "start_time": 1534237165,
    "type": "poller",
    "version": "2.0.0rc2"
}


Daemon type: scheduler
----------------------

A scheduler daemon statistics example:
 ::

    {
    "_freshness": 1534237174,
    "alignak": "My Alignak",
    "counters": {
        "actions.count": 0,
        "actions.in_poller": 5,
        "actions.scheduled": 107,
        "actions.zombie": 0,
        "brokers": 1,
        "checks.count": 112,
        "checks.in_poller": 5,
        "checks.scheduled": 107,
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
    "load": 0.8928079710206434,
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
        "hostgroups": 5,
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
    "program_start": 1534237165.786159,
    "running_id": "1534237165.64076814",
    "spare": false,
    "start_time": 1534237165,
    "type": "scheduler",
    "version": "2.0.0rc2"
}
