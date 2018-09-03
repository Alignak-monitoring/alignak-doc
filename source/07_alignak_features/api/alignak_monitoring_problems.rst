.. _alignak_features/monitoring_problems:

.. Built from the test_daemons_api.py unit test last run!

===========================
Alignak monitoring problems
===========================

On a scheduler endpoint: ``/monitoring_problems``

::

    {
    "alignak": "My Alignak",
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
        "services_critical_hard": 7,
        "services_critical_soft": 3,
        "services_flapping": 0,
        "services_in_downtime": 0,
        "services_not_monitored": 0,
        "services_ok_hard": 70,
        "services_ok_soft": 0,
        "services_total": 100,
        "services_unknown_hard": 4,
        "services_unknown_soft": 6,
        "services_unreachable_hard": 0,
        "services_unreachable_soft": 0,
        "services_warning_hard": 6,
        "services_warning_soft": 4
    },
    "name": "scheduler-master",
    "problems": {
        "19fef53f-3b9d-4d61-818f-a7354a7c8afa": {
            "host": "host-all-5",
            "last_hard_state": "CRITICAL",
            "last_hard_state_change": 1534237740,
            "last_state": "CRITICAL",
            "last_state_change": 1534237679,
            "last_state_type": "SOFT",
            "last_state_update": 1534237740,
            "output": "Hi, checking host-all-5/dummy_critical -> exit=2",
            "service": "dummy_critical",
            "state": "CRITICAL",
            "state_type": "HARD"
        },
        "475e66d2-b53b-400a-9cfb-517456194932": {
            "host": "host-all-3",
            "last_hard_state": "CRITICAL",
            "last_hard_state_change": 1534237729,
            "last_state": "CRITICAL",
            "last_state_change": 1534237669,
            "last_state_type": "SOFT",
            "last_state_update": 1534237729,
            "output": "Hi, checking host-all-3/dummy_critical -> exit=2",
            "service": "dummy_critical",
            "state": "CRITICAL",
            "state_type": "HARD"
        },
        "48df8941-0bc0-49f6-9e8b-ab2e2c956a24": {
            "host": "host-all-5",
            "last_hard_state": "WARNING",
            "last_hard_state_change": 1534237740,
            "last_state": "WARNING",
            "last_state_change": 1534237679,
            "last_state_type": "SOFT",
            "last_state_update": 1534237740,
            "output": "Hi, checking host-all-5/dummy_warning -> exit=1",
            "service": "dummy_warning",
            "state": "WARNING",
            "state_type": "HARD"
        },
        "5146fd28-eb1b-4780-a884-da0e7ac8f678": {
            "host": "host-all-4",
            "last_hard_state": "CRITICAL",
            "last_hard_state_change": 1534237726,
            "last_state": "CRITICAL",
            "last_state_change": 1534237666,
            "last_state_type": "SOFT",
            "last_state_update": 1534237726,
            "output": "Hi, checking host-all-4/dummy_critical -> exit=2",
            "service": "dummy_critical",
            "state": "CRITICAL",
            "state_type": "HARD"
        },
        "525abed4-f9bc-4c10-9d0e-1365b37909a5": {
            "host": "host-all-9",
            "last_hard_state": "WARNING",
            "last_hard_state_change": 1534237748,
            "last_state": "WARNING",
            "last_state_change": 1534237688,
            "last_state_type": "SOFT",
            "last_state_update": 1534237748,
            "output": "Hi, checking host-all-9/dummy_warning -> exit=1",
            "service": "dummy_warning",
            "state": "WARNING",
            "state_type": "HARD"
        },
        "68438fc6-adc6-4c69-9ed1-a3b312f9a626": {
            "host": "host-all-9",
            "last_hard_state": "CRITICAL",
            "last_hard_state_change": 1534237748,
            "last_state": "CRITICAL",
            "last_state_change": 1534237688,
            "last_state_type": "SOFT",
            "last_state_update": 1534237748,
            "output": "Hi, checking host-all-9/dummy_critical -> exit=2",
            "service": "dummy_critical",
            "state": "CRITICAL",
            "state_type": "HARD"
        },
        "7f1e2f28-4617-4091-bc2d-cd9f523fd3be": {
            "host": "host-all-7",
            "last_hard_state": "WARNING",
            "last_hard_state_change": 1534237712,
            "last_state": "WARNING",
            "last_state_change": 1534237651,
            "last_state_type": "SOFT",
            "last_state_update": 1534237712,
            "output": "Hi, checking host-all-7/dummy_warning -> exit=1",
            "service": "dummy_warning",
            "state": "WARNING",
            "state_type": "HARD"
        },
        "89076df5-c886-4596-9631-9de7ec37ec1f": {
            "host": "host-all-6",
            "last_hard_state": "CRITICAL",
            "last_hard_state_change": 1534237726,
            "last_state": "CRITICAL",
            "last_state_change": 1534237666,
            "last_state_type": "SOFT",
            "last_state_update": 1534237726,
            "output": "Hi, checking host-all-6/dummy_critical -> exit=2",
            "service": "dummy_critical",
            "state": "CRITICAL",
            "state_type": "HARD"
        },
        "8bffc83b-ae6c-4e0f-8019-3ecb8ed413c0": {
            "host": "host-all-0",
            "last_hard_state": "CRITICAL",
            "last_hard_state_change": 1534237744,
            "last_state": "CRITICAL",
            "last_state_change": 1534237684,
            "last_state_type": "SOFT",
            "last_state_update": 1534237744,
            "output": "Hi, checking host-all-0/dummy_critical -> exit=2",
            "service": "dummy_critical",
            "state": "CRITICAL",
            "state_type": "HARD"
        },
        "9be4a873-b99c-4e02-8d86-8eb446ab5cd9": {
            "host": "host-all-1",
            "last_hard_state": "WARNING",
            "last_hard_state_change": 1534237734,
            "last_state": "WARNING",
            "last_state_change": 1534237674,
            "last_state_type": "SOFT",
            "last_state_update": 1534237734,
            "output": "Hi, checking host-all-1/dummy_warning -> exit=1",
            "service": "dummy_warning",
            "state": "WARNING",
            "state_type": "HARD"
        },
        "c03cd3ce-fb1a-42c6-ad85-312fd2f50cb9": {
            "host": "host-all-4",
            "last_hard_state": "WARNING",
            "last_hard_state_change": 1534237745,
            "last_state": "WARNING",
            "last_state_change": 1534237685,
            "last_state_type": "SOFT",
            "last_state_update": 1534237745,
            "output": "Hi, checking host-all-4/dummy_warning -> exit=1",
            "service": "dummy_warning",
            "state": "WARNING",
            "state_type": "HARD"
        },
        "c3ab6204-61d8-4bef-9db6-83c4b5fe7eff": {
            "host": "host-all-0",
            "last_hard_state": "WARNING",
            "last_hard_state_change": 1534237744,
            "last_state": "WARNING",
            "last_state_change": 1534237684,
            "last_state_type": "SOFT",
            "last_state_update": 1534237744,
            "output": "Hi, checking host-all-0/dummy_warning -> exit=1",
            "service": "dummy_warning",
            "state": "WARNING",
            "state_type": "HARD"
        },
        "eba3c2c0-4312-41b5-b925-4a06c52bb455": {
            "host": "host-all-1",
            "last_hard_state": "CRITICAL",
            "last_hard_state_change": 1534237734,
            "last_state": "CRITICAL",
            "last_state_change": 1534237674,
            "last_state_type": "SOFT",
            "last_state_update": 1534237734,
            "output": "Hi, checking host-all-1/dummy_critical -> exit=2",
            "service": "dummy_critical",
            "state": "CRITICAL",
            "state_type": "HARD"
        }
    },
    "running_id": "1534237617.26046306",
    "start_time": 1534237617,
    "type": "scheduler",
    "version": "2.0.0rc2"
}

