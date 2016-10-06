.. _extending/checks:

=================
Installing checks
=================

The default configuration does not include any plugins to check the monitored hosts and services.
If we leave it in this state, our system will be really unuseful !

The plugins used to check if an host or service is ok are not part of Alignak.
There are litteraly thousands of plugins available to check various systems.
To make Alignak use a new plugin to check, you must:

    * install the plugin
    * declare a new command that specifies how the plugin must be used
    * declare a new service on the monitored host

Alignak propose an easy way to deal with these operations by providing the most common checks
plugins already packaged in an easy installable package called an **Alignak checks package**.

*** TO BE COMPLETED ***

Alignak checks packs
====================

All the Alignak checks packs have their own repository and are easily installable thanks to Python PIP.

Using each pack is documented in the pack repository README file.


Checking with the Monitoring plugins
------------------------------------

For checking an host and its most common services with the "most common" plugins, `install monitoring package <https://github.com/Alignak-monitoring-contrib/alignak-checks-monitoring>`_.

The monitoring plugins are most often known as Nagios plugins... they provide many checks for network services.

Short story::

    pip install alignak-checks-monitoring

    define host{
        use                 dns, ftp, http
        host_name           snmp_host
        address             127.0.0.1
    }



Checking with SNMP
------------------

For checking an host and its most common services through SNMP, `install SNMP package <https://github.com/Alignak-monitoring-contrib/alignak-checks-snmp>`_.

Hosts inherit from a check command that gets the host uptime with an SNMP get, this to confirm that the host is alive and that SNMP connection is ok.

Short story::

    pip install alignak-checks-snmp

    define host{
        use                 linux-snmp
        host_name           snmp_host
        address             127.0.0.1
    }


Checking with NRPE
------------------

For checking an host and its most common services through NRPE, `install NRPE package <https://github.com/Alignak-monitoring-contrib/alignak-checks-nrpe>`_.

Hosts inherit from a check command that gets the host NRPE daemon version, this to confirm that the host is alive and that NRPE connection is ok.

Short story::

    pip install alignak-checks-nrpe

    define host{
        use                 linux-nrpe
        host_name           nrpe_host
        address             127.0.0.1
    }


Checking with WMI
-----------------

For checking an host and its most common services through WMI, `install WMI package <https://github.com/Alignak-monitoring-contrib/alignak-checks-wmi>`_.

Hosts inherit from a check command that gets the host uptime with a WMI request, this to confirm that the host is alive and that WMI connection is ok.

Short story::

    pip install alignak-checks-wmi

    define host{
        use                 windows-wmi
        host_name           wmi_host
        address             127.0.0.1
    }


Passive checking Windows with NSCA
----------------------------------

For checking a Windows host and its most common services through NSCA, `install Windows NSCA package <https://github.com/Alignak-monitoring-contrib/alignak-checks-windows-nsca>`_.

With this type of checking, hosts do not have any check_command (indeed they have a fake one ...) because Alignak is waiting for the hosts and services to send their own check information.

**Note**: this checks pack assumes that your Windows host is using the `NSClient agent`_.

Short story::

    pip install alignak-checks-windows-nsca

    define host{
        use                 windows-nsca
        host_name           nsca_windows_host
        address             0.0.0.0
    }


.. _NSClient agent: https://www.nsclient.org
