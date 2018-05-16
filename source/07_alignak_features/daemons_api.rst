.. _alignak_features/daemons_api:

===================
Alignak daemons API
===================
Daemon: scheduler
-----------------
/api
~~~~
::

	List the methods available on the daemon Web service interface

        :return: a list of methods and parameters
           :rtype: dict



/fill_initial_broks
~~~~~~~~~~~~~~~~~~~
::
	Get initial_broks from the scheduler

        This is used by the brokers to prepare the initial status broks

        This do not send broks, it only makes scheduler internal processing. Then the broker
        must use the *get_broks* API to get all the stuff

        :param broker_name: broker name, used to filter broks
           :type broker_name: str
           :return: None



/get_broks
~~~~~~~~~~
::
	Get the broks from a scheduler, used by brokers

        This is used by the brokers to get the broks list of a scheduler

        :param broker_name: broker name, used to filter broks
           :type broker_name: str
           :return: serialized brok list
           :rtype: dict



/get_checks
~~~~~~~~~~~
::
	Get checks from scheduler, used by poller or reactionner when they are
        in active mode (passive = False)

        This function is not intended for external use. Let the poller and reactionner
        manage all this stuff by themselves ;)

        :param do_checks: used for poller to get checks
           :type do_checks: bool
           :param do_actions: used for reactionner to get actions
           :type do_actions: bool
           :param poller_tags: poller tags to filter on this poller
           :type poller_tags: list
           :param reactionner_tags: reactionner tags to filter on this reactionner
           :type reactionner_tags: list
           :param worker_name: Worker name asking (so that the scheduler add it to actions objects)
           :type worker_name: str
           :param module_types: Module type to filter actions/checks
           :type module_types: list
           :return: serialized check/action list
           :rtype: str



/get_external_commands
~~~~~~~~~~~~~~~~~~~~~~
::
	Get the external commands from the daemon

        Use a lock for this function to protect

        :return: serialized external command list
           :rtype: str



/get_host
~~~~~~~~~
::
	Get host configuration from the scheduler, used mainly by the receiver

        :param host_name: searched host name
           :type host_name: str
           :return: serialized host information
           :rtype: str



/get_id
~~~~~~~
::
	Get the daemon identity

        This will return an object containing some properties:
        - alignak: the Alignak instance name
        - version: the Alignak version
        - type: the daemon type
        - name: the daemon name

        :return: daemon identity
           :rtype: dict



/get_log_level
~~~~~~~~~~~~~~
::
	Get the current daemon log level

        Returns an object with the daemon identity and a `log_level` property.

        running_id
        :return: current log level
           :rtype: str



/get_managed_configurations
~~~~~~~~~~~~~~~~~~~~~~~~~~~
::
	Get the scheduler configuration managed by the daemon

        For an arbiter daemon, it returns an empty object

        For all other daemons it returns a dcitionary formated list of the scheduler
        links managed by the daemon:
        {
            'instance_id': {
                'hash': ,
                'push_flavor': ,
                'managed_conf_id':
            }
        }

        :return: managed configuration
           :rtype: list



/get_results
~~~~~~~~~~~~
::
	Get the results of the executed actions for the scheduler which instance id is provided

        :param scheduler_instance_id: instance id of the scheduler
           :type scheduler_instance_id: string
           :return: serialized list
           :rtype: str



/get_running_id
~~~~~~~~~~~~~~~
::
	Get the current running identifier of the daemon

        The running identifier of the daemon is a float number made of its start timestamp
        (integer part) and a random number (decimal part). This make it unique and allows
        to get sure that the daemon did not changed since the last communication.

        Returns an object with the daemon identity and a `running_id` property.

        :return: running identifier
           :rtype: dict



/get_start_time
~~~~~~~~~~~~~~~
::
	Get the start time of the daemon

        The start timestamp of a daemon is the integer timestamp got from the system
        when the daemon started.

        Returns an object with the daemon identity and a `start_time` property.

        :return: start timestamp
           :rtype: dict



/get_stats
~~~~~~~~~~
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



