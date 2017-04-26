.. _annexes/external_commands_list:

======================
External Commands list
======================


Below you will find descriptions of each external command.

.. warning:: all those external commands are not implemented in Alignak! This list contains all the commonly known external commands and keep you informed if the command is implemented or not!

ACKNOWLEDGE_HOST_PROBLEM
~~~~~~~~~~~~~~~~~~~~~~~~

    ACKNOWLEDGE_HOST_PROBLEM;<host_name>;<sticky>;<notify>;<persistent>;<author>;<comment>

        Allows you to acknowledge the current problem for the specified host. By acknowledging the current problem, future notifications (for the same host state) are disabled.

        If the "sticky" option is set to two (2), the acknowledgement will remain until the host recovers (returns to an UP state). Otherwise the acknowledgement will automatically be removed when the host changes state.

        If the "notify" option is set to one (1), a notification will be sent out to contacts indicating that the current host problem has been acknowledged, if set to null (0) there will be no notification.

        If the "persistent" option is set to one (1), the comment associated with the acknowledgement will remain even after the host recovers.

**Note** contrary to the legacy Nagios behavior, Alignak will automatically set an acknowledge on all the host services that are currently problems when an host problem is acknowledged.

**Note** that Alignak will always consider an acknowledge as persistent. Thus it will ignore the "persistent" information value.

ACKNOWLEDGE_HOST_PROBLEM_EXPIRE
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    ``ACKNOWLEDGE_HOST_PROBLEM_EXPIRE;<host_name>;<sticky>;<notify>;<persistent>;<timestamp>;<author>;<comment>``

    Allows you to define the time (seconds since the UNIX epoch) when the acknowledgement will expire (will be deleted).

ACKNOWLEDGE_SVC_PROBLEM
~~~~~~~~~~~~~~~~~~~~~~~

    ``ACKNOWLEDGE_SVC_PROBLEM;<host_name>;<service_description>;<sticky>;<notify>;<persistent>;<author>;<comment>``

        Allows you to acknowledge the current problem for the specified service. By acknowledging the current problem, future notifications (for the same service state) are disabled.

        If the "sticky" option is set to two (2), the acknowledgement will remain until the service recovers (returns to an OK state). Otherwise the acknowledgement will automatically be removed when the service changes state.

        If the "notify" option is set to one (1), a notification will be sent out to contacts indicating that the current service problem has been acknowledged, if set to null (0) there will be no notification.

        If the "persistent" option is set to one (1), the comment associated with the acknowledgement will remain even after the service recovers.

**Note** that Alignak will always consider an acknowledge as persistent. Thus it will ignore the "persistent" information value.

ACKNOWLEDGE_SVC_PROBLEM_EXPIRE
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    ``ACKNOWLEDGE_SVC_PROBLEM_EXPIRE;<host_name>;<service_description>;<sticky>;<notify>;<persistent>;<timestamp>;<author>;<comment>``

        Allows you to define the time (seconds since the UNIX epoch) when the acknowledgement will expire (will be deleted).

ADD_HOST_COMMENT
~~~~~~~~~~~~~~~~

   ``ADD_HOST_COMMENT;<host_name>;<persistent>;<author>;<comment>``

      Adds a comment to a particular host. If the "persistent" field is set to zero (0), the comment will be deleted the next time Alignak is restarted. Otherwise, the comment will persist across program restarts until it is deleted manually.

ADD_SVC_COMMENT
~~~~~~~~~~~~~~~

   ``ADD_SVC_COMMENT;<host_name>;<service_description>;<persistent>;<author>;<comment>``

      Adds a comment to a particular service. If the "persistent" field is set to zero (0), the comment will be deleted the next time Alignak is restarted. Otherwise, the comment will persist across program restarts until it is deleted manually.

CHANGE_CONTACT_HOST_NOTIFICATION_TIMEPERIOD
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

``CHANGE_CONTACT_HOST_NOTIFICATION_TIMEPERIOD;<contact_name>;<notification_timeperiod>``

      Changes the host notification timeperiod for a particular contact to what is specified by the "notification_timeperiod" option. The "notification_timeperiod" option should be the short name of the timeperiod that is to be used as the contact's host notification timeperiod. The timeperiod must have been configured in Alignak before it was last (re)started.

CHANGE_CONTACT_MODATTR
~~~~~~~~~~~~~~~~~~~~~~

   ``CHANGE_CONTACT_MODATTR;<contact_name>;<value>``

      This command changes the modified attributes value for the specified contact. Modified attributes values are used by Alignak to determine which object properties should be retained across program restarts. Thus, modifying the value of the attributes can affect data retention. This is an advanced option and should only be used by people who are intimately familiar with the data retention logic in Alignak.

CHANGE_CONTACT_MODHATTR
~~~~~~~~~~~~~~~~~~~~~~~

   ``CHANGE_CONTACT_MODHATTR;<contact_name>;<value>``

      This command changes the modified host attributes value for the specified contact. Modified attributes values are used by Alignak to determine which object properties should be retained across program restarts. Thus, modifying the value of the attributes can affect data retention. This is an advanced option and should only be used by people who are intimately familiar with the data retention logic in Alignak.

CHANGE_CONTACT_MODSATTR
~~~~~~~~~~~~~~~~~~~~~~~

   ``CHANGE_CONTACT_MODSATTR;<contact_name>;<value>``

      This command changes the modified service attributes value for the specified contact. Modified attributes values are used by Alignak to determine which object properties should be retained across program restarts. Thus, modifying the value of the attributes can affect data retention. This is an advanced option and should only be used by people who are intimately familiar with the data retention logic in Alignak.

CHANGE_CONTACT_SVC_NOTIFICATION_TIMEPERIOD
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``CHANGE_CONTACT_SVC_NOTIFICATION_TIMEPERIOD;<contact_name>;<notification_timeperiod>``

      Changes the service notification timeperiod for a particular contact to what is specified by the "notification_timeperiod" option. The "notification_timeperiod" option should be the short name of the timeperiod that is to be used as the contact's service notification timeperiod. The timeperiod must have been configured in Alignak before it was last (re)started.

CHANGE_CUSTOM_CONTACT_VAR
~~~~~~~~~~~~~~~~~~~~~~~~~

   ``CHANGE_CUSTOM_CONTACT_VAR;<contact_name>;<varname>;<varvalue>``

      Changes the value of a custom contact variable.

CHANGE_CUSTOM_HOST_VAR
~~~~~~~~~~~~~~~~~~~~~~

   ``CHANGE_CUSTOM_HOST_VAR;<host_name>;<varname>;<varvalue>``

      Changes the value of a custom host variable.

