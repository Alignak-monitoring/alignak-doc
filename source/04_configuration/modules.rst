
=======
Modules
=======

There are 2 types of modules


Alignak modules (official way)
------------------------------

Alignak modules use namespace of alignak. 
To install them, install with pip command::

     pip install nameofmodule

The configuration file will be available in folder::

    etc/alignak/arbiter_cfg/modules


Shinken modules
---------------

Shinken modules can be compatibles, but need these information:

* Modules with dependencies with internal shinken modules like webui or bottle can't works
* Install manually, so add modules/ files into modules folder /var/lib/alignak/modules/[name of your module without mod-]
* Install manually, so add configuration files into folder etc/alignak/arbiter_cfg/modules/