/have_conf
~~~~~~~~~~
::
	Get the daemon current configuration state

        If the daemon has received a configuration from its arbiter, this will
        return True

        If a `magic_hash` is provided it is compared with the one included in the
        daemon configuration and this function returns True only if they match!

        :return: boolean indicating if the daemon has a configuration
           :rtype: bool



/index
~~~~~~
::
	Wrapper to call api from /

        :return: function list



/ping
~~~~~
::
	Test the connection to the daemon.

        This function always returns the string 'pong'

        :return: string 'pong'
           :rtype: str



/push_actions
~~~~~~~~~~~~~
::
	Push actions to the poller/reactionner

        This function is used by the scheduler to send the actions to get executed to
        the poller/reactionner

        {'actions': actions, 'scheduler_id': scheduler_id}

        :return:None



/push_configuration
~~~~~~~~~~~~~~~~~~~
::
	Send a new configuration to the daemon

        This function is not intended for external use. It is quite complex to
        build a configuration for a daemon and it is the arbiter dispatcher job ;)

        :param pushed_configuration: new conf to send
           :return: None



/put_results
~~~~~~~~~~~~
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



/run_external_commands
~~~~~~~~~~~~~~~~~~~~~~
::
	Post external_commands to scheduler (from arbiter)
        Wrapper to to app.sched.run_external_commands method

        :return: None



/set_log_level
~~~~~~~~~~~~~~
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



/stop_request
~~~~~~~~~~~~~
::
	Request the daemon to stop

        If `stop_now` is set to '1' the daemon will stop now. Else, the daemon
        will enter the stop wait mode. In this mode the daemon stops its activity and
        waits until it receives a new `stop_now` request to stop really.

        :param stop_now: stop now or go to stop wait mode
           :type stop_now: bool
           :return: None



/wait_new_conf
~~~~~~~~~~~~~~
::
	Ask the scheduler to drop its configuration and wait for a new one

        :return: None



Daemon: reactionner
-------------------
/api
~~~~
::
	List the methods available on the daemon Web service interface

        :return: a list of methods and parameters
           :rtype: dict



/get_broks
~~~~~~~~~~
::
	Get the broks from the daemon

        This is used by the brokers to get the broks list of a daemon

        :return: Brok list serialized
           :rtype: dict



/get_external_commands
~~~~~~~~~~~~~~~~~~~~~~
::
	Get the external commands from the daemon

        Use a lock for this function to protect

        :return: serialized external command list
           :rtype: str



/get_id
~~~~~~~
::
	Get the daemon identity

        This will return an object containing some properties:
        - alignak: the Alignak instance name
        - version: the Alignak version
        - type: the daemon type
        - name: the daemon name

        :return: daemon identity
           :rtype: dict



/get_log_level
~~~~~~~~~~~~~~
::
	Get the current daemon log level

        Returns an object with the daemon identity and a `log_level` property.

        running_id
        :return: current log level
           :rtype: str



/get_managed_configurations
~~~~~~~~~~~~~~~~~~~~~~~~~~~
::
	Get the scheduler configuration managed by the daemon

        For an arbiter daemon, it returns an empty object

        For all other daemons it returns a dcitionary formated list of the scheduler
        links managed by the daemon:
        {
            'instance_id': {
                'hash': ,
                'push_flavor': ,
                'managed_conf_id':
            }
        }

        :return: managed configuration
           :rtype: list



/get_results
~~~~~~~~~~~~
::
	Get the results of the executed actions for the scheduler which instance id is provided

        :param scheduler_instance_id: instance id of the scheduler
           :type scheduler_instance_id: string
           :return: serialized list
           :rtype: str



/get_running_id
~~~~~~~~~~~~~~~
::
	Get the current running identifier of the daemon

        The running identifier of the daemon is a float number made of its start timestamp
        (integer part) and a random number (decimal part). This make it unique and allows
        to get sure that the daemon did not changed since the last communication.

        Returns an object with the daemon identity and a `running_id` property.

        :return: running identifier
           :rtype: dict



/get_start_time
~~~~~~~~~~~~~~~
::
	Get the start time of the daemon

        The start timestamp of a daemon is the integer timestamp got from the system
        when the daemon started.

        Returns an object with the daemon identity and a `start_time` property.

        :return: start timestamp
           :rtype: dict



