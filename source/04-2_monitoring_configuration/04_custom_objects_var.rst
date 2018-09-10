.. _configuration/customobjectvars:

=======================
Custom Object Variables
=======================


Introduction
------------

Users often request that new variables to be added to host, service, or contact definitions. These include variables for "SNMP" community, MAC address, AIM username, Skype number, etc. The list is endless. Adding many information into the host, service or contact definition makes Alignak less generic and more infrastructure-specific.

Alignak allows the users to define their own custom variables to add extra properties in their host, service, and contact definitions. These additional properties can then be used in notifications, event handlers, and host and service checks.


Custom Variable Basics
----------------------

There are a few important things that you should note about custom variables:

  * Custom variable names must begin with an underscore (_) to prevent name collision with standard variables
  * Custom variable names are case-insensitive
  * Custom variables are inherited from object templates like normal variables
  * Scripts can reference custom variable values with :ref:`macros and environment variables <monitoring_features/macros>`


Examples
--------

Here's an example of how custom variables can be defined in different types of object definitions::

  define host{
     host_name       linuxserver
     _mac_address    00:06:5B:A6:AD:AA ; <-- Custom MAC_ADDRESS variable
     _rack_number    R32               ; <-- Custom RACK_NUMBER variable
  ...
  }

  define service{
     host_name        linuxserver
     description      Memory Usage
     _SNMP_community  public         ; <-- Custom SNMP_COMMUNITY variable
     _TechContact     Jane Doe       ; <-- Custom TECHCONTACT variable
     ....
  }

  define contact{
     contact_name    john
     _AIM_username   john16        ; <-- Custom AIM_USERNAME variable
     _YahooID        john32        ; <-- Custom YAHOOID variable
     ...
  }


Custom Variables As Macros
--------------------------

Custom variable values can be referenced in scripts and executables that Alignak runs for checks, notifications, etc. by using :ref:`macros <monitoring_features/macros>` or environment variables.

In order to prevent name collision among custom variables from different object types, Alignak prepends ``_HOST``, ``_SERVICE``, or ``_CONTACT`` to the beginning of custom host, service, or contact variables, respectively, in macro and environment variable names.

The table below shows the corresponding macro and environment variable names for the custom variables that were defined in the example above.

=========== ============== =================== ======================== ==============================
Object Type Variable Name  Variable definition Macro Name               Environment Variable
=========== ============== =================== ======================== ==============================
Host        MAC_ADDRESS    _MAC_ADDRESS        $_HOSTMAC_ADDRESS$       ALIGNAK__HOSTMAC_ADDRESS
Host        RACK_NUMBER    _RACK_NUMBER        $_HOSTRACK_NUMBER$       ALIGNAK__HOSTRACK_NUMBER
Service     SNMP_COMMUNITY _SNMP_COMMUNITY     $_SERVICESNMP_COMMUNITY$ ALIGNAK__SERVICESNMP_COMMUNITY
Service     TECHCONTACT    _TECHCONTACT        $_SERVICETECHCONTACT$    ALIGNAK__SERVICETECHCONTACT
Contact     AIM_USERNAME   _AIM_USERNAME       $_CONTACTAIM_USERNAME$   ALIGNAK__CONTACTAIM_USERNAME
Contact     YAHOOID        _YAHOOID            $_CONTACTYAHOOID$        ALIGNAK__CONTACTYAHOOID
=========== ============== =================== ======================== ==============================

.. note:: Alignak is still using an ``ALIGNAK_`` prefix for the environment variables. You can set another prefix (eg. ``NAGIOS_``) to maintain compatibility with former existing plugin or notification scripts. To change the default Alignak prefix, use the :ref:`environment variables prefix configuration variable <configuration/core#env_variables_prefix>`.


Custom Variables And Inheritance
--------------------------------

Custom object variables are inherited just like standard host, service, or contact variables.

