.. _monitoring_features/timeperiods:

==============
 Time Periods 
==============

**Abstract**

or...“Is This a Good Time?"


Introduction 
=============

.. image:: /_static/images///official/images/objects-timeperiods.png
   :scale: 90 %

:ref:`Timeperiod <monitoring_objects/timeperiod>` definitions allow you to control when various aspects of the monitoring and alerting logic can operate. For instance, you can restrict:

  * When regularly scheduled host and service checks can be performed
  * When notifications can be sent out
  * When notification escalations can be used
  * When dependencies are valid


Precedence in Time Periods 
===========================

Timeperod :ref:`definitions <monitoring_objects/timeperiod>` may contain multiple types of attributes, including weekdays, days of the month, and calendar dates. Different types of attributes have different precedence levels and may override other attributes in your timeperiod definitions. The order of precedence for different types of attributes (in descending order) is as follows:

  * Calendar date (2008-01-01)
  * Specific month date (January 1st)
  * Generic month date (Day 15)
  * Offset weekday of specific month (2nd Tuesday in December)
  * Offset weekday (3rd Monday)
  * Normal weekday (Tuesday)

Examples of different timeperiod attributes can be found :ref:`here <monitoring_objects/timeperiod>`.


.. _monitoring_features/timeperiods#how_time_periods_work_with_host_and_service_checks:

How Time Periods Work With Host and Service Checks 
===================================================


Host and service definitions have an optional "check_period" attribute that allows you to specify a timeperiod that should be used to restrict when regularly scheduled, active checks of the host or service can be made.

If you do not use the "check_period attribute" to specify a timeperiod, Alignak will be able to schedule active checks of the host or service anytime it needs to. This is essentially a 24x7 monitoring scenario.

Specifying a timeperiod in the "check_period attribute" allows you to restrict the time that Alignak perform regularly scheduled, active checks of the host or service. When Alignak attempts to reschedule a host or service check, it will make sure that the next check falls within a valid time range within the defined timeperiod. If it doesn't, Alignak will adjust the next check time to coincide with the next “valid" time in the specified timeperiod. This means that the host or service may not get checked again for another hour, day, or week, etc.

On-demand checks and passive checks are not restricted by the timeperiod you specify in the "check_period attribute". Only regularly scheduled active checks are restricted.

A service's timeperiod is inherited from its host only if it's not already defined for the service. In a fresh new default Alignak installation, it is defined to "24x7" in the ``generic-service`` service template. If you want that the service notifications are not sent out when the host is outside its notification period, you will have to comment ``notification_period`` and/or ``notification_enabled`` in the ``generic-service`` template.

Unless you have a good reason not to do so, a good practice is to monitor all the hosts and services using timeperiods that cover a 24x7 time range. If you don't do this, you can run into some problems during the *blackout* time (times that are not valid in the timeperiod definition):

  * The status change of the host or service will not appear during the blackout time.
  * Contacts will mostly likely not get re-notified of problems with a host or service during blackout times.
  * If a host or service recovers during a blackout time, contacts will not be immediately notified of the recovery.


How Time Periods Work With Contact Notifications 
=================================================

By specifying a timeperiod in the ``notification_period`` attribute of a host or service definition, you can control when Alignak is allowed to send notifications out regarding problems or recoveries for that host or service. When a host notification is about to get sent out, Alignak will make sure that the current time is within a valid range in the "notification_period" timeperiod. If it is a valid time, then Alignak will attempt to notify each contact of the problem or recovery.

You can also use timeperiods to control when notifications can be sent out to individual contacts. By using the ``service_notification_period`` and ``host_notification_period`` attributes in :ref:`the contact definition <monitoring_objects/contact>`, you're able to essentially define an “on call" period for each contact. Contacts will only receive host and service notifications in the time frame that are specified in the his/her notification periods.


How Time Periods Work With Notification Escalations 
====================================================

Service and host :ref:`notification escalations <alignak_features/notifications_escalations>` have an optional escalation_period attribute that allows to specify a timeperiod when the escalation is valid and can be used. If you do not use the ``escalation_period`` attribute in an escalation definition, the escalation is considered as permanently valid. If you specify a timeperiod in the ``escalation_period`` attribute, Alignak will only use the escalation definition in the time frame that are valid in the timeperiod definition.


How Time Periods Work With Dependencies 
========================================

:ref:`Host and service dependencies <monitoring_features/dependencies>` have an optional ``dependency_period`` attribute that allows to specify a timeperiod when the dependendies are valid and can be used. If you do not use the ``dependency_period`` attribute in a dependency definition, the dependency can be used at any time. If you specify a timeperiod in the "dependency_period" attribute, Alignak will only use the dependency definition in the time frame that are valid in the timeperiod definition.


