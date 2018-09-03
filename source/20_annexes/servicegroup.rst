.. _monitoring_objects/servicegroup:

=========================
Service Group Definition 
=========================

A service group definition is used to group one or more services together for simplifying configuration or display purposes in the User Interface.


Syntax
======

Bold variables are required, while others are optional.
Emphasized variables are Alignak extensions with reference to the Nagios legacy definition.

===================== =======================
define servicegroup{                         
**servicegroup_name** ***servicegroup_name***
**alias**             ***alias***            
members               *services*             
servicegroup_members  *servicegroups*        
notes                 *note_string*          
notes_url             *url*                  
action_url            *url*                  
}                                            
===================== =======================


Example
=======
::

  define servicegroup{
      servicegroup_name       dbservices
      alias                   Database Services
      members                 ms1,SQL Server,ms1,SQL Serverc Agent,ms1,SQL DTC
  }


Variables
=========

servicegroup_name
  This directive is used to define a short name used to identify the service group.

alias
  This directive is used to define is a longer name or description used to identify the service group. It is provided in order to allow you to more easily identify a particular service group.

members
  This is a list of the *descriptions* of :ref:`services <monitoring_objects/service>` (and the names of their corresponding hosts) that should be included in this group. Host and service names should be separated by commas. This directive may be used as an alternative to the *servicegroups* directive in :ref:`service definitions <monitoring_objects/service>`. The format of the member directive is as follows (note that a host name must precede a service name/description):
  
  ::
  
    members=<host1>,<service1>,<host2>,<service2>,...,<host*n*>,<service*n*>
  

servicegroup_members
  This optional directive can be used to include services from other "sub" service groups in this service group. Specify a comma-delimited list of short names of other service groups whose members should be included in this group.

notes
  This directive is used to define an optional string of notes pertaining to the service group in the User Interface.

notes_url
  This directive is used to define an optional URL that can be used to provide more information about the service group. If you specify an URL, you will see a red folder icon in the CGIs (when you are viewing service group information) that links to the URL you specify here. Any valid URL can be used. If you plan on using relative paths, the base path will the the same as what is used to access the CGIs (i.e. ///cgi-bin/shinken///). This can be very useful if you want to make detailed information on the service group, emergency contact methods, etc. available to other support staff.

action_url
  This directive is used to define an optional URL that can be used to provide more actions to be performed on the service group. If you specify an URL, you will see a red "splat" icon in the CGIs (when you are viewing service group information) that links to the URL you specify here. Any valid URL can be used. If you plan on using relative paths, the base path will the the same as what is used to access the CGIs (i.e. ///cgi-bin/shinken///).
