.. raw:: LaTeX

    \newpage

.. _configuration/index:

Configuration
=============

.. raw:: LaTeX

    \newpage


The Alignak configuration is splitted in three main parts:

- arbiter configuration,
- daemons configuration,
- external modules configuration.

The arbiter configuration part describes the Alignak framework infrastructure
(which daemons are used and how they are) and the monitoring objects (what is monitored and how it is).

All the configuration is stored in the folder */usr/local/etc/alignak/* (or */etc/alignak/*).
Each main part of the configuration is described in the following chapters:

The last chapter gives more information about the default installed configuration.

.. toctree::

   arbiter
   daemons
   modules
   default_configuration

