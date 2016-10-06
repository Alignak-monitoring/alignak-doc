.. _extending/alignak_webui:

=============
Alignak WebUI
=============

The Alignak Web User Interface is an independent Web application using the Alignak REST backend and
dedicated to the Alignak framework. It is used to display and edit the monitoring configuration.

More information is available in the `project repository <https://github.com/Alignak-monitoring-contrib/alignak-webui>`_.

The WebUI documentation is available on http://alignak-web-ui.readthedocs.io/


Installing the WebUI
====================

Installing the Alignak WebUI is an easy operation.
The different installation methods are detailed in the documentation but the most easy way is to::

    pip install alignak-webui


Once installed, the WebUI is ready to run and you can start with::

    ./bin/run.sh


Configuring the Alignak WebUI is made easy thanks to a *settings.cfg* file.
The default configuration makes the WebUI run on the local interface, on port 5001, and use a local Alignak backend running on port 5000.

**Note**: to be updated after the WebUI packaging ...

A WebUI installation tutorial exists on the `SysAdmin.cool web site <http://sysadmin.cool/>`_.

Using the WebUI
===============

As soon as the WebUI is running, you can open your Web browser and make it point to http://localhost:5001.

For more information about the WebUI configuration and features, have a look in the documentation on http://alignak-webui.readthedocs.io/en/latest/intro.html
