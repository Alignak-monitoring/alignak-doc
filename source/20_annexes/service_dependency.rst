.. _monitoring_objects/servicedependency:

==============================
Service Dependency Definition
==============================

Service dependencies are an advanced feature of Alignak that allow you to suppress notifications and active checks of services based on the status of one or more other services. Service dependencies are optional and are mainly targeted at advanced users who have complicated monitoring setups. More information on how service dependencies work (read this!) can be found :ref:`here <monitoring_features/dependencies>`.

Syntax
======

Bold variables are required, while others are optional.
Emphasized variables are Alignak extensions with reference to the Nagios legacy definition.

================================= =========================
define servicedependency{
**dependent_host_name**           ***host_name***
dependent_hostgroup_name          *hostgroup_name*
**dependent_service_description** ***service_description***
**host_name**                     ***host_name***
hostgroup_name                    *hostgroup_name*
**service_description**           ***service_description***
inherits_parent                   [0/1]
execution_failure_criteria        [o,w,u,c,p,n]
notification_failure_criteria     [o,w,u,c,p,n]
dependency_period                 timeperiod_name
}
================================= =========================


Example
=======

::

  define servicedependency{
      host_name                       WWW1
      service_description             Apache Web Server
      dependent_host_name             WWW1
      dependent_service_description   Main Web Site
      execution_failure_criteria      n
      notification_failure_criteria   w,u,c
  }


Variables
=========

dependent_host_name
  This directive is used to identify the *short name(s)* of the :ref:`host(s) <monitoring_objects/host>` that the *dependent* service “runs" on or is associated with. Multiple hosts should be separated by commas. Leaving this directive blank can be used to create :ref:`“same host" dependencies <configuration/time_saving_tricks#same_host_dependencies>`.

dependent_hostgroup
  This directive is used to specify the *short name(s)* of the :ref:`hostgroup(s) <monitoring_objects/hostgroup>` that the *dependent* service "runs" on or is associated with. Multiple hostgroups should be separated by commas. The "dependent_hostgroup" may be used instead of, or in addition to, the "dependent_host" directive.

dependent_service_description
  This directive is used to identify the *description* of the *dependent* :ref:`service <monitoring_objects/service>`.

host_name
  This directive is used to identify the *short name(s)* of the :ref:`host(s) <monitoring_objects/host>` that the service *that is being depended upon* (also referred to as the master service) "runs" on or is associated with. Multiple hosts should be separated by commas.

hostgroup_name
  This directive is used to identify the *short name(s)* of the :ref:`hostgroup(s) <monitoring_objects/host>` that the service *that is being depended upon* (also referred to as the master service) "runs" on or is associated with. Multiple hostgroups should be separated by commas. The "hostgroup_name" may be used instead of, or in addition to, the "host_name" directive.

service_description
  This directive is used to identify the *description* of the :ref:`service <monitoring_objects/service>` *that is being depended upon* (also referred to as the master service).

inherits_parent
  This directive indicates whether or not the dependency inherits dependencies of the service *that is being depended upon* (also referred to as the master service). In other words, if the master service is dependent upon other services and any one of those dependencies fail, this dependency will also fail.

execution_failure_criteria
  This directive is used to specify the criteria that determine when the dependent service should *not* be actively checked. If the *master* service is in one of the failure states we specify, the *dependent* service will not be actively checked. Valid options are a combination of one or more of the following (multiple options are separated with commas):

    * **o** = fail on an OK state
    * **w** = fail on a WARNING state
    * **u** = fail on an UNKNOWN state
    * **c** = fail on a CRITICAL state
    * **p** = fail on a pending state (e.g. the service has not yet been checked).
    * **n** (none) : the execution dependency will never fail and checks of the dependent service will always be actively checked (if other conditions allow for it to be).

  If you specify **o,c,u** in this field, the *dependent* service will not be actively checked if the *master* service is in either an OK, a CRITICAL, or an UNKNOWN state.

notification_failure_criteria
  This directive is used to define the criteria that determine when notifications for the dependent service should *not* be sent out. If the *master* service is in one of the failure states we specify, notifications for the *dependent* service will not be sent to contacts. Valid options are a combination of one or more of the following:

    * **o** = fail on an OK state
    * **w** = fail on a WARNING state
    * **u** = fail on an UNKNOWN state
    * **c** = fail on a CRITICAL state
    * **p** = fail on a pending state (e.g. the service has not yet been checked).
    * **n** = (none) : the notification dependency will never fail and notifications for the dependent service will always be sent out.

  If you specify **w** in this field, the notifications for the *dependent* service will not be sent out if the *master* service is in a WARNING state.

dependency_period
  This directive is used to specify the short name of the :ref:`time period <monitoring_objects/timeperiod>` during which this dependency is valid. If this directive is not specified, the dependency is considered to be valid during all times.
