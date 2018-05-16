.. _alignak_features/web_service_api:

===============
Web service API
===============


Introduction
------------

All the Alignak daemons communicate thanks to a Web based API, but the main interesting services are exposed by the Alignak arbiter.


The services exposed by the Arbiter:

   * Alignak configuration reload
   * get the Alignak daemons list
   * get the Alignak overall state
   * get the Alignak daemons state
   * ...

The following chapters detail the main services.

**Note** that all the daemons implement an `/api` endpoint that returns many information about the services and their parameters.


Alignak daemons API
-------------------

 A request on the daemon ``/api`` endpoint returns an object containing the attributes:

   - *doc* for a global API information
   - *api* which is a list of the available API endpoints

 For each API endpoint object in the list, the following atributes are available:

   - *endpoint* contains the endpoint to use with the daemon URL
   - *doc* is the documentation of the endpoint
   - *args* is the list and format of the attributes to provide in the request

The exhaustive daemons API is listed :ref:`on this page <alignak_features/daemons_api>`.


Alignak configuration reload
----------------------------

POSTing on http://arbiter:7770/configuration_reload provokes a configuration reload by the arbiter. The arbiter reloads its configuration and all the daemons receive this new configuration.

POSTing on http://arbiter:7770/backend_notification with an event and some parameters may also provoke a configuration reload.

The `event` data is an event notificationparameter.
The `parameters` data is only a text string that append information to the `event`.

As of now, the Arbiter will only log the request and provoke a configuration reload if `event` is *creation* or *deletion*.


Alignak overall state
---------------------

http://arbiter:7770/get_stats returns many information about the Alignak configuration and live state.

The `load` is an approximate information about the arbiter work load.
The `live_state` is a synthesis of all the daemons state; `state` is 0 for Ok, 1 for Warning and 2 for
The `daemons_states` contains the configuration and live state of each satellite managed by the arbiter.

If the `details` parameter is used in the URL, a `monitoring_objects` field contain a list of all the monitoring objects (hosts, services, ...) including their count and names.


   As an example:
::

    {
        "name": "arbiter-master",
        "type": "arbiter",
        "alignak": "My Alignak",
        "version": "1.1.0",
        "program_start": 1522386192.083035,
        "spare": false,
        "load": 0.8697290540617235,
        "live_state": {
            "daemons": {"poller-master": 0, "reactionner-master": 0, "scheduler-master": 0, "broker-master": 0, "receiver-master": 0},
            "timestamp": 1522386203,
            "state": 0,
            "output": "all daemons are up and running."
        },
        "modules": {
            "internal": {}, "external": {}
        },
        "daemons_states": {
            "poller-master": {
                "manage_sub_realms": true, "live_state": 0, "live_output": "poller/poller-master is up and running.", "configuration_sent": true, "uri": "http://127.0.0.1:7771/", "alive": true, "realm_name": "All", "passive": false, "reachable": true, "polling_interval": 1, "active": true, "spare": false, "max_check_attempts": 3, "last_check": 1522386201.787781, "type": "poller", "name": "poller-master"
            },
            "reactionner-master": {
                "manage_sub_realms": true, "live_state": 0, "live_output": "reactionner/reactionner-master is up and running.", "configuration_sent": true, "uri": "http://127.0.0.1:7769/", "alive": true, "realm_name": "All", "passive": false, "reachable": true, "polling_interval": 1, "active": true, "spare": false, "max_check_attempts": 3, "last_check": 1522386201.781261, "type": "reactionner", "name": "reactionner-master"
            },
            "scheduler-master": {
                "manage_sub_realms": true, "live_state": 0, "live_output": "scheduler/scheduler-master is up and running.", "configuration_sent": true, "uri": "http://127.0.0.1:7768/", "alive": true, "realm_name": "All", "passive": false, "reachable": true, "polling_interval": 1, "active": true, "spare": false, "max_check_attempts": 3, "last_check": 1522386201.805775, "type": "scheduler", "name": "scheduler-master"
            },
            "broker-master": {
                "manage_sub_realms": true, "live_state": 0, "live_output": "broker/broker-master is up and running.", "configuration_sent": true, "uri": "http://127.0.0.1:7772/", "alive": true, "realm_name": "All", "passive": false, "reachable": true, "polling_interval": 1, "active": true, "spare": false, "max_check_attempts": 3, "last_check": 1522386201.794226, "type": "broker", "name": "broker-master"
            },
            "receiver-master": {
                "manage_sub_realms": true, "live_state": 0, "live_output": "receiver/receiver-master is up and running.", "configuration_sent": true, "uri": "http://127.0.0.1:7773/", "alive": true, "realm_name": "All", "passive": false, "reachable": true, "polling_interval": 1, "active": true, "spare": false, "max_check_attempts": 3, "last_check": 1522386201.800199, "type": "receiver", "name": "receiver-master"
            }
        },
        "counters": {
            "servicesextinfo": 0, "modules": 0, "hostgroups": 1, "external-commands": 0, "escalations": 0, "dispatcher.receivers": 1, "dispatcher.arbiters": 1, "schedulers": 1, "hostsextinfo": 0, "contacts": 2, "servicedependencies": 0, "resultmodulations": 0, "hosts": 1, "pollers": 1, "broks": 6, "arbiters": 1, "receivers": 1, "macromodulations": 0, "reactionners": 1, "contactgroups": 2, "brokers": 1, "realms": 1, "services": 0, "dispatcher.pollers": 1, "dispatcher.reactionners": 1, "dispatcher.schedulers": 1, "commands": 7, "notificationways": 2, "timeperiods": 4, "businessimpactmodulations": 0, "checkmodulations": 0, "dispatcher.brokers": 1, "servicegroups": 0, "hostdependencies": 0
        }
    }