CHANGE_CUSTOM_SVC_VAR
~~~~~~~~~~~~~~~~~~~~~

   ``CHANGE_CUSTOM_SVC_VAR;<host_name>;<service_description>;<varname>;<varvalue>``

      Changes the value of a custom service variable.

CHANGE_GLOBAL_HOST_EVENT_HANDLER
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``CHANGE_GLOBAL_HOST_EVENT_HANDLER;<event_handler_command>``

      Changes the global host event handler command to be that specified by the "event_handler_command" option. The "event_handler_command" option specifies the short name of the command that should be used as the new host event handler. The command must have been configured in Alignak before it was last (re)started.

.. note:: this command is not currently implemented in Alignak

CHANGE_GLOBAL_SVC_EVENT_HANDLER
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``CHANGE_GLOBAL_SVC_EVENT_HANDLER;<event_handler_command>``

      Changes the global service event handler command to be that specified by the "event_handler_command" option. The "event_handler_command" option specifies the short name of the command that should be used as the new service event handler. The command must have been configured in Alignak before it was last (re)started.

.. note:: this command is not currently implemented in Alignak

CHANGE_HOST_CHECK_COMMAND
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``CHANGE_HOST_CHECK_COMMAND;<host_name>;<check_command>``

      Changes the check command for a particular host to be that specified by the "check_command" option. The "check_command" option specifies the short name of the command that should be used as the new host check command. The command must have been configured in Alignak before it was last (re)started.

CHANGE_HOST_CHECK_TIMEPERIOD
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``CHANGE_HOST_CHECK_TIMEPERIOD;<host_name>;<timeperiod>``

      Changes the valid check period for the specified host.

CHANGE_HOST_EVENT_HANDLER
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``CHANGE_HOST_EVENT_HANDLER;<host_name>;<event_handler_command>``

      Changes the event handler command for a particular host to be that specified by the "event_handler_command" option. The "event_handler_command" option specifies the short name of the command that should be used as the new host event handler. The command must have been configured in Alignak before it was last (re)started.

CHANGE_HOST_MODATTR
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``CHANGE_HOST_MODATTR;<host_name>;<value>``

      This command changes the modified attributes value for the specified host. Modified attributes values are used by Alignak to determine which object properties should be retained across program restarts. Thus, modifying the value of the attributes can affect data retention. This is an advanced option and should only be used by people who are intimately familiar with the data retention logic in Alignak.

CHANGE_HOST_NOTIFICATION_TIMEPERIOD
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``CHANGE_HOST_NOTIFICATION_TIMEPERIOD;<host_name>;<notification_timeperiod>``

      Changes the notification timeperiod for a particular host to what is specified by the "notification_timeperiod" option. The "notification_timeperiod" option should be the short name of the timeperiod that is to be used as the service notification timeperiod. The timeperiod must have been configured in Alignak before it was last (re)started.

CHANGE_MAX_HOST_CHECK_ATTEMPTS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``CHANGE_MAX_HOST_CHECK_ATTEMPTS;<host_name>;<check_attempts>``

      Changes the maximum number of check attempts (retries) for a particular host.

CHANGE_MAX_SVC_CHECK_ATTEMPTS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``CHANGE_MAX_SVC_CHECK_ATTEMPTS;<host_name>;<service_description>;<check_attempts>``

      Changes the maximum number of check attempts (retries) for a particular service.

CHANGE_NORMAL_HOST_CHECK_INTERVAL
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``CHANGE_NORMAL_HOST_CHECK_INTERVAL;<host_name>;<check_interval>``

   Changes the normal (regularly scheduled) check interval for a particular host.

CHANGE_NORMAL_SVC_CHECK_INTERVAL
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``CHANGE_NORMAL_SVC_CHECK_INTERVAL;<host_name>;<service_description>;<check_interval>``

      Changes the normal (regularly scheduled) check interval for a particular service

CHANGE_RETRY_HOST_CHECK_INTERVAL
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``CHANGE_RETRY_HOST_CHECK_INTERVAL;<host_name>;<check_interval>``

      Changes the retry check interval for a particular host.

CHANGE_RETRY_SVC_CHECK_INTERVAL
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``CHANGE_RETRY_SVC_CHECK_INTERVAL;<host_name>;<service_description>;<check_interval>``

      Changes the retry check interval for a particular service.

CHANGE_SVC_CHECK_COMMAND
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``CHANGE_SVC_CHECK_COMMAND;<host_name>;<service_description>;<check_command>``

      Changes the check command for a particular service to be that specified by the "check_command" option. The "check_command" option specifies the short name of the command that should be used as the new service check command. The command must have been configured in Alignak before it was last (re)started.

CHANGE_SVC_CHECK_TIMEPERIOD
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``CHANGE_SVC_CHECK_TIMEPERIOD;<host_name>;<service_description>;<check_timeperiod>``

      Changes the check timeperiod for a particular service to what is specified by the "check_timeperiod" option. The "check_timeperiod" option should be the short name of the timeperod that is to be used as the service check timeperiod. The timeperiod must have been configured in Alignak before it was last (re)started.

CHANGE_SVC_EVENT_HANDLER
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``CHANGE_SVC_EVENT_HANDLER;<host_name>;<service_description>;<event_handler_command>``

      Changes the event handler command for a particular service to be that specified by the "event_handler_command" option. The "event_handler_command" option specifies the short name of the command that should be used as the new service event handler. The command must have been configured in Alignak before it was last (re)started.

CHANGE_SVC_MODATTR
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``CHANGE_SVC_MODATTR;<host_name>;<service_description>;<value>``

      This command changes the modified attributes value for the specified service. Modified attributes values are used by Alignak to determine which object properties should be retained across program restarts. Thus, modifying the value of the attributes can affect data retention. This is an advanced option and should only be used by people who are intimately familiar with the data retention logic in Alignak.

CHANGE_SVC_NOTIFICATION_TIMEPERIOD
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``CHANGE_SVC_NOTIFICATION_TIMEPERIOD;<host_name>;<service_description>;<notification_timeperiod>``

      Changes the notification timeperiod for a particular service to what is specified by the "notification_timeperiod" option. The "notification_timeperiod" option should be the short name of the timeperiod that is to be used as the service notification timeperiod. The timeperiod must have been configured in Alignak before it was last (re)started.

DEL_ALL_HOST_COMMENTS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``DEL_ALL_HOST_COMMENTS;<host_name>``

      Deletes all comments associated with a particular host.

DEL_ALL_SVC_COMMENTS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``DEL_ALL_SVC_COMMENTS;<host_name>;<service_description>``

      Deletes all comments associated with a particular service.

