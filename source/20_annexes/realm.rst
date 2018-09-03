.. _monitoring_objects/realm:

=================
Realm Definition 
=================

The realms is an Alignak optional, despite recommended, feature which is very useful if the administrator wants to group / separate the monitored resources.

The Realm definition is optional. If no realm is defined, Alignak will "create" one for the user and it will be the default one.


Syntax
======

Bold variables are required, while others are optional.

================= ===============
define realm{                
``realm_name``    **realm_name**
``alias``         *alias*
``realm_members`` *realm_members*
``group_members`` *group_members*
``members``       *members*
``higher_realms`` *higher_realms*
``default``       [0/1]
}                            
================= ===============


Example
=======

Define the default realm with its sub-realms::

  define realm{
      realm_name     World
      realm_members  Europe,America,Asia
      default        1
  }

All the hosts and hosts groups that do not have a realm defined will belong to this default realm.


Variables
=========

**realm_name**
  This variable is used to identify the *short name* of the realm.

alias
  This variable is used to define the friendly name of the realm.

higher_realms
  This variable is used to define the parent realms of this realms. It is a comma separated list of realms short names that contain the *parents* realms of the current realm.

realm_members
  This variable is used to define the sub-realms of this realms. It is a comma separated list of realms short names that contain the *children* realms of the current realm.

group_members
  This variable is used to define the sub-realms of this realms. It is a comma separated list of hostgroups short names. You can also define the realm an hostgroup belongs to in the host definition.

members
  This variable is used to define the hosts of this realms. It is a comma separated list of hosts short names. You can also define the realm an host belongs to in the host definition.

default
  This optional directive is used to define that this realm is the default one (untagged host and daemons will be attached to the default realm). The default value is *0*.
