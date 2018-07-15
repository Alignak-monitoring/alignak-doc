.. _configuration/time_saving_tricks:


Time-Saving tricks for objects definition
=========================================


Services
--------


Same service on several hosts
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Identical services assigned to several hosts can be specified with a list of hosts names in the ``host_name`` service property.

::

    define service{
        host_name                HOST1,HOST2,HOST3,...,HOSTN
        service_description      SOMESERVICE
        other service properties ...
    }


Same service on hosts in multiple hostgroups
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Identical services assigned to all the hosts in one or more hostgroups can be specified with a list of hosts groups names in the ``hostgroup_name`` service property.

::

    define service{
        hostgroup_name          HOSTGROUP1,HOSTGROUP2,...,HOSTGROUPN
        service_description     SOMESERVICE
        #other service properties ...
    }


Same service on all hosts
~~~~~~~~~~~~~~~~~~~~~~~~~

Identical services assigned to all the hosts in your monitoring configuration is as simple as:

::

    define service{
        host_name               *
        service_description     SOMESERVICE
        # other service properties ...
    }


Excluding Hosts
~~~~~~~~~~~~~~~

If you want to create identical services on numerous hosts or hostgroups, but would like to exclude some hosts from the definition, this can be accomplished by preceding the host or hostgroup with a ``!`` symbol.


::

    define service{
        host_name             HOST1,HOST2,!HOST3,!HOST4,...,HOSTN
        hostgroup_name        HOSTGROUP1,HOSTGROUP2,!HOSTGROUP3,!HOSTGROUP4,...,HOSTGROUPN
        service_description   SOMESERVICE
        # other service properties ...
    }



Service escalations
-------------------


Multiple hosts
~~~~~~~~~~~~~~

The same rules as the one used for the services may be used for the service escalations definitions. Specify several hosts, hosts from hosts groups, all hosts, and hosts exclusions apply on service escalations.


All services on the same host
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you want to create service escalations for all the services of a particular host, you can use a wildcard in the ``service_description`` property. The definition below will create a service escalation for all the services of the host *HOST1*.

::

    define serviceescalation{
        host_name               HOST1
        service_description     *
        # other escalation properties ...
    }


Several services on the same host
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Using a service name list in the ``service_description`` property of an escalation will assign this escalation to the specified services of the host defined in the ``host_name`` property.

::

    define serviceescalation{
        host_name               HOST1
        service_description     SERVICE1,SERVICE2,...,SERVICEN
        # other escalation properties ...
    }


All the services in several services groups
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Specifying a list of services groups names in the ``servicegroup_name`` property will target all the services defined in the specified groups.


::

    define serviceescalation{
        servicegroup_name          SERVICEGROUP1,SERVICEGROUP2,...,SERVICEGROUPN
        # other escalation properties ...
    }



Services dependencies
---------------------


Several hosts
~~~~~~~~~~~~~

To create service dependencies for services with the same ``service_description`` that are assigned to multiple hosts, you can specify multiple hosts in the ``host_name`` and/or ``dependent_host_name`` properties. In the example below, service *SERVICE2* on hosts *HOST3* and *HOST4* will be dependent of service *SERVICE1* on hosts *HOST1* and *HOST2*.

::

    define servicedependency{
        host_name                       HOST1,HOST2
        service_description             SERVICE1
        dependent_host_name             HOST3,HOST4
        dependent_service_description   SERVICE2
        # other dependency properties ...
    }


All hosts in multiple hostgroups
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you want to create service dependencies for services with the same ``service_description`` that are assigned to all hosts in one or more hosts groups, you can use the ``hostgroup_name`` and/or ``dependent_hostgroup_name`` properties. In the example below, service *SERVICE2* on all hosts in hosts groups *HOSTGROUP3* and *HOSTGROUP4* will be dependent on service *SERVICE1* on all hosts in hostgroups *HOSTGROUP1* and *HOSTGROUP2*.

.. note: Assuming there were five hosts in each of the hostgroups, this definition would be equivalent to creating 100 single service dependency definitions !


::

  define servicedependency{
      hostgroup_name                  HOSTGROUP1,HOSTGROUP2
      service_description             SERVICE1
      dependent_hostgroup_name        HOSTGROUP3,HOSTGROUP4
      dependent_service_description   SERVICE2
      # other dependency properties ...
  }


All services on an host
~~~~~~~~~~~~~~~~~~~~~~~

If you want to create service dependencies for all the services assigned to a specific host, you can use a wildcard in the ``service_description`` and/or ``dependent_service_description`` properties. In the example below, all services on host *HOST2* will be dependent on all services on host *HOST1*.


