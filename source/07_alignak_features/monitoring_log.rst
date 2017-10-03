.. _alignak_features/monitoring_log:

==============
Monitoring log
==============


Introduction
------------

The monitoring log is what Alignak is made for!.


This log contains all the monitoring events that Alignak is able to raise:

    * active host/service checks
    * passive host/service checks
    * alerts
    * notifications
    * acknowledgements
    * downtimes

As soon as one of this event is raised by Alignak, it is sent to the monitoring log. Thanks to the Alignak configuration (see the :ref:`logger configuration <configuration/logger>`), the raised events may be logged to a file, sent by email, pushed to a Web service,...