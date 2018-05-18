.. _Installation/verify:

===========================
Check Alignak configuration
===========================

Check installation
==================

.. note:: the */usr/local* directory prefix is the Python prefix and it may be different according to a specific configuration. Adpat the following examples if needed...

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
      total 24
      drwxr-xr-x 4 root root 4096 juin  21 05:39 ./
      drwxr-xr-x 9 root root 4096 juin  21 05:39 ../
      drwxr-xr-x 5 root root 4096 juin  21 05:39 bin/
      drwxr-xr-x 6 root root 4096 juin  21 05:39 etc/
      -rwxr-xr-x 1 root root  477 juin  21 05:38 post-install.sh*
      -rw-r--r-- 1 root root 1887 juin  21 05:38 requirements.txt

The *post-install.sh* script is used by the system packaging installation to prepare using the Alignak application. This script will:

- install the required python packages,
- create the alignak user account if it does not exist,
- create some directories and set appropriate credentials for the user account

You can run this script with some parameters: a user account name and a python prefix. Default parameters are `alignak` and `/usr/local`.

      # Verify the default shipped configuration
      alignak-arbiter -V -e /usr/local/share/alignak/etc/alignak.ini
         Loading configuration files: ['/usr/local/share/alignak/etc/alignak.ini', '/usr/local/share/alignak/etc/alignak.d/daemons.ini', '/usr/local/share/alignak/etc/alignak.d/modules.ini']
         Daemon 'arbiter-master' logger configuration file: /usr/local/etc/alignak/alignak-logger.json
         Error message: Daemon directory '/usr/local/var/log/alignak' did not exist, and I could not create.'. Exception: [Errno 13] Permission denied: '/usr/local/var/log'
         Error message: Daemon directory '/usr/local/var/log/alignak/monitoring-log' did not exist, and I could not create.'. Exception: [Errno 13] Permission denied: '/usr/local/var/log'
         Daemon 'arbiter-master' pid file: /usr/local/var/run/alignak/arbiter-master.pid
         Error message: Daemon directory '/usr/local//var/run/alignak' did not exist, and I could not create.'. Exception: [Errno 13] Permission denied: '/usr/local//var/run'
         Error message: Error changing to working directory: /usr/local//var/run/alignak. Error: [Errno 2] No such file or directory: '/usr/local//var/run/alignak'. Check the existence of /usr/local//var/run/alignak and the alignak/alignak account permissions on this directory.

You may receive some errors when verifying the Alignak configuration
      sudo alignak-arbiter -V -e /usr/local/share/alignak/etc/alignak.ini
         Loading configuration files: ['/usr/local/share/alignak/etc/alignak.ini', '/usr/local/share/alignak/etc/alignak.d/daemons.ini', '/usr/local/share/alignak/etc/alignak.d/modules.ini']
         Daemon 'arbiter-master' logger configuration file: /usr/local/etc/alignak/alignak-logger.json
         Created the directory: /usr/local/var/log/alignak, stat: posix.stat_result(st_mode=16877, st_ino=141828, st_dev=2049, st_nlink=2, st_uid=0, st_gid=0, st_size=4096, st_atime=1529552405, st_mtime=1529552405, st_ctime=1529552405)
         Created the directory: /usr/local/var/log/alignak/monitoring-log, stat: posix.stat_result(st_mode=16877, st_ino=141829, st_dev=2049, st_nlink=2, st_uid=0, st_gid=0, st_size=4096, st_atime=1529552405, st_mtime=1529552405, st_ctime=1529552405)
         Daemon 'arbiter-master' pid file: /usr/local/var/run/alignak/arbiter-master.pid
         Error message: You want the application to run with the root account credentials? It is not a safe configuration! If you really want it, set: 'idontcareaboutsecurity=1' in the configuration file.
