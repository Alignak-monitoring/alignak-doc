.. _Installation/verify:

===========================
Check Alignak configuration
===========================

Check installation
==================

.. note:: the */usr/local* directory prefix is the Python prefix and it may be different according to a specific configuration. Please adapt the following examples if needed...

The installation process installs the *alignak* python library in the system global Python packages. To check a correct Python installation::

   # Get the alignak version
   $ python -c 'from alignak.bin import VERSION; print(VERSION)'

It also installs some startup scripts::

   # Some startup scripts for the Alignak dameons
   $ ll /usr/local/bin/alignak*
      -rwxr-xr-x 1 root root 166 janv. 30 09:30 /usr/local/bin/alignak-arbiter*
      -rwxr-xr-x 1 root root 165 janv. 30 09:30 /usr/local/bin/alignak-broker*
      -rwxr-xr-x 1 root root 170 janv. 30 09:30 /usr/local/bin/alignak-environment*
      -rwxr-xr-x 1 root root 165 janv. 30 09:30 /usr/local/bin/alignak-poller*
      -rwxr-xr-x 1 root root 170 janv. 30 09:30 /usr/local/bin/alignak-reactionner*
      -rwxr-xr-x 1 root root 167 janv. 30 09:30 /usr/local/bin/alignak-receiver*
      -rwxr-xr-x 1 root root 168 janv. 30 09:30 /usr/local/bin/alignak-scheduler*

Some help scripts and a default configuration are shipped with the application. They are located in the */usr/local/share/alignak/* directory::

   # Check Alignak installation
   # It copied the default shipped files and sample configuration.
   ll /usr/local/share/alignak/
      total 27
      -rw-r--r--  1 root  wheel   124 Jul 28 11:05 alignak-log-rotate
      -rw-r--r--  1 root  wheel  2846 Jul 28 11:05 alignak.ico
      drwxr-xr-x  6 root  wheel     6 Jul 28 11:05 bin/
      drwxr-xr-x  6 root  wheel    10 Jul 28 11:05 etc/
      -rwxr-xr-x  1 root  wheel  4285 Jul 28 11:05 post-install.sh*
      -rw-r--r--  1 root  wheel  1859 Jul 28 11:05 requirements.txt

The *post-install.sh* script is used by the system packaging installation to prepare using the Alignak application. This script will:

- install the required python packages,
- create the alignak user account if it does not exist,
- create some directories and set appropriate credentials for the user account on some directories

You can run this script with some parameters: a user account name and a python prefix. Default parameters are `alignak` and `/usr/local`.

The default configuration is shipped in the */usr/local/share/alignak/etc* directory::

   ll /usr/local/share/alignak/etc/
      total 52
      -rw-r--r--  1 root  wheel    691 Jul 28 11:05 README
      -rw-r--r--  1 root  wheel   1944 Jul 28 11:05 alignak-logger.json
      -rw-r--r--  1 root  wheel   2024 Jul 28 11:05 alignak.cfg
      drwxr-xr-x  2 root  wheel      8 Jul 28 11:05 alignak.d/
      -rw-r--r--  1 root  wheel  24321 Jul 28 11:05 alignak.ini
      drwxr-xr-x  7 root  wheel      7 Jul 28 11:05 arbiter/
      drwxr-xr-x  2 root  wheel      6 Jul 28 11:05 certs/
      drwxr-xr-x  3 root  wheel      4 Jul 28 11:05 sample/

The *alignak.ini* is the main configuration file that must be provided as a parameter to any launched daemon. See the Alignak configuration chapter for more information on this file and its content.

To verify that the configuration provided to Alignak is correct and will allow starting the application, you can run the Alignak arbiter daemon in verify mode using the ``-V`` parameter::

   # Verify the default shipped configuration
   alignak-arbiter -V -e /usr/local/share/alignak/etc/alignak.ini
      Loading configuration files: ['/usr/local/share/alignak/etc/alignak.ini', '/usr/local/share/alignak/etc/alignak.d/daemons.ini', '/usr/local/share/alignak/etc/alignak.d/modules.ini']
      Daemon 'arbiter-master' logger configuration file: /usr/local/etc/alignak/alignak-logger.json
      Error message: Daemon directory '/usr/local/var/log/alignak' did not exist, and I could not create.'. Exception: [Errno 13] Permission denied: '/usr/local/var/log'
      Error message: Daemon directory '/usr/local/var/log/alignak/monitoring-log' did not exist, and I could not create.'. Exception: [Errno 13] Permission denied: '/usr/local/var/log'
      Daemon 'arbiter-master' pid file: /usr/local/var/run/alignak/arbiter-master.pid
      Error message: Daemon directory '/usr/local//var/run/alignak' did not exist, and I could not create.'. Exception: [Errno 13] Permission denied: '/usr/local//var/run'
      Error message: Error changing to working directory: /usr/local//var/run/alignak. Error: [Errno 2] No such file or directory: '/usr/local//var/run/alignak'. Check the existence of /usr/local//var/run/alignak and the alignak/alignak account permissions on this directory.