Defining On-Call rotations
==========================

Administrators often have to shoulder the burden of answering pagers, cell phone calls, etc. when they least desire them. No one likes to be woken up at 4 am to fix a problem. But its often better to fix the problem in the middle of the night, rather than face the wrath of an unhappy boss when you stroll in at 9 am the next morning ;)

For those lucky admins who have a team of gurus who can help share the responsibility of answering alerts, on-call rotations are often setup. Multiple admins will often alternate taking notifications on weekends, weeknights, holidays, etc.

I'll show you how you can create timeperiod definitions in a way that can facilitate most on-call notification rotations. These definitions won't handle human issues that will inevitably crop up (admins calling in sick, swapping shifts, or throwing their pagers into the river), but they will allow you to setup a basic structure that should work the majority of the time.


Scenario 1: Holidays and Weekends
---------------------------------

Two admins - John and Bob - are responsible for responding to Alignak alerts. John receives all notifications for weekdays (and weeknights) - except for holidays - and Bob gets handles notifications during the weekends and holidays. Lucky Bob. Here's how you can define this type of rotation using timeperiods...

First, define a timeperiod that contains time ranges for holidays:


::

  define timeperiod{
    name    holidays
    timeperiod_name holidays
    january 1    00:00-24:00    ; New Year's Day
    2008-03-23    00:00-24:00    ; Easter (2008)
    2009-04-12    00:00-24:00    ; Easter (2009)
    monday -1 may    00:00-24:00    ; Memorial Day (Last Monday in May)
    july 4    00:00-24:00    ; Independence Day
    monday 1 september    00:00-24:00    ; Labor Day (1st Monday in September)
    thursday 4 november    00:00-24:00    ; Thanksgiving (4th Thursday in November)
    december 25    00:00-24:00    ; Christmas
    december 31    17:00-24:00    ; New Year's Eve (5pm onwards)
   }

Next, define a timeperiod for John's on-call times that include weekdays and weeknights, but excludes the dates/times defined in the holidays timeperiod above:


::

  define timeperiod{
    timeperiod_name    john-oncall
    monday    00:00-24:00
    tuesday    00:00-24:00
    wednesday    00:00-24:00
    thursday    00:00-24:00
    friday    00:00-24:00
    exclude     holidays    ; Exclude holiday dates/times defined elsewhere
  }

You can now reference this timeperiod in John's contact definition:


::

  define contact{
    contact_name    john
    ...
    host_notification_period    john-oncall
    service_notification_period    john-oncall
  }

Define a new timeperiod for Bob's on-call times that include weekends and the dates/times defined in the holidays timeperiod above:


::

  define timeperiod{
    timeperiod_name    bob-oncall
    friday    00:00-24:00
    saturday    00:00-24:00
    use    holidays    ; Also include holiday date/times defined elsewhere
  }

You can now reference this timeperiod in Bob's contact definition:


::

  define contact{
    contact_name    bob
    ...
    host_notification_period    bob-oncall
    service_notification_period    bob-oncall
  }


Scenario 2: Alternating Days
----------------------------

In this scenario John and Bob alternate handling alerts every other day - regardless of whether its a weekend, weekday, or holiday.

Define a timeperiod for when John should receive notifications. Assuming today's date is August 1st, 2007 and John is handling notifications starting today, the definition would look like this:


::

  define timeperiod{
    timeperiod_name    john-oncall
    2007-08-01 / 2 00:00-24:00    ; Every two days, starting August 1st, 2007
  }

Now define a timeperiod for when Bob should receive notifications. Bob gets notifications on the days that John doesn't, so his first on-call day starts tomorrow (August 2nd, 2007).


::

  define timeperiod{
    timeperiod_name    bob-oncall
    2007-08-02 / 2 00:00-24:00    ; Every two days, starting August 2nd, 2007
  }

Now you need to reference these timeperiod definitions in the contact definitions for John and Bob:


::

  define contact{
    contact_name    john
    ...
    host_notification_period    john-oncall
    service_notification_period    john-oncall
  }
  define contact{
    contact_name    bob
    ...
    host_notification_period    bob-oncall
    service_notification_period    bob-oncall
  }


Scenario 3: Alternating Weeks
-----------------------------

In this scenario John and Bob alternate handling alerts every other week. John handles alerts Sunday through Saturday one week, and Bob handles alerts for the following seven days. This continues in perpetuity.

