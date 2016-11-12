.. _introduction/introduction:


=============
About Alignak
=============

Alignak is an open source monitoring framework written in Python under the terms of the `GNU Affero General Public License`_ .
It is a fork of the Shinken project.

More information about Alignak is available in the `Alignak documentation <http://alignak-doc.readthedocs.io/en/latest/>`_.

Alignak Project
===============

The main idea when developing Alignak is the flexibility which is our definition of framework.
Nevertheless, Alignak was first made differently and we try to keep all the good things that made it a monitoring solution :

   * Easy to install : we will always recommend packages installations. You can still install it with pip or setup.py directly, see :ref:`installation/index`.
   * Easy for new users : this documentation should helps you to discover Alignak and show simple use-case.
   * Easy to migrate from Nagios : Nagios configuration and plugins will work with Alignak, we try to keep this compatibility while developing it.
   * Multi-platform : python is available in a lot of OS. We try to write generic code to keep this possible. However, Linux is the most tested OS so far.
   * Utf8 compliant : python is here to do that. For now Alignak is compatible with 2.6-2.7 version but will work with python 3.x and Pypy in the future.
   * Independent from other monitoring solution : Alignak is a framework that can integrate with others through standard interfaces). Flexibility first.
   * Flexible : in an architecture point view. It is very close to our scalability wish. OpenStack integration is our long term goal (short lifetime architecture)
   * Easy to contribute : contribution has to be an easy process. Alignak follow pep8 and pylint coding standard add readability. Step by step help to contribute can be found in :ref:`Contributing <contributing/index>`

This is basically what Alignak is made of. Maybe add the "keep it simple" Linux principle and it's perfect. There is nothing we don't want, we consider every features / ideas.


Features
========

Alignak has a lot of features, we started to list some of them in the last paragraph. Let's go into details:

  * Role separated daemons : we want a daemon to do one thing but doing it good. There are 6 of them but one is not compulsory.
  * Great flexibility : you didn't got that already? Alignak modules allow it to talk to almost everything you can imagine.

  Those two points involve all the following :

  * Data export to databases :

      * Graphite
      * InfluxDB
      * RRD
      * GLPI
      * CouchDB
      * Livestatus  (MK_Livestatus reimplementation)
      * Socket write for other purpose (Splunk, Logstash, Elastic Search)
      * MySQL (NDO reimplementation)
      * Oracle (NDO reimplementation)

  * Integration with web user interface :

      * WebUI (Alignak own UI)
      * Thruk
      * Adagios
      * Multisite
      * Nagvis
      * PNP4Nagios
      * NConf
      * Centreon (With NDO, not fully working, not recommended)

  * Import configuration from databases :

      * GLPI
      * Amazon EC2
      * MySQL
      * MongoDB
      * Canonical Landscape

 * Alignak provide sets of configuration, named packs, for a huge number of services :

      * Databases (Mysql, Oracle, MSSQL, memcached, mongodb, influxdb etc.)
      * Routers, Switches (Cisco, Nortel, Procurve etc.)
      * OS (Linux, windows, Aix, HP-UX etc.)
      * Hypervisors (VMWare, Vsphere)
      * Protocols (HTTP, SSH, LDAP, DNS, IMAP, FTP, etc.)
      * Application (Weblogic, Exchange, Active Directory, Tomcat, Asterisk, etc.)
      * Storage (IBM-DS, Safekit, Hacmp, etc.)

  * Smart SNMP polling : The SNMP Booster module is a must have if you have a huge infrastructure of routers and switches.

  * Scalability : no server overloading, you just have to install new daemons on another server and load balancing is done.


But Alignak is even more :

  * Realm concept : you can monitor independent environments / customer
  * DMZ monitoring : some daemons have passive facilities so that firewall don't block monitoring.
  * Business impact : Alignak can differentiate impact of a critical alert on a toaster versus the web store
  * Efficient correlation between parent-child relationship and business process rules
  * High availability : daemons can have spare ones.
  * Business rules :  For a higher level of monitoring. Alignak can notify you only if 3 out 5 of your server are down
  * Very open-minded team : help is always welcome, there is job for everyone.


Release cycle
=============

Alignak has no strict schedule for now on release date. We will keep you updated on the user mailing list on what we plan for next release.
Developers meeting will define a realease cycle later in 2015 fall.
Roadmap is available in a `specific Github issue`_, feature addition can be discussed there.
Technical point of view about a specific feature are discussed in a separated issue.


.. _Nagios: http://www.nagios.org
.. _GNU Affero General Public License: http://www.gnu.org/licenses/agpl.txt
.. _alignak-monitoring organization's page: https://github.com/Alignak-monitoring
.. _specific Github issue: https://github.com/Alignak-monitoring/alignak/issues/262