DEL_HOST_COMMENT
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``DEL_HOST_COMMENT;<comment_id>``

      Deletes a host comment. The id number of the comment that is to be deleted must be specified.

DEL_DOWNTIME_BY_HOST_NAME
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

``DEL_DOWNTIME_BY_HOST_NAME;<host_name>[;<servicedesc>[;<starttime>[;<commentstring>]]]``

Deletes the host downtime entry and associated services for the host whose host_name matches the "host_name" argument. If the downtime is currently in effect, the host will come out of scheduled downtime (as long as there are no other overlapping active downtime entries). Please note that you can add more (optional) "filters" to limit the scope.

[Note]	Note
Changes provided by the Opsview team.

DEL_DOWNTIME_BY_HOSTGROUP_NAME
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

``DEL_DOWNTIME_BY_HOSTGROUP_NAME;<hostgroup_name>[;<hostname>[;<servicedesc>[;<starttime>[;<commentstring>]]]]``

Deletes the host downtime entries and associated services of all hosts of the host group matching the "hostgroup_name" argument. If the downtime is currently in effect, the host will come out of scheduled downtime (as long as there are no other overlapping active downtime entries). Please note that you can add more (optional) "filters" to limit the scope.

[Note]	Note
Changes provided by the Opsview team.

DEL_DOWNTIME_BY_START_TIME_COMMENT
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

``DEL_DOWNTIME_BY_START_TIME_COMMENT;<start time[;comment_string]>``

Deletes downtimes with start times matching the timestamp specified by the "start time" argument and an optional comment string.

[Note]	Note
Changes provided by the Opsview team.

DEL_HOST_DOWNTIME
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

``DEL_HOST_DOWNTIME;<downtime_id>``

Deletes the host downtime entry that has an ID number matching the "downtime_id" argument. If the downtime is currently in effect, the host will come out of scheduled downtime (as long as there are no other overlapping active downtime entries).

DEL_SVC_COMMENT
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

``DEL_SVC_COMMENT;<comment_id>``

Deletes a service comment. The id number of the comment that is to be deleted must be specified.

DEL_SVC_DOWNTIME
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

``DEL_SVC_DOWNTIME;<downtime_id>``

Deletes the service downtime entry that has an ID number matching the "downtime_id" argument. If the downtime is currently in effect, the service will come out of scheduled downtime (as long as there are no other overlapping active downtime entries).

DELAY_HOST_NOTIFICATION
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

``DELAY_HOST_NOTIFICATION;<host_name>;<notification_time>``

Delays the next notification for a particular host until "notification_time". The "notification_time" argument is specified in time_t format (seconds since the UNIX epoch). Note that this will only have an affect if the host stays in the same problem state that it is currently in. If the host changes to another state, a new notification may go out before the time you specify in the "notification_time" argument.

DELAY_SVC_NOTIFICATION
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``DELAY_SVC_NOTIFICATION;<host_name>;<service_description>;<notification_time>``

      Delays the next notification for a particular service until "notification_time". The "notification_time" argument is specified in time_t format (seconds since the UNIX epoch). Note that this will only have an affect if the service stays in the same problem state that it is currently in. If the service changes to another state, a new notification may go out before the time you specify in the "notification_time" argument.

DISABLE_ALL_NOTIFICATIONS_BEYOND_HOST
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``DISABLE_ALL_NOTIFICATIONS_BEYOND_HOST;<host_name>``

      Disables notifications for all hosts and services "beyond" (e.g. on all child hosts of) the specified host. The current notification setting for the specified host is not affected.

.. note:: this command is not currently implemented in Alignak

DISABLE_CONTACT_HOST_NOTIFICATIONS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``DISABLE_CONTACT_HOST_NOTIFICATIONS;<contact_name>``

      Disables host notifications for a particular contact.

DISABLE_CONTACT_SVC_NOTIFICATIONS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``DISABLE_CONTACT_SVC_NOTIFICATIONS;<contact_name>``

      Disables service notifications for a particular contact.

DISABLE_CONTACTGROUP_HOST_NOTIFICATIONS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    ``DISABLE_CONTACTGROUP_HOST_NOTIFICATIONS;<contactgroup_name>``

    Disables host notifications for all contacts in a particular contactgroup.

DISABLE_CONTACTGROUP_SVC_NOTIFICATIONS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    ``DISABLE_CONTACTGROUP_SVC_NOTIFICATIONS;<contactgroup_name>``

    Disables service notifications for all contacts in a particular contactgroup.

DISABLE_EVENT_HANDLERS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    ``DISABLE_EVENT_HANDLERS``

    Disables host and service event handlers on a program-wide basis.

DISABLE_FAILURE_PREDICTION
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``DISABLE_FAILURE_PREDICTION``

      Disables failure prediction on a program-wide basis.

DISABLE_FLAP_DETECTION
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``DISABLE_FLAP_DETECTION``

      Disables host and service flap detection on a program-wide basis.

DISABLE_HOST_AND_CHILD_NOTIFICATIONS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``DISABLE_HOST_AND_CHILD_NOTIFICATIONS;<host_name>``

      Disables notifications for the specified host, as well as all hosts "beyond" (e.g. on all child hosts of) the specified host.

.. note:: this command is not currently implemented in Alignak

DISABLE_HOST_CHECK
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``DISABLE_HOST_CHECK;<host_name>``

      Disables (regularly scheduled and on-demand) active checks of the specified host.

DISABLE_HOST_EVENT_HANDLER
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``DISABLE_HOST_EVENT_HANDLER;<host_name>``

      Disables the event handler for the specified host.

DISABLE_HOST_FLAP_DETECTION
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

``DISABLE_HOST_FLAP_DETECTION;<host_name>``

Disables flap detection for the specified host.

DISABLE_HOST_FRESHNESS_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

``DISABLE_HOST_FRESHNESS_CHECKS``

Disables freshness checks of all hosts on a program-wide basis.

DISABLE_HOST_NOTIFICATIONS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    ``DISABLE_HOST_NOTIFICATIONS;<host_name>``

    Disables notifications for a particular host.

DISABLE_HOST_SVC_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    ``DISABLE_HOST_SVC_CHECKS;<host_name>``

    Disables active checks of all services on the specified host.

DISABLE_HOST_SVC_NOTIFICATIONS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    ``DISABLE_HOST_SVC_NOTIFICATIONS;<host_name>``

    Disables notifications for all services on the specified host.

DISABLE_HOSTGROUP_HOST_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    ``DISABLE_HOSTGROUP_HOST_CHECKS;<hostgroup_name>``

    Disables active checks for all hosts in a particular hostgroup.