/get_stats
~~~~~~~~~~
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



/have_conf
~~~~~~~~~~
::
	Get the daemon current configuration state

        If the daemon has received a configuration from its arbiter, this will
        return True

        If a `magic_hash` is provided it is compared with the one included in the
        daemon configuration and this function returns True only if they match!

        :return: boolean indicating if the daemon has a configuration
           :rtype: bool



/index
~~~~~~
::
	Wrapper to call api from /

        :return: function list



/ping
~~~~~
::
	Test the connection to the daemon.

        This function always returns the string 'pong'

        :return: string 'pong'
           :rtype: str



/push_actions
~~~~~~~~~~~~~
::
	Push actions to the poller/reactionner

        This function is used by the scheduler to send the actions to get executed to
        the poller/reactionner

        {'actions': actions, 'scheduler_id': scheduler_id}

        :return:None



/push_configuration
~~~~~~~~~~~~~~~~~~~
::
	Send a new configuration to the daemon

        This function is not intended for external use. It is quite complex to
        build a configuration for a daemon and it is the arbiter dispatcher job ;)

        :param pushed_configuration: new conf to send
           :return: None



/set_log_level
~~~~~~~~~~~~~~
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



/stop_request
~~~~~~~~~~~~~
::
	Request the daemon to stop

        If `stop_now` is set to '1' the daemon will stop now. Else, the daemon
        will enter the stop wait mode. In this mode the daemon stops its activity and
        waits until it receives a new `stop_now` request to stop really.

        :param stop_now: stop now or go to stop wait mode
           :type stop_now: bool
           :return: None



/wait_new_conf
~~~~~~~~~~~~~~
::
	Ask the daemon to drop its configuration and wait for a new one

        :return: None



Daemon: poller
--------------
/api
~~~~
::
	List the methods available on the daemon Web service interface

        :return: a list of methods and parameters
           :rtype: dict



/get_broks
~~~~~~~~~~
::
	Get the broks from the daemon

        This is used by the brokers to get the broks list of a daemon

        :return: Brok list serialized
           :rtype: dict



/get_external_commands
~~~~~~~~~~~~~~~~~~~~~~
::
	Get the external commands from the daemon

        Use a lock for this function to protect

        :return: serialized external command list
           :rtype: str



/get_id
~~~~~~~
::
	Get the daemon identity

        This will return an object containing some properties:
        - alignak: the Alignak instance name
        - version: the Alignak version
        - type: the daemon type
        - name: the daemon name

        :return: daemon identity
           :rtype: dict



/get_log_level
~~~~~~~~~~~~~~
::
	Get the current daemon log level

        Returns an object with the daemon identity and a `log_level` property.

        running_id
        :return: current log level
           :rtype: str



/get_managed_configurations
~~~~~~~~~~~~~~~~~~~~~~~~~~~
::
	Get the scheduler configuration managed by the daemon

        For an arbiter daemon, it returns an empty object

        For all other daemons it returns a dcitionary formated list of the scheduler
        links managed by the daemon:
        {
            'instance_id': {
                'hash': ,
                'push_flavor': ,
                'managed_conf_id':
            }
        }

        :return: managed configuration
           :rtype: list



/get_results
~~~~~~~~~~~~
::
	Get the results of the executed actions for the scheduler which instance id is provided

        :param scheduler_instance_id: instance id of the scheduler
           :type scheduler_instance_id: string
           :return: serialized list
           :rtype: str



/get_running_id
~~~~~~~~~~~~~~~
::
	Get the current running identifier of the daemon

        The running identifier of the daemon is a float number made of its start timestamp
        (integer part) and a random number (decimal part). This make it unique and allows
        to get sure that the daemon did not changed since the last communication.

        Returns an object with the daemon identity and a `running_id` property.

        :return: running identifier
           :rtype: dict



/get_start_time
~~~~~~~~~~~~~~~
::
	Get the start time of the daemon

        The start timestamp of a daemon is the integer timestamp got from the system
        when the daemon started.

        Returns an object with the daemon identity and a `start_time` property.

        :return: start timestamp
           :rtype: dict