Satellites list
---------------

http://arbiter:7770/get_satellites_list returns the list of each satellites grouped by daemon type. As an example:
::

    {
        "reactionner": ["reactionner-master"],
        "broker": ["broker-master"],
        "arbiter": ["arbiter-master"],
        "scheduler": ["scheduler-master"],
        "receiver": ["receiver-master"],
        "poller": ["poller-master"]
    }


Satellites configuration
------------------------

http://arbiter:7770/get_satellites_configuration returns the list of each satellites grouped by daemon type. For each satellite, all its configuration is included. As an example:
::

    {'scheduler': ['Scheduler1']},
    {'poller': ['Poller1', 'Poller2']}


Alignak daemons information
---------------------------

http://daemon:port/get_id returns information about the daemon and Alignak version:
::

    {"alignak": "My Alignak", "version": "1.1.0", "type": "arbiter", "name": "arbiter-master"}


http://daemon:port/get_start_time returns information about the daemon startup timestamp:
::

    {"alignak": "My Alignak", "version": "1.1.0", "type": "arbiter", "name": "arbiter-master", "start_time": 1522385883}

*Note* that it returns the same information as ``/get_id`` with one more start_time field

http://daemon:port/get_running_id returns information about the daemon startup timestamp:
::

    {"alignak": "My Alignak", "version": "1.1.0", "type": "arbiter", "name": "arbiter-master", "start_time": 1522385883}

*Note* that it returns the same information as ``/get_id`` with one more start_time field

http://daemon:port/get_stats returns information about the daemon and Alignak. The statistics information are depending upon the daemon type. As an example, for a reactionner daemon:
::

    {
        "load": 0.9442897365918568,
        "program_start": 1522386196.912419,
        "name": "reactionner-master",
        "alignak": "My Alignak",
        "modules": {"internal": {}, "external": {}},
        "metrics": [
            "reactionner.reactionner-master.external-commands.queue 0 1522386203"
        ],
        "version": "1.1.0",
        "spare": false,
        "type": "reactionner",
        "counters": {"broks": 0, "arbiters": 0, "schedulers": 1, "workers": 3, "modules": 0, "external-commands": 0}
    }


Alignak external commands
-------------------------

Some external commands can be notified to make Alignak change its behavior.
More information on external commands can be found :ref:`here <monitoring_features/external_commands>`.

http://daemon:port/get_id returns information about the daemon and Alignak:
::

    {"alignak": "My Alignak", "version": "1.1.0", "type": "arbiter", "name": "arbiter-master"}


