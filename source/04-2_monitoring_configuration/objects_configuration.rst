.. _configuration/objects_configuration:

Monitored objects
=================


Monitored objects are all the elements that are involved in the monitoring and notification logic. Alignak has several types of objects that include:

  * Hosts
  * Services
  * Contacts
  * Groups: hosts, services, contacts
  * Commands
  * Time Periods
  * Notification Escalations
  * Execution Dependencies
  * Modulations

The monitored objects can be defined in one or more configuration files and/or directories that are specified using the :ref:`cfg_file <configuration/core#cfg_file>` and/or :ref:`cfg_dir <configuration/core#cfg_dir>` properties in the :ref:`core configuration <configuration/core>`.

.. note: defining the monitored objects in some configuration flat files is the plain old Nagios/Shinken way of defining the configuration. Alignak introduces a brand new solution thanks to its backend. For more information, see :ref:̀̀`extending/alignak_backend`.

The monitored objects are defined with a flexible template format, which can make it much easier to manage your Alignak configuration in the long term. You can create object definitions that inherit properties from other object definitions.

Inheritance mechanism and some advanced tips and tricks may be found below.


Objects explained
=================

Hosts
~~~~~

Hosts are one the main objects in the monitoring logic. Important attributes of hosts are as follows:

    * Hosts are usually physical devices on your network (servers, workstations, routers, switches, printers, etc).
    * Hosts have an address of some kind (e.g. an IP or MAC address).
    * Hosts have one or more more services associated with them.
    * Hosts can have parent/child relationships with other hosts, often representing real-world network connections, which is used in the :ref:`network reachability logic<monitoring_features/network_reachability>`.


Host Groups
~~~~~~~~~~~

Hosts groups are logical groups of one or more hosts. They make it easier to:
  - view the status of related hosts in the Alignak web interface and
  - simplify your configuration through the use of :ref:`object tricks <configuration/objects_advanced>`.


Services
~~~~~~~~

Services are associated with hosts and can be:

  * Attributes of a host (CPU load, disk usage, uptime, etc.)
  * Services provided by the host ("HTTP", "POP3", "FTP", "SSH", etc.)
  * Other things associated with the host ("DNS" records, etc.)


Contacts
~~~~~~~~

Contacts are people involved in the notification process:

  * they have one or more notification methods (cellphone, pager, email, instant messaging, etc.)
  * they receive notifications for hosts and service they are responsible of


Groups
~~~~~~

Groups are logical groups of hosts, services, contacts that make it easier to configure common properties or inheritance.


Timeperiods
~~~~~~~~~~~

Time periods are used to control:

    * When hosts and services can be monitored
    * When contacts can receive notifications


Commands
~~~~~~~~

Commands define the actions to perform for:

    * Host and service checks
    * Notifications
    * Event handlers
    * and more...


.. _configuration/objects_definition:

Objects definition
==================

Alignak objects definiion follow the standard and well known Nagios/shinken rules. They will not be developed more in this document :)

The detail for each type of object definition is available :ref:`in this document annexes<annexes/index>`.


.. _configuration/objects_inheritance:

Objects inheritance
===================


Basics
~~~~~~


Three properties are affecting recursion and inheritance and they are present in all object definitions.


::

   define someobjecttype{
       name            template_name
       use             name_of_template_to_use
       register        [0/1]

       object-specific variables ...

   }

``name`` is only the **template** name that will be referenced in other object definitions so they can inherit the template defined properties/variables. Template names must be unique amongst objects of the same type, so you can't have two or more host definitions that have the same ``name`` property.

``use`` specifies the ``name`` of the templates that you want to inherit properties/variables from. The name(s) you specify in this property must be defined as another object's template ``name``.

``register`` is used to indicate whether or not the object definition should be *registered*. By default, all object definitions are registered as real objects. If you are creating a partial object definition as a template, you would want to prevent it from being registered as a real object, so you will need to set register as 0. Values are as follows: 0 = do NOT register object definition, 1 = register object definition (this is the default). 

.. note: the ``use`` property is never inherited



Local properties vs. inherited properties
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


The *local* object properties always take precedence over the properties defined in the inherited templates objects. Take a look at the following example of two host definitions (not all required variables have been supplied):


::

    define host {
        host_name               host1
        check_command           check-host-alive
        notification_options    d,u,r
        max_check_attempts      5
        name                    hosttemplate1
    }
    
    define host {
        host_name               host2
        max_check_attempts      3
        use                     hosttemplate1
    }