DISABLE_HOSTGROUP_HOST_NOTIFICATIONS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    ``DISABLE_HOSTGROUP_HOST_NOTIFICATIONS;<hostgroup_name>``

    Disables notifications for all hosts in a particular hostgroup. This does not disable notifications for the services associated with the hosts in the hostgroup - see the DISABLE_HOSTGROUP_SVC_NOTIFICATIONS command for that.

DISABLE_HOSTGROUP_PASSIVE_HOST_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    ``DISABLE_HOSTGROUP_PASSIVE_HOST_CHECKS;<hostgroup_name>``

    Disables passive checks for all hosts in a particular hostgroup.

DISABLE_HOSTGROUP_PASSIVE_SVC_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    ``DISABLE_HOSTGROUP_PASSIVE_SVC_CHECKS;<hostgroup_name>``

    Disables passive checks for all services associated with hosts in a particular hostgroup.

DISABLE_HOSTGROUP_SVC_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    ``DISABLE_HOSTGROUP_SVC_CHECKS;<hostgroup_name>``

    Disables active checks for all services associated with hosts in a particular hostgroup.

DISABLE_HOSTGROUP_SVC_NOTIFICATIONS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    ``DISABLE_HOSTGROUP_SVC_NOTIFICATIONS;<hostgroup_name>``

    Disables notifications for all services associated with hosts in a particular hostgroup. This does not disable notifications for the hosts in the hostgroup - see the DISABLE_HOSTGROUP_HOST_NOTIFICATIONS command for that.

DISABLE_NOTIFICATIONS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    ``DISABLE_NOTIFICATIONS``

    Disables host and service notifications on a program-wide basis.

DISABLE_NOTIFICATIONS_EXPIRE_TIME
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    ``DISABLE_NOTIFICATIONS_EXPIRE_TIME;<schedule_time>;<expire_time>``

    <schedule_time> has no effect currently, set it to current timestamp in your scripts.

    Disables host and service notifications on a program-wide basis, with given expire time.

DISABLE_PASSIVE_HOST_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    ``DISABLE_PASSIVE_HOST_CHECKS;<host_name>``

    Disables acceptance and processing of passive host checks for the specified host.

DISABLE_PASSIVE_SVC_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    ``DISABLE_PASSIVE_SVC_CHECKS;<host_name>;<service_description>``

    Disables passive checks for the specified service.

DISABLE_PERFORMANCE_DATA
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    ``DISABLE_PERFORMANCE_DATA``

    Disables the processing of host and service performance data on a program-wide basis.

DISABLE_SERVICE_FRESHNESS_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    ``DISABLE_SERVICE_FRESHNESS_CHECKS``

    Disables freshness checks of all services on a program-wide basis.

DISABLE_SERVICEGROUP_HOST_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    ``DISABLE_SERVICEGROUP_HOST_CHECKS;<servicegroup_name>``

    Disables active checks for all hosts that have services that are members of a particular servicegroup.

DISABLE_SERVICEGROUP_HOST_NOTIFICATIONS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    ``DISABLE_SERVICEGROUP_HOST_NOTIFICATIONS;<servicegroup_name>``

    Disables notifications for all hosts that have services that are members of a particular servicegroup.

DISABLE_SERVICEGROUP_PASSIVE_HOST_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    ``DISABLE_SERVICEGROUP_PASSIVE_HOST_CHECKS;<servicegroup_name>``

    Disables the acceptance and processing of passive checks for all hosts that have services that are members of a particular service group.

DISABLE_SERVICEGROUP_PASSIVE_SVC_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    ``DISABLE_SERVICEGROUP_PASSIVE_SVC_CHECKS;<servicegroup_name>``

    Disables the acceptance and processing of passive checks for all services in a particular servicegroup.

DISABLE_SERVICEGROUP_SVC_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``DISABLE_SERVICEGROUP_SVC_CHECKS;<servicegroup_name>``

      Disables active checks for all services in a particular servicegroup.

DISABLE_SERVICEGROUP_SVC_NOTIFICATIONS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``DISABLE_SERVICEGROUP_SVC_NOTIFICATIONS;<servicegroup_name>``

      Disables notifications for all services that are members of a particular servicegroup.

DISABLE_SVC_CHECK
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``DISABLE_SVC_CHECK;<host_name>;<service_description>``

      Disables active checks for a particular service.

DISABLE_SVC_EVENT_HANDLER
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``DISABLE_SVC_EVENT_HANDLER;<host_name>;<service_description>``

      Disables the event handler for the specified service.

DISABLE_SVC_FLAP_DETECTION
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``DISABLE_SVC_FLAP_DETECTION;<host_name>;<service_description>``

      Disables flap detection for the specified service.

DISABLE_SVC_NOTIFICATIONS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``DISABLE_SVC_NOTIFICATIONS;<host_name>;<service_description>``

Disables notifications for a particular service.

ENABLE_ALL_NOTIFICATIONS_BEYOND_HOST
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``ENABLE_ALL_NOTIFICATIONS_BEYOND_HOST;<host_name>``

Enables notifications for all hosts and services "beyond" (e.g. on all child hosts of) the specified host. The current notification setting for the specified host is not affected. Notifications will only be sent out for these hosts and services if notifications are also enabled on a program-wide basis.

.. note:: this command is not currently implemented in Alignak

ENABLE_CONTACT_HOST_NOTIFICATIONS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``ENABLE_CONTACT_HOST_NOTIFICATIONS;<contact_name>``

Enables host notifications for a particular contact.

ENABLE_CONTACT_SVC_NOTIFICATIONS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``ENABLE_CONTACT_SVC_NOTIFICATIONS;<contact_name>``

      Disables service notifications for a particular contact.

ENABLE_CONTACTGROUP_HOST_NOTIFICATIONS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``ENABLE_CONTACTGROUP_HOST_NOTIFICATIONS;<contactgroup_name>``

      Enables host notifications for all contacts in a particular contactgroup.

ENABLE_CONTACTGROUP_SVC_NOTIFICATIONS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``ENABLE_CONTACTGROUP_SVC_NOTIFICATIONS;<contactgroup_name>``

      Enables service notifications for all contacts in a particular contactgroup.

ENABLE_EVENT_HANDLERS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``ENABLE_EVENT_HANDLERS``

      Enables host and service event handlers on a program-wide basis.

ENABLE_FAILURE_PREDICTION
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``ENABLE_FAILURE_PREDICTION``

      Enables failure prediction on a program-wide basis.

ENABLE_FLAP_DETECTION
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``ENABLE_FLAP_DETECTION``

      Enables host and service flap detection on a program-wide basis.

