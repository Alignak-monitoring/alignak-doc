.. _alignak_features/web_service_api:

===============
Web service API
===============


Introduction
------------

All the Alignak daemons communicate thanks to a Web based API, but the main interesting services are exposed by the Alignak arbiter.


The services exposed by the Arbiter:

  * get Alignak status information
  * get the list of the currently known problems
  * get the alignak events log
  * force configuration reload
  * get the daemons list
  * get the Alignak instance overall state
  * get the Alignak daemons state
  * ...

The following chapters detail the main services.

.. note:: all the daemons implement a `/api` endpoint that returns many information about the exposed services and their parameters.


Alignak daemons API
-------------------

Each daemon has its own address:port it listens to. As per defaut, the configuration is:

  - scheduler: http://127.0.0.1:7768
  - reactionner: http://127.0.0.1:7769
  - arbiter: http://127.0.0.1:7770
  - poller: http://127.0.0.1:7771
  - broker: http://127.0.0.1:7772
  - receiver: http://127.0.0.1:7773

A request on the daemon ``/`` endpoint returns the daemon identity in a JSON object.

A request on the daemon ``/api`` endpoint returns an object containing some information about the daemon available services:

   - *doc* for a global API information
   - *api* which is a list of the available API endpoints

 For each API endpoint object in the list, the following atributes are available:

   - *endpoint* contains the endpoint to use with the daemon URL
   - *doc* is the documentation of the endpoint
   - *args* is the list and format of the attributes to provide in the request

The exhaustive daemons API is listed :ref:`on this page <alignak_features/daemons_api>` which is built automatically from the source code.


Alignak overall state
---------------------

The ``/status`` endpoint returns information about the overall Alignak status.

The ``livestate`` is a synthesis of all the daemons state; ``state`` is *up* if every thing is ok. The ``output`` and ``long_output`` contain information about the arbiter satellites status.
The ``services`` contains the same live state information for each satellite involved in the Alignak configuration.

If the `details` parameter is used in the URL, a `monitoring_objects` field contain a list of all the monitoring objects (hosts, services, ...) including their count and names.

The exhaustive information returned by this endpoint is described :ref:`on this page <alignak_features/alignak_status>`.


Alignak detailed status
-----------------------

Each daemon implements a ``/stats`` endpoint that returns many information about the daemon configuration and live state.

The `load` is an approximate information about the daemon work load.
The `livestate` is a synthesis of all the daemons state; `state` is 0 for Ok, 1 for Warning and 2 for
The `daemons_states` contains the configuration and live state of each satellite managed by the arbiter.

If the `details` parameter is used in the URL, a `monitoring_objects` field contain a list of all the monitoring objects (hosts, services, ...) including their count and names.

``http://daemon:port/stats`` returns information about the daemon and Alignak. The statistics information are depending upon the daemon type.

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

The ``/livesynthesis`` endpoint returns the overall Alignak live synthesis.

This will return an object containing the properties of the `get_id`, plus a `livesynthesis` object which contains 2 properties for each known scheduler:
  - _freshness, which is the timestamp when the provided data were fetched
  - livesynthesis, which is an object with the scheduler live synthesis.

An `_overall` fake scheduler is also contained in the schedulers list to provide the cumulated live synthesis. Before sending the results, the arbiter sums-up all its schedulers live synthesis counters in the `_overall` live synthesis.

A real example of data returned by this endpoint is described :ref:`on this page <alignak_features/livesynthesis>`.

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

The ``/monitoring_problems`` (or ``/problems``) endpoint returns the overall Alignak known problems list.

This will return an object containing the properties of the Alignak arbiter ``identity``, and a *_freshness* property, plus a *problems* object which contains the list of known problems for each scheduler.

The *_freshness* is the timestamp when the provided data were fetched from the schedulers.

Each problem is referenced by an uuid and a series of attributes explaining which host/service is concerned and why it is a problem.

.. note:: a **problem** is an host (or service) which is not in an UP (or OK) HARD state. For more information, :ref:`see this page<alignak_features/problems-and-impacts>`.

A real example of data returned by this endpoint is described :ref:`on this page <alignak_features/monitoring_problems>`.

