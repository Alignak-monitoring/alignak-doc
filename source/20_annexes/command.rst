.. _monitoring_objects/command:

===================
Command Definition
===================

A command definition is just that. It defines a command. Commands that can be defined include service checks, service notifications, service event handlers, host checks, host notifications, and host event handlers. Command definitions can contain :ref:`macros <monitoring_features/macros>`, but you must make sure that you include only some macros that are “valid" when the command is executed. More information on which macros are available and when they are “valid" can be found :ref:`here <monitoring_features/macros>`. The different arguments to a command definition are outlined below.

.. tip::  If you need to have the '$' character in one of your command (and not referring to a macro), please put "$$" instead. Alignak will make the substitution


Syntax
======

Bold variables are required, while others are optional.
Emphasized variables are Alignak extensions with reference to the Nagios legacy definition.

============================== ==================
define command{
**command_name**               ***command_name***
**command_line**               ***command_line***
``enable_environment_macros``  *[0,1]*
``timeout``                    [-1, timeout value]
``poller_tag``
``reactionner_tag``
}
============================== ==================


Example
=======

::

  define command{
      command_name   check_pop
      command_line   /var/lib/shinken/libexec/check_pop -H $HOSTADDRESS$
  }


Variables
=========

command_name
  This directive is the short name used to identify the command. It is referenced in :ref:`contact <monitoring_objects/contact>`, :ref:`host <monitoring_objects/host>`, and :ref:`service <monitoring_objects/service>` definitions (in notification, check, and event handler directives), among other places.

command_line
  This directive is used to define what is actually executed by Alignak when the command is used for service or host checks, notifications, or :ref:`event handlers <monitoring_features/event_handlers>`. Before the command line is executed, all valid :ref:`macros <monitoring_features/macros>` are replaced with their respective values. See the documentation on macros for determining when you can use different macros. Note that the command line is *not* surrounded in quotes. Also, if you want to pass a dollar sign ($) on the command line, you have to escape it with another dollar sign.

  You may not include a **semicolon** (;) in the *command_line* directive, because everything after it will be ignored as a configuration file comment. You can workaround this limitation by setting one of the :ref:`$USERn$ <monitoring_features/macros>` macros in your configuration to a semicolon and then referencing the appropriate $USER$ macro in the *command_line* directive in place of the semicolon.

  If you want to pass arguments to commands during runtime, you can use :ref:`$ARGn$ macros <monitoring_features/macros>` in the *command_line* directive of the command definition and then separate individual arguments from the command name (and from each other) using bang (!) characters in the object definition directive (host check command, service event handler command, etc.) that references the command. More information on how arguments in command definitions are processed during runtime can be found in the documentation on :ref:`macros <monitoring_features/macros>`.

.. _monitoring_objects/command#enable_environment_macros:

enable_environment_macros
  This directive enabbles passing command parameters through the environment. See the global :ref:`enable_environment_macros <configuration/core#enable_environment_macros>` for more details. Enabling it on a command rather than globally allows to limit how much commands will receive environment macros. This is the preferred way, as processing environment macros and passing them to the command has a high cost in term of CPU and Memory.

poller_tag
  This directive is used to define the poller tag of this command. This parameter may be defined, in order of precedence, on a`command`, a `host` or a `service`. If a poller tag is set, only pollers holding the same tag will handle the corresponding action.

  By default there is no poller_tag, so all untagged pollers can execute the corresponding action.

reactionner_tag
  This directive is used to define the reactionner tag of this command. This parameter may be defined, in order of precedence, on a`command`, a `host` or a `service`. If a reactionner tag is set, only reactionners holding the same tag will handle the corresponding action.

  By default there is no reactionner_tag, so all untagged reactionners can execute the corresponding action.

priority
  This options defines the command's priority regarding checks execution. When a poller or a reactionner is asking for new actions to execute to the scheduler, it will return the highest priority tasks first (the lower the number, the higher the priority). The `priority` parameter may be set, in order of ascending precedence, on a `command`, on a `host` and on a `service`. Priority defaults to `100`.