ENABLE_HOST_AND_CHILD_NOTIFICATIONS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``ENABLE_HOST_AND_CHILD_NOTIFICATIONS;<host_name>``

      Enables notifications for the specified host, as well as all hosts "beyond" (e.g. on all child hosts of) the specified host. Notifications will only be sent out for these hosts if notifications are also enabled on a program-wide basis.

.. note:: this command is not currently implemented in Alignak

ENABLE_HOST_CHECK
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``ENABLE_HOST_CHECK;<host_name>``

      Enables (regularly scheduled and on-demand) active checks of the specified host.

ENABLE_HOST_EVENT_HANDLER
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``ENABLE_HOST_EVENT_HANDLER;<host_name>``

      Enables the event handler for the specified host.

ENABLE_HOST_FLAP_DETECTION
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``ENABLE_HOST_FLAP_DETECTION;<host_name>``

      Enables flap detection for the specified host. In order for the flap detection algorithms to be run for the host, flap detection must be enabled on a program-wide basis as well.

ENABLE_HOST_FRESHNESS_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``ENABLE_HOST_FRESHNESS_CHECKS``

      Enables freshness checks of all hosts on a program-wide basis. Individual hosts that have freshness checks disabled will not be checked for freshness.

ENABLE_HOST_NOTIFICATIONS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``ENABLE_HOST_NOTIFICATIONS;<host_name>``

      Enables notifications for a particular host. Notifications will be sent out for the host only if notifications are enabled on a program-wide basis as well.

ENABLE_HOST_SVC_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``ENABLE_HOST_SVC_CHECKS;<host_name>``

      Enables active checks of all services on the specified host.

ENABLE_HOST_SVC_NOTIFICATIONS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``ENABLE_HOST_SVC_NOTIFICATIONS;<host_name>``

      Enables notifications for all services on the specified host. Note that notifications will not be sent out if notifications are disabled on a program-wide basis.

ENABLE_HOSTGROUP_HOST_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``ENABLE_HOSTGROUP_HOST_CHECKS;<hostgroup_name>``

Enables active checks for all hosts in a particular hostgroup.

ENABLE_HOSTGROUP_HOST_NOTIFICATIONS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``ENABLE_HOSTGROUP_HOST_NOTIFICATIONS;<hostgroup_name>``

Enables notifications for all hosts in a particular hostgroup. This does not enable notifications for the services associated with the hosts in the hostgroup - see the ENABLE_HOSTGROUP_SVC_NOTIFICATIONS command for that. In order for notifications to be sent out for these hosts, notifications must be enabled on a program-wide basis as well.

ENABLE_HOSTGROUP_PASSIVE_HOST_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``ENABLE_HOSTGROUP_PASSIVE_HOST_CHECKS;<hostgroup_name>``

Enables passive checks for all hosts in a particular hostgroup.

ENABLE_HOSTGROUP_PASSIVE_SVC_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``ENABLE_HOSTGROUP_PASSIVE_SVC_CHECKS;<hostgroup_name>``

Enables passive checks for all services associated with hosts in a particular hostgroup.

ENABLE_HOSTGROUP_SVC_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``ENABLE_HOSTGROUP_SVC_CHECKS;<hostgroup_name>``

Enables active checks for all services associated with hosts in a particular hostgroup.

ENABLE_HOSTGROUP_SVC_NOTIFICATIONS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``ENABLE_HOSTGROUP_SVC_NOTIFICATIONS;<hostgroup_name>``

Enables notifications for all services that are associated with hosts in a particular hostgroup. This does not enable notifications for the hosts in the hostgroup - see the ENABLE_HOSTGROUP_HOST_NOTIFICATIONS command for that. In order for notifications to be sent out for these services, notifications must be enabled on a program-wide basis as well.

ENABLE_NOTIFICATIONS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``ENABLE_NOTIFICATIONS``

Enables host and service notifications on a program-wide basis.

ENABLE_PASSIVE_HOST_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``ENABLE_PASSIVE_HOST_CHECKS;<host_name>``

Enables acceptance and processing of passive host checks for the specified host.

ENABLE_PASSIVE_SVC_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``ENABLE_PASSIVE_SVC_CHECKS;<host_name>;<service_description>``

Enables passive checks for the specified service.

ENABLE_PERFORMANCE_DATA
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``ENABLE_PERFORMANCE_DATA``

Enables the processing of host and service performance data on a program-wide basis.

ENABLE_SERVICE_FRESHNESS_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``ENABLE_SERVICE_FRESHNESS_CHECKS``

Enables freshness checks of all services on a program-wide basis. Individual services that have freshness checks disabled will not be checked for freshness.

ENABLE_SERVICEGROUP_HOST_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``ENABLE_SERVICEGROUP_HOST_CHECKS;<servicegroup_name>``

Enables active checks for all hosts that have services that are members of a particular servicegroup.

ENABLE_SERVICEGROUP_HOST_NOTIFICATIONS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``ENABLE_SERVICEGROUP_HOST_NOTIFICATIONS;<servicegroup_name>``

Enables notifications for all hosts that have services that are members of a particular servicegroup. In order for notifications to be sent out for these hosts, notifications must also be enabled on a program-wide basis.

ENABLE_SERVICEGROUP_PASSIVE_HOST_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``ENABLE_SERVICEGROUP_PASSIVE_HOST_CHECKS;<servicegroup_name>``

Enables the acceptance and processing of passive checks for all hosts that have services that are members of a particular service group.

ENABLE_SERVICEGROUP_PASSIVE_SVC_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``ENABLE_SERVICEGROUP_PASSIVE_SVC_CHECKS;<servicegroup_name>``

Enables the acceptance and processing of passive checks for all services in a particular servicegroup.

ENABLE_SERVICEGROUP_SVC_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``ENABLE_SERVICEGROUP_SVC_CHECKS;<servicegroup_name>``

Enables active checks for all services in a particular servicegroup.

ENABLE_SERVICEGROUP_SVC_NOTIFICATIONS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``ENABLE_SERVICEGROUP_SVC_NOTIFICATIONS;<servicegroup_name>``

Enables notifications for all services that are members of a particular servicegroup. In order for notifications to be sent out for these services, notifications must also be enabled on a program-wide basis.

ENABLE_SVC_CHECK
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``ENABLE_SVC_CHECK;<host_name>;<service_description>``

Enables active checks for a particular service.

ENABLE_SVC_EVENT_HANDLER
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``ENABLE_SVC_EVENT_HANDLER;<host_name>;<service_description>``

Enables the event handler for the specified service.

ENABLE_SVC_FLAP_DETECTION
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``ENABLE_SVC_FLAP_DETECTION;<host_name>;<service_description>``

Enables flap detection for the specified service. In order for the flap detection algorithms to be run for the service, flap detection must be enabled on a program-wide basis as well.