*host1* is defined as a template named *hosttemplate1*. *host2* is inheriting from the template *hosttemplate1*. 

Once those definition are parsed by Alignak, the resulting definition of host *host2* will be equivalent to this definition:


::

    define host{
        host_name               host2
        check_command           check-host-alive
        notification_options    d,u,r
        max_check_attempts      3
    }

The ``check_command`` and ``notification_options`` properties were inherited from the *hosttemplate1* template. However, the ``host_name`` and ``max_check_attempts`` variables were not inherited because they were yet defined locally in *host2*. 


Inheritance chaining
~~~~~~~~~~~~~~~~~~~~

Objects can inherit properties from multiple levels of template objects. 

::

    define host{
         host_name               host1
         check_command           check-host-alive
         notification_options    d,u,r
         max_check_attempts      5
         name                    hosttemplate1
    }

    define host{
         host_name               host2
         max_check_attempts      3
         use                     hosttemplate1
         name                    hosttemplate2
    }

    define host{
         host_name               host3
         use                     hosttemplate2
    }

*host3* inherits from *host2*, which inherits from *host1*. 

Once those definition are parsed by Alignak, the resulting configuration will be equivalent to this definition:

::

    define host{
        host_name               host1
        check_command           check-host-alive
        notification_options    d,u,r
        max_check_attempts      5
    }
    
    define host{
        host_name               host2
        check_command           check-host-alive
        notification_options    d,u,r
        max_check_attempts      3
    }
    
    define host{
        host_name               host3
        check_command           check-host-alive
        notification_options    d,u,r
        max_check_attempts      3
    }

.. note: There is no inherent limit on the inheritance deepness, but too much levels may become very complex to maintain.



Partial object definitions as templates
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

It is possible to use incomplete object definitions as templates to be used by other object definitions. A partial definition means that all mandatory properties in the object are not supplied in the object definition. 

As an example:

::

   define host{
      check_command           check-host-alive
      notification_options    d,u,r
      max_check_attempts      5

      name                    base-host
      register                0
   }
   
   define host{
      host_name               host1
      address                 192.168.1.3
      use                     base-host
   }
   
   define host{
      host_name               host2
      address                 192.168.1.4
      use                     base-host
   }

Note that the first definition is not complete it is missing the required ``host_name`` property. We don't need to supply a host name because we just want to use this definition as a generic host template. In order to prevent this definition from being registered with Shinken as a normal host, we set the ``register`` property as 0.

The definitions of hosts *host1* and *host2* inherit their properties from the *base-host* template. The only variable we have choosen to override is the ``address`` variable. Which means that both hosts will have the exact same properties, except for their ``host_name`` and ``address`` properties.

Once those definition are parsed by Alignak, the resulting configuration will be equivalent to this definition:


::

   define host{
      host_name               host1
      address                 192.168.1.3
      check_command           check-host-alive
      notification_options    d,u,r
      max_check_attempts      5
   }

   define host{
      host_name               host2
      address                 192.168.1.4
      check_command           check-host-alive
      notification_options    d,u,r
      max_check_attempts      5
   }

Using a template definition for default properties saves a lot of typing ;)



Custom variables inheritance
~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Custom objects variables that are defined in an host, service, or contact template will be inherited just like other standard variables. Take the following example:


::

   define host{
      name                    base-host
      register                0

      _customvar1             somevalue  ; <-- Custom host variable
      _snmp_community         public  ; <-- Custom host variable
   }

   define host{
      host_name               host1
      address                 192.168.1.3
      use                     base-host
   }

*host1* will inherit the custom host variables ``_customvar1`` and ``_snmp_community``, as well as their respective values, from the *base-host* template.


Stopping properties inheritance
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Sometimes you may not want your host, service, or contact definition to inherit values of some properties from their templates. To stop inheritance for a property, you can specify **null** as the value of the property that you do not want to inherit.


::

   define host{
      event_handler           my-event-handler-command
      name                    base-host
      register                0
   }

   define host{
      host_name               host1
      address                 192.168.1.3
      event_handler           null
      use                     base-host
   }

The host *host1* will not inherit the value of the ``event_handler`` property that is defined in the *base-host*.

.. _advanced/objectinheritance#add_string:

Additive inheritance
~~~~~~~~~~~~~~~~~~~~


By default, Alignak gives preference to local properties instead of inherited properties. Sometimes, it makes sense to use the values of inherited and local properties together.

