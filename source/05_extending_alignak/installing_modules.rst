.. _extending/modules:

==================
Installing modules
==================

Extending Alignak features is made thanks to daemons modules. Some modules are already packaged in an easy installable package called an **Alignak module package**.

Alignak modules
===============

NSCA collector
--------------

Managing passive hosts and services requires that Alignak *listen* to passive checks.
To achieve this, the Receiver daemon needs to be extended with an NSCA module. This module listens
on a TCP port for NSCA packets, decode the packets and builds an external command corresponding to
the received check: HOST PASSIVE CHECK or SERVICE_PASSIVE_CHECK.

Short story::

    pip install alignak-module-nsca

    # Update your receiver daemon configuration
    define receiver{
        receiver_name           receiver-master

        modules                 nsca
    }


    # Edit the NSCA configuration (etc/alignak/arbiter/modules/mod-nsca.cfg)
    define module {
        module_alias             nsca
        python_name              alignak_module_nsca
        ...
        ...

    }

The module default configuration allows to collect non-encrypted NSCA checks for hosts and services.
The file is largely commented to help understand and configure this module.

More information is available in the `module project repository <https://github.com/Alignak-monitoring-contrib/alignak-module-nsca>`_.


Alignak backend
---------------

The Alignak backend module(s) implements several features for several Alignak daemons:

    - loads the configuration for the Arbiter
    - updates the monitored objects live state for the Broker
    - state retention of the live state for the Scheduler

Installing this module will, in fact, install the three modules.

**Note**: this module implies that you already installed the Alignak backend.

Short story::

    pip install alignak-module-backend

    # Update your arbiter daemon configuration
    define arbiter{
        arbiter_name            arbiter-master

        modules                 backend_arbiter
    }


    # Edit the backend arbiter module configuration (etc/alignak/arbiter/modules/mod-alignak_backend_arbiter.cfg)
    define module {
        module_alias            backend_arbiter
        python_name             alignak_module_backend.arbiter
        ...
        ...

    }

    # Update your broker daemon configuration
    define broker{
        broker_name             broker-master

        modules                 backend_broker
    }


    # Edit the backend broker module configuration (etc/alignak/arbiter/modules/mod-alignak_backend_broker.cfg)
    define module {
        module_alias            backend_broker
        python_name             alignak_module_backend.broker
        ...
        ...

    }

    # Update your arbiter scheduler configuration
    define arbiter{
        scheduler_name          scheduler-master

        modules                 backend_scheduler
    }


    # Edit the backend scheduler module configuration (etc/alignak/arbiter/modules/mod-alignak_backend_scheduler.cfg)
    define module {
        module_alias            backend_scheduler
        python_name             alignak_module_backend.scheduler
        ...
        ...

    }

The modules default configuration needs to be updated with your backend connection and login information.
The file is largely commented to help understand and configure this module.

More information is available in the `module project repository <https://github.com/Alignak-monitoring-contrib/alignak-module-backend>`_.

