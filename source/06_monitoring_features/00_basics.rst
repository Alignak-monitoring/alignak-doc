.. _monitoring_features/plugins:

=======================
Monitoring basics
=======================


Introduction 
=============

Alignak includes a set of scalable internal mechanisms for checking the status of hosts and services on your network. Alignak also relies on external programs (called check plugins) to monitor a very wide variety of devices, applications and networked services.


What Are Plugins? 
==================

Plugins are compiled executables or scripts (Perl scripts, shell scripts, etc.) that can be run from a command line to check the status of an host or service. Alignak uses some check plugins as test probes to know about the monitored system health. Indeed, the results of the check plugins execution is used to set the current status of the monitored hosts and services and get some performance data about the monitored services.

Alignak will execute a plugin whenever it needs to check the status of a service or host. The plugin does something (*notice the very general term*) to perform the check and then simply returns the results to Alignak. Alignak will then process the results received from the plugin and take any necessary actions (set host/service status, run an :ref:`event handler <monitoring_features/event_handlers>`, raise an alert, send out some :ref:`notifications <monitoring_features/notifications>`, etc).


Alignak integrated data acquisition modules 
============================================

**To be improved**

Alignak integrates some data acquisition modules. These modules replace the traditional unscalable plugins with high performance variants that are more tightly coupled with Alignak.

Integrated Alignak data acquisition modules support the following protocols:
  * :ref:`NRPE <alignak_features/integrated_protocols>`
  * :ref:`SNMP <alignak_features/integrated_protocols>`


Plugins as an abstraction layer
===============================

Plugins act as an abstraction layer between the monitoring logic implemented in the Alignak daemon and the actual services and hosts that are being monitored.

The upside of this type of plugin architecture is that you can monitor just about anything you can think of. If you can automate the process of checking something, you can monitor it with Alignak.

There are already thousands of plugins that have been created to monitor basic resources such as processor load, disk usage, ping rates, etc. If you want to monitor something else, take a look at the documentation on :ref:`writing plugins <annexes/plugin_api>` and roll your own. It's simple!

The downside to this type of plugin architecture is the fact that Alignak has absolutely no idea about what is monitored. You could be monitoring network traffic statistics, data error rates, room temperate, CPU voltage, fan speed, processor load, disk space, or the ability of your super-fantastic toaster to properly brown your bread in the morning...

Alignak doesn't understand the specifics of what's being monitored - it just tracks changes in the state of those resources. Only the plugins know exactly what they're monitoring and how to perform the actual checks.


Which plugins are available?
============================

There are plugins to monitor many different kinds of devices and services.

They use basic monitoring protocols including:

  * WMI, SNMP, SSH, NRPE, TCP, UDP, ICMP, OPC, LDAP and more

They can monitor pretty much anything:

  * Unix/Linux, Windows, and Netware Servers
  * Routers, Switches, VPNs
  * Networked services: "HTTP", "POP3", "IMAP", "FTP", "SSH", "DHCP"
  * CPU Load, Disk Usage, Memory Usage, Current Users
  * Applications, databases, logs and more.


Obtaining Plugins 
==================

Alignak plugin API is fully compatible with the Nagios one. Thaks to this compatibility, you can download and use the official Monitoring-plugins and many additional plugins created and maintained by Nagios users from the following locations:

  * Monitoring Plugins Project: https://www.monitoring-plugins.org/
  * Nagios Downloads Page: http://www.nagios.org/download/
  * NagiosExchange.org: http://www.nagiosexchange.org/

Alignak also provides some checks packages. See :ref:`this page for more information <contributing/modules-and-checks-packages>`.


How do I use plugin X?
======================

Most plugins will display basic usage information when you execute them using "-h" or "--help" on the command line.
For example, if you want to know how the **check_http** plugin works or what options it accepts, you should try executing the following command:
  
::

  ./check_http --help


Plugin API 
===========

You can find information on the technical aspects of plugins, as well as how to go about creating your own custom plugins :ref:`here <annexes/plugin_api>`.