The *additive inheritance* can be accomplished by prepending the local variable value with a plus sign (+). This feature is only available for standard (non custom) properties that contain string values.

As an example:

::

   define host{
      name                    base-host
      hostgroups              all-servers
      register                0
   }

   define host{
      host_name              linuxserver1
      hostgroups             +linux-servers,web-servers
      use                    base-host
   }

The host *linuxserver1* will append the value of its local ``hostgroups`` variable to the one inherited from *base-host*. The resulting definition of *linuxserver1* is as following:


::

   define host{
      host_name              linuxserver1
      hostgroups             all-servers,linux-servers,web-servers
   }



Implied inheritance
~~~~~~~~~~~~~~~~~~~


Usually you have to either explicitly specify the value of a required property in an object definition or inherit it from a template. There are some exceptions to this rule, where Alignak will assume that you want to use a value that comes from a related object.

For example, the values of some service variables will be copied from the host the service is associated with if you don't explicitely specify them.

The following table lists the object variables that will be implicitly inherited from related objects if you don't explicitly specify their value in your object definition or inherit them from a template.



======================= ============================================================ =====================================================
Object Type             Object Variable                                              Implied Source
**Services**            *contact_groups*                                             *contact_groups* in the associated host definition
*notification_interval* *notification_interval* in the associated host definition
*notification_period*   *notification_period* in the associated host definition
*check_period*          *check_period* in the associated host definition
**Host Escalations**    *contact_groups*                                             *contact_groups* in the associated host definition
*notification_interval* *notification_interval* in the associated host definition
*escalation_period*     *notification_period* in the associated host definition
**Service Escalations** *contact_groups*                                             *contact_groups* in the associated service definition
*notification_interval* *notification_interval* in the associated service definition
*escalation_period*     *notification_period* in the associated service definition
======================= ============================================================ =====================================================



Implied/additive inheritance in escalations
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Service and host escalation definitions can make use of a special rule that combines the features of implied and additive inheritance.

If escalations

   1) do not inherit the values of their ``contact_groups`` or ``contacts`` properties from another escalation template and
   2) their ``contact_groups`` or ``contacts`` properties begin with a plus sign (+),

then the values of their corresponding host or service definition's ``contact_groups`` or ``contacts`` properties will be used in the additive inheritance logic.

Confused? Here's an example:


::

   define host{
      name                    linux-server
      contact_groups          linux-admins
      ...
   }

   define hostescalation{
      host_name               linux-server
      contact_groups          +management
      ...
   }


is equivalent to:

::

   define hostescalation{
      host_name               linux-server
      contact_groups          linux-admins,management
      ...
   }



Multiple inheritance sources
~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Thus far, all examples of inheritance have shown object definitions inheriting properties from a single source template. You are also able to inherit variables/values from multiple templates for more complex configurations, as shown below.


::

   # Generic host template
   define host{
      name                    generic-host
      active_checks_enabled   1
      check_interval          10
      register                0
   }


::

   # Development web server template
   define host{
      name                    development-server
      check_interval          15
      notification_options    d,u,r
      ...
      register                0
   }


::

   # Development web server
   define host{
      use                    generic-host,development-server
      host_name              devweb1
      ...
   }



.. image:: /_static/images/official/images/multiple-templates1.png
   :scale: 90 %



In the example above, *devweb1* is inheriting properties from the templates: *generic-host* and *development-server*. ``check_interval`` is defined in both templates. Since *generic-host* is the first template specified in *devweb1*'s ``use`` property, its value is the one retained for the ``check_interval`` of *devweb1*. After inheritance, the effective definition of *devweb1* would be as follows:

::

   # Development web serve
   define host{
      host_name               devweb1
      active_checks_enabled   1
      check_interval          10
      notification_options    d,u,r
      ...
   }


Precedence with multiple inheritance sources
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When using multiple inheritance templates, the property from the first specified template is the one that wil be retained. Since templates can themselves inherit properties from one or more other templates, it can get tricky to figure out which property takes precedence.


Consider the following host definition that references three templates:

::

   # Development web server
   define host{
      use        1, 4, 8
      host_name  devweb1
      ...
   }

If some of the referenced templates themselves inherit properties from one or more other templates, the precedence rules are shown below.

.. image:: /_static/images///official/images/multiple-templates2.png
   :scale: 90 %



Inheritance overriding
~~~~~~~~~~~~~~~~~~~~~~

