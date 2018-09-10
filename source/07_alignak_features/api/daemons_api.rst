.. _alignak_features/daemons_api:

.. Built from the test_daemons_api.py unit test last run!

===================
Alignak daemons API
===================

Daemon type: arbiter
--------------------
/api
~~~~

Python source code documentation
 ::

    List the methods available on the daemon Web service interface

        :return: a list of methods and parameters
        :rtype: dict
        

/backend_notification
~~~~~~~~~~~~~~~~~~~~~

Python source code documentation
 ::

    The Alignak backend raises an event to the Alignak arbiter
        -----
        Possible events are:
        - creation, for a realm or an host creation
        - deletion, for a realm or an host deletion

        Calls the reload configuration function if event is creation or deletion

        Else, nothing for the moment!

        In case of any error, this function returns an object containing some properties:
        '_status': 'ERR' because of the error
        `_message`: some more explanations about the error

        The `_status` field is 'OK' with an according `_message` to explain what the Arbiter
        will do depending upon the notification.

        :return: dict
        

/command
~~~~~~~~

Python source code documentation
 ::

     Request to execute an external command

        Allowed parameters are:
        `command`: mandatory parameter containing the whole command line or only the command name

        `timestamp`: optional parameter containing the timestamp. If not present, the
        current timestamp is added in the command line

        `element`: the targeted element that will be appended after the command name (`command`).
        If element contains a '/' character it is split to make an host and service.

        `host`, `service` or `user`: the targeted host, service or user. Takes precedence over
        the `element` to target a specific element

        `parameters`: the parameter that will be appended after all the arguments

        In case of any error, this function returns an object containing some properties:
        '_status': 'ERR' because of the error
        `_message`: some more explanations about the error

        The `_status` field is 'OK' with an according `_message` to explain what the Arbiter
        will do depending upon the notification. The `command` property contains the formatted
        external command.

        :return: dict
        

/dump
~~~~~