As an example::

  {
    _freshness: 1532452260,
    version: "1.1.0rc8",
    name: "arbiter-master",
    alignak: "My Alignak",
    start_time: 1532451465,
    type: "arbiter",
    running_id: "1532451465.77049649"
    problems: {
      scheduler-master-3: {
        problems: {
          9c5de32f-2e83-457f-9ab1-fbda77e993a9: {
            last_state: "DOWN",
            service: null,
            last_state_type: "HARD",
            last_state_update: 1532452229,
            last_hard_state: "DOWN",
            last_hard_state_change: 1532451509,
            last_state_change: 1532451509,
            state: "DOWN",
            state_type: "HARD",
            host: "host_2",
            output: "I am always Down"
          }
        }
      },
      scheduler-master-2: {
        problems: {
          856c5f44-93fd-4909-b051-444d17f3a272: {
            last_state: "WARNING",
            service: "BR_Simple_And",
            last_state_type: "HARD",
            last_state_update: 1532452232,
            last_hard_state: "WARNING",
            last_hard_state_change: 1532451932,
            last_state_change: 1532451692,
            state: "WARNING",
            state_type: "HARD",
            host: "BR_host",
            output: ""
          },
          494f06c6-8b77-40c4-b6ce-b0f746580270: {
            last_state: "WARNING",
            service: "dummy_warning",
            last_state_type: "HARD",
            last_state_update: 1532452234,
            last_hard_state: "WARNING",
            last_hard_state_change: 1532451634,
            last_state_change: 1532451574,
            state: "WARNING",
            state_type: "HARD",
            host: "host_0",
            output: "host_0-dummy_warning-1"
          },
          d3bb6510-d02b-412a-87a2-1aa4344c21c5: {
            last_state: "WARNING",
            service: "dummy_warning",
            last_state_type: "HARD",
            last_state_update: 1532452232,
            last_hard_state: "WARNING",
            last_hard_state_change: 1532451572,
            last_state_change: 1532451512,
            state: "WARNING",
            state_type: "HARD",
            host: "BR_host",
            output: "BR_host-dummy_warning-1"
          },
        }
      },
      ...
      ...


    }
  }


Alignak objects
---------------

It may be interesting to know exactly what Alignak knows about a monitored object. This is what the ``/object`` endpoint is made for...

This endpoint will return the full JSON dump of the requested object. For this, you need to provide:
  - the object type (eg. hostgroup, host, realm, ...)
  - the object name (or uuid)

.. note:: that there is not any documentation (except the source code one) for all the attributes you will get with this API... :/ At minimum you will find all the configuration properties that can be provided in the object configuration plus the inner Alignak peoperties ...


As an example, to get the list of the hosts groups::

   GET ``http://daemon:port/object/hostgroup``
   {
      "__sys_python_module__": "alignak.objects.hostgroup.Hostgroups",
      "content": {
          "4a0f49c5-3ced-4bd9-b184-287aa24f07e9": {
              "name": "", "notes_url": "", "imported_from": "unknown", "use": [], "action_url": "", "members": [], "conf_is_correct": true, "alias": "All Router Hosts", "register": true, "tags": [], "configuration_warnings": [], "realm": "", "hostgroup_members": [], "configuration_errors": [], "downtimes": {}, "uuid": "4a0f49c5-3ced-4bd9-b184-287aa24f07e9", "hostgroup_name": "router", "unknown_members": [], "definition_order": 100, "notes": ""
          },
          "767c5cb0-00dc-4792-a1ff-2d386480747f": {"name": "", "notes_url": "", "imported_from": "unknown", "use": [], "action_url": "", "members": ["256ef0c0-8b3c-4418-a34b-946d577ddb45", "18d89da0-5e7d-4c73-a3b4-4ed80e99abd5", "8e03762e-18c9-4573-ade7-72e234bfe4d5", "8a544de0-7032-462b-8ac9-641bef1ea4e8", "b713abeb-f167-48ce-b9c2-ace78acca8ba", "7d0be735-f706-4a7d-be69-cae17593d890", "e3086789-833d-4e06-a102-80049895241e", "475bbf42-433d-4a3d-95de-8f0a616720c1", "b51cdb58-e2ff-48b5-b0cf-1c9471e15cf0", "b565b005-a454-4f0d-bd99-12f5f49b78c5", "64ea4e7b-662d-4a4f-ac3f-288f055b1811"], "conf_is_correct": true, "alias": "All Hosts", "register": true, "tags": [], "configuration_warnings": [], "realm": "", "hostgroup_members": [], "configuration_errors": [], "downtimes": {}, "uuid": "767c5cb0-00dc-4792-a1ff-2d386480747f", "hostgroup_name": "allhosts", "unknown_members": [], "definition_order": 100, "notes": ""
                                                   },
          "dc48ba3f-f22c-46f6-b7b0-9d15cd499314": {
              "name": "", "notes_url": "", "imported_from": "unknown", "use": [], "action_url": "", "members": ["256ef0c0-8b3c-4418-a34b-946d577ddb45", "f26c3694-4738-4eac-a1be-97e4e92da23c"], "conf_is_correct": true, "alias": "monitoring_servers", "register": true, "tags": [], "configuration_warnings": [], "realm": "", "hostgroup_members": [], "configuration_errors": [], "downtimes": {}, "uuid": "dc48ba3f-f22c-46f6-b7b0-9d15cd499314", "hostgroup_name": "monitoring_servers", "unknown_members": [], "definition_order": 100, "notes": ""
          }
      }
  }


