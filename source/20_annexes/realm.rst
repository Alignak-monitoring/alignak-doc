.. _monitoring_objects/realm:

=================
Realm Definition 
=================

The realms is an Alignak optional, despite recommended, feature which is very useful if the administrator wants to group / separate the monitored resources.

The Realm definition is optional. If no scheduler is defined, Alignak will "create" one for the user and it will be the default one.


Syntax
======

Bold variables are required, while others are optional.
Emphasized variables are Alignak extensions with reference to the Nagios legacy definition.

================= ===============
define realm{                
``realm_name``    *realm_name*
``alias``         *alias*
``realm_members`` *realm_members*
``default``       [0/1]
}                            
================= ===============


Example
=======

::

  define realm{
      realm_name     World
      realm_members  Europe,America,Asia
      default        0
  }


Variables
=========

realm_name
  This variable is used to identify the *short name* of the realm.

alias
  This variable is used to define the friendly name of the realm.

realm_members
  This variable is used to define the sub-realms of this realms. It is a comma separated list of realms short names

default
  This optional directive is used to define that this realm is the default one (untagged host and daemons will be attached to the default realm). The default value is *0*.