Inheritance is a core feature allowing to factorize configuration. It is possible from a host or a service template to build a very large set of checks with relatively few lines. The drawback of this approach is that it requires all hosts or services to be consistent. But if it is easy to instanciate new hosts with their own definitions attributes sets, it is generally more complicated with services, because the order of magnitude is larger (hosts * services per host), and because few attributes may come from the host. This is is especially true for packs, which is a generalization of the inheritance usage.

If some hosts require special properties for the services they are hosting (values that are different from those defined at template level), it is generally necessary to define new service.

Imagine two web servers clusters, one for the frontend, the other for the backend, where the frontend servers should notify any HTTP service in ``CRITICAL`` and ``WARNING`` state, and backend servers should only notify on ``CRITICAL`` state.

To implement this configuration, we may define 2 different HTTP services with different notification options.

Example:

::

   define service {
      service_description     HTTP Front
      hostgroup_name          front-web
      notification_options    c,w,r
      ...
   }

   define service {
      service_description     HTTP Back
      hostgroup_name          front-back
      notification_options    c,r
      ...
   }

   define host {
      host_name               web-front-01
      hostgroups              web-front
      ...
   }

   define host {
      host_name               web-back-01
      hostgroups              web-back
      ...
   }


Another way is to inherit attributes on the service side directly from the host: some service attributes may be inherited directly from the host if they are not defined on the service template side (see `Implied Inheritance`_), but not all. Our ``notification_options`` in our example cannot be picked up from the host.

If the attribute you want to be set a custom value cannot be inherited from the host, you may use the ``service_overrides`` host directive. Its role is to enforce a service directive directly from the host. This allows to define specific service instance attributes from a same generalized service definition.

Its syntax is:

::

  service_overrides xxx,yyy zzz

It could be summarized as "*For the service bound to me, named ``xxx``, I want the directive ``yyy`` set to ``zzz`` rather tran the inherited value*"

The service description selector (represented by ``xxx`` in the previous example) may be:

   - A service name (default)
     The ``service_description`` of one of the services attached to the host.

   - ``*`` (wildcard)
     Means *all the services attached to the host*

   - A regular expression
      A regular expression against the ``service_description`` of the services attached to the host (it has to be prefixed by ``r:``).


Example:

::

  define service {
         service_description     HTTP
         hostgroup_name          web
         notification_options    c,w,r
         ...
  }

  define host {
         host_name               web-front-01
         hostgroups              web
         ...
  }
  ...

  define host {
         host_name               web-back-01
         hostgroups              web
         service_overrides       HTTP,notification_options c,r
         ...
  }
  ...
  define host {
         host_name               web-back-02
         hostgroups              web
         service_overrides       *,notification_options c
         ...
  }
  ...
  define host {
         host_name               web-back-03
         hostgroups              web
         service_overrides       r:^HTTP,notification_options r
         ...
  }
  ...

In the previous example, we defined only one instance of the HTTP service, and we enforced the service ``notification_options`` for some web servers composing the backend. The final result is the same, but the second example is shorter, and does not require the second service definition.

Using packs allows an even shorter configuration.

Example:

::

  define host {
         use                     http
         host_name               web-front-01
         ...
  }
  ...

  define host {
         use                     http
         host_name               web-back-01
         service_overrides       HTTP,notification_options c,r
         ...
  }
  ...
  define host {
         use                     http
         host_name               web-back-02
         service_overrides       HTTP,notification_options c
         ...
  }
  ...
  define host {
         use                     http
         host_name               web-back-03
         service_overrides       HTTP,notification_options r
         ...
  }
  ...

In this example, the web server from the front-end cluster uses the value defined in the pack, and the one from the backend cluster has its HTTP service (inherited from the HTTP pack also) enforced its ``notification_options`` directive.

.. important:: The ``service_overrides`` attribute may himself be inherited from an upper host template. This is a multivalued attribute which syntax requires that each value is set on its own line. If you add a line on a host instance, it will not add it to the ones defined at template level, it will overload them. If some of the values on the template level are needed, they have to be explicitely copied.

Example:

::

  define host {
         name                    web-front
         service_overrides       HTTP,notification_options c,r
         ...
         register                0
  }
  ...

  define host {
         use                     web-fromt
         host_name               web-back-01
         hostgroups              web
         service_overrides       HTTP,notification_options c,r
         service_overrides       HTTP,notification_interval 15
         ...
  }
  ...