As an example, to get the hostgroup named ``allhosts``::

   GET ``http://daemon:port/object/hostgroup/all``
   {
      "__sys_python_module__": "alignak.objects.hostgroup.Hostgroup",
      "content": {
         "imported_from": "unknown", "hostgroup_name": "allhosts", "use": [], "uuid": "f64590c3-1c40-4c7a-ae8c-968ca53a4231", "alias": "All Hosts", "unknown_members": [], "downtimes": {}, "conf_is_correct": true, "configuration_warnings": [], "action_url": "", "members": ["390d944b-2ee2-41ed-b15b-65d51c78012c", "cefa6c8c-0c47-45ee-8b18-79809884c52f", "42fee4e4-08b3-4fdb-9aee-a8e1798c1c9c", "1d1d46e7-6f43-44de-91dd-ceef2ea3c0ac", "32abdd11-07f8-421b-af2d-eab0c0e6ea26", "2f9bbf60-a956-4445-aa4b-36ffa1de0be5", "67a346ce-32a2-4b38-9ec4-bb64d771c418", "9229341e-4421-46d2-bc6b-ccb906824f1c", "e35afdb8-6207-43ef-8013-e972d27878c6", "4128a394-9479-4c3f-b23a-8fee060c7704", "0c0c8c87-93ec-4934-b1da-30023855475a"], "configuration_errors": [], "notes_url": "", "hostgroup_members": [], "name": "", "notes": "", "definition_order": 100, "tags": [], "register": true, "realm": ""
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

The ``/satellites_list`` returns the list of each satellites grouped by daemon type.

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

The ``/satellites_configuration`` returns the list of each satellites grouped by daemon type. For each satellite, all its configuration is included.

As an example::

    {'scheduler': ['Scheduler1']},
    {'poller': ['Poller1', 'Poller2']}


Alignak configuration reload
----------------------------

If you change something in the monitored configuration (eg. add a new host, change a service parameter, ...) it is not necessary to stop and start all the Alignak processes. ou simply need to inform Alignak that something changed...

POSTing on the ``/configuration_reload`` endpoint provokes a configuration reload by the arbiter. The arbiter reloads its configuration and all the daemons receive this new configuration.

POSTing on the ``/backend_notification`` endpoint with an event and some parameters may also provoke a configuration reload.

.. note:: the ``/backend_notification`` is used by the Alignak backend. TYou would rather use the ``/configuration_reload`` service to provoke a reload except if you need to specify the reaload reason to the Alignak arbiter.

The `event` data is an event notification parameter.
The `parameters` data is only a text string that append information to the `event`.

As of now, the Arbiter will only log the request and provoke a configuration reload if `event` is *creation* or *deletion*.

Alignak daemons information
---------------------------

``http://daemon:port/identity`` returns information about the daemon and Alignak version::

   {
      "type": "arbiter",
      "name": "arbiter-master",
      "alignak": "My Alignak",
      "version": "1.1.0",
      "start_time": 1522385883,
      "running_id": 1522385883.123456
   }