/get_stats
~~~~~~~~~~
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



/have_conf
~~~~~~~~~~
::
	Get the daemon current configuration state

        If the daemon has received a configuration from its arbiter, this will
        return True

        If a `magic_hash` is provided it is compared with the one included in the
        daemon configuration and this function returns True only if they match!

        :return: boolean indicating if the daemon has a configuration
           :rtype: bool



/index
~~~~~~
::
	Wrapper to call api from /

        :return: function list



/ping
~~~~~
::
	Test the connection to the daemon.

        This function always returns the string 'pong'

        :return: string 'pong'
           :rtype: str



/push_actions
~~~~~~~~~~~~~
::
	Push actions to the poller/reactionner

        This function is used by the scheduler to send the actions to get executed to
        the poller/reactionner

        {'actions': actions, 'scheduler_id': scheduler_id}

        :return:None



/push_configuration
~~~~~~~~~~~~~~~~~~~
::
	Send a new configuration to the daemon

        This function is not intended for external use. It is quite complex to
        build a configuration for a daemon and it is the arbiter dispatcher job ;)

        :param pushed_configuration: new conf to send
           :return: None



/set_log_level
~~~~~~~~~~~~~~
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



/stop_request
~~~~~~~~~~~~~
::
	Request the daemon to stop

        If `stop_now` is set to '1' the daemon will stop now. Else, the daemon
        will enter the stop wait mode. In this mode the daemon stops its activity and
        waits until it receives a new `stop_now` request to stop really.

        :param stop_now: stop now or go to stop wait mode
           :type stop_now: bool
           :return: None



/wait_new_conf
~~~~~~~~~~~~~~
::
	Ask the daemon to drop its configuration and wait for a new one

        :return: None



Daemon: arbiter
---------------
/api
~~~~
::
	List the methods available on the daemon Web service interface

        :return: a list of methods and parameters
           :rtype: dict



/backend_notification
~~~~~~~~~~~~~~~~~~~~~
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



/do_not_run
~~~~~~~~~~~
::
	The master arbiter tells to its spare arbiters to not run.

        A master arbiter will ignore this request

        :return: None



/get_alignak_status
~~~~~~~~~~~~~~~~~~~
::
	Get the overall alignak status

        Returns a list of the satellites as in:
        {
            'scheduler': ['Scheduler1']
            'poller': ['Poller1', 'Poller2']
            ...
        }

        :param details: Details are required (different from 0)
           :type details str

           :return: dict with key *daemon_type* and value list of daemon name
        :rtype: dict



/get_broks
~~~~~~~~~~
::
	Get the broks from the daemon

        This is used by the brokers to get the broks list of a daemon

        :return: Brok list serialized
           :rtype: dict



/get_external_commands
~~~~~~~~~~~~~~~~~~~~~~
::
	Get the external commands from the daemon

        Use a lock for this function to protect

        :return: serialized external command list
           :rtype: str



/get_id
~~~~~~~
::
	Get the daemon identity

        This will return an object containing some properties:
        - alignak: the Alignak instance name
        - version: the Alignak version
        - type: the daemon type
        - name: the daemon name

        :return: daemon identity
           :rtype: dict



/get_log_level
~~~~~~~~~~~~~~
::
	Get the current daemon log level

        Returns an object with the daemon identity and a `log_level` property.

        running_id
        :return: current log level
           :rtype: str



/get_managed_configurations
~~~~~~~~~~~~~~~~~~~~~~~~~~~
::
	Get the scheduler configuration managed by the daemon

        For an arbiter daemon, it returns an empty object

        For all other daemons it returns a dcitionary formated list of the scheduler
        links managed by the daemon:
        {
            'instance_id': {
                'hash': ,
                'push_flavor': ,
                'managed_conf_id':
            }
        }

        :return: managed configuration
           :rtype: list



/get_objects_properties
~~~~~~~~~~~~~~~~~~~~~~~
::
	'Dump all objects of the required type existing in the configuration:
            - hosts, services, contacts,
            - hostgroups, servicegroups, contactgroups
            - commands, timeperiods
            - ...

        :param table: table name
           :type table: str
           :return: list all properties of all objects
           :rtype: list



