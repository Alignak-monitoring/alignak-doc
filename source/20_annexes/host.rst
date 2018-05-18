.. _monitoring_objects/host:

===============
Host Definition
===============

An host definition is used to define a physical server, workstation, device, etc. that resides on your network.


Syntax
======

Bold variables are required, while others are optional.
Emphasized variables are Alignak extensions with reference to the Nagios legacy definition.


========================================== ======================================
define host{
**host_name**                               **host_name**
alias                                       alias
display_name                                display_name
**address**                                 **address**
parents                                     host_names
hostgroups                                  hostgroup_names
check_command                               command_name
initial_state                               [o,d,u]
initial_output                              output
**max_check_attempts**                      **#**
check_interval                              #
retry_interval                              #
active_checks_enabled                       [0/1]
passive_checks_enabled                      [0/1]
check_period                                *timeperiod_name*
check_freshness                             [0/1]
freshness_threshold                         #
``freshness_state``                         ``[o,d,x]``
event_handler                               command_name
event_handler_enabled                       [0/1]
flap_detection_enabled                      [0/1]
flap_detection_options                      [o,d,x]
low_flap_threshold                          #
high_flap_threshold                         #
process_perf_data                           [0/1]
**contacts**                                **contacts**
**contact_groups**                          **contact_groups**
**notification_interval**                   **#**
first_notification_delay                    #
**notification_period**                         **timeperiod_name**
notification_options                        [d,u,r,f,s]
notifications_enabled                       [0/1]
stalking_options                            [o,d,u]
notes                                       *note_string*
notes_url                                   *url*
action_url                                  *url*
``realm``                                   ``realm``
``poller_tag``                              ``poller_tag``
``reactionner_tag``                         ``reactionner_tag``
``business_impact``                         ``[0/1/2/3/4/5]``
resultmodulations                           *resultmodulations*
escalations                                 *escalations names*
business_impact_modulations                 *business_impact_modulations names*
maintenance_period                          *timeperiod_name*
service_overrides                           *service_description,directive value*
service_excludes                            *service_description,...*
service_includes                            *service_description,...*
labels                                      *labels*
business_rule_output_template               *template*
business_rule_smart_notifications           [0/1]
business_rule_downtime_as_ack               [0/1]
business_rule_host_notification_options     [d,u,r,f,s]
business_rule_service_notification_options  [w,u,c,r,f,s]
snapshot_enabled                            [0/1]
snapshot_command                            *command_name*
snapshot_period                             *timeperiod_name*
snapshot_criteria                           [d,u]
snapshot_interval                           #
}
========================================== ======================================


Example
=======

 ::

  define host{
         host_name                      bogus-router
         alias                          Bogus Router #1
         address                        192.168.1.254
         parents                        server-backbone
         check_command                  check-host-alive
         check_interval                 5
         retry_interval                 1
         max_check_attempts             5
         check_period                   24x7
         process_perf_data              0
         retain_nonstatus_information   0
         contact_groups                 router-admins
         notification_interval          30
         notification_period            24x7
         notification_options           d,u,r
         realm                          Europe
         poller_tag                     DMZ
         icon_set                       server
         }


Variables
=========

host_name
  This directive is used to define a short name used to identify the host. It is used in host group and service definitions to reference this particular host. Hosts can have multiple services (which are monitored) associated with them. When used properly, the $HOSTNAME$ :ref:`macro <monitoring_features/macros>` will contain this short name.

alias
  This directive is used to define a longer name or description used to identify the host. It is provided in order to allow you to more easily identify a particular host. When used properly, the $HOSTALIAS$ :ref:`macro <monitoring_features/macros>` will contain this alias/description.

address
  This directive is used to define the address of the host. Normally, this is an IP address, although it could really be anything you want (so long as it can be used to check the status of the host). You can use a FQDN to identify the host instead of an IP address, but if "DNS" services are not available this could cause problems. When used properly, the $HOSTADDRESS$ :ref:`macro <monitoring_features/macros>` will contain this address.

.. note :: If you do not specify an address directive in a host definition, the name of the host will be used as its address. A word of caution about doing this, however - if "DNS" fails, most of your service checks will fail because the plugins will be unable to resolve the host name.

display_name
  This directive is used to define an alternate name that should be displayed in the web interface for this host. If not specified, this defaults to the value you specify for the *host_name* directive.