Define a timeperiod for when John should receive notifications. Assuming today's date is Sunday, July 29th, 2007 and John is handling notifications this week (starting today), the definition would look like this:


::

  define timeperiod{
     timeperiod_name    john-oncall
    2007-07-29 / 14 00:00-24:00    ; Every 14 days (two weeks), starting Sunday, July 29th, 2007
    2007-07-30 / 14 00:00-24:00    ; Every other Monday starting July 30th, 2007
    2007-07-31 / 14 00:00-24:00    ; Every other Tuesday starting July 31st, 2007
    2007-08-01 / 14 00:00-24:00    ; Every other Wednesday starting August 1st, 2007
    2007-08-02 / 14 00:00-24:00    ; Every other Thursday starting August 2nd, 2007
    2007-08-03 / 14 00:00-24:00    ; Every other Friday starting August 3rd, 2007
    2007-08-04 / 14 00:00-24:00    ; Every other Saturday starting August 4th, 2007
  }

Now define a timeperiod for when Bob should receive notifications. Bob gets notifications on the weeks that John doesn't, so his first on-call day starts next Sunday (August 5th, 2007).


::

  define timeperiod{
    timeperiod_name    bob-oncall
    2007-08-05 / 14 00:00-24:00    ; Every 14 days (two weeks), starting Sunday, August 5th, 2007
    2007-08-06 / 14 00:00-24:00    ; Every other Monday starting August 6th, 2007
    2007-08-07 / 14 00:00-24:00    ; Every other Tuesday starting August 7th, 2007
    2007-08-08 / 14 00:00-24:00    ; Every other Wednesday starting August 8th, 2007
    2007-08-09 / 14 00:00-24:00    ; Every other Thursday starting August 9th, 2007
    2007-08-10 / 14 00:00-24:00    ; Every other Friday starting August 10th, 2007
    2007-08-11 / 14 00:00-24:00    ; Every other Saturday starting August 11th, 2007
  }

Now you need to reference these timeperiod definitions in the contact definitions for John and Bob:


::

  define contact{
    contact_name    mjohn
    ...
    host_notification_period    john-oncall
    service_notification_period    john-oncall
  }
  define contact{
    contact_name    bob
    ...
    host_notification_period    bob-oncall
    service_notification_period    bob-oncall
  }


Scenario 4: Vacation Days
-------------------------

In this scenarios, John handles notifications for all days except those he has off. He has several standing days off each month, as well as some planned vacations. Bob handles notifications when John is on vacation or out of the office.

First, define a timeperiod that contains time ranges for John's vacation days and days off:


::

  define timeperiod{
    name    john-out-of-office
    timeperiod_name    john-out-of-office
    day 15    00:00-24:00    ; 15th day of each month
    day -1    00:00-24:00    ; Last day of each month (28th, 29th, 30th, or 31st)
    day -2    00:00-24:00    ; 2nd to last day of each month (27th, 28th, 29th, or 30th)
    january 2    00:00-24:00    ; January 2nd each year
    june 1 - july 5    00:00-24:00    ; Yearly camping trip (June 1st - July 5th)
    2007-11-01 - 2007-11-10 00:00-24:00    ; Vacation to the US Virgin Islands (November 1st-10th, 2007)
  }

Next, define a timeperiod for John's on-call times that excludes the dates/times defined in the timeperiod above:


::

  define timeperiod{
    timeperiod_name    john-oncall
    monday    00:00-24:00
    tuesday    00:00-24:00
    wednesday    00:00-24:00
    thursday    00:00-24:00
    friday    00:00-24:00
    exclude    john-out-of-office    ; Exclude dates/times John is out
  }

You can now reference this timeperiod in John's contact definition:


::

  define contact{
    contact_name    john
    ...
    host_notification_period    john-oncall
    service_notification_period    john-oncall
  }

Define a new timeperiod for Bob's on-call times that include the dates/times that John is out of the office:


::

  define timeperiod{
    timeperod_name    bob-oncall
    use    john-out-of-office    ; Include holiday date/times that John is out
  }

You can now reference this timeperiod in Bob's contact definition:


::

  define contact{
    contact_name    bob
    ...
    host_notification_period    bob-oncall
    service_notification_period    bob-oncall
  }


Other Scenarios
---------------

There are a lot of other on-call notification rotation scenarios that you might have. The date exception attribute in the timeperiod definitions is capable of handling most dates and date ranges that you might need to use, so check out the different formats that you can use. If you make a mistake when creating timeperiod definitions, always err on the side of giving someone else more on-call duty time. :-)

