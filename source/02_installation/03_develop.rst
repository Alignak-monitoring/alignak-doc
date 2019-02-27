.. _Installation/develop:

============================
Installation for development
============================

Requirements
============

The requirements listed for a `pip` installation are also applicable. See :ref:`this page<Installation/pip>`.


Installation from the git repository
====================================

This procedure is the recommended one if you want to be able to hack the code base. For only installing, see :ref:`this page<Installation/pip>`.

* Clone the Alignak repository::

    $ git clone https://github.com/Alignak-monitoring/alignak
    $ cd alignak

* Install with python pip (**sudo needed**)::

    # Install requirements for development and tests
    $ sudo ./tests/setup_test.sh

    # Install Alignak in develop mode
    $ sudo pip install -e .
        ...
        ...
        Successfully installed ...

.. note:: that installing with root/sudo credentials is needed because some scripts are created in the */usr/local/bin* directory. The drawback is that some files are owned by the root user...