Inheritance exclusions
~~~~~~~~~~~~~~~~~~~~~~

Packs and hostgroups allow to factorize the configuration and greatly reduce the amount of configuration to describe monitoring infrastructures. The drawback is that it forces hosts to be consistent, as the same configuration is applied to a possibly very large set of machines.

Imagine a web servers cluster. All machines except one should be checked its managenent interface (ILO, iDRAC). In the cluster, there is one virtual server that should be checked the exact same services than the others, except the management interface (as checking it on a virtual server has no meaning). The corresponding service comes from a pack.

In this situation, there is several ways to manage the situation:

   - create an intermadiate template on the pack level to have the management interface check attached to an upper level template

   - re define all the services for the specifed host.

   - use service overrides to set a dummy command on the corresponding service.

None of these options are satisfying.

There is a last solution that consists of excluding the corresponding service from the specified host. This may be done using the ``service_excludes`` directive.

Its syntax is:

::

  service_excludes xxx

The service description selector (represented by ``xxx`` in the previous example) may be:

   - A service name (default)
     The ``service_description`` of one of the services attached to the host.

   - ``*`` (wildcard)
     Means *all the services attached to the host*

   - A regular expression
      A regular expression against the ``service_description`` of the services attached to the host (it has to be prefixed by ``r:``).

Example:


::

  define host {
         use                     web-front
         host_name               web-back-01
         ...
  }

  define host {
         use                     web-front
         host_name               web-back-02    ; The virtual server
         service_excludes        Management interface
         ...
  }
  ...
  define host {
         use                     web-front
         host_name               web-back-03    ; The virtual server
         service_excludes        *
         ...
  }
  ...
  define host {
         use                     web-front
         host_name               web-back-04    ; The virtual server
         service_excludes        r^Management
         ...
  }
  ...


In the case you want the opposite (exclude all except) you can use the ``service_includes`` directive which is its corollary.


.. _configuration/objects_advanced:


Time-Saving tricks for objects definition
=========================================


Services
~~~~~~~~


Same service on several hosts
-----------------------------

Identical services assigned to several hosts can be specified with a list of hosts names in the ``host_name`` service property.

::

    define service{
        host_name                HOST1,HOST2,HOST3,...,HOSTN
        service_description      SOMESERVICE
        other service properties ...
    }


Same service on hosts in multiple hostgroups
--------------------------------------------

Identical services assigned to all the hosts in one or more hostgroups can be specified with a list of hosts groups names in the ``hostgroup_name`` service property.

::

    define service{
        hostgroup_name          HOSTGROUP1,HOSTGROUP2,...,HOSTGROUPN
        service_description     SOMESERVICE
        #other service properties ...
    }


Same service on all hosts
-------------------------

Identical services assigned to all the hosts in your monitoing configuration is as simple as:

::

    define service{
        host_name               *
        service_description     SOMESERVICE
        #other service properties ...
    }


Excluding Hosts
---------------

If you want to create identical services on numerous hosts or hostgroups, but would like to exclude some hosts from the definition, this can be accomplished by preceding the host or hostgroup with a ``!`` symbol.


::

    define service{
        host_name             HOST1,HOST2,!HOST3,!HOST4,...,HOSTN
        hostgroup_name        HOSTGROUP1,HOSTGROUP2,!HOSTGROUP3,!HOSTGROUP4,...,HOSTGROUPN
        service_description   SOMESERVICE
        other service properties ...
    }



Service escalations
~~~~~~~~~~~~~~~~~~~


Multiple hosts
--------------

The same rules as the one used for the services may be used for the service escalations definitions. Specify several hosts, hosts from hosts groups, all hosts, and hosts exclusions apply on service escalations.


All services on the same host
-----------------------------

If you want to create service escalations for all the services of a particular host, you can use a wildcard in the ``service_description`` property. The definition below will create a service escalation for all the services of the host *HOST1*.

::

    define serviceescalation{
        host_name               HOST1
        service_description     *
        # other escalation properties ...
    }


Several services on the same host
---------------------------------

Using a service name list in the ``service_description`` property of an escalation will assign this escalation to the specified services of the host defined in the ``host_name`` property.

::

    define serviceescalation{
        host_name               HOST1
        service_description     SERVICE1,SERVICE2,...,SERVICEN
        # other escalation properties ...
    }


All the services in several services groups
-------------------------------------------

Specifying a list of services groups names in the ``servicegroup_name`` property will target all the services defined in the specified groups.