On the arbiter endpoint: ``/monitoring_problems``

::

    {
    "_freshness": 1534237749,
    "alignak": "My Alignak",
    "name": "arbiter-master",
    "problems": {
        "scheduler-master": {
            "problems": {
                "19fef53f-3b9d-4d61-818f-a7354a7c8afa": {
                    "host": "host-all-5",
                    "last_hard_state": "CRITICAL",
                    "last_hard_state_change": 1534237740,
                    "last_state": "CRITICAL",
                    "last_state_change": 1534237679,
                    "last_state_type": "SOFT",
                    "last_state_update": 1534237740,
                    "output": "Hi, checking host-all-5/dummy_critical -> exit=2",
                    "service": "dummy_critical",
                    "state": "CRITICAL",
                    "state_type": "HARD"
                },
                "475e66d2-b53b-400a-9cfb-517456194932": {
                    "host": "host-all-3",
                    "last_hard_state": "CRITICAL",
                    "last_hard_state_change": 1534237729,
                    "last_state": "CRITICAL",
                    "last_state_change": 1534237669,
                    "last_state_type": "SOFT",
                    "last_state_update": 1534237729,
                    "output": "Hi, checking host-all-3/dummy_critical -> exit=2",
                    "service": "dummy_critical",
                    "state": "CRITICAL",
                    "state_type": "HARD"
                },
                "48df8941-0bc0-49f6-9e8b-ab2e2c956a24": {
                    "host": "host-all-5",
                    "last_hard_state": "WARNING",
                    "last_hard_state_change": 1534237740,
                    "last_state": "WARNING",
                    "last_state_change": 1534237679,
                    "last_state_type": "SOFT",
                    "last_state_update": 1534237740,
                    "output": "Hi, checking host-all-5/dummy_warning -> exit=1",
                    "service": "dummy_warning",
                    "state": "WARNING",
                    "state_type": "HARD"
                },
                "5146fd28-eb1b-4780-a884-da0e7ac8f678": {
                    "host": "host-all-4",
                    "last_hard_state": "CRITICAL",
                    "last_hard_state_change": 1534237726,
                    "last_state": "CRITICAL",
                    "last_state_change": 1534237666,
                    "last_state_type": "SOFT",
                    "last_state_update": 1534237726,
                    "output": "Hi, checking host-all-4/dummy_critical -> exit=2",
                    "service": "dummy_critical",
                    "state": "CRITICAL",
                    "state_type": "HARD"
                },
                "525abed4-f9bc-4c10-9d0e-1365b37909a5": {
                    "host": "host-all-9",
                    "last_hard_state": "WARNING",
                    "last_hard_state_change": 1534237748,
                    "last_state": "WARNING",
                    "last_state_change": 1534237688,
                    "last_state_type": "SOFT",
                    "last_state_update": 1534237748,
                    "output": "Hi, checking host-all-9/dummy_warning -> exit=1",
                    "service": "dummy_warning",
                    "state": "WARNING",
                    "state_type": "HARD"
                },
                "68438fc6-adc6-4c69-9ed1-a3b312f9a626": {
                    "host": "host-all-9",
                    "last_hard_state": "CRITICAL",
                    "last_hard_state_change": 1534237748,
                    "last_state": "CRITICAL",
                    "last_state_change": 1534237688,
                    "last_state_type": "SOFT",
                    "last_state_update": 1534237748,
                    "output": "Hi, checking host-all-9/dummy_critical -> exit=2",
                    "service": "dummy_critical",
                    "state": "CRITICAL",
                    "state_type": "HARD"
                },
                "7f1e2f28-4617-4091-bc2d-cd9f523fd3be": {
                    "host": "host-all-7",
                    "last_hard_state": "WARNING",
                    "last_hard_state_change": 1534237712,
                    "last_state": "WARNING",
                    "last_state_change": 1534237651,
                    "last_state_type": "SOFT",
                    "last_state_update": 1534237712,
                    "output": "Hi, checking host-all-7/dummy_warning -> exit=1",
                    "service": "dummy_warning",
                    "state": "WARNING",
                    "state_type": "HARD"
                },
                "89076df5-c886-4596-9631-9de7ec37ec1f": {
                    "host": "host-all-6",
                    "last_hard_state": "CRITICAL",
                    "last_hard_state_change": 1534237726,
                    "last_state": "CRITICAL",
                    "last_state_change": 1534237666,
                    "last_state_type": "SOFT",
                    "last_state_update": 1534237726,
                    "output": "Hi, checking host-all-6/dummy_critical -> exit=2",
                    "service": "dummy_critical",
                    "state": "CRITICAL",
                    "state_type": "HARD"
                },
                "8bffc83b-ae6c-4e0f-8019-3ecb8ed413c0": {
                    "host": "host-all-0",
                    "last_hard_state": "CRITICAL",
                    "last_hard_state_change": 1534237744,
                    "last_state": "CRITICAL",
                    "last_state_change": 1534237684,
                    "last_state_type": "SOFT",
                    "last_state_update": 1534237744,
                    "output": "Hi, checking host-all-0/dummy_critical -> exit=2",
                    "service": "dummy_critical",
                    "state": "CRITICAL",
                    "state_type": "HARD"
                },
                "9be4a873-b99c-4e02-8d86-8eb446ab5cd9": {
                    "host": "host-all-1",
                    "last_hard_state": "WARNING",
                    "last_hard_state_change": 1534237734,
                    "last_state": "WARNING",
                    "last_state_change": 1534237674,
                    "last_state_type": "SOFT",
                    "last_state_update": 1534237734,
                    "output": "Hi, checking host-all-1/dummy_warning -> exit=1",
                    "service": "dummy_warning",
                    "state": "WARNING",
                    "state_type": "HARD"
                },
                "c03cd3ce-fb1a-42c6-ad85-312fd2f50cb9": {
                    "host": "host-all-4",
                    "last_hard_state": "WARNING",
                    "last_hard_state_change": 1534237745,
                    "last_state": "WARNING",
                    "last_state_change": 1534237685,
                    "last_state_type": "SOFT",
                    "last_state_update": 1534237745,
                    "output": "Hi, checking host-all-4/dummy_warning -> exit=1",
                    "service": "dummy_warning",
                    "state": "WARNING",
                    "state_type": "HARD"
                },
                "c3ab6204-61d8-4bef-9db6-83c4b5fe7eff": {
                    "host": "host-all-0",
                    "last_hard_state": "WARNING",
                    "last_hard_state_change": 1534237744,
                    "last_state": "WARNING",
                    "last_state_change": 1534237684,
                    "last_state_type": "SOFT",
                    "last_state_update": 1534237744,
                    "output": "Hi, checking host-all-0/dummy_warning -> exit=1",
                    "service": "dummy_warning",
                    "state": "WARNING",
                    "state_type": "HARD"
                },
                "eba3c2c0-4312-41b5-b925-4a06c52bb455": {
                    "host": "host-all-1",
                    "last_hard_state": "CRITICAL",
                    "last_hard_state_change": 1534237734,
                    "last_state": "CRITICAL",
                    "last_state_change": 1534237674,
                    "last_state_type": "SOFT",
                    "last_state_update": 1534237734,
                    "output": "Hi, checking host-all-1/dummy_critical -> exit=2",
                    "service": "dummy_critical",
                    "state": "CRITICAL",
                    "state_type": "HARD"
                }
            }
        }
    },
    "running_id": "1534237614.73657398",
    "start_time": 1534237614,
    "type": "arbiter",
    "version": "2.0.0rc2"
}

