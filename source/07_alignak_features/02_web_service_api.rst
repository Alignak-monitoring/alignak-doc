.. _alignak_features/web_service_api:

===============
Web service API
===============


Introduction
------------

All the Alignak daemons communicate thanks to a Web based API, but the main interesting services are exposed by the Alignak arbiter.


The services exposed by the Arbiter:

   * force configuration reload
   * get the daemons list
   * get the Alignak instance overall state
   * get the Alignak daemons state
   * ...

The following chapters detail the main services.

.. note:: all the daemons implement a `/api` endpoint that returns many information about the exposed services and their parameters.


Alignak daemons API
-------------------

A request on the daemon ``/api`` endpoint returns an object containing the attributes:

   - *doc* for a global API information
   - *api* which is a list of the available API endpoints

 For each API endpoint object in the list, the following atributes are available:

   - *endpoint* contains the endpoint to use with the daemon URL
   - *doc* is the documentation of the endpoint
   - *args* is the list and format of the attributes to provide in the request

The exhaustive daemons API is listed :ref:`on this page <alignak_features/daemons_api>` wich is built automatically from the source code.


Alignak configuration reload
----------------------------

POSTing on the ``/configuration_reload`` endpoint provokes a configuration reload by the arbiter. The arbiter reloads its configuration and all the daemons receive this new configuration.

POSTing on the ``/backend_notification`` endpoint with an event and some parameters may also provoke a configuration reload.

The `event` data is an event notificationparameter.
The `parameters` data is only a text string that append information to the `event`.

As of now, the Arbiter will only log the request and provoke a configuration reload if `event` is *creation* or *deletion*.


Alignak overall state
---------------------

The ``/get_alignak_status`` endpoint returns many information about the overall Alignak configuration and live state.

The `load` is an approximate information about the arbiter work load.
The `livestate` is a synthesis of all the daemons state; `state` is 0 for Ok, 1 for Warning and 2 for
The `daemons_states` contains the configuration and live state of each satellite managed by the arbiter.

If the `details` parameter is used in the URL, a `monitoring_objects` field contain a list of all the monitoring objects (hosts, services, ...) including their count and names.

The exhaustive information returned by this endpoint is described :ref:`on this page <alignak_features/alignak_status>`.


Alignak detailed status
-----------------------

Each daemon implements a ``/get_stats`` endpoint that returns many information about the daemon configuration and live state.

The `load` is an approximate information about the daemon work load.
The `livestate` is a synthesis of all the daemons state; `state` is 0 for Ok, 1 for Warning and 2 for
The `daemons_states` contains the configuration and live state of each satellite managed by the arbiter.

If the `details` parameter is used in the URL, a `monitoring_objects` field contain a list of all the monitoring objects (hosts, services, ...) including their count and names.

``http://daemon:port/get_stats`` returns information about the daemon and Alignak. The statistics information are depending upon the daemon type.

As an example, for a reactionner daemon::

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
        "counters": {
            "broks": 0,
            "arbiters": 0,
            "schedulers": 1,
            "workers": 3,
            "modules": 0,
            "external-commands": 0
        }
    }


The exhaustive information returned by each daemons is described :ref:`on this page <alignak_features/daemons_stats>`.


Alignak live synthesis
----------------------

The ``/get_livesynthesis`` endpoint returns the overall Alignak live synthesis.

This will return an object containing the properties of the `get_id`, plus a `livesynthesis` object which contains 2 properties for each known scheduler:
  - _freshness, which is the timestamp when the provided data were fetched
  - livesynthesis, which is an object with the scheduler live synthesis.

An `_overall` fake scheduler is also contained in the schedulers list to provide the cumulated live synthesis. Before sending the results, the arbiter sums-up all its schedulers live synthesis counters in the `_overall` live synthesis.

As an example::

  {
      ...

      "livesynthesis": {
          "_overall": {
              "_freshness": 1528947526,
              "livesynthesis": {
                  "hosts_total": 11,
                  "hosts_not_monitored": 0,
                  "hosts_up_hard": 11,
                  "hosts_up_soft": 0,
                  "hosts_down_hard": 0,
                  "hosts_down_soft": 0,
                  "hosts_unreachable_hard": 0,
                  "hosts_unreachable_soft": 0,
                  "hosts_flapping": 0,
                  "hosts_acknowledged": 0,
                  "hosts_in_downtime": 0,
                  "services_total": 100,
                  "services_not_monitored": 0,
                  "services_ok_hard": 70,
                  "services_ok_soft": 0,
                  "services_warning_hard": 4,
                  "services_warning_soft": 6,
                  "services_critical_hard": 6,
                  "services_critical_soft": 4,
                  "services_unknown_hard": 3,
                  "services_unknown_soft": 7,
                  "services_unreachable_hard": 0,
                  "services_unreachable_soft": 0,
                  "services_flapping": 0,
                  "services_acknowledged": 0,
                  "services_in_downtime": 0
                  }
              }
          },
          "scheduler-master": {
              "_freshness": 1528947522,
              "livesynthesis": {
                  "hosts_total": 11,
                  "hosts_not_monitored": 0,
                  "hosts_up_hard": 11,
                  "hosts_up_soft": 0,
                  "hosts_down_hard": 0,
                  "hosts_down_soft": 0,
                  "hosts_unreachable_hard": 0,
                  "hosts_unreachable_soft": 0,
                  "hosts_flapping": 0,
                  "hosts_acknowledged": 0,
                  "hosts_in_downtime": 0,
                  "services_total": 100,
                  "services_not_monitored": 0,
                  "services_ok_hard": 70,
                  "services_ok_soft": 0,
                  "services_warning_hard": 4,
                  "services_warning_soft": 6,
                  "services_critical_hard": 6,
                  "services_critical_soft": 4,
                  "services_unknown_hard": 3,
                  "services_unknown_soft": 7,
                  "services_unreachable_hard": 0,
                  "services_unreachable_soft": 0,
                  "services_flapping": 0,
                  "services_acknowledged": 0,
                  "services_in_downtime": 0
                  }
              }
          }
      }
  }