parents
  This directive is used to define a comma-delimited list of short names of the "parent" hosts for this particular host. Parent hosts are typically routers, switches, firewalls, etc. that lie between the monitoring host and a remote hosts. A router, switch, etc. which is closest to the remote host is considered to be that host's "parent". Read the "Determining Status and Reachability of Network Hosts" document located :ref:`here <monitoring_features/network_reachability>` for more information. If this host is on the same network segment as the host doing the monitoring (without any intermediate routers, etc.) the host is considered to be on the local network and will not have a parent host. Leave this value blank if the host does not have a parent host (i.e. it is on the same segment as the Alignak host). The order in which you specify parent hosts has no effect on how things are monitored.

hostgroups
  This directive is used to identify the *short name(s)* of the :ref:`hostgroup(s) <monitoring_objects/hostgroup>` that the host belongs to. Multiple hostgroups should be separated by commas. This directive may be used as an alternative to (or in addition to) using the *members* directive in :ref:`hostgroup <monitoring_objects/hostgroup>` definitions.

check_command
  This directive is used to specify the *short name* of the :ref:`command <monitoring_objects/command>` that should be used to check if the host is up or down. Typically, this command would try and ping the host to see if it is "alive". The command must return a status of OK (0) or Alignak will assume the host is down. If you leave this argument blank, the host will *not* be actively checked. Thus, Alignak will likely always assume the host is up (it may show up as being in a "PENDING" state in the web interface). This is useful if you are monitoring printers or other devices that are frequently turned off. The maximum amount of time that the notification command can run is controlled by the :ref:`host_check_timeout <configuration/core#host_check_timeout>` option.

initial_state
  By default Alignak will assume that all hosts are in PENDING state when in starts. You can override the initial state for a host by using this directive. Valid options are: **o** = UP, **d** = DOWN, and **u** = UNREACHABLE.

initial_output
  As of the initial state, the initial check output may also be overridden by this directive.

max_check_attempts
  This directive is used to define the number of times that Alignak will retry the host check command if it returns any state other than an OK state. Setting this value to 1 will cause Alignak to generate an alert without retrying the host check again.

  If you do not want to check the status of the host, you must still set this to a minimum value of 1. To bypass the host check, just leave the "check_command" option blank.


check_interval
  This directive is used to define the number of “time units" between periodical scheduled checks of the host. Unless you've changed the ``interval_length`` global variable from the default value of 60, this number will mean minutes.

retry_interval
  This directive is used to define the number of “time units" to wait before scheduling a re-check of the hosts. Hosts are rescheduled at the retry interval when they have changed to a non-UP state. Once the host has been retried **max_check_attempts** times without a change in its status, it will revert to being scheduled at its “normal" rate as defined by the **check_interval** value. Unless you've changed the ``interval_length`` global variable from the default value of 60, this number will mean minutes.

active_checks_enabled
  This directive is used to determine whether or not active checks (either regularly scheduled or on-demand) of this host are enabled. Values: 0 = disable active host checks, 1 = enable active host checks.

passive_checks_enabled
  This directive is used to determine whether or not passive checks are enabled for this host. Values: 0 = disable passive host checks, 1 = enable passive host checks.

check_period
  This directive is used to specify the short name of the time period during which active checks of this host can be made.

check_freshness
  This directive is used to determine whether or not freshness checks are enabled for this host. Values: 0 = disable freshness checks, 1 = enable freshness checks.

freshness_threshold
  This directive is used to specify the freshness threshold (in seconds) for this host. If you set this directive to a value of 0, Alignak will determine a freshness threshold to use automatically.

event_handler
  This directive is used to specify the name of the command that should be run whenever a change in the state of the host is detected (i.e. whenever it goes down or recovers). Read the documentation on :ref:`event handlers <monitoring_features/event_handlers>` for a more detailed explanation of how to write scripts for handling events. The maximum amount of time that the event handler command can run is controlled by the ``event_handler_timeout`` option.

event_handler_enabled
  This directive is used to determine whether or not the event handler for this host is enabled. Values: 0 = disable host event handler, 1 = enable host event handler.

low_flap_threshold
  This directive is used to specify the low state change threshold used in flap detection for this host. More information on flap detection can be found :ref:`here <monitoring_features/flapping>`. If you set this directive to a value of 0, the program-wide value specified by the ``low_host_flap_threshold`` directive will be used.

high_flap_threshold
  This directive is used to specify the high state change threshold used in flap detection for this host. More information on flap detection can be found :ref:`here <monitoring_features/flapping>`. If you set this directive to a value of 0, the program-wide value specified by the ``high_host_flap_threshold`` directive will be used.

