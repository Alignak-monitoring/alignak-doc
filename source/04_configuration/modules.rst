
=======
Modules
=======


Alignak modules (official way)
------------------------------

Alignak modules are available on pip with name begin with "alignak_module_".
To install them, install with pip command::

     pip install alignak_module_nameofmodule

The configuration file of the installed module will be installed in the folder::

    etc/alignak/arbiter/modules


Documentation links
~~~~~~~~~~~~~~~~~~~

So there are the steps to install all:

* install Alignak_
* install the alignak-backend_
* install the mod-alignakbackend_ (documentation not yet writen, so link to repository)
* install the alignak-webui_

.. _Alignak: http://alignak-doc.readthedocs.org/en/latest/02_gettingstarted/installations/index.html
.. _alignak-backend: http://alignak-backend.readthedocs.org/en/latest/install.html
.. _mod-alignakbackend: https://github.com/Alignak-monitoring-contrib/alignak-module-backend
.. _alignak-webui: http://alignak-web-ui.readthedocs.io/en/latest/index.html


Shinken modules
---------------

The Shinken module are not compatible *as is* with Alignak. For some of them, only small
modifications are necessary in the source code, but we do not recommend it. A lot of the features
needing external modules in Shinken are now available thanks to the still existing Alignak modules.

Do not hesitate to contact the Alignak team on our IRC channel or developers list.

This matter should be discussed; for this please log an issue in the `Alignak
contributions project repository <https://github.com/Alignak-monitoring-contrib>`_.