.. note:: You may have some errors when running the Alignak arbiter from the command line. This is almost often because you current user account is not allowed to use the default configured directories!


Troubleshooting daemons start
=============================

Configured user account
-----------------------

You get this error::

   sudo alignak-arbiter -V -e /usr/local/share/alignak/etc/alignak.ini
      Loading configuration files: ['/usr/local/share/alignak/etc/alignak.ini', '/usr/local/share/alignak/etc/alignak.d/daemons.ini', '/usr/local/share/alignak/etc/alignak.d/modules.ini']
      Daemon 'arbiter-master' logger configuration file: /usr/local/etc/alignak/alignak-logger.json
      Created the directory: /usr/local/var/log/alignak, stat: posix.stat_result(st_mode=16877, st_ino=141828, st_dev=2049, st_nlink=2, st_uid=0, st_gid=0, st_size=4096, st_atime=1529552405, st_mtime=1529552405, st_ctime=1529552405)
      Created the directory: /usr/local/var/log/alignak/monitoring-log, stat: posix.stat_result(st_mode=16877, st_ino=141829, st_dev=2049, st_nlink=2, st_uid=0, st_gid=0, st_size=4096, st_atime=1529552405, st_mtime=1529552405, st_ctime=1529552405)
      Daemon 'arbiter-master' pid file: /usr/local/var/run/alignak/arbiter-master.pid
      Error message: You want the application to run with the root account credentials? It is not a safe configuration! If you really want it, set: 'idontcareaboutsecurity=1' in the configuration file.

This is because you are starting the Alignak arbiter as a root user and you did not configured the ``user`` and ``group`` variables in the *alignak.ini* file.

**Solutions**:

 - configure the ``user`` and ``group`` variables with a user account in the alignak configuration file.
 - define the ``ALIGNAK_USER`` and ``ALIGNAK_GROUP`` environment variables

Directory access rights
-----------------------

You get this error::

   alignak-arbiter -V -e /usr/local/share/alignak/etc/alignak.ini
      Loading configuration files: ['/usr/local/share/alignak/etc/alignak.ini', '/usr/local/share/alignak/etc/alignak.d/daemons.ini', '/usr/local/share/alignak/etc/alignak.d/modules.ini']
      Daemon 'arbiter-master' logger configuration file: /usr/local/etc/alignak/alignak-logger.json
      Error message: Daemon directory '/usr/local/var/log/alignak' did not exist, and I could not create.'. Exception: [Errno 13] Permission denied: '/usr/local/var/log'
      Error message: Daemon directory '/usr/local/var/log/alignak/monitoring-log' did not exist, and I could not create.'. Exception: [Errno 13] Permission denied: '/usr/local/var/log'
      Daemon 'arbiter-master' pid file: /usr/local/var/run/alignak/arbiter-master.pid
      Error message: Daemon directory '/usr/local//var/run/alignak' did not exist, and I could not create.'. Exception: [Errno 13] Permission denied: '/usr/local//var/run'
      Error message: Error changing to working directory: /usr/local//var/run/alignak. Error: [Errno 2] No such file or directory: '/usr/local//var/run/alignak'. Check the existence of /usr/local//var/run/alignak and the alignak/alignak account permissions on this directory.


Alignak arbiter is trying to create some directories (*/usr/local/var/log/alignak* and * /usr/local/var/run/alignak*) but it is not allowed to because of the current user account credentials.

If you are usually using the Aligak system services, you should already have some existing directories: */var/log/alignak* and */var/run/alignak*. You only have ti update the configuration file accordingly::

    _dist_RUN=/var/run/alignak
    _dist_LOG=/var/log/alignak

Create the directories and make sure that the current user account is allowed to write in those directories. The best solution is to::

   sudo mkdir /usr/local/var/run/alignak
   sudo chown alignak:wheel /usr/local/var/run/alignak
   sudo chmod 775 /usr/local/var/run/alignak
   sudo mkdir /usr/local/var/log/alignak
   chown alignak:wheel /usr/local/var/log/alignak
   chmod 775 /usr/local/var/log/alignak

Because your current user account is ``sudo`` enabled, use the *wheel* group.