::

    define serviceescalation{
        servicegroup_name          SERVICEGROUP1,SERVICEGROUP2,...,SERVICEGROUPN
        # other escalation properties ...
    }



Services dependencies
~~~~~~~~~~~~~~~~~~~~~


Several hosts
-------------

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
--------------------------------

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
-----------------------

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
---------------------------

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
---------------------------------------

If you want to create service dependencies for all services that belong to one or more services groups, you can use the ``servicegroup_name`` and/or ``dependent_servicegroup_name`` properties as follows:


::

  define servicedependency{
      servicegroup_name               SERVICEGROUP1,SERVICEGROUP2,...,SERVICEGROUPN
      dependent_servicegroup_name     SERVICEGROUP3,SERVICEGROUP4,...SERVICEGROUPN
      other dependency properties ...
  }


Same host dependencies
----------------------

If you want to create service dependencies for multiple services that are dependent on other services on the same host, leave the ``dependent_host_name`` and ``dependent_hostgroup_name`` properties empty. The example below assumes that hosts *HOST1* and *HOST2* have at least the following four services associated with them: *SERVICE1*, *SERVICE2*, *SERVICE3*, and *SERVICE4*. In this example, *SERVICE3* and *SERVICE4* on *HOST1* will be dependent on both *SERVICE1* and *SERVICE2* on *HOST1*. Similarly, the same dependencies will exist for the corresponding services on *HOTS2*.


::

  define servicedependency{
      host_name                       HOST1,HOST2
      service_description             SERVICE1,SERVICE2
      dependent_service_description   SERVICE3,SERVICE4
      other dependency properties ...
  }


Hosts escalations
~~~~~~~~~~~~~~~~~


Several hosts
-------------

To create host escalations for multiple hosts, specify several hosts in the ``host_name`` property.

::

  define hostescalation{
      host_name              HOST1,HOST2,HOST3,...,HOSTN
      # other escalation properties ...
  }

.. note: specifying a ``*`` in the ``host_name`` property will apply the escalation on all the monitored hosts.

All hosts in several hosts groups
---------------------------------

To create host escalations for all hosts in one or more hostgroups, use the ``hostgroup_name`` property.

::

  define hostescalation{
      hostgroup_name            HOSTGROUP1,HOSTGROUP2,...,HOSTGROUPN
      # other escalation properties ...
  }


Excluding some hosts
--------------------

If you want to create identical host escalations on several hosts or hostgroups, but you wish tp to exclude some hosts from the definition, you can prepend the host or hostgroup with a ``!`` symbol.


::

  define hostescalation{
      host_name             HOST1,HOST2,!HOST3,!HOST4,...,HOSTN
      hostgroup_name        HOSTGROUP1,HOSTGROUP2,!HOSTGROUP3,!HOSTGROUP4,...,HOSTGROUPN
      # other escalation properties ...
  }



Hosts dependencies
~~~~~~~~~~~~~~~~~~


Several hosts
-------------

If you want to create host dependencies for several hosts, you can specify multiple hosts in the ``host_name`` and/or ``dependent_host_name`` properties. The definition below would be equivalent to creating six separate host dependencies. In the example above, hosts HOST3, HOST4 and HOST5 would be dependent upon both HOST1 and HOST2.
::

  define hostdependency{
      host_name               HOST1,HOST2
      dependent_host_name     HOST3,HOST4,HOST5
      other dependency properties ...
  }


All hosts in several hosts groups
---------------------------------

If you want to create host dependencies for all hosts in one or more hostgroups, you can use the ``hostgroup_name`` and /or ``dependent_hostgroup_name`` properties. In the example below, all hosts in hostgroups HOSTGROUP3 and HOSTGROUP4 would be dependent on all hosts in hostgroups HOSTGROUP1 and HOSTGROUP2.

::

  define hostdependency{
      hostgroup_name                  HOSTGROUP1,HOSTGROUP2
      dependent_hostgroup_name        HOSTGROUP3,HOSTGROUP4
      other dependency properties ...
  }



Hosts groups
~~~~~~~~~~~~


All hosts
---------

If you want to create an hosts group that group all hosts defined in your monitored objects, you can use a wildcard in the ``members`` directive. The definition below will create an hostgroup called *HOSTGROUP1* that has all hosts as members.


::

  define hostgroup{
      hostgroup_name          HOSTGROUP1
      members                 *
      # other hostgroup properties ...
  }

