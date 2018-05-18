.. _monitoring_objects/hostgroup:

====================
Hostgroup Definition
====================

A hostgroup definition is used to define a group of hosts.


Syntax
======

Bold variables are required, while others are optional.
Emphasized variables are Alignak extensions with reference to the Nagios legacy definition.

========================================== ======================================
define hostgroup{
**hostgroup_name**                          **hostgroup_name**
alias                                       alias
display_name                                display_name

...
To be updated / completed
...

}
========================================== ======================================


Example
=======

::

   define hostgroup{
      hostgroup_name                 my_hostgroup
   }


Variables
=========

hostgroup_name
  This directive is used to define a short name used to identify the hosts group.