ENABLE_SVC_NOTIFICATIONS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``ENABLE_SVC_NOTIFICATIONS;<host_name>;<service_description>``

      Enables notifications for a particular service. Notifications will be sent out for the service only if notifications are enabled on a program-wide basis as well.

LAUNCH_HOST_EVENT_HANDLER
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``LAUNCH_HOST_EVENT_HANDLER;<host_name>``

      Runs the event handler for the specified host.

LAUNCH_SVC_EVENT_HANDLER
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``LAUNCH_SVC_EVENT_HANDLER;<host_name>;<service_description>``

      Runs the event handler for the specified service.

PROCESS_FILE
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``PROCESS_FILE;<file_name>;<delete>``

      Directs Alignak to process all external commands that are found in the file specified by the <file_name> argument. If the <delete> option is non-zero, the file will be deleted once it has been processes. If the <delete> option is set to zero, the file is left untouched.

.. note:: this command is not currently implemented in Alignak

PROCESS_HOST_CHECK_RESULT
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``PROCESS_HOST_CHECK_RESULT;<host_name>;<status_code>;<plugin_output>``

      This is used to submit a passive check result for a particular host. The "status_code" indicates the state of the host check and should be one of the following: 0=UP, 1=DOWN, 2=UNREACHABLE. The "plugin_output" argument contains the text returned from the host check, along with optional performance data.

PROCESS_SERVICE_CHECK_RESULT
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``PROCESS_SERVICE_CHECK_RESULT;<host_name>;<service_description>;<return_code>;<plugin_output>``

      This is used to submit a passive check result for a particular service. The "return_code" field should be one of the following: 0=OK, 1=WARNING, 2=CRITICAL, 3=UNKNOWN. The "plugin_output" field contains text output from the service check, along with optional performance data.

READ_STATE_INFORMATION
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``READ_STATE_INFORMATION``

      Causes Alignak to load all current monitoring status information from the state retention file. Normally, state retention information is loaded when the Alignak process starts up and before it starts monitoring. WARNING: This command will cause Alignak to discard all current monitoring status information and use the information stored in state retention file! Use with care.

.. note:: this command is not currently implemented in Alignak

RELOAD_CONFIG
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``RELOAD_CONFIG``

      Reloads the Alignak monitoring configuration.


REMOVE_HOST_ACKNOWLEDGEMENT
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``REMOVE_HOST_ACKNOWLEDGEMENT;<host_name>``

This removes the problem acknowledgement for a particular host. Once the acknowledgement has been removed, notifications can once again be sent out for the given host.

REMOVE_SVC_ACKNOWLEDGEMENT
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``REMOVE_SVC_ACKNOWLEDGEMENT;<host_name>;<service_description>``

This removes the problem acknowledgement for a particular service. Once the acknowledgement has been removed, notifications can once again be sent out for the given service.

RESTART_PROGRAM
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``RESTART_PROGRAM``

      Restarts the Alignak daemons.

SAVE_STATE_INFORMATION
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``SAVE_STATE_INFORMATION``

      Causes Alignak to save all current monitoring status information to the state retention file. Normally, state retention information is saved before the Alignak process shuts down and (potentially) at regularly scheduled intervals. This command allows you to force Alignak to save this information to the state retention file immediately. This does not affect the current status information in the Alignak process.

.. note:: this command is not currently implemented in Alignak

SCHEDULE_AND_PROPAGATE_HOST_DOWNTIME
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``SCHEDULE_AND_PROPAGATE_HOST_DOWNTIME;<host_name>;<start_time>;<end_time>;<fixed>;<trigger_id>;<duration>;<author>;<comment>``

      Schedules downtime for a specified host and all of its children (hosts). If the "fixed" argument is set to one (1), downtime will start and end at the times specified by the "start" and "end" arguments. Otherwise, downtime will begin between the "start" and "end" times and last for "duration" seconds. The "start" and "end" arguments are specified in time_t format (seconds since the UNIX epoch). The specified (parent) host downtime can be triggered by another downtime entry if the "trigger_id" is set to the ID of another scheduled downtime entry. Set the "trigger_id" argument to zero (0) if the downtime for the specified (parent) host should not be triggered by another downtime entry.

.. note:: this command is not currently implemented in Alignak

SCHEDULE_AND_PROPAGATE_TRIGGERED_HOST_DOWNTIME
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``SCHEDULE_AND_PROPAGATE_TRIGGERED_HOST_DOWNTIME;<host_name>;<start_time>;<end_time>;<fixed>;<trigger_id>;<duration>;<author>;<comment>``

      Schedules downtime for a specified host and all of its children (hosts). If the "fixed" argument is set to one (1), downtime will start and end at the times specified by the "start" and "end" arguments. Otherwise, downtime will begin between the "start" and "end" times and last for "duration" seconds. The "start" and "end" arguments are specified in time_t format (seconds since the UNIX epoch). Downtime for child hosts are all set to be triggered by the downtime for the specified (parent) host. The specified (parent) host downtime can be triggered by another downtime entry if the "trigger_id" is set to the ID of another scheduled downtime entry. Set the "trigger_id" argument to zero (0) if the downtime for the specified (parent) host should not be triggered by another downtime entry.

.. note:: this command is not currently implemented in Alignak

SCHEDULE_FORCED_HOST_CHECK
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``SCHEDULE_FORCED_HOST_CHECK;<host_name>;<check_time>``

Schedules a forced active check of a particular host at "check_time". The "check_time" argument is specified in time_t format (seconds since the UNIX epoch). Forced checks are performed regardless of what time it is (e.g. timeperiod restrictions are ignored) and whether or not active checks are enabled on a host-specific or program-wide basis.

SCHEDULE_FORCED_HOST_SVC_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``SCHEDULE_FORCED_HOST_SVC_CHECKS;<host_name>;<check_time>``

Schedules a forced active check of all services associated with a particular host at "check_time". The "check_time" argument is specified in time_t format (seconds since the UNIX epoch). Forced checks are performed regardless of what time it is (e.g. timeperiod restrictions are ignored) and whether or not active checks are enabled on a service-specific or program-wide basis.

SCHEDULE_FORCED_SVC_CHECK
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``SCHEDULE_FORCED_SVC_CHECK;<host_name>;<service_description>;<check_time>``

Schedules a forced active check of a particular service at "check_time". The "check_time" argument is specified in time_t format (seconds since the UNIX epoch). Forced checks are performed regardless of what time it is (e.g. timeperiod restrictions are ignored) and whether or not active checks are enabled on a service-specific or program-wide basis.

SCHEDULE_HOST_CHECK
~~~~~~~~~~~~~~~~~~~

   ``SCHEDULE_HOST_CHECK;<host_name>;<check_time>``

