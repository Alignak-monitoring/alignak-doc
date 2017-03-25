.. _configuration/modules:

=======
Modules
=======


Alignak modules (official way)
------------------------------

Alignak modules are available on PyPI_. Their name starts with "alignak_module".
To install them, install with pip command::

     pip install alignak_module_nameofmodule

The configuration file of the installed module will be installed in the folder::

    etc/alignak/arbiter/modules

Each module installed on Alignak stores its configuration file in this part of the configuration.


.. _PyPI: https://pypi.python.org/pypi


Shinken modules
---------------

The Shinken modules are not compatible *as is* with Alignak. For some of them, only small
modifications are necessary in the source code, but we do not recommend it. A lot of the features
needing external modules in Shinken are now available thanks to the still existing Alignak modules.

Do not hesitate to contact the Alignak team on our IRC channel or developers list.

This matter should be discussed; for this please log an issue in the `Alignak
contributions project repository <https://github.com/Alignak-monitoring-contrib>`_.

