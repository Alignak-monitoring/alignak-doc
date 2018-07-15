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
        "perf_data": "'servicegroups'=1 'notificationways'=2 'contacts'=2 'hosts'=13 'realms'=1 'services'=100 'timeperiods'=4 'arbiters'=1 'hostdependencies'=0 'schedulers'=1 'servicesextinfo'=0 'modules'=0 'pollers'=1 'servicedependencies'=40 'hostgroups'=5 'resultmodulations'=0 'hostsextinfo'=0 'escalations'=0 'commands'=10 'checkmodulations'=0 'businessimpactmodulations'=0 'brokers'=1 'macromodulations'=0 'reactionners'=1 'receivers'=1 'contactgroups'=2",
        "state": "up",
        "timestamp": 1534237174
    },
    "name": "My Alignak",
    "services": [
        {
            "livestate": {
                "long_output": "",
                "output": "all daemons are up and running.",
                "perf_data": "",
                "state": "ok",
                "timestamp": 1534237174
            },
            "name": "arbiter-master"
        },
        {
            "livestate": {
                "long_output": "Realm: All (True). Listening on: http://127.0.0.1:7772/",
                "name": "broker_broker-master",
                "output": "daemon is alive and reachable.",
                "perf_data": "last_check=1534237171.97",
                "state": "ok",
                "timestamp": 1534237174
            },
            "name": "broker-master"
        },
        {
            "livestate": {
                "long_output": "Realm: All (True). Listening on: http://127.0.0.1:7771/",
                "name": "poller_poller-master",
                "output": "daemon is alive and reachable.",
                "perf_data": "last_check=1534237171.96",
                "state": "ok",
                "timestamp": 1534237174
            },
            "name": "poller-master"
        },
        {
            "livestate": {
                "long_output": "Realm: All (True). Listening on: http://127.0.0.1:7769/",
                "name": "reactionner_reactionner-master",
                "output": "daemon is alive and reachable.",
                "perf_data": "last_check=1534237171.95",
                "state": "ok",
                "timestamp": 1534237174
            },
            "name": "reactionner-master"
        },
        {
            "livestate": {
                "long_output": "Realm: All (True). Listening on: http://127.0.0.1:7773/",
                "name": "receiver_receiver-master",
                "output": "daemon is alive and reachable.",
                "perf_data": "last_check=1534237171.98",
                "state": "ok",
                "timestamp": 1534237174
            },
            "name": "receiver-master"
        },
        {
            "livestate": {
                "long_output": "Realm: All (True). Listening on: http://127.0.0.1:7768/",
                "name": "scheduler_scheduler-master",
                "output": "daemon is alive and reachable.",
                "perf_data": "last_check=1534237171.99",
                "state": "ok",
                "timestamp": 1534237174
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