Schedules the next active check of a particular host at "check_time". The "check_time" argument is specified in time_t format (seconds since the UNIX epoch). Note that the host may not actually be checked at the time you specify. This could occur for a number of reasons: active checks are disabled on a program-wide or host-specific basis, the host is already scheduled to be checked at an earlier time, etc. If you want to force the host check to occur at the time you specify, look at the SCHEDULE_FORCED_HOST_CHECK command.

SCHEDULE_HOST_DOWNTIME
~~~~~~~~~~~~~~~~~~~~~~

   ``SCHEDULE_HOST_DOWNTIME;<host_name>;<start_time>;<end_time>;<fixed>;<trigger_id>;<duration>;<author>;<comment>``

      Schedules a downtime for a specified host.

      If the "fixed" argument is set to one (1), the downtime will start and end at the times specified by the "start" and "end" arguments. Otherwise, the downtime will begin between the "start" and "end" times and will last for "duration" seconds.

      The "start" and "end" arguments are specified in time_t format (seconds since the UNIX epoch). The specified host downtime can be triggered by another downtime entry if the "trigger_id" is set to the ID of another scheduled downtime entry. Set the "trigger_id" argument to zero (0) if the downtime for the specified host should not be triggered by another downtime entry.

**Note** Alignak will automatically set an acknowledge on the downtimed host when the downtime is scheduled. Thereby, the host problem and the host services problems will be acknowledged.


SCHEDULE_HOST_SVC_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~

   ``SCHEDULE_HOST_SVC_CHECKS;<host_name>;<check_time>``

      Schedules the next active check of all services on a particular host at "check_time". The "check_time" argument is specified in time_t format (seconds since the UNIX epoch). Note that the services may not actually be checked at the time you specify. This could occur for a number of reasons: active checks are disabled on a program-wide or service-specific basis, the services are already scheduled to be checked at an earlier time, etc. If you want to force the service checks to occur at the time you specify, look at the SCHEDULE_FORCED_HOST_SVC_CHECKS command.

SCHEDULE_HOST_SVC_DOWNTIME
~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``SCHEDULE_HOST_SVC_DOWNTIME;<host_name>;<start_time>;<end_time>;<fixed>;<trigger_id>;<duration>;<author>;<comment>``

      Schedules downtime for all services associated with a particular host. If the "fixed" argument is set to one (1), downtime will start and end at the times specified by the "start" and "end" arguments. Otherwise, downtime will begin between the "start" and "end" times and last for "duration" seconds. The "start" and "end" arguments are specified in time_t format (seconds since the UNIX epoch). The service downtime entries can be triggered by another downtime entry if the "trigger_id" is set to the ID of another scheduled downtime entry. Set the "trigger_id" argument to zero (0) if the downtime for the services should not be triggered by another downtime entry.

SCHEDULE_HOSTGROUP_HOST_DOWNTIME
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``SCHEDULE_HOSTGROUP_HOST_DOWNTIME;<hostgroup_name>;<start_time>;<end_time>;<fixed>;<trigger_id>;<duration>;<author>;<comment>``

      Schedules downtime for all hosts in a specified hostgroup. If the "fixed" argument is set to one (1), downtime will start and end at the times specified by the "start" and "end" arguments. Otherwise, downtime will begin between the "start" and "end" times and last for "duration" seconds. The "start" and "end" arguments are specified in time_t format (seconds since the UNIX epoch). The host downtime entries can be triggered by another downtime entry if the "trigger_id" is set to the ID of another scheduled downtime entry. Set the "trigger_id" argument to zero (0) if the downtime for the hosts should not be triggered by another downtime entry.

SCHEDULE_HOSTGROUP_SVC_DOWNTIME
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``SCHEDULE_HOSTGROUP_SVC_DOWNTIME;<hostgroup_name>;<start_time>;<end_time>;<fixed>;<trigger_id>;<duration>;<author>;<comment>``

      Schedules downtime for all services associated with hosts in a specified hostgroup. If the "fixed" argument is set to one (1), downtime will start and end at the times specified by the "start" and "end" arguments. Otherwise, downtime will begin between the "start" and "end" times and last for "duration" seconds. The "start" and "end" arguments are specified in time_t format (seconds since the UNIX epoch). The service downtime entries can be triggered by another downtime entry if the "trigger_id" is set to the ID of another scheduled downtime entry. Set the "trigger_id" argument to zero (0) if the downtime for the services should not be triggered by another downtime entry.

SCHEDULE_SERVICEGROUP_HOST_DOWNTIME
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``SCHEDULE_SERVICEGROUP_HOST_DOWNTIME;<servicegroup_name>;<start_time>;<end_time>;<fixed>;<trigger_id>;<duration>;<author>;<comment>``

      Schedules downtime for all hosts that have services in a specified servicegroup. If the "fixed" argument is set to one (1), downtime will start and end at the times specified by the "start" and "end" arguments. Otherwise, downtime will begin between the "start" and "end" times and last for "duration" seconds. The "start" and "end" arguments are specified in time_t format (seconds since the UNIX epoch). The host downtime entries can be triggered by another downtime entry if the "trigger_id" is set to the ID of another scheduled downtime entry. Set the "trigger_id" argument to zero (0) if the downtime for the hosts should not be triggered by another downtime entry.

SCHEDULE_SERVICEGROUP_SVC_DOWNTIME
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``SCHEDULE_SERVICEGROUP_SVC_DOWNTIME;<servicegroup_name>;<start_time>;<end_time>;<fixed>;<trigger_id>;<duration>;<author>;<comment>``

      Schedules downtime for all services in a specified servicegroup. If the "fixed" argument is set to one (1), downtime will start and end at the times specified by the "start" and "end" arguments. Otherwise, downtime will begin between the "start" and "end" times and last for "duration" seconds. The "start" and "end" arguments are specified in time_t format (seconds since the UNIX epoch). The service downtime entries can be triggered by another downtime entry if the "trigger_id" is set to the ID of another scheduled downtime entry. Set the "trigger_id" argument to zero (0) if the downtime for the services should not be triggered by another downtime entry.

SCHEDULE_SVC_CHECK
~~~~~~~~~~~~~~~~~~

   ``SCHEDULE_SVC_CHECK;<host_name>;<service_description>;<check_time>``

      Schedules the next active check of a specified service at "check_time". The "check_time" argument is specified in time_t format (seconds since the UNIX epoch). Note that the service may not actually be checked at the time you specify. This could occur for a number of reasons: active checks are disabled on a program-wide or service-specific basis, the service is already scheduled to be checked at an earlier time, etc. If you want to force the service check to occur at the time you specify, look at the SCHEDULE_FORCED_SVC_CHECK command.