/get_results
~~~~~~~~~~~~
::
	Get the results of the executed actions for the scheduler which instance id is provided

        :param scheduler_instance_id: instance id of the scheduler
           :type scheduler_instance_id: string
           :return: serialized list
           :rtype: str



/get_running_id
~~~~~~~~~~~~~~~
::
	Get the current running identifier of the daemon

        The running identifier of the daemon is a float number made of its start timestamp
        (integer part) and a random number (decimal part). This make it unique and allows
        to get sure that the daemon did not changed since the last communication.

        Returns an object with the daemon identity and a `running_id` property.

        :return: running identifier
           :rtype: dict



/get_satellites_configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
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



/get_satellites_list
~~~~~~~~~~~~~~~~~~~~
::
	Get the arbiter satellite names sorted by type

        Returns a list of the satellites as in:
        {
            'scheduler': ['Scheduler1']
            'poller': ['Poller1', 'Poller2']
            ...
        }

        If a specific daemon type is requested, the list is reduced to this unique daemon type.

        :param daemon_type: daemon type to filter
           :type daemon_type: str
           :return: dict with key *daemon_type* and value list of daemon name
        :rtype: dict



/get_start_time
~~~~~~~~~~~~~~~
::
	Get the start time of the daemon

        The start timestamp of a daemon is the integer timestamp got from the system
        when the daemon started.

        Returns an object with the daemon identity and a `start_time` property.

        :return: start timestamp
           :rtype: dict



/get_stats
~~~~~~~~~~
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



/have_conf
~~~~~~~~~~
::
	Get the daemon current configuration state

        If the daemon has received a configuration from its arbiter, this will
        return True

        If a `magic_hash` is provided it is compared with the one included in the
        daemon configuration and this function returns True only if they match!

        :return: boolean indicating if the daemon has a configuration
           :rtype: bool



/index
~~~~~~
::
	Wrapper to call api from /

        :return: function list



/ping
~~~~~
::
	Test the connection to the daemon.

        This function always returns the string 'pong'

        :return: string 'pong'
           :rtype: str



/push_actions
~~~~~~~~~~~~~
::
	Push actions to the poller/reactionner

        This function is used by the scheduler to send the actions to get executed to
        the poller/reactionner

        {'actions': actions, 'scheduler_id': scheduler_id}

        :return:None



/push_configuration
~~~~~~~~~~~~~~~~~~~
::
	Send a new configuration to the daemon

        Used by the master arbiter to send its configuration to a spare arbiter

        This function is not intended for external use. It is quite complex to
        build a configuration for a daemon and it is the arbter dispatcher job ;)

        :param pushed_configuration: new conf to send
           :return: None



/push_external_command
~~~~~~~~~~~~~~~~~~~~~~
::
	Only to maintain ascending compatibility... this function uses the inner
        *command* endpoint.

        :param command: Alignak external command
           :type command: string
           :return: None



/reload_configuration
~~~~~~~~~~~~~~~~~~~~~
::
	Ask to the arbiter to reload the monitored configuration

        In case of any error, this function returns an object containing some properties:
        '_status': 'ERR' because of the error
        `_message`: some more explanations about the error

        :return: True if configuration reload is accepted



/set_log_level
~~~~~~~~~~~~~~
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



/stop_request
~~~~~~~~~~~~~
::
	Request the daemon to stop

        If `stop_now` is set to '1' the daemon will stop now. Else, the daemon
        will enter the stop wait mode. In this mode the daemon stops its activity and
        waits until it receives a new `stop_now` request to stop really.

        :param stop_now: stop now or go to stop wait mode
           :type stop_now: bool
           :return: None



/wait_new_conf
~~~~~~~~~~~~~~
::
	Ask the daemon to drop its configuration and wait for a new one

        :return: None



Daemon: receiver
----------------
/api
~~~~
::
	List the methods available on the daemon Web service interface

        :return: a list of methods and parameters
           :rtype: dict