flap_detection_enabled
  This directive is used to determine whether or not flap detection is enabled for this host. More information on flap detection can be found :ref:`here <monitoring_features/flapping>`. Values: 0 = disable host flap detection, 1 = enable host flap detection.

flap_detection_options
  This directive is used to determine what host states the :ref:`flap detection logic <monitoring_features/flapping>` will use for this host. Valid options are a combination of one or more of the following: **o** = UP states, **d** = DOWN states, **u** = UNREACHABLE states.

process_perf_data
  This directive is used to determine whether or not the processing of performance data is enabled for this host. Values: 0 = disable performance data processing, 1 = enable performance data processing.

contacts
  This is a list of the *short names* of the :ref:`contacts <monitoring_objects/contact>` that should be notified whenever there are problems (or recoveries) with this host. Multiple contacts should be separated by commas. Useful if you want notifications to go to just a few people and don't want to configure :ref:`contact groups <monitoring_objects/contactgroup>`. You must specify at least one contact or contact group in each host definition.

contact_groups
  This is a list of the *short names* of the :ref:`contact groups <monitoring_objects/contactgroup>` that should be notified whenever there are problems (or recoveries) with this host. Multiple contact groups should be separated by commas. You must specify at least one contact or contact group in each host definition.

notification_interval
  This directive is used to define the number of “time units" to wait before re-notifying a contact that this service is *still* down or unreachable. Unless you've changed the ``interval_length`` global variable from the default value of 60, this number will mean minutes. If you set this value to 0, Alignak will *not* re-notify contacts about problems for this host - only one problem notification will be sent out.

first_notification_delay
  This directive is used to define the number of “time units" to wait before sending out the first problem notification when this host enters a non-UP state. Unless you've changed the ``interval_length`` global variable from the default value of 60, this number will mean minutes. If you set this value to 0, Alignak will start sending out notifications immediately.

notification_period
  This directive is used to specify the short name of the time period during which notifications of events for this host can be sent out to contacts. If a host goes down, becomes unreachable, or recoveries during a time which is not covered by the time period, no notifications will be sent out.

notification_options
  This directive is used to determine when notifications for the host should be sent out. Valid options are a combination of one or more of the following: **d** = send notifications on a DOWN state, **u** = send notifications on an UNREACHABLE state, **r** = send notifications on recoveries (OK state), **f** = send notifications when the host starts and stops :ref:`flapping <monitoring_features/flapping>`, and **s** = send notifications when :ref:`scheduled downtime <monitoring_features/downtime>` starts and ends. If you specify **n** (none) as an option, no host notifications will be sent out. If you do not specify any notification options, Alignak will assume that you want notifications to be sent out for all possible states.

  If you specify **d,r** in this field, notifications will only be sent out when the host goes DOWN and when it recovers from a DOWN state.


notifications_enabled
  This directive is used to determine whether or not notifications for this host are enabled. Values: 0 = disable host notifications, 1 = enable host notifications.

stalking_options
  This directive determines which host states "stalking" is enabled for. Valid options are a combination of one or more of the following: **o** = stalk on UP states, **d** = stalk on DOWN states, and **u** = stalk on UNREACHABLE states. More information on state stalking can be found :ref:`here <monitoring_features/stalking>`.

notes
  This directive is used to define an optional string of notes pertaining to the host. If you specify a note here, you will see the it in the extended information CGI (when you are viewing information about the specified host).