SCHEDULE_SVC_DOWNTIME
~~~~~~~~~~~~~~~~~~~~~

   ``SCHEDULE_SVC_DOWNTIME;<host_name>;<service_description>;<start_time>;<end_time>;<fixed>;<trigger_id>;<duration>;<author>;<comment>``

      Schedules downtime for a specified service. If the "fixed" argument is set to one (1), downtime will start and end at the times specified by the "start" and "end" arguments. Otherwise, downtime will begin between the "start" and "end" times and last for "duration" seconds. The "start" and "end" arguments are specified in time_t format (seconds since the UNIX epoch). The specified service downtime can be triggered by another downtime entry if the "trigger_id" is set to the ID of another scheduled downtime entry. Set the "trigger_id" argument to zero (0) if the downtime for the specified service should not be triggered by another downtime entry.

**Note** Alignak will automatically set an acknowledge on the downtimed service.


SEND_CUSTOM_HOST_NOTIFICATION
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``SEND_CUSTOM_HOST_NOTIFICATION;<host_name>;<options>;<author>;<comment>``

      Allows you to send a custom host notification. Very useful in dire situations, emergencies or to communicate with all admins that are responsible for a particular host. When the host notification is sent out, the $NOTIFICATIONTYPE$ macro will be set to "CUSTOM". The <options> field is a logical OR of the following integer values that affect aspects of the notification that are sent out: 0 = No option (default), 1 = Broadcast (send notification to all normal and all escalated contacts for the host), 2 = Forced (notification is sent out regardless of current time, whether or not notifications are enabled, etc.), 4 = Increment current notification # for the host (this is not done by default for custom notifications). The contents of the comment field is available in notification commands using the $NOTIFICATIONCOMMENT$ macro.

.. note:: this command is not currently implemented in Alignak

SEND_CUSTOM_SVC_NOTIFICATION
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``SEND_CUSTOM_SVC_NOTIFICATION;<host_name>;<service_description>;<options>;<author>;<comment>``

      Allows you to send a custom service notification. Very useful in dire situations, emergencies or to communicate with all admins that are responsible for a particular service. When the service notification is sent out, the $NOTIFICATIONTYPE$ macro will be set to "CUSTOM". The <options> field is a logical OR of the following integer values that affect aspects of the notification that are sent out: 0 = No option (default), 1 = Broadcast (send notification to all normal and all escalated contacts for the service), 2 = Forced (notification is sent out regardless of current time, whether or not notifications are enabled, etc.), 4 = Increment current notification # for the service(this is not done by default for custom notifications). The contents of the comment field is available in notification commands using the $NOTIFICATIONCOMMENT$ macro.

.. note:: this command is not currently implemented in Alignak

SET_HOST_NOTIFICATION_NUMBER
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``SET_HOST_NOTIFICATION_NUMBER;<host_name>;<notification_number>``

      Sets the current notification number for a particular host. A value of 0 indicates that no notification has yet been sent for the current host problem. Useful for forcing an escalation (based on notification number) or replicating notification information in redundant monitoring environments. Notification numbers greater than zero have no noticeable affect on the notification process if the host is currently in an UP state.

.. note:: this command is not currently implemented in Alignak

SET_SVC_NOTIFICATION_NUMBER
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``SET_SVC_NOTIFICATION_NUMBER;<host_name>;<service_description>;<notification_number>``

      Sets the current notification number for a particular service. A value of 0 indicates that no notification has yet been sent for the current service problem. Useful for forcing an escalation (based on notification number) or replicating notification information in redundant monitoring environments. Notification numbers greater than zero have no noticeable affect on the notification process if the service is currently in an OK state.

.. note:: this command is not currently implemented in Alignak

SHUTDOWN_PROGRAM
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``SHUTDOWN_PROGRAM``

      Shuts down the Alignak process.

.. note:: this command is not currently implemented in Alignak

START_ACCEPTING_PASSIVE_HOST_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``START_ACCEPTING_PASSIVE_HOST_CHECKS``

      Enables acceptance and processing of passive host checks on a program-wide basis.

START_ACCEPTING_PASSIVE_SVC_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``START_ACCEPTING_PASSIVE_SVC_CHECKS``

Enables passive service checks on a program-wide basis.

START_EXECUTING_HOST_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``START_EXECUTING_HOST_CHECKS``

Enables active host checks on a program-wide basis.

START_EXECUTING_SVC_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``START_EXECUTING_SVC_CHECKS``

Enables active checks of services on a program-wide basis.

START_OBSESSING_OVER_HOST
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``START_OBSESSING_OVER_HOST;<host_name>``

Enables processing of host checks via the OCHP command for the specified host.

START_OBSESSING_OVER_HOST_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``START_OBSESSING_OVER_HOST_CHECKS``

Enables processing of host checks via the OCHP command on a program-wide basis.

START_OBSESSING_OVER_SVC
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``START_OBSESSING_OVER_SVC;<host_name>;<service_description>``

Enables processing of service checks via the OCSP command for the specified service.

START_OBSESSING_OVER_SVC_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``START_OBSESSING_OVER_SVC_CHECKS``

Enables processing of service checks via the OCSP command on a program-wide basis.

STOP_ACCEPTING_PASSIVE_HOST_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``STOP_ACCEPTING_PASSIVE_HOST_CHECKS``

Disables acceptance and processing of passive host checks on a program-wide basis.

STOP_ACCEPTING_PASSIVE_SVC_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``STOP_ACCEPTING_PASSIVE_SVC_CHECKS``

Disables passive service checks on a program-wide basis.

STOP_EXECUTING_HOST_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``STOP_EXECUTING_HOST_CHECKS``

      Disables active host checks on a program-wide basis.

STOP_EXECUTING_SVC_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``STOP_EXECUTING_SVC_CHECKS``

      Disables active checks of services on a program-wide basis.

STOP_OBSESSING_OVER_HOST
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``STOP_OBSESSING_OVER_HOST;<host_name>``

      Disables processing of host checks via the OCHP command for the specified host.

STOP_OBSESSING_OVER_HOST_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``STOP_OBSESSING_OVER_HOST_CHECKS````

      Disables processing of host checks via the OCHP command on a program-wide basis.

STOP_OBSESSING_OVER_SVC
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``STOP_OBSESSING_OVER_SVC;<host_name>;<service_description>``

      Disables processing of service checks via the OCSP command for the specified service.

STOP_OBSESSING_OVER_SVC_CHECKS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ``STOP_OBSESSING_OVER_SVC_CHECKS``

      Disables processing of service checks via the OCSP command on a program-wide basis.
