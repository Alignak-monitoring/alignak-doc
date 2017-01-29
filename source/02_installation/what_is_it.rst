.. _Installation/what_is_it:

===========================
What the installation does?
===========================

The setup script
================

The python *setup.py* script used by Alignak does the following:

* test if an ``alignak`` user and an ``alignak`` users group exist on the system

* determine the installation directory ``prefix`` according to the system (most often it is */usr/local*)

* copy the alignak default configuration files in *prefix/etc/alignak* and update some files:

    - update the *alignak.ini* file according to the used installation directory
    - update the *alignak.cfg* file according to the used installation directory
    - update the *daemons/*.ini* files according to the used installation directory
    - update the *paths.cfg* file with the Alignak plugins directory

Installation procedure
======================

First step - installation directories
-------------------------------------

Some variables are used by the installation script to update some of the installed files. All these variables are prefixed with the installation directory ``prefix``, except for the ``USER`` and ``GROUP`` variables.

Default variables values::

    'USER': 'alignak',
    'GROUP': 'alignak',
    'BIN': '/bin',
    'ETC': '/etc/alignak',
    'VAR': '/var/libexec/alignak',
    'RUN': '/var/run/alignak',
    'LOG': '/var/log/alignak'

If it exists an */etc/default/alignak* or */usr/local/etc/default/alignak* file on the system, the the variables are updated with their existing respective values defined in this file. This file may exist from a former installation of Alignak on a system using the init.d script system. Most often, this file will not exist on your system and the variables will keep their default values!

Second step - updating macro definitions
----------------------------------------

All the **.cfg* files located in the *etc/alignak* directory and its sub-directories are parsed to be updated. If they contain a line starting with a macro declaration for one of the script variables, this line is replaced with the variable value. As an example:
::

    #-- Alignak main directories
    #-- Those macros are automatically updated during the Alignak installation
    #-- process (eg. python setup.py install)
    $BIN$=/usr/local/bin
    $ETC$=/usr/local/alignak/etc
    $VAR$=/usr/local/var
    $RUN$=$VAR$/run
    $LOG$=$VAR$/log

    $USER$=alignak
    $GROUP$=alignak

This allows to add those macro definition in any configuration located in the Alignak configuration. Currently, only the *etc/alignak/arbiter/resource.d/paths.cfg* file is including those macros.


Third step - updating configuration
-----------------------------------

All the **.in* files located in the *etc/alignak* directory are parsed to be updated. If they contain a line starting with one of the script variables, this line is replaced with the variable value. As an example:
::

    [DEFAULT]
    BIN=../alignak/bin
    ETC=../etc
    VAR=/tmp
    RUN=/tmp
    LOG=/tmp
    USER=alignak
    GROUP=alignak


This allows to add those macro definition in any configuration located in the Alignak configuration. Currently, only the *etc/alignak/alignak.ini* file and all the files located in the *etc/aligank/daemons* directory are concerned.

**Note:** For the **.ini* files, the script also replaces the *workdir* , *logdir* and *etcdir* variables and they are respectively replaced with *RUN*, *LOG* and *ETC* variables values.
