.. _contributing/modules-and-checks-packages:

===================================
Alignak modules and checks packages
===================================


Modules
=======

Alignak framework can be extended with daemon modules. A module is an extra Python module installed and configured for as an Alignak daemon to feed it with some broks.
Broks are events and pieces of information resulting from the Alignak internal monitoring process.

Some modules examples:

    * logs module that build a log of all the monitoring events (alerts, notifications, ...)
    * Web services module that exposes some web services to interact with the Alignak framework
    * NSCA collector module that collects NSCA passive checks to feed Alignak with
    * backend scheduler module that saves and loads retention data


An `Alignak example module`_ is available in the `Alignak organization on GitHub`_ .
This module is documented to explain how to build a module and which information are available into the modules.

The existing modules available in the `Alignak contribution organization on GitHub`_ are also good examples to help digging into the module code.


Checks packs
============

Alignak framework configuration can be enriched with checks packages. A check package is an extra
Python module installed to provide some more configuration and/or plugins in the Alignak framework.

Some checks packages examples:

    * check with NRPE
    * Windows checks using WMI or NSCA
    * HTML notifications


An `Alignak example checks package`_ exists is available in the `Alignak organization on GitHub`_ .
This package is documented to explain how to build a checks package.

The existing checks packages available in the `Alignak contribution organization on GitHub`_
are also good examples to help digging into such a code ;)


.. _Alignak contribution organization on GitHub: https://github.com/Alignak-monitoring
.. _Alignak organization on GitHub: https://github.com/Alignak-monitoring
.. _Alignak example module: https://github.com/Alignak-monitoring/alignak-module-example
.. _Alignak example checks package: https://github.com/Alignak-monitoring/alignak-checks-example