::

  define servicedependency{
      host_name                       HOST1
      service_description             *
      dependent_host_name             HOST2
      dependent_service_description   *
      # other dependency properties ...
  }


Several services on an host
~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you want to create service dependencies for several services assigned to a specific host, you can specify more than one service description in the ``service_description`` and/or ``dependent_service_description`` properties as follows:

::

  define servicedependency{
      host_name                       HOST1
      service_description             SERVICE1,SERVICE2,...,SERVICEN
      dependent_host_name             HOST2
      dependent_service_description   SERVICE1,SERVICE2,...,SERVICEN
      # other dependency properties ...
  }


All services in several services groups
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you want to create service dependencies for all services that belong to one or more services groups, you can use the ``servicegroup_name`` and/or ``dependent_servicegroup_name`` properties as follows:


::

  define servicedependency{
      servicegroup_name               SERVICEGROUP1,SERVICEGROUP2,...,SERVICEGROUPN
      dependent_servicegroup_name     SERVICEGROUP3,SERVICEGROUP4,...SERVICEGROUPN
      other dependency properties ...
  }


Same host dependencies
~~~~~~~~~~~~~~~~~~~~~~

If you want to create service dependencies for multiple services that are dependent on other services on the same host, leave the ``dependent_host_name`` and ``dependent_hostgroup_name`` properties empty. The example below assumes that hosts *HOST1* and *HOST2* have at least the following four services associated with them: *SERVICE1*, *SERVICE2*, *SERVICE3*, and *SERVICE4*. In this example, *SERVICE3* and *SERVICE4* on *HOST1* will be dependent on both *SERVICE1* and *SERVICE2* on *HOST1*. Similarly, the same dependencies will exist for the corresponding services on *HOTS2*.


::

  define servicedependency{
      host_name                       HOST1,HOST2
      service_description             SERVICE1,SERVICE2
      dependent_service_description   SERVICE3,SERVICE4
      other dependency properties ...
  }


Hosts escalations
-----------------


Several hosts
~~~~~~~~~~~~~

To create host escalations for multiple hosts, specify several hosts in the ``host_name`` property.

::

  define hostescalation{
      host_name              HOST1,HOST2,HOST3,...,HOSTN
      # other escalation properties ...
  }

.. note: specifying a ``*`` in the ``host_name`` property will apply the escalation on all the monitored hosts.

All hosts in several hosts groups
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To create host escalations for all hosts in one or more hostgroups, use the ``hostgroup_name`` property.

::

  define hostescalation{
      hostgroup_name            HOSTGROUP1,HOSTGROUP2,...,HOSTGROUPN
      # other escalation properties ...
  }


Excluding some hosts
~~~~~~~~~~~~~~~~~~~~

If you want to create identical host escalations on several hosts or hostgroups, but you wish to exclude some hosts from the definition, you can prepend the host or hostgroup with a ``!`` symbol.


::

  define hostescalation{
      host_name             HOST1,HOST2,!HOST3,!HOST4,...,HOSTN
      hostgroup_name        HOSTGROUP1,HOSTGROUP2,!HOSTGROUP3,!HOSTGROUP4,...,HOSTGROUPN
      # other escalation properties ...
  }



Hosts dependencies
------------------


Several hosts
~~~~~~~~~~~~~

If you want to create host dependencies for several hosts, you can specify multiple hosts in the ``host_name`` and/or ``dependent_host_name`` properties. The definition below would be equivalent to creating six separate host dependencies. In the example above, hosts HOST3, HOST4 and HOST5 would be dependent upon both HOST1 and HOST2.
::

  define hostdependency{
      host_name               HOST1,HOST2
      dependent_host_name     HOST3,HOST4,HOST5
      other dependency properties ...
  }


All hosts in several hosts groups
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you want to create host dependencies for all hosts in one or more hostgroups, you can use the ``hostgroup_name`` and /or ``dependent_hostgroup_name`` properties. In the example below, all hosts in hostgroups HOSTGROUP3 and HOSTGROUP4 would be dependent on all hosts in hostgroups HOSTGROUP1 and HOSTGROUP2.

::

  define hostdependency{
      hostgroup_name                  HOSTGROUP1,HOSTGROUP2
      dependent_hostgroup_name        HOSTGROUP3,HOSTGROUP4
      other dependency properties ...
  }



Hosts groups
------------


All hosts
~~~~~~~~~

If you want to create an hosts group that group all hosts defined in your monitored objects, you can use a wildcard in the ``members`` directive. The definition below will create an hostgroup called *HOSTGROUP1* that has all hosts as members.


::

  define hostgroup{
      hostgroup_name          HOSTGROUP1
      members                 *
      # other hostgroup properties ...
  }

