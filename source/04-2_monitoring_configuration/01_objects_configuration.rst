.. _configuration/objects_configuration:

Monitored objects configuration
===============================


Monitored objects are all the elements that are involved in the monitoring and notification logic.

Alignak manages all the Nagios legacy types of objects and proposes some extra objects types to enrich and ease the modnitoring configuration. Managed objects types include:

  * Hosts and services
  * Commands
  * Time Periods
  * Contacts
  * Groups: hosts, services, contacts
  * Dependencies
  * Escalations
  * Modulations (checks, macros, etc.)

The monitored objects can be defined in one or more configuration files and/or directories that are specified using the Nagios/Shinken legacy configuration as explained in the :ref:`core configuration <configuration/core>`.

.. note: defining the monitored objects in some configuration flat files is the Nagios/Shinken legacy way of defining the configuration. Alignak introduces a brand new solution thanks to its backend. For more information, see :ref:̀̀`extending/alignak_backend`.

The monitored objects are defined with a flexible template format, which can make it much easier to manage your Alignak configuration in the long term. You can create object definitions that inherit properties from other object definitions.

Inheritance mechanism and some advanced tips and tricks may be found below.


Objects explained
-----------------

Hosts
~~~~~

Hosts are one the main objects in the monitoring logic. Important attributes of hosts are as follows:

    * Hosts are usually physical devices on your network (servers, workstations, routers, switches, printers, etc).
    * Hosts have an address of some kind (e.g. an IP or MAC address).
    * Hosts have one or more more services associated with them.
    * Hosts can have parent/child relationships with other hosts, often representing real-world network connections, which is used in the :ref:`network reachability logic<monitoring_features/network_reachability>`.


Hosts Groups
~~~~~~~~~~~~

Hosts groups are logical groups of one or more hosts. They make it easier to:
   * view the status of related hosts in the Alignak web interface and
   * simplify your configuration through the use of :ref:`configuration tricks <configuration/time_saving_tricks>`.


Services
~~~~~~~~

Services are associated with hosts and can be:

   * Attributes of a host (CPU load, disk usage, uptime, etc.)
   * Services provided by the host ("HTTP", "POP3", "FTP", "SSH", etc.)
   * Other things associated with the host ("DNS" records, etc.)
   * High level services for end users (database, Web application, etc.)


Contacts
~~~~~~~~

Contacts are people involved in the notification process:

   * they have one or more notification methods (cellphone, pager, email, instant messaging, etc.)
   * they receive notifications for hosts and services they are related with


Contacts groups
~~~~~~~~~~~~~~~

Contacts groups are logical groups of contacts that make it easier to configure notifications.


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
------------------

Alignak objects definition follow the standard and well known Nagios/Shinken legacy syntax. This syntax will not be developed more in this document.

If you need more information concerning the configuration files organization or syntax you are invited to read `this Nagios documentation <https://assets.nagios.com/downloads/nagioscore/docs/nagioscore/3/en/objectdefinitions.html>`_.

The Shinken configuration is documented in the `Shinken online documentation <https://shinken.readthedocs.io/en/latest/08_configobjects/index.html>`_.

Some details for all objects definition are available :ref:`in this document annexes<annexes/index>`.

