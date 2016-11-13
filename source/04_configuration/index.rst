.. raw:: LaTeX

    \newpage

.. _configuration/index:

Configuration
=============

.. raw:: LaTeX

    \newpage


The Alignak framework configuration is splitted in three main parts:

   - arbiter configuration,
   - daemons configuration,
   - external modules configuration.

The arbiter configuration part describes the Alignak framework infrastructure (which daemons are used and how they are) and the monitoring objects (what is monitored and how it is).

All the configuration is stored in the folder */usr/local/etc/alignak/* (or */etc/alignak/*).
Each main part of the configuration is described in the following chapters:

.. toctree::
   :maxdepth: 1

   arbiter
   daemons
   modules

This chapter gives information about configuring the inter-deamon communication with SSL.

.. toctree::
   :maxdepth: 1

   ssl_certificate

The last chapters give more information about the default installed configuration and some specific objects configuration features.

.. toctree::
   :maxdepth: 1

   default_configuration
   main_configuration_file
   custom_objects_var
