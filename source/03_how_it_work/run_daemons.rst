.. _howitworks/run_alignak:

===============
Running Alignak
===============

With packaging
==============

If you install with packaging (DEB, RPM...), see the package documentation for the best soltuion to start/stop Alignak.


With sources and pip
====================

You can start all daemons (as alignak user) like this::

    /usr/local/etc/init.d/alignak start

    Starting scheduler:
       ...done.
    Starting poller:
       ...done.
    Starting reactionner:
       ...done.
    Starting broker:
       ...done.
    Starting receiver:
       ...done.
    Starting arbiter:
       ...done.

You can stop all daemons (as alignak user) like this::

    /usr/local/etc/init.d/alignak stop

    Stopping scheduler
       ...done.
    Stopping poller
       ...done.
    Stopping reactionner
       ...done.
    Stopping broker
       ...done.
    Stopping receiver
       ...done.
    Stopping arbiter
       ...done.


Restart to load a new configuration::

    Restarting scheduler
       ...done.
    Restarting poller
       ...done.
    Restarting reactionner
       ...done.
    Restarting broker
       ...done.
    Restarting receiver
       ...done.
    Restarting arbiter
    Doing config check
       ...done.
       ...done.

You can also start each daemon individually.

For Broker::

    alignak-broker -c /usr/local/etc/alignak/daemons/brokerd.ini

For Scheduler::

    alignak-scheduler -c /usr/local/etc/alignak/daemons/schedulerd.ini

For Poller::

    alignak-poller -c /usr/local/etc/alignak/daemons/pollerd.ini

for Reactionner::

    alignak-reactionner -c /usr/local/etc/alignak/daemons/reactionnerd.ini

For Receiver::

    alignak-receiver -c /usr/local/etc/alignak/daemons/receiverd.ini


For Arbiter (be carefull, this daemon does not start like other)::

    alignak-arbiter -c /usr/local/etc/alignak/alignak.cfg



Log files
=========

When running, the Alignak daemons are logging their activity in log files that can be found in the
*/usr/local/var/log/* directory. Each daemon has its own log file. Log files are kept on the system
for a period of 6 rotating days.

In case of problem, make sure that there is no ERROR and/or WARNING logs in the log files.
