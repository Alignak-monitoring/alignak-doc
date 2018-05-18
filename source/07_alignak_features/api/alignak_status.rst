.. _alignak_features/alignak_status:
.. Built from the test_daemons_api.py unit test last run!

======================
Alignak overall status
======================
An Alignak overall status example:

::

    {
    "livestate": {
        "long_output": "broker-master - daemon is alive and reachable.\npoller-master - daemon is alive and reachable.\nreactionner-master - daemon is alive and reachable.\nreceiver-master - daemon is alive and reachable.\nscheduler-master - daemon is alive and reachable.",
        "output": "All my daemons are up and running.",
        "perf_data": "'contactgroups'=2 'modules'=0 'businessimpactmodulations'=0 'hostsextinfo'=0 'timeperiods'=4 'realms'=1 'servicegroups'=1 'notificationways'=2 'hostgroups'=3 'commands'=10 'contacts'=2 'receivers'=1 'servicesextinfo'=0 'hosts'=13 'escalations'=0 'servicedependencies'=40 'schedulers'=1 'brokers'=1 'checkmodulations'=0 'reactionners'=1 'arbiters'=1 'pollers'=1 'services'=100 'hostdependencies'=0 'macromodulations'=0 'resultmodulations'=0",
        "state": "up",
        "timestamp": 1529901001
    },
    "name": "My Alignak",
    "services": [
        {
            "livestate": {
                "long_output": "",
                "output": "all daemons are up and running.",
                "perf_data": "",
                "state": "ok",
                "timestamp": 1529901001
            },
            "name": "arbiter-master"
        },
        {
            "livestate": {
                "long_output": "Realm: All (True). Listening on: http://127.0.0.1:7772/",
                "name": "broker_broker-master",
                "output": "daemon is alive and reachable.",
                "perf_data": "last_check=1529900998.77",
                "state": "ok",
                "timestamp": 1529901001
            },
            "name": "broker-master"
        },
        {
            "livestate": {
                "long_output": "Realm: All (True). Listening on: http://127.0.0.1:7771/",
                "name": "poller_poller-master",
                "output": "daemon is alive and reachable.",
                "perf_data": "last_check=1529900998.76",
                "state": "ok",
                "timestamp": 1529901001
            },
            "name": "poller-master"
        },
        {
            "livestate": {
                "long_output": "Realm: All (True). Listening on: http://127.0.0.1:7769/",
                "name": "reactionner_reactionner-master",
                "output": "daemon is alive and reachable.",
                "perf_data": "last_check=1529900998.75",
                "state": "ok",
                "timestamp": 1529901001
            },
            "name": "reactionner-master"
        },
        {
            "livestate": {
                "long_output": "Realm: All (True). Listening on: http://127.0.0.1:7773/",
                "name": "receiver_receiver-master",
                "output": "daemon is alive and reachable.",
                "perf_data": "last_check=1529900998.79",
                "state": "ok",
                "timestamp": 1529901001
            },
            "name": "receiver-master"
        },
        {
            "livestate": {
                "long_output": "Realm: All (True). Listening on: http://127.0.0.1:7768/",
                "name": "scheduler_scheduler-master",
                "output": "daemon is alive and reachable.",
                "perf_data": "last_check=1529900998.79",
                "state": "ok",
                "timestamp": 1529901001
            },
            "name": "scheduler-master"
        }
    ],
    "template": {
        "_templates": [
            "alignak",
            "important"
        ],
        "active_checks_enabled": false,
        "alias": "My Alignak",
        "notes": "",
        "passive_checks_enabled": true
    },
    "variables": {}
}