/get_broks
~~~~~~~~~~
::
	Get the broks from the daemon

        This is used by the brokers to get the broks list of a daemon

        :return: Brok list serialized
           :rtype: dict



/get_external_commands
~~~~~~~~~~~~~~~~~~~~~~
::
	Get the external commands from the daemon

        Use a lock for this function to protect

        :return: serialized external command list
           :rtype: str



/get_id
~~~~~~~
::
	Get the daemon identity

        This will return an object containing some properties:
        - alignak: the Alignak instance name
        - version: the Alignak version
        - type: the daemon type
        - name: the daemon name

        :return: daemon identity
           :rtype: dict



/get_log_level
~~~~~~~~~~~~~~
::
	Get the current daemon log level

        Returns an object with the daemon identity and a `log_level` property.

        running_id
        :return: current log level
           :rtype: str



/get_managed_configurations
~~~~~~~~~~~~~~~~~~~~~~~~~~~
::
	Get the scheduler configuration managed by the daemon

        For an arbiter daemon, it returns an empty object

        For all other daemons it returns a dcitionary formated list of the scheduler
        links managed by the daemon:
        {
            'instance_id': {
                'hash': ,
                'push_flavor': ,
                'managed_conf_id':
            }
        }

        :return: managed configuration
           :rtype: list



/get_results
~~~~~~~~~~~~
::
	Get the results of the executed actions for the scheduler which instance id is provided

        :param scheduler_instance_id: instance id of the scheduler
           :type scheduler_instance_id: string
           :return: serialized list
           :rtype: str



/get_running_id
~~~~~~~~~~~~~~~
::
	Get the current running identifier of the daemon

        The running identifier of the daemon is a float number made of its start timestamp
        (integer part) and a random number (decimal part). This make it unique and allows
        to get sure that the daemon did not changed since the last communication.

        Returns an object with the daemon identity and a `running_id` property.

        :return: running identifier
           :rtype: dict



/get_start_time
~~~~~~~~~~~~~~~
::
	Get the start time of the daemon

        The start timestamp of a daemon is the integer timestamp got from the system
        when the daemon started.

        Returns an object with the daemon identity and a `start_time` property.

        :return: start timestamp
           :rtype: dict



/get_stats
~~~~~~~~~~
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



/have_conf
~~~~~~~~~~
::
	Get the daemon current configuration state

        If the daemon has received a configuration from its arbiter, this will
        return True

        If a `magic_hash` is provided it is compared with the one included in the
        daemon configuration and this function returns True only if they match!

        :return: boolean indicating if the daemon has a configuration
           :rtype: bool



/index
~~~~~~
::
	Wrapper to call api from /

        :return: function list



/ping
~~~~~
::
	Test the connection to the daemon.

        This function always returns the string 'pong'

        :return: string 'pong'
           :rtype: str



/push_actions
~~~~~~~~~~~~~
::
	Push actions to the poller/reactionner

        This function is used by the scheduler to send the actions to get executed to
        the poller/reactionner

        {'actions': actions, 'scheduler_id': scheduler_id}

        :return:None



/push_configuration
~~~~~~~~~~~~~~~~~~~
::
	Send a new configuration to the daemon

        This function is not intended for external use. It is quite complex to
        build a configuration for a daemon and it is the arbiter dispatcher job ;)

        :param pushed_configuration: new conf to send
           :return: None



/set_log_level
~~~~~~~~~~~~~~
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



/stop_request
~~~~~~~~~~~~~
::
	Request the daemon to stop

        If `stop_now` is set to '1' the daemon will stop now. Else, the daemon
        will enter the stop wait mode. In this mode the daemon stops its activity and
        waits until it receives a new `stop_now` request to stop really.

        :param stop_now: stop now or go to stop wait mode
           :type stop_now: bool
           :return: None



/wait_new_conf
~~~~~~~~~~~~~~
::
	Ask the daemon to drop its configuration and wait for a new one

        :return: None



Daemon: broker
--------------
/api
~~~~
::
	List the methods available on the daemon Web service interface

        :return: a list of methods and parameters
           :rtype: dict



