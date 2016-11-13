.. _monitoring_features/external_commands:

=================
External Commands
=================


Introduction
------------

Alignak can process commands from external applications to alter various aspects of its monitoring functions based on the commands it receives.


Enabling external commands
--------------------------

.. image:: /_static/images///official/images/externalcommands.png
   :scale: 90 %


In order to have Alignak process external commands, make sure you do the following:

    * Enable external command checking with the ``check_external_commands`` parameter in the monitoring configuration file.
    * Install an external commands capable module near the Alignak receiver daemon.

An external commands capable module implements a solution to provide the external commands to Alignak.

The :ref:`external commands named pipe module <modules/named_pipe>` allows an external application residing on the same host as Alignak to simply write the external commands directly to a named pipe file as outlined above. However, applications on remote hosts can't do this so easily.

The :ref:`NSCA collector module <modules/nsca>` collects the passive checks sent by the *send_nsca*  command or from an NSCA agent (eg. Windows NSClient ++). This module only manages the external commands for receiving the passive checks.

The :ref:`Web services module <modules/web_services>` exposes a Web service (``POST /alignak_command``) that allows to notify external commands to the Alignak framework.


Using external commands
-----------------------

External commands can be used to accomplish a lot of different things while Alignak is running. Example of what can be done include temporarily disabling notifications for services and hosts, temporarily disabling service checks, forcing immediate service checks, adding comments to hosts and services, etc.


Command format
--------------

External commands have the following format:


::

    [<timestamp>] COMMAND;command_arguments


where ``timestamp`` is the time (in "time_t" format) that the external application submitted the external command. The values for the ``COMMAND`` and ``command_arguments`` will depend on what command is being submitted to Alignak.

A full listing of external commands that can be used (along with examples of how to use them) can be found online at the following URL:

http://www.nagios.org/developerinfo/externalcommands/


External commands list
----------------------

The :ref:`external commands list annex <annexes/external_commands_list>` contain a descriptions of each external command.

.. warning:: all those external commands are not implemented in Alignak! This list contains all the commonly known external commands and keep you informed if the command is implemented or not!
