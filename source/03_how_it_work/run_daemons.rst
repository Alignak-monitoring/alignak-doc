.. _howitworks/run_daemons:

==================
How to run daemons
==================

With packaging
==============

If you install with packaging (DEB, RPM...), see on it how to run.


With sources and pip
====================

You must start each daemons manually.

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


For Arbiter (be carefull, this daemon not start like other)::

    alignak-arbiter -c /usr/local/etc/alignak/alignak.cfg