/get_broks
~~~~~~~~~~
::
	Get the broks from the daemon

        This is used by the brokers to get the broks list of a daemon

        :return: Brok list serialized
           :rtype: dict



/get_external_commands
~~~~~~~~~~~~~~~~~~~~~~
::
	Get the external commands from the daemon

        Use a lock for this function to protect

        :return: serialized external command list
           :rtype: str



/get_id
~~~~~~~
::
	Get the daemon identity

        This will return an object containing some properties:
        - alignak: the Alignak instance name
        - version: the Alignak version
        - type: the daemon type
        - name: the daemon name

        :return: daemon identity
           :rtype: dict



/get_log_level
~~~~~~~~~~~~~~
::
	Get the current daemon log level

        Returns an object with the daemon identity and a `log_level` property.

        running_id
        :return: current log level
           :rtype: str



/get_managed_configurations
~~~~~~~~~~~~~~~~~~~~~~~~~~~
::
	Get the scheduler configuration managed by the daemon

        For an arbiter daemon, it returns an empty object

        For all other daemons it returns a dcitionary formated list of the scheduler
        links managed by the daemon:
        {
            'instance_id': {
                'hash': ,
                'push_flavor': ,
                'managed_conf_id':
            }
        }

        :return: managed configuration
           :rtype: list



/get_results
~~~~~~~~~~~~
::
	Get the results of the executed actions for the scheduler which instance id is provided

        :param scheduler_instance_id: instance id of the scheduler
           :type scheduler_instance_id: string
           :return: serialized list
           :rtype: str



/get_running_id
~~~~~~~~~~~~~~~
::
	Get the current running identifier of the daemon

        The running identifier of the daemon is a float number made of its start timestamp
        (integer part) and a random number (decimal part). This make it unique and allows
        to get sure that the daemon did not changed since the last communication.

        Returns an object with the daemon identity and a `running_id` property.

        :return: running identifier
           :rtype: dict



/get_start_time
~~~~~~~~~~~~~~~
::
	Get the start time of the daemon

        The start timestamp of a daemon is the integer timestamp got from the system
        when the daemon started.

        Returns an object with the daemon identity and a `start_time` property.

        :return: start timestamp
           :rtype: dict



/get_stats
~~~~~~~~~~
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



/have_conf
~~~~~~~~~~
::
	Get the daemon current configuration state

        If the daemon has received a configuration from its arbiter, this will
        return True

        If a `magic_hash` is provided it is compared with the one included in the
        daemon configuration and this function returns True only if they match!

        :return: boolean indicating if the daemon has a configuration
           :rtype: bool



/index
~~~~~~
::
	Wrapper to call api from /

        :return: function list



/ping
~~~~~
::
	Test the connection to the daemon.

        This function always returns the string 'pong'

        :return: string 'pong'
           :rtype: str



/push_actions
~~~~~~~~~~~~~
::
	Push actions to the poller/reactionner

        This function is used by the scheduler to send the actions to get executed to
        the poller/reactionner

        {'actions': actions, 'scheduler_id': scheduler_id}

        :return:None



/push_broks
~~~~~~~~~~~
::
	Push the provided broks objects to the broker daemon

        Only used on a Broker daemon by the Arbiter

        :param: broks
           :type: list
           :return: None



/push_configuration
~~~~~~~~~~~~~~~~~~~
::
	Send a new configuration to the daemon

        This function is not intended for external use. It is quite complex to
        build a configuration for a daemon and it is the arbiter dispatcher job ;)

        :param pushed_configuration: new conf to send
           :return: None



/set_log_level
~~~~~~~~~~~~~~
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



/stop_request
~~~~~~~~~~~~~
::
	Request the daemon to stop

        If `stop_now` is set to '1' the daemon will stop now. Else, the daemon
        will enter the stop wait mode. In this mode the daemon stops its activity and
        waits until it receives a new `stop_now` request to stop really.

        :param stop_now: stop now or go to stop wait mode
           :type stop_now: bool
           :return: None



/wait_new_conf
~~~~~~~~~~~~~~
::
	Ask the daemon to drop its configuration and wait for a new one

        :return: None