Python source code documentation
 ::

    Dump an host (all hosts) from the arbiter.

        The arbiter will get the host (all hosts) information from all its schedulers.

        This gets the main host information from the scheduler. If details is set, then some
        more information are provided. This will not get all the host known attributes but only
        a reduced set that will inform about the host and its services status

        If raw is set the information are provided in two string lists formated as CSV strings.
        The first list element contains the hosts information and the second one contains the
        services information.

        If an host name is provided, this function will get only this host information, else
        all the scheduler hosts are returned.

        As an example (in raw format):
        {
            scheduler-master-3: [
                [
                    "type;host;name;last_check;state_id;state;state_type;is_problem;
                    is_impact;output",
                    "localhost;host;localhost;1532451740;0;UP;HARD;False;False;
                    Host assumed to be UP",
                    "host_2;host;host_2;1532451988;1;DOWN;HARD;True;False;I am always Down"
                ],
                [
                    "type;host;name",
                    "host_2;service;dummy_no_output;1532451981;0;OK;HARD;False;True;
                    Service internal check result: 0",
                    "host_2;service;dummy_warning;1532451960;4;UNREACHABLE;HARD;False;True;
                    host_2-dummy_warning-1",
                    "host_2;service;dummy_unreachable;1532451987;4;UNREACHABLE;HARD;False;True;
                    host_2-dummy_unreachable-4",
                    "host_2;service;dummy_random;1532451949;4;UNREACHABLE;HARD;False;True;
                    Service internal check result: 2",
                    "host_2;service;dummy_ok;1532452002;0;OK;HARD;False;True;host_2",
                    "host_2;service;dummy_critical;1532451953;4;UNREACHABLE;HARD;False;True;
                    host_2-dummy_critical-2",
                    "host_2;service;dummy_unknown;1532451945;4;UNREACHABLE;HARD;False;True;
                    host_2-dummy_unknown-3",
                    "host_2;service;dummy_echo;1532451973;4;UNREACHABLE;HARD;False;True;"
                ]
            ],
            scheduler-master-2: [
            [
                "type;host;name;last_check;state_id;state;state_type;is_problem;is_impact;output",
                "host_0;host;host_0;1532451993;0;UP;HARD;False;False;I am always Up",
                "BR_host;host;BR_host;1532451991;0;UP;HARD;False;False;Host assumed to be UP"
            ],
            [
                "type;host;name;last_check;state_id;state;state_type;is_problem;is_impact;output",
                "host_0;service;dummy_no_output;1532451970;0;OK;HARD;False;False;
                Service internal check result: 0",
                "host_0;service;dummy_unknown;1532451964;3;UNKNOWN;HARD;True;False;
                host_0-dummy_unknown-3",
                "host_0;service;dummy_random;1532451991;1;WARNING;HARD;True;False;
                Service internal check result: 1",
                "host_0;service;dummy_warning;1532451945;1;WARNING;HARD;True;False;
                host_0-dummy_warning-1",
                "host_0;service;dummy_unreachable;1532451986;4;UNREACHABLE;HARD;True;False;
                host_0-dummy_unreachable-4",
                "host_0;service;dummy_ok;1532452012;0;OK;HARD;False;False;host_0",
                "host_0;service;dummy_critical;1532451987;2;CRITICAL;HARD;True;False;
                host_0-dummy_critical-2",
                "host_0;service;dummy_echo;1532451963;0;OK;HARD;False;False;",
                "BR_host;service;dummy_critical;1532451970;2;CRITICAL;HARD;True;False;
                BR_host-dummy_critical-2",
                "BR_host;service;BR_Simple_And;1532451895;1;WARNING;HARD;True;True;",
                "BR_host;service;dummy_unreachable;1532451981;4;UNREACHABLE;HARD;True;False;
                BR_host-dummy_unreachable-4",
                "BR_host;service;dummy_no_output;1532451975;0;OK;HARD;False;False;
                Service internal check result: 0",
                "BR_host;service;dummy_unknown;1532451955;3;UNKNOWN;HARD;True;False;
                BR_host-dummy_unknown-3",
                "BR_host;service;dummy_echo;1532451981;0;OK;HARD;False;False;",
                "BR_host;service;dummy_warning;1532451972;1;WARNING;HARD;True;False;
                BR_host-dummy_warning-1",
                "BR_host;service;dummy_random;1532451976;4;UNREACHABLE;HARD;True;False;
                Service internal check result: 4",
                "BR_host;service;dummy_ok;1532451972;0;OK;HARD;False;False;BR_host"
            ]
        ],
        ...

        More information are available in the scheduler correponding API endpoint.

        :param o_type: searched object type
        :type o_type: str
        :param o_name: searched object name (or uuid)
        :type o_name: str
        :return: serialized object information
        :rtype: str
        

/events_log
~~~~~~~~~~~

Python source code documentation
 ::

    Get the most recent Alignak events

        The arbiter maintains a list of the most recent Alignak events. This endpoint
        provides this list.

        The default format is:
        [
            "2018-07-23 15:14:43 - E - SERVICE NOTIFICATION: guest;host_0;dummy_random;CRITICAL;1;
            notify-service-by-log;Service internal check result: 2",
            "2018-07-23 15:14:43 - E - SERVICE NOTIFICATION: admin;host_0;dummy_random;CRITICAL;1;
            notify-service-by-log;Service internal check result: 2",
            "2018-07-23 15:14:42 - E - SERVICE ALERT: host_0;dummy_critical;CRITICAL;SOFT;1;
            host_0-dummy_critical-2",
            "2018-07-23 15:14:42 - E - SERVICE ALERT: host_0;dummy_random;CRITICAL;HARD;2;
            Service internal check result: 2",
            "2018-07-23 15:14:42 - I - SERVICE ALERT: host_0;dummy_unknown;UNKNOWN;HARD;2;
            host_0-dummy_unknown-3"
        ]

        If you request on this endpoint with the *details* parameter (whatever its value...),
        you will get a detailed JSON output:
        [
            {
                timestamp: "2018-07-23 15:16:35",
                message: "SERVICE ALERT: host_11;dummy_echo;UNREACHABLE;HARD;2;",
                level: "info"
            },
            {
                timestamp: "2018-07-23 15:16:32",
                message: "SERVICE NOTIFICATION: guest;host_0;dummy_random;OK;0;
                notify-service-by-log;Service internal check result: 0",
                level: "info"
            },
            {
                timestamp: "2018-07-23 15:16:32",
                message: "SERVICE NOTIFICATION: admin;host_0;dummy_random;OK;0;
                notify-service-by-log;Service internal check result: 0",
                level: "info"
            },
            {
                timestamp: "2018-07-23 15:16:32",
                message: "SERVICE ALERT: host_0;dummy_random;OK;HARD;2;
                Service internal check result: 0",
                level: "info"
            },
            {
                timestamp: "2018-07-23 15:16:19",
                message: "SERVICE ALERT: host_11;dummy_random;OK;HARD;2;
                Service internal check result: 0",
                level: "info"
            }
        ]

        In this example, only the 5 most recent events are provided whereas the default value is
        to provide the 100 last events. This default counter may be changed thanks to the
        ``events_log_count`` configuration variable or
        ``ALIGNAK_EVENTS_LOG_COUNT`` environment variable.

        The date format may also be changed thanks to the ``events_date_format`` configuration
        variable.

        :return: list of the most recent events
        :rtype: list
        

/external_commands
~~~~~~~~~~~~~~~~~~

Python source code documentation
 ::

    Get the external commands from the daemon

        Use a lock for this function to protect

        :return: serialized external command list
        :rtype: str
        

/get_log_level
~~~~~~~~~~~~~~

Python source code documentation
 ::

    Get the current daemon log level

        Returns an object with the daemon identity and a `log_level` property.

        running_id
        :return: current log level
        :rtype: str
        

/identity
~~~~~~~~~

Python source code documentation
 ::

    Get the daemon identity

        This will return an object containing some properties:
        - alignak: the Alignak instance name
        - version: the Alignak version
        - type: the daemon type
        - name: the daemon name

        :return: daemon identity
        :rtype: dict
        

/index
~~~~~~

Python source code documentation
 ::

    Wrapper to call api from /

        This will return the daemon identity and main information

        :return: function list
        

/livesynthesis
~~~~~~~~~~~~~~

Python source code documentation
 ::

    Get Alignak live synthesis

        This will return an object containing the properties of the `identity`, plus a
        `livesynthesis`
        object which contains 2 properties for each known scheduler:
        - _freshness, which is the timestamp when the provided data were fetched
        - livesynthesis, which is an object with the scheduler live synthesis.

        An `_overall` fake scheduler is also contained in the schedulers list to provide the
        cumulated live synthesis. Before sending the results, the arbiter sums-up all its
        schedulers live synthesis counters in the `_overall` live synthesis.

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

        :return: scheduler live synthesis
        :rtype: dict
        

/managed_configurations
~~~~~~~~~~~~~~~~~~~~~~~

Python source code documentation
 ::

    Get the arbiter configuration managed by the daemon

        For an arbiter daemon, it returns an empty object

        For all other daemons it returns a dictionary formated list of the scheduler
        links managed by the daemon:
        {
            'instance_id': {
                'hash': ,
                'push_flavor': ,
                'managed_conf_id':
            }
        }

        If a daemon returns an empty list, it means that it has not yet received its configuration
        from the arbiter.

        :return: managed configuration
        :rtype: list
        

/monitoring_problems
~~~~~~~~~~~~~~~~~~~~

Python source code documentation
 ::

    Get Alignak detailed monitoring status

        This will return an object containing the properties of the `identity`, plus a `problems`
        object which contains 2 properties for each known scheduler:
        - _freshness, which is the timestamp when the provided data were fetched
        - problems, which is an object with the scheduler known problems:

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

        :return: schedulers live synthesis list
        :rtype: dict
        

/object
~~~~~~~

Python source code documentation
 ::

    Get a monitored object from the arbiter.

        Indeed, the arbiter requires the object from its schedulers. It will iterate in
        its schedulers list until a matching object is found. Else it will return a Json
        structure containing _status and _message properties.

        When found, the result is a serialized object which is a Json structure containing:
        - content: the serialized object content
        - __sys_python_module__: the python class of the returned object

        The Alignak unserialize function of the alignak.misc.serialization package allows
        to restore the initial object.

        .. code-block:: python

            from alignak.misc.serialization import unserialize
            from alignak.objects.hostgroup import Hostgroup
            raw_data = req.get("http://127.0.0.1:7768/object/hostgroup/allhosts")
            print("Got: %s / %s" % (raw_data.status_code, raw_data.content))
            assert raw_data.status_code == 200
            object = raw_data.json()
            group = unserialize(object, True)
            assert group.__class__ == Hostgroup
            assert group.get_name() == 'allhosts'

        As an example:
        {
            "__sys_python_module__": "alignak.objects.hostgroup.Hostgroup",
            "content": {
                "uuid": "32248642-97dd-4f39-aaa2-5120112a765d",
                "name": "",
                "hostgroup_name": "allhosts",
                "use": [],
                "tags": [],
                "alias": "All Hosts",
                "notes": "",
                "definition_order": 100,
                "register": true,
                "unknown_members": [],
                "notes_url": "",
                "action_url": "",

                "imported_from": "unknown",
                "conf_is_correct": true,
                "configuration_errors": [],
                "configuration_warnings": [],
                "realm": "",
                "downtimes": {},
                "hostgroup_members": [],
                "members": [
                    "553d47bc-27aa-426c-a664-49c4c0c4a249",
                    "f88093ca-e61b-43ff-a41e-613f7ad2cea2",
                    "df1e2e13-552d-43de-ad2a-fe80ad4ba979",
                    "d3d667dd-f583-4668-9f44-22ef3dcb53ad"
                ]
            }
        }

        :param o_type: searched object type
        :type o_type: str
        :param o_name: searched object name (or uuid)
        :type o_name: str
        :return: serialized object information
        :rtype: str
        

/problems
~~~~~~~~~

Python source code documentation
 ::

    Alias for monitoring_problems

/realms
~~~~~~~

Python source code documentation
 ::

    Return the realms / satellites configuration

        Returns an object containing the hierarchical realms configuration with the main
        information about each realm:
        {
            All: {
                satellites: {
                    pollers: [
                        "poller-master"
                    ],
                    reactionners: [
                        "reactionner-master"
                    ],
                    schedulers: [
                        "scheduler-master", "scheduler-master-3", "scheduler-master-2"
                    ],
                    brokers: [
                    "broker-master"
                    ],
                    receivers: [
                    "receiver-master", "receiver-nsca"
                    ]
                },
                children: { },
                name: "All",
                members: [
                    "host_1", "host_0", "host_3", "host_2", "host_11", "localhost"
                ],
                level: 0
            },
            North: {
                ...
            }
        }

        Sub realms defined inside a realm are provided in the `children` property of their
        parent realm and they contain the same information as their parent..
        The `members` realm contain the list of the hosts members of the realm.

        If ``details`` is required, each realm will contain more information about each satellite
        involved in the realm management:
        {
            All: {
                satellites: {
                    pollers: [
                        {
                            passive: false,
                            name: "poller-master",
                            livestate_output: "poller/poller-master is up and running.",
                            reachable: true,
                            uri: "http://127.0.0.1:7771/",
                            alive: true,
                            realm_name: "All",
                            manage_sub_realms: true,
                            spare: false,
                            polling_interval: 5,
                            configuration_sent: true,
                            active: true,
                            livestate: 0,
                            max_check_attempts: 3,
                            last_check: 1532242300.593074,
                            type: "poller"
                        }
                    ],
                    reactionners: [
                        {
                            passive: false,
                            name: "reactionner-master",
                            livestate_output: "reactionner/reactionner-master is up and running.",
                            reachable: true,
                            uri: "http://127.0.0.1:7769/",
                            alive: true,
                            realm_name: "All",
                            manage_sub_realms: true,
                            spare: false,
                            polling_interval: 5,
                            configuration_sent: true,
                            active: true,
                            livestate: 0,
                            max_check_attempts: 3,
                            last_check: 1532242300.587762,
                            type: "reactionner"
                        }
                    ]

        :return: dict containing realms / satellites
        :rtype: dict
        

/reload_configuration
~~~~~~~~~~~~~~~~~~~~~

Python source code documentation
 ::

    Ask to the arbiter to reload the monitored configuration

        **Note** tha the arbiter will not reload its main configuration file (eg. alignak.ini)
        but it will reload the monitored objects from the Nagios legacy files or from the
        Alignak backend!

        In case of any error, this function returns an object containing some properties:
        '_status': 'ERR' because of the error
        `_message`: some more explanations about the error

        :return: True if configuration reload is accepted
        

/satellites_configuration
~~~~~~~~~~~~~~~~~~~~~~~~~

Python source code documentation
 ::

    Return all the configuration data of satellites

        :return: dict containing satellites data
        Output looks like this ::

        {'arbiter' : [{'property1':'value1' ..}, {'property2', 'value11' ..}, ..],
        'scheduler': [..],
        'poller': [..],
        'reactionner': [..],
        'receiver': [..],
         'broker: [..]'
        }

        :rtype: dict
        

/satellites_list
~~~~~~~~~~~~~~~~

Python source code documentation
 ::

    Get the arbiter satellite names sorted by type

        Returns a list of the satellites as in:
        {
            reactionner: [
                "reactionner-master"
            ],
            broker: [
                "broker-master"
            ],
            arbiter: [
                "arbiter-master"
            ],
            scheduler: [
                "scheduler-master-3",
                "scheduler-master",
                "scheduler-master-2"
            ],
            receiver: [
                "receiver-nsca",
                "receiver-master"
            ],
            poller: [
                "poller-master"
            ]
        }

        If a specific daemon type is requested, the list is reduced to this unique daemon type:
        {
            scheduler: [
                "scheduler-master-3",
                "scheduler-master",
                "scheduler-master-2"
            ]
        }

        :param daemon_type: daemon type to filter
        :type daemon_type: str
        :return: dict with key *daemon_type* and value list of daemon name
        :rtype: dict
        

/set_log_level
~~~~~~~~~~~~~~

Python source code documentation
 ::

    Set the current log level for the daemon

        The `log_level` parameter must be in [DEBUG, INFO, WARNING, ERROR, CRITICAL]

        In case of any error, this function returns an object containing some properties:
        '_status': 'ERR' because of the error
        `_message`: some more explanations about the error

        Else, this function returns True

        :param log_level: a value in one of the above
        :type log_level: str
        :return: see above
        :rtype: dict
        

/stats
~~~~~~

Python source code documentation
 ::

    Get statistics and information from the daemon

        Returns an object with the daemon identity, the daemon start_time
        and some extra properties depending upon the daemon type.

        All daemons provide these ones:
        - program_start: the Alignak start timestamp
        - spare: to indicate if the daemon is a spare one
        - load: the daemon load
        - modules: the daemon modules information
        - counters: the specific daemon counters

        :param details: Details are required (different from 0)
        :type details str

        :return: daemon stats
        :rtype: dict
        

/status
~~~~~~~

Python source code documentation
 ::

    Get the overall alignak status

        Returns a list of the satellites as in:
        {
            services: [
                {
                    livestate: {
                        perf_data: "",
                        timestamp: 1532106561,
                        state: "ok",
                        long_output: "",
                        output: "all daemons are up and running."
                    },
                    name: "arbiter-master"
                },
                {
                    livestate: {
                        name: "poller_poller-master",
                        timestamp: 1532106561,
                        long_output: "Realm: (True). Listening on: http://127.0.0.1:7771/",
                        state: "ok",
                        output: "daemon is alive and reachable.",
                        perf_data: "last_check=1532106560.17"
                    },
                    name: "poller-master"
                },
                ...
                ...
            ],
            variables: { },
            livestate: {
                timestamp: 1532106561,
                long_output: "broker-master - daemon is alive and reachable.
                poller-master - daemon is alive and reachable.
                reactionner-master - daemon is alive and reachable.
                receiver-master - daemon is alive and reachable.
                receiver-nsca - daemon is alive and reachable.
                scheduler-master - daemon is alive and reachable.
                scheduler-master-2 - daemon is alive and reachable.
                scheduler-master-3 - daemon is alive and reachable.",
                state: "up",
                output: "All my daemons are up and running.",
                perf_data: "
                    'servicesextinfo'=0 'businessimpactmodulations'=0 'hostgroups'=2
                    'resultmodulations'=0 'escalations'=0 'schedulers'=3 'hostsextinfo'=0
                    'contacts'=2 'servicedependencies'=0 'servicegroups'=1 'pollers'=1
                    'arbiters'=1 'receivers'=2 'macromodulations'=0 'reactionners'=1
                    'contactgroups'=2 'brokers'=1 'realms'=3 'services'=32 'commands'=11
                    'notificationways'=2 'timeperiods'=4 'modules'=0 'checkmodulations'=0
                    'hosts'=6 'hostdependencies'=0"
            },
            name: "My Alignak",
            template: {
                notes: "",
                alias: "My Alignak",
                _templates: [
                    "alignak",
                    "important"
                ],
                active_checks_enabled: false,
                passive_checks_enabled: true
            }
        }

        :param details: Details are required (different from 0)
        :type details bool

        :return: dict with key *daemon_type* and value list of daemon name
        :rtype: dict
        

/stop_request
~~~~~~~~~~~~~

Python source code documentation
 ::

    Request the daemon to stop

        If `stop_now` is set to '1' the daemon will stop now. Else, the daemon
        will enter the stop wait mode. In this mode the daemon stops its activity and
        waits until it receives a new `stop_now` request to stop really.

        :param stop_now: stop now or go to stop wait mode
        :type stop_now: bool
        :return: None
        

/system
~~~~~~~

Python source code documentation
 ::

    Return the realms / satellites configuration

        Returns an object containing the hierarchical realms configuration with the main
        information about each realm:
        {
            All: {
                satellites: {
                    pollers: [
                        "poller-master"
                    ],
                    reactionners: [
                        "reactionner-master"
                    ],
                    schedulers: [
                        "scheduler-master", "scheduler-master-3", "scheduler-master-2"
                    ],
                    brokers: [
                    "broker-master"
                    ],
                    receivers: [
                    "receiver-master", "receiver-nsca"
                    ]
                },
                children: { },
                name: "All",
                members: [
                    "host_1", "host_0", "host_3", "host_2", "host_11", "localhost"
                ],
                level: 0
            },
            North: {
                ...
            }
        }

        Sub realms defined inside a realm are provided in the `children` property of their
        parent realm and they contain the same information as their parent..
        The `members` realm contain the list of the hosts members of the realm.

        If ``details`` is required, each realm will contain more information about each satellite
        involved in the realm management:
        {
            All: {
                satellites: {
                    pollers: [
                        {
                            passive: false,
                            name: "poller-master",
                            livestate_output: "poller/poller-master is up and running.",
                            reachable: true,
                            uri: "http://127.0.0.1:7771/",
                            alive: true,
                            realm_name: "All",
                            manage_sub_realms: true,
                            spare: false,
                            polling_interval: 5,
                            configuration_sent: true,
                            active: true,
                            livestate: 0,
                            max_check_attempts: 3,
                            last_check: 1532242300.593074,
                            type: "poller"
                        }
                    ],
                    reactionners: [
                        {
                            passive: false,
                            name: "reactionner-master",
                            livestate_output: "reactionner/reactionner-master is up and running.",
                            reachable: true,
                            uri: "http://127.0.0.1:7769/",
                            alive: true,
                            realm_name: "All",
                            manage_sub_realms: true,
                            spare: false,
                            polling_interval: 5,
                            configuration_sent: true,
                            active: true,
                            livestate: 0,
                            max_check_attempts: 3,
                            last_check: 1532242300.587762,
                            type: "reactionner"
                        }
                    ]

        :return: dict containing realms / satellites
        :rtype: dict
        

Daemon type: broker
-------------------
/api
~~~~

Python source code documentation
 ::

    List the methods available on the daemon Web service interface

        :return: a list of methods and parameters
        :rtype: dict
        

/get_log_level
~~~~~~~~~~~~~~

Python source code documentation
 ::

    Get the current daemon log level

        Returns an object with the daemon identity and a `log_level` property.

        running_id
        :return: current log level
        :rtype: str
        

/identity
~~~~~~~~~

Python source code documentation
 ::

    Get the daemon identity

        This will return an object containing some properties:
        - alignak: the Alignak instance name
        - version: the Alignak version
        - type: the daemon type
        - name: the daemon name

        :return: daemon identity
        :rtype: dict
        

/index
~~~~~~

Python source code documentation
 ::

    Wrapper to call api from /

        This will return the daemon identity and main information

        :return: function list
        

/managed_configurations
~~~~~~~~~~~~~~~~~~~~~~~

Python source code documentation
 ::

    Get the arbiter configuration managed by the daemon

        For an arbiter daemon, it returns an empty object

        For all other daemons it returns a dictionary formated list of the scheduler
        links managed by the daemon:
        {
            'instance_id': {
                'hash': ,
                'push_flavor': ,
                'managed_conf_id':
            }
        }

        If a daemon returns an empty list, it means that it has not yet received its configuration
        from the arbiter.

        :return: managed configuration
        :rtype: list
        

/set_log_level
~~~~~~~~~~~~~~

Python source code documentation
 ::

    Set the current log level for the daemon

        The `log_level` parameter must be in [DEBUG, INFO, WARNING, ERROR, CRITICAL]

        In case of any error, this function returns an object containing some properties:
        '_status': 'ERR' because of the error
        `_message`: some more explanations about the error

        Else, this function returns True

        :param log_level: a value in one of the above
        :type log_level: str
        :return: see above
        :rtype: dict
        

/stats
~~~~~~

Python source code documentation
 ::

    Get statistics and information from the daemon

        Returns an object with the daemon identity, the daemon start_time
        and some extra properties depending upon the daemon type.

        All daemons provide these ones:
        - program_start: the Alignak start timestamp
        - spare: to indicate if the daemon is a spare one
        - load: the daemon load
        - modules: the daemon modules information
        - counters: the specific daemon counters

        :param details: Details are required (different from 0)
        :type details str

        :return: daemon stats
        :rtype: dict
        

/stop_request
~~~~~~~~~~~~~

Python source code documentation
 ::

    Request the daemon to stop

        If `stop_now` is set to '1' the daemon will stop now. Else, the daemon
        will enter the stop wait mode. In this mode the daemon stops its activity and
        waits until it receives a new `stop_now` request to stop really.

        :param stop_now: stop now or go to stop wait mode
        :type stop_now: bool
        :return: None
        

Daemon type: poller
-------------------
/api
~~~~

Python source code documentation
 ::

    List the methods available on the daemon Web service interface

        :return: a list of methods and parameters
        :rtype: dict
        

/get_log_level
~~~~~~~~~~~~~~

Python source code documentation
 ::

    Get the current daemon log level

        Returns an object with the daemon identity and a `log_level` property.

        running_id
        :return: current log level
        :rtype: str
        

/identity
~~~~~~~~~

Python source code documentation
 ::

    Get the daemon identity

        This will return an object containing some properties:
        - alignak: the Alignak instance name
        - version: the Alignak version
        - type: the daemon type
        - name: the daemon name

        :return: daemon identity
        :rtype: dict
        

/index
~~~~~~

Python source code documentation
 ::

    Wrapper to call api from /

        This will return the daemon identity and main information

        :return: function list
        

/managed_configurations
~~~~~~~~~~~~~~~~~~~~~~~

Python source code documentation
 ::

    Get the arbiter configuration managed by the daemon

        For an arbiter daemon, it returns an empty object

        For all other daemons it returns a dictionary formated list of the scheduler
        links managed by the daemon:
        {
            'instance_id': {
                'hash': ,
                'push_flavor': ,
                'managed_conf_id':
            }
        }

        If a daemon returns an empty list, it means that it has not yet received its configuration
        from the arbiter.

        :return: managed configuration
        :rtype: list
        

/set_log_level
~~~~~~~~~~~~~~

Python source code documentation
 ::

    Set the current log level for the daemon

        The `log_level` parameter must be in [DEBUG, INFO, WARNING, ERROR, CRITICAL]

        In case of any error, this function returns an object containing some properties:
        '_status': 'ERR' because of the error
        `_message`: some more explanations about the error

        Else, this function returns True

        :param log_level: a value in one of the above
        :type log_level: str
        :return: see above
        :rtype: dict
        

/stats
~~~~~~

Python source code documentation
 ::

    Get statistics and information from the daemon

        Returns an object with the daemon identity, the daemon start_time
        and some extra properties depending upon the daemon type.

        All daemons provide these ones:
        - program_start: the Alignak start timestamp
        - spare: to indicate if the daemon is a spare one
        - load: the daemon load
        - modules: the daemon modules information
        - counters: the specific daemon counters

        :param details: Details are required (different from 0)
        :type details str

        :return: daemon stats
        :rtype: dict
        

/stop_request
~~~~~~~~~~~~~

Python source code documentation
 ::

    Request the daemon to stop

        If `stop_now` is set to '1' the daemon will stop now. Else, the daemon
        will enter the stop wait mode. In this mode the daemon stops its activity and
        waits until it receives a new `stop_now` request to stop really.

        :param stop_now: stop now or go to stop wait mode
        :type stop_now: bool
        :return: None
        

Daemon type: reactionner
------------------------
/api
~~~~

Python source code documentation
 ::

    List the methods available on the daemon Web service interface

        :return: a list of methods and parameters
        :rtype: dict
        

/get_log_level
~~~~~~~~~~~~~~

Python source code documentation
 ::

    Get the current daemon log level

        Returns an object with the daemon identity and a `log_level` property.

        running_id
        :return: current log level
        :rtype: str
        

/identity
~~~~~~~~~

Python source code documentation
 ::

    Get the daemon identity

        This will return an object containing some properties:
        - alignak: the Alignak instance name
        - version: the Alignak version
        - type: the daemon type
        - name: the daemon name

        :return: daemon identity
        :rtype: dict
        

/index
~~~~~~

Python source code documentation
 ::

    Wrapper to call api from /

        This will return the daemon identity and main information

        :return: function list
        

/managed_configurations
~~~~~~~~~~~~~~~~~~~~~~~

Python source code documentation
 ::

    Get the arbiter configuration managed by the daemon

        For an arbiter daemon, it returns an empty object

        For all other daemons it returns a dictionary formated list of the scheduler
        links managed by the daemon:
        {
            'instance_id': {
                'hash': ,
                'push_flavor': ,
                'managed_conf_id':
            }
        }

        If a daemon returns an empty list, it means that it has not yet received its configuration
        from the arbiter.

        :return: managed configuration
        :rtype: list
        

/set_log_level
~~~~~~~~~~~~~~

Python source code documentation
 ::

    Set the current log level for the daemon

        The `log_level` parameter must be in [DEBUG, INFO, WARNING, ERROR, CRITICAL]

        In case of any error, this function returns an object containing some properties:
        '_status': 'ERR' because of the error
        `_message`: some more explanations about the error

        Else, this function returns True

        :param log_level: a value in one of the above
        :type log_level: str
        :return: see above
        :rtype: dict
        

/stats
~~~~~~

Python source code documentation
 ::

    Get statistics and information from the daemon

        Returns an object with the daemon identity, the daemon start_time
        and some extra properties depending upon the daemon type.

        All daemons provide these ones:
        - program_start: the Alignak start timestamp
        - spare: to indicate if the daemon is a spare one
        - load: the daemon load
        - modules: the daemon modules information
        - counters: the specific daemon counters

        :param details: Details are required (different from 0)
        :type details str

        :return: daemon stats
        :rtype: dict
        

/stop_request
~~~~~~~~~~~~~

Python source code documentation
 ::

    Request the daemon to stop

        If `stop_now` is set to '1' the daemon will stop now. Else, the daemon
        will enter the stop wait mode. In this mode the daemon stops its activity and
        waits until it receives a new `stop_now` request to stop really.

        :param stop_now: stop now or go to stop wait mode
        :type stop_now: bool
        :return: None
        

Daemon type: receiver
---------------------
/api
~~~~

Python source code documentation
 ::

    List the methods available on the daemon Web service interface

        :return: a list of methods and parameters
        :rtype: dict
        

/get_log_level
~~~~~~~~~~~~~~

Python source code documentation
 ::

    Get the current daemon log level

        Returns an object with the daemon identity and a `log_level` property.

        running_id
        :return: current log level
        :rtype: str
        

/identity
~~~~~~~~~

Python source code documentation
 ::

    Get the daemon identity

        This will return an object containing some properties:
        - alignak: the Alignak instance name
        - version: the Alignak version
        - type: the daemon type
        - name: the daemon name

        :return: daemon identity
        :rtype: dict
        

/index
~~~~~~

Python source code documentation
 ::

    Wrapper to call api from /

        This will return the daemon identity and main information

        :return: function list
        

/managed_configurations
~~~~~~~~~~~~~~~~~~~~~~~

Python source code documentation
 ::

    Get the arbiter configuration managed by the daemon

        For an arbiter daemon, it returns an empty object

        For all other daemons it returns a dictionary formated list of the scheduler
        links managed by the daemon:
        {
            'instance_id': {
                'hash': ,
                'push_flavor': ,
                'managed_conf_id':
            }
        }

        If a daemon returns an empty list, it means that it has not yet received its configuration
        from the arbiter.

        :return: managed configuration
        :rtype: list
        

/set_log_level
~~~~~~~~~~~~~~

Python source code documentation
 ::

    Set the current log level for the daemon

        The `log_level` parameter must be in [DEBUG, INFO, WARNING, ERROR, CRITICAL]

        In case of any error, this function returns an object containing some properties:
        '_status': 'ERR' because of the error
        `_message`: some more explanations about the error

        Else, this function returns True

        :param log_level: a value in one of the above
        :type log_level: str
        :return: see above
        :rtype: dict
        

/stats
~~~~~~

Python source code documentation
 ::

    Get statistics and information from the daemon

        Returns an object with the daemon identity, the daemon start_time
        and some extra properties depending upon the daemon type.

        All daemons provide these ones:
        - program_start: the Alignak start timestamp
        - spare: to indicate if the daemon is a spare one
        - load: the daemon load
        - modules: the daemon modules information
        - counters: the specific daemon counters

        :param details: Details are required (different from 0)
        :type details str

        :return: daemon stats
        :rtype: dict
        

/stop_request
~~~~~~~~~~~~~

Python source code documentation
 ::

    Request the daemon to stop

        If `stop_now` is set to '1' the daemon will stop now. Else, the daemon
        will enter the stop wait mode. In this mode the daemon stops its activity and
        waits until it receives a new `stop_now` request to stop really.

        :param stop_now: stop now or go to stop wait mode
        :type stop_now: bool
        :return: None
        

Daemon type: scheduler
----------------------
/api
~~~~

Python source code documentation
 ::

    List the methods available on the daemon Web service interface

        :return: a list of methods and parameters
        :rtype: dict
        

/dump
~~~~~

Python source code documentation
 ::

    Dump an host (all hosts) from the scheduler.

        This gets the main host information from the scheduler. If details is set, then some
        more information are provided. This will not get all the host known attributes but only
        a reduced set that will inform about the host and its services status

        If raw is set the information are provided in two string lists formated as CSV strings.
        The first list element contains the hosts information and the second one contains the
        services information.

        If an host name is provided, this function will get only this host information, else
        all the scheduler hosts are returned.

        As an example (raw format):
        [
            [   # Host information
                "type;host;name;last_check;state_id;state;state_type;is_problem;is_impact;output",
                "BR_host;host;BR_host;1532451511;0;UP;HARD;False;False;Host assumed to be UP"
            ],
            [   # Services information
                "type;host;name;last_check;state_id;state;state_type;is_problem;is_impact;output",
                "BR_host;service;dummy_critical;1532451490;2;CRITICAL;SOFT;False;False;
                BR_host-dummy_critical-2",
                "BR_host;service;BR_Simple_And;0;0;OK;HARD;False;False;",
                "BR_host;service;dummy_unreachable;1532451501;4;UNREACHABLE;SOFT;False;False;
                BR_host-dummy_unreachable-4",
                "BR_host;service;dummy_no_output;1532451495;0;OK;HARD;False;False;
                Service internal check result: 0",
                "BR_host;service;dummy_unknown;1532451475;3;UNKNOWN;SOFT;False;False;
                BR_host-dummy_unknown-3",
                "BR_host;service;dummy_echo;1532451501;0;OK;HARD;False;False;",
                "BR_host;service;dummy_warning;1532451492;1;WARNING;SOFT;False;False;
                BR_host-dummy_warning-1",
                "BR_host;service;dummy_random;1532451496;2;CRITICAL;SOFT;False;False;
                Service internal check result: 2",
                "BR_host;service;dummy_ok;1532451492;0;OK;HARD;False;False;BR_host"
            ]
        ]

        As an example (json format):
        {
            is_impact: false,
            name: "BR_host",
            state: "UP",
            last_check: 1532451811,
            state_type: "HARD",
            host: "BR_host",
            output: "Host assumed to be UP",
            services: [
                {
                    is_impact: false,
                    name: "dummy_critical",
                    state: "CRITICAL",
                    last_check: 1532451790,
                    state_type: "HARD",
                    host: "BR_host",
                    output: "BR_host-dummy_critical-2",
                    state_id: 2,
                    type: "service",
                    is_problem: true
                },
                {
                    is_impact: true,
                    name: "BR_Simple_And",
                    state: "WARNING",
                    last_check: 1532451775,
                    state_type: "SOFT",
                    host: "BR_host",
                    output: "",
                    state_id: 1,
                    type: "service",
                    is_problem: false
                },
                ....
                ....
            },
            state_id: 0,
            type: "host",
            is_problem: false
        }

        :param o_name: searched host name (or uuid)
        :type o_name: str
        :param details: less or more details
        :type details: bool
        :param raw: json or raw text format
        :type raw: bool
        :return: list of host and services information
        :rtype: list
        

/get_log_level
~~~~~~~~~~~~~~

Python source code documentation
 ::

    Get the current daemon log level

        Returns an object with the daemon identity and a `log_level` property.

        running_id
        :return: current log level
        :rtype: str
        

/identity
~~~~~~~~~

Python source code documentation
 ::

    Get the daemon identity

        This will return an object containing some properties:
        - alignak: the Alignak instance name
        - version: the Alignak version
        - type: the daemon type
        - name: the daemon name

        :return: daemon identity
        :rtype: dict
        

/index
~~~~~~

Python source code documentation
 ::

    Wrapper to call api from /

        This will return the daemon identity and main information

        :return: function list
        

/managed_configurations
~~~~~~~~~~~~~~~~~~~~~~~

Python source code documentation
 ::

    Get the arbiter configuration managed by the daemon

        For an arbiter daemon, it returns an empty object

        For all other daemons it returns a dictionary formated list of the scheduler
        links managed by the daemon:
        {
            'instance_id': {
                'hash': ,
                'push_flavor': ,
                'managed_conf_id':
            }
        }

        If a daemon returns an empty list, it means that it has not yet received its configuration
        from the arbiter.

        :return: managed configuration
        :rtype: list
        

/monitoring_problems
~~~~~~~~~~~~~~~~~~~~

Python source code documentation
 ::

    Get Alignak scheduler monitoring status

        Returns an object with the scheduler livesynthesis
        and the known problems

        :return: scheduler live synthesis
        :rtype: dict
        

/object
~~~~~~~

Python source code documentation
 ::

    Get an object from the scheduler.

        The result is a serialized object which is a Json structure containing:
        - content: the serialized object content
        - __sys_python_module__: the python class of the returned object

        The Alignak unserialize function of the alignak.misc.serialization package allows
        to restore the initial object.

        .. code-block:: python

            from alignak.misc.serialization import unserialize
            from alignak.objects.hostgroup import Hostgroup
            raw_data = req.get("http://127.0.0.1:7768/object/hostgroup/allhosts")
            print("Got: %s / %s" % (raw_data.status_code, raw_data.content))
            assert raw_data.status_code == 200
            object = raw_data.json()
            group = unserialize(object, True)
            assert group.__class__ == Hostgroup
            assert group.get_name() == 'allhosts'

        As an example:
        {
            "__sys_python_module__": "alignak.objects.hostgroup.Hostgroup",
            "content": {
                "uuid": "32248642-97dd-4f39-aaa2-5120112a765d",
                "name": "",
                "hostgroup_name": "allhosts",
                "use": [],
                "tags": [],
                "alias": "All Hosts",
                "notes": "",
                "definition_order": 100,
                "register": true,
                "unknown_members": [],
                "notes_url": "",
                "action_url": "",

                "imported_from": "unknown",
                "conf_is_correct": true,
                "configuration_errors": [],
                "configuration_warnings": [],
                "realm": "",
                "downtimes": {},
                "hostgroup_members": [],
                "members": [
                    "553d47bc-27aa-426c-a664-49c4c0c4a249",
                    "f88093ca-e61b-43ff-a41e-613f7ad2cea2",
                    "df1e2e13-552d-43de-ad2a-fe80ad4ba979",
                    "d3d667dd-f583-4668-9f44-22ef3dcb53ad"
                ]
            }
        }

        :param o_type: searched object type
        :type o_type: str
        :param o_name: searched object name (or uuid)
        :type o_name: str
        :return: serialized object information
        :rtype: str
        

/put_results
~~~~~~~~~~~~

Python source code documentation
 ::

    Put results to scheduler, used by poller or reactionner when they are
        in active mode (passive = False)

        This function is not intended for external use. Let the poller and reactionner
        manage all this stuff by themselves ;)

        :param from: poller/reactionner identification
        :type from: str
        :param results: list of actions results
        :type results: list
        :return: True
        :rtype: bool
        

/set_log_level
~~~~~~~~~~~~~~

Python source code documentation
 ::

    Set the current log level for the daemon

        The `log_level` parameter must be in [DEBUG, INFO, WARNING, ERROR, CRITICAL]

        In case of any error, this function returns an object containing some properties:
        '_status': 'ERR' because of the error
        `_message`: some more explanations about the error

        Else, this function returns True

        :param log_level: a value in one of the above
        :type log_level: str
        :return: see above
        :rtype: dict
        

/stats
~~~~~~

Python source code documentation
 ::

    Get statistics and information from the daemon

        Returns an object with the daemon identity, the daemon start_time
        and some extra properties depending upon the daemon type.

        All daemons provide these ones:
        - program_start: the Alignak start timestamp
        - spare: to indicate if the daemon is a spare one
        - load: the daemon load
        - modules: the daemon modules information
        - counters: the specific daemon counters

        :param details: Details are required (different from 0)
        :type details str

        :return: daemon stats
        :rtype: dict
        

/stop_request
~~~~~~~~~~~~~

Python source code documentation
 ::

    Request the daemon to stop

        If `stop_now` is set to '1' the daemon will stop now. Else, the daemon
        will enter the stop wait mode. In this mode the daemon stops its activity and
        waits until it receives a new `stop_now` request to stop really.

        :param stop_now: stop now or go to stop wait mode
        :type stop_now: bool
        :return: None
        
