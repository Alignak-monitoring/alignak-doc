.. _alignak_features/internal_checks:

===============
Internal Checks
===============


Introduction
------------

Alignak allows to define a ``check_command`` that do not require executing a plugin to make the host or service state change. The main interest of this feature is to allow defining and testing a monitored system configuration with many hosts dependencies without the need to check everything in a real environment. It also allows having an Alignak demo mode with some activity ;)

.. note:: the default shipped configuration defines some few hosts / services with only internal checks :)


Internal hosts check command
----------------------------

Alignak allows to define a ``check_command`` that makes it consider an host to have always the same state. Defining the **_host_internal_check** command as the host ``check_command`` will make the host always have the same state and output.

The **_host_internal_check** must be specified with 2 parameters:
- plugin exit code: 0 (Up), 1 (Down), 2 (Down), 3 (Unknown), 4 (Unreachable)
- plugin output: string

If the plugin output is an empty string, Alignak will build an output message as ``Host internal check result: X`` where X is the plugin exit code.

If the plugin exit code is composed as a comma separated list, Alignak will randomly choose one of the values as the exit code. This allows to use internal checks to simply simulate hosts states changing.

.. note:: If the syntax of this command is not correct, the check will be considered as and unknown check and the output will be *Malformed host internal check*.

 ::

   # Define an internal check:
   # - 1st parameter is the plugin exit code
   # - 2nd parameter is the plugin output message
   # ------
   # hen check output is empty, Alginak builds a string with the exit code

   # Some hosts that are always in the same state
   define host{
      use                  generic-host
      host_name            host_0
      address              127.0.0.1

      check_interval       5

      check_command        _internal_host_check!0!I am always Up
   }
   define host{
      use                  generic-host
      host_name            host_1
      address              127.0.0.1

      check_interval       5

      check_command        _internal_host_check!1!I am always Down
   }
   define host{
      use                  generic-host
      host_name            host_2
      address              127.0.0.1

      check_interval       5

      check_command        _internal_host_check!2!I am always Down
   }
   define host{
      use                  generic-host
      host_name            host_3
      address              127.0.0.1

      check_interval       5

      check_command        _internal_host_check!3!I am always Unknown
   }
   define host{
      use                  generic-host
      host_name            host_4
      address              127.0.0.1

      check_interval       5

      check_command        _internal_host_check!4!I am always Unreachable
   }

   # No check output
   define host{
      use                  generic-host
      host_name            host_5
      address              127.0.0.1

      check_interval       5

      check_command        _internal_host_check!0!
   }

   # Define multiple possible exit codes - Alignak randomly chooses one on each check laungh
   # Check output is empty
   define host{
      use                  generic-host
      host_name            host_6
      address              127.0.0.1

      check_interval       5

      check_command        _internal_host_check!0,2!
   }



Internal services check command
-------------------------------

Alignak allows to define a ``check_command`` that makes it consider a service to have always the same state. Defining the **_service_internal_check** command as the service ``check_command`` will make the service always have the same state and output.

The **_service_internal_check** must be specified with 2 parameters:
- plugin exit code: 0 (Up), 1 (Warning), 2 (Critical), 3 (Unknown), 4 (Unreachable)
- plugin output: string

If the plugin output is an empty string, Alignak will build an output message as ``Service internal check result: X`` where X is the plugin exit code.

If the plugin exit code is composed as a comma separated list, Alignak will randomly choose one of the values as the exit code. This allows to use internal checks to simply simulate services states changing.

.. note:: If the syntax of this command is not correct, the check will be considered as and unknown check and the output will be *Malformed host internal check*.

 ::

   # Define some internal service checks:
   # - 1st parameter is the plugin exit code
   # - 2nd parameter is the plugin output message
   # ------
   # When the check output is empty, Alignak builds a string with the exit code

   # Some services that are always in the same state
   define service{
      check_command               _echo
      host_name                   test-host
      service_description         dummy_echo
   }
   define service{
      check_command               _internal_service_check!0!$HOSTNAME$!$SERVICEDESC$!%d
      host_name                   test-host
      service_description         dummy_ok
   }
   define service{
      check_command               _internal_service_check!1!$HOSTNAME$-$SERVICEDESC$-%d
      host_name                   test-host
      service_description         dummy_warning
   }
   define service{
      check_command               _internal_service_check!2!$HOSTNAME$-$SERVICEDESC$-%d
      host_name                   test-host
      service_description         dummy_critical
   }
   define service{
      check_command               _internal_service_check!3!$HOSTNAME$-$SERVICEDESC$-%d
      host_name                   test-host
      service_description         dummy_unknown
   }
   define service{
      check_command               _internal_service_check!4!$HOSTNAME$-$SERVICEDESC$-%d
      host_name                   test-host
      service_description         dummy_unreachable
   }

   # No check output
   define service{
      check_command               _internal_service_check!0!
      host_name                   test-host
      service_description         dummy_no_output
   }

   # Define multiple possible exit codes - Alignak randomly chooses one on each check laungh
   # Check output is empty
   define service{
      check_command               _internal_service_check!0,1,2,3,4!
      host_name                   test-host
      service_description         dummy_random
   }


Services state changes
----------------------

When Alignak checks the status of services, it will be able to detect when a service changes between OK, WARNING, UNKNOWN, and CRITICAL states and take appropriate action. These state changes result in different state types (HARD or SOFT), which can trigger :ref:`event handlers <monitoring_features/event_handlers>` to be run and :ref:`notifications <monitoring_features/notifications>` to be sent out. Service state changes can also trigger on-demand :ref:`host checks <monitoring_features/hosts_checks>`. Detecting and dealing with state changes is what Alignak is all about.

Soft (state type is SOFT) states occur when the service checks return a non-OK state and are in the process of being retried. Hard states (state type is HARD) result when the service checks have been checked a specified maximum number of times and the current state is confirmed.

When services change state too frequently they are considered to be “flapping". Alignak can detect when services start flapping, and can suppress notifications until flapping stops and the service's state stabilizes. More information on the flap detection logic can be found :ref:`here <monitoring_features/flapping>`.

