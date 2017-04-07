.. _monitoring_features/snapshot:

=========
Snapshots
=========


Overview
========

Snapshots are on demand command launched during a specific time period and only when an host/service is in a specific state. This is useful, as an example, to export a quick-view/snapshot of an element when a problem happen (like a list of processes) ...


Snapshots are commands attached to hosts and/or services. It's not a good idea to enable them on all hosts and services as it will add a lot of load on your system :/


Snapshots are like event handlers except that they are launched periodically (``snapshot_interval``) since the targeted element is in one of the specified states (``snapshot_criteria``) during a specified time perio (``snapshot_period``).

They are launched from the reactionner daemon with the same properties (``reactionner_tag``) than the event handlers. The scheduler is grabbing the output and it will create a specific brok object (``host_snapshot_brok`` or ``service_snapshot_brok``) so that the brokers can be informed and notify specific modules.

