.. _monitoring_objects/contactgroup:

========================
Contact Group Definition
========================

A contact group definition is used to group one or more :ref:`contacts <monitoring_objects/contact>` together for the purpose of sending out alert/recovery :ref:`notifications <monitoring_features/notifications>`.


Syntax
======

Bold variables are required, while others are optional.
Emphasized variables are Alignak extensions with reference to the Nagios legacy definition.

===================== =======================
define contactgroup{                         
**contactgroup_name** **contactgroup_name**
**alias**             **alias**
members               *contacts*             
contactgroup_members  *contactgroups*        
}                                            
===================== =======================


Example
=======

::

  define contactgroup{
      contactgroup_name       novell-admins
      alias                   Novell Administrators
      members                 jdoe,rtobert,tzach
  }


Variables
=========

contactgroup_name
  This directive is a short name used to identify the contact group.

alias
  This directive is used to define a longer name or description used to identify the contact group.

members
  This directive is used to define a list of the *short names* of :ref:`contacts <monitoring_objects/contact>` that should be included in this group. Multiple contact names should be separated by commas. This directive may be used as an alternative to (or in addition to) using the *contactgroups* directive in :ref:`contact <monitoring_objects/contact>` definitions.

contactgroup_members
  This optional directive can be used to include contacts from other "sub" contact groups in this contact group. Specify a comma-delimited list of short names of other contact groups whose members should be included in this group.