Alignak known problems
----------------------

The ``/get_monitoring_problems`` endpoint returns the overall Alignak known problems list.

This will return an object containing the properties of the `get_id`, plus a `problems` object which contains 2 properties for each known scheduler:
  - _freshness, which is the timestamp when the provided data were fetched
  - problems, which is an object with the scheduler known problems

As an example::

  {
      ...

      "problems": {
          "scheduler-master": {
              "_freshness": 1528903945,
              "problems": {
                  "fdfc986d-4ab4-4562-9d2f-4346832745e6": {
                      "last_state": "CRITICAL",
                      "service": "dummy_critical",
                      "last_state_type": "SOFT",
                      "last_state_update": 1528902442,
                      "last_hard_state": "CRITICAL",
                      "last_hard_state_change": 1528902442,
                      "last_state_change": 1528902381,
                      "state": "CRITICAL",
                      "state_type": "HARD",
                      "host": "host-all-8",
                      "output": "Hi, checking host-all-8/dummy_critical -> exit=2"
                  },
                  "2445f2a3-2a3b-4b13-96ed-4cfb60790e7e": {
                      "last_state": "WARNING",
                      "service": "dummy_warning",
                      "last_state_type": "SOFT",
                      "last_state_update": 1528902463,
                      "last_hard_state": "WARNING",
                      "last_hard_state_change": 1528902463,
                      "last_state_change": 1528902400,
                      "state": "WARNING",
                      "state_type": "HARD",
                      "host": "host-all-6",
                      "output": "Hi, checking host-all-6/dummy_warning -> exit=1"
                  },
                  ...
              }
          }
      }
  }

Alignak external commands
-------------------------

Some external commands can be notified to make Alignak change its behavior. More information on external commands can be found :ref:`here <monitoring_features/external_commands>`.

POSTing on the ``/command`` endpoint allows to request the execution of an external command.

Allowed parameters are:

   - `command`: mandatory parameter containing the whole command line or only the command name

   - `timestamp`: optional parameter containing the timestamp. If not present, the current timestamp is added in the command line

   - `element`: the targeted element that will be appended after the command name (`command`). If element contains a '/' character it is split to make an host and service.

   - `host`, `service` or `user`: the targeted host, service or user. Takes precedence over the `element` to target a specific element

   - `parameters`: the parameter that will be appended after all the arguments

In case of any error, this service returns an object containing some properties:

   - '_status': 'ERR' because of the error
   - `_message`: some more explanations about the error

The `_status` field is 'OK' with an according `_message` to explain what the Arbiter will do depending upon the notification. The `command` property contains the formatted external command.


Satellites list
---------------

The ``/get_satellites_list`` returns the list of each satellites grouped by daemon type.

As an example::

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

The ``/get_satellites_configuration`` returns the list of each satellites grouped by daemon type. For each satellite, all its configuration is included.

As an example::

    {'scheduler': ['Scheduler1']},
    {'poller': ['Poller1', 'Poller2']}


Alignak daemons information
---------------------------

``http://daemon:port/get_id`` returns information about the daemon and Alignak version::

   {
      "type": "arbiter",
      "name": "arbiter-master",
      "alignak": "My Alignak",
      "version": "1.1.0"
   }


``http://daemon:port/get_start_time`` returns information about the daemon startup timestamp::


   {
      "alignak": "My Alignak", "version": "1.1.0", "type": "arbiter", "name": "arbiter-master",
      "start_time": 1522385883
   }

.. Note:: that it returns the same information as ``/get_id`` with one more `start_time` field

``http://daemon:port/get_running_id`` returns information about the daemon startup timestamp::

   {
      "alignak": "My Alignak", "version": "1.1.0", "type": "arbiter", "name": "arbiter-master",
      "running_id": 1522385883.123456
   }

.. Note:: that it returns the same information as ``/get_id`` with one more `running_id` field.