notes_url
  This variable is used to define an optional URL that can be used to provide more information about the host. If you specify an URL, you will see a red folder icon in the CGIs (when you are viewing host information) that links to the URL you specify here. Any valid URL can be used. If you plan on using relative paths, the base path will the the same as what is used to access the CGIs (i.e. ///cgi-bin/Alignak///). This can be very useful if you want to make detailed information on the host, emergency contact methods, etc. available to other support staff.

action_url
  This directive is used to define one or more optional URL that can be used to provide more actions to be performed on the host. If you specify an URL, you will see a red “splat" icon in the CGIs (when you are viewing host information) that links to the URL you specify here. Any valid URL can be used. If you plan on using relative paths, the base path will the the same as what is used to access the CGIs (i.e. */cgi-bin/Alignak/*).

realm
  This variable is used to define the :ref:`realm <monitoring_objects/realm>` where the host will be put. By putting the host in a realm, it will be manage by one of the scheduler of this realm.

poller_tag
  This variable is used to define the poller_tag of the host. All checks of this hosts will only take by pollers that have this value in their poller_tags parameter.

  By default the poller_tag value is 'None', so all untagged pollers can take it because None is set by default for them.

reactionner_tag
  This variable is used to define the reactionner_tag of notifications_commands from this service. All of theses notifications will be taken by reactionners that have this value in their reactionner_tags parameter.

  By default there is no reactionner_tag, so all untagged reactionners can take it.

business_impact
  This variable is used to set the importance we gave to this host for the business from the less important (0 = nearly nobody will see if it's in error) to the maximum (5 = you lost your job if it fail). The default value is 2.

resultmodulations
  This variable is used to link with resultmodulations  objects. It will allow such modulation to apply, like change a warning in critical for this host.

escalations
  This variable is used to link with escalations objects. It will allow such escalations rules to apply. Look at escalations objects for more details.

business_impact_modulations
  This variable is used to link with business_impact_modulations objects. It will allow such modulation to apply (for example if the host is a payment server, it will be important only in a specific timeperiod: near the pay day). Look at business_impact_modulations objects for more details.

maintenance_period
  Alignak-specific variable to specify a recurring downtime period. This works like a scheduled downtime, so unlike a check_period with exclusions, checks will still be made.

service_overrides
  This variable may be used to override services directives for a specific host. This is especially useful when services are inherited (for instance from packs), because it allows to have a host attached service set one of its directives a specific value. For example, on a set of web servers, **HTTP** service (inherited from **http** pack) on *production* servers should have notifications enabled **24x7**, and *staging* server should only notify during **workhours**. To do so, staging server should be set the following directive: **service_overrides HTTP,notification_period workhours**. Several overrides may be specified, each override should be written on a single line. *Caution*, *service_overrides* may be inherited (through the **use** directive), but specifying an override on a host overloads all values inherited from parent hosts, it does not append it (as of any single valued attribute). See :ref:`inheritance description<configuration/objects_inheritance>` for more details.

service_excludes
  This variable may be used to *exclude* a service from a host. It addresses the situations where a set of services is inherited from a pack or attached from a hostgroup, and an identified host should **NOT** have one (or more, comma separated) services defined. This allows to manage exceptions in the service assignment without having to define intermediary templates/hostgroups. See :ref:`inheritance description<configuration/objects_inheritance>` for more details.
  This will be **ignored** if there is *service_includes*

service_includes
  This variable may be used to *include only* a service from a host. It addresses the situations where a set of services is inherited from a pack or attached from a hostgroup, and an identified host should **have only** one (or more, comma separated) services defined. This allows to manage exceptions in the service assignment without having to define intermediary templates/hostgroups. See :ref:`inheritance description<configuration/objects_inheritance>` for more details.
  This variable is considered **before** *service_excludes*

labels
  This variable may be used to place arbitrary labels (separated by comma character). Those labels may be used in other configuration objects such as :ref:`business rules <alignak_features/business_rules>` grouping expressions.

business_rule_output_template
  Classic host check output is managed by the underlying plugin (the check output is the plugin stdout). For :ref:`business rules <alignak_features/business_rules>`, as there's no real plugin behind, the output may be controlled by a template string defined in ``business_rule_output_template directive``.

business_rule_smart_notifications
  This variable may be used to activate smart notifications on :ref:`business rules <alignak_features/business_rules>`. This allows to stop sending notification if all underlying problems have been acknowledged.

business_rule_smart_notifications
  By default, downtimes are not taken into account by :ref:`business rules <alignak_features/business_rules>` smart notifications processing. This variable allows to extend smart notifications to underlying hosts or service checks under downtime (they are treated as if they were acknowledged).

business_rule_host_notification_options
  This option allows to enforce :ref:`business rules <alignak_features/business_rules>` underlying hosts notification options to easily compose a consolidated meta check. This is especially useful for business rules relying on grouping expansion.

business_rule_service_notification_options
  This option allows to enforce :ref:`business rules <alignak_features/business_rules>` underlying services notification options to easily compose a consolidated meta check. This is especially useful for business rules relying on grouping expansion.

snapshot_enabled
  This option allows to enable snapshots on this element.

snapshot_command
  Command to launch when a snapshot launch occurs

snapshot_period
  Timeperiod when the snapshot call is allowed

snapshot_criteria
  List of states that enable the snapshot launch. Mainly bad states.

snapshot_interval
  Minimum interval between two launch of snapshots to not hammering the host, in interval_length units (by default 60s) :)

