.. raw:: LaTeX

    \newpage

.. _introduction/index:

Alignak Project
===============

Alignak is an open source monitoring framework written in Python under the terms of the `GNU Affero General Public License`_ .
The project started in 2015 from a fork of the Shinken project. Since the project creation, we achieved a huge code documentation and cleaning, we tested the application in several environments and we developed some new features.

Alignak has `its own website <http://alignak.net>`_ which is more general presentation oriented than this documentation. If you are reading this documentation, you probably already know about this website, else you are invited to `have a look`_.

The main idea when developing Alignak is the flexibility which is our definition of framework. We target the following goals:

   * Easy to install: we will always deliver packages (OS and Python) installation.
      You can install Alignak with OSes packages, Python PIP packages or *setup.py* directly, see :ref:`installation/index`.

   * Easy for new users: this documentation should help you to discover Alignak.
      This documentation shows simple use-cases and helps building more complex configurations.

   * Easy to migrate from Nagios: Nagios flat-files configuration and plugins will work with Alignak.
      We try to keep as much as possible an ascending compatibility with former Nagios configuration...

   * Multi-platform: python is available in a lot of Operating Systems.
      We try to write generic code to keep this possible. However, Linux and FreeBSD are the most tested OSes so far.
      As of now, Alignak is tested with Python 2.7, 3.5 and 3.6 versions but will work with Pypy in the future.

   * UTF-8 compliant: whatever your language, we take care of it.
      We are testing Alignak I/O with several languages and take care of localization.

   * Independent from other monitoring solution:
      Alignak is a framework that can integrate with other applications through standard interfaces.
      Flexibility first!

   * Flexible: in an architecture point of view.
      Alignak may be distributed across several servers, datacenters to suit the monitoring needs and constraints.
      It is our scalability wish!

   * Scalable: no server overloading,
      You just have to set-up new daemons on another server and load balancing is done.

   * Extensible: Alignak provides extension packs and modules
      For a large number of services :

      * Databases (MySQL, Oracle, Microsoft SQL Server, Memcached, MongoDB, InfluxDB etc.)
      * Routers, Switches (Cisco, Nortel, HP ProCurve etc.)
      * OSes (Linux, Windows, AIX, HP-UX etc.)
      * Hypervisors (VMware, vSphere)
      * Protocols (HTTP, SSH, LDAP, DNS, IMAP, FTP, etc.)
      * Application (WebLogic, Exchange, Active Directory, Tomcat, Asterisk, etc.)
      * Storage (IBM-DS, SafeKit, HACMP, etc.)

      * Smart NRPE polling : The NRPE Booster module is a must have to improve NRPE checks performance.
      * Smart SNMP polling : The SNMP Booster module is a must have if you have a huge infrastructure of routers and switches.

   * Easy to contribute: contribution has to be an easy process.
      Alignak follow pycodestyle (former pep8), pylint and pep257 coding standards to ease code readability.
      Step by step help to contribute to the project can be found in :ref:`Contributing <contributing/index>`

This is basically what Alignak is made of. May be add the *keep it simple* Linux principle and it's perfect.

But Alignak is even more :

    * Realm concept: you can monitor independent environments / customers

    * DMZ monitoring: some daemons have passive facilities so that firewall don't block monitoring protocols.

    * Business impact: Alignak can differentiate impact of a critical alert on a toaster versus the web store

    * Efficient correlation between parent-child relationship and business process rules

    * High availability: daemons can have spare instances.

    * Business rules:  For a higher level of monitoring. Alignak can notify you only if 3 out 5 of your server are down

    * Very open-minded team: help is always welcome, there is job for everyone :)


There is nothing we don't want, we consider every features / ideas and we really appreciate discussing about monitoring stuff! Feel free to join `by mail`_, `on the IRC`_, `on gitter chat room`_ to discuss with us or ask for more information

.. raw:: LaTeX

    \newpage

.. _Nagios: http://www.nagios.org
.. _GNU Affero General Public License: http://www.gnu.org/licenses/agpl.txt
.. _alignak-monitoring organization's page: https://github.com/Alignak-monitoring
.. _specific GitHub issue: https://github.com/Alignak-monitoring/alignak/issues/262
.. _by mail: mailto://contact@alignak.net/
.. _on the IRC: http://webchat.freenode.net/?channels=%23alignak
.. _on gitter chat room: https://gitter.im/Alignak-monitoring/alignak?utm_source=share-link&utm_medium=link&utm_campaign=share-link
.. _have a look: http://alignak.net
