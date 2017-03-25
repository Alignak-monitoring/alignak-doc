.. _alignak_features/cached_checks:

=============
Cached Checks
=============


Introduction
------------

.. image:: /_static/images///official/images/cachedchecks1.png
   :scale: 90 %

The performance of Alignak's monitoring logic can be significantly improved by implementing the use of
cached checks. Cached checks allow Alignak to forget executing a host or service check command if it determines
a relatively recent check result will do instead.


For on-demand checks only
-------------------------

Regularly scheduled host and service checks will not see a performance improvement with use of cached checks. Cached checks are only useful for improving the performance of on-demand host and service checks. Scheduled checks help to ensure that host and service states are updated regularly, which may result in a greater possibility their results can be used as cached checks in the future.

For reference, on-demand host checks occur...

   * When a service associated with the host changes state.
   * As needed as part of the :ref:`host reachability <monitoring_features/network_reachability>` logic.
   * As needed for :ref:`host dependency checks <monitoring_features/dependencies>`.

And on-demand service checks occur...

  * As needed for :ref:`service dependency checks <monitoring_features/dependencies>`.

Unless you make use of service dependencies, Alignak will not be able to use cached check results to improve the performance of service checks. Don't worry about it - that's normal.


How caching works
-----------------

.. image:: /_static/images///official/images/cachedchecks.png
   :scale: 90 %


When Alignak needs to perform an on-demand host or service check, it will check whether it can use a cached check result or if it needs to perform a real check by executing a plugin. The implemented logic is to check if the last check for the host or service occurred within the last X seconds, where X is the cached host or service check horizon.

If the last check was performed within the timeframe specified by the cached check horizon variable, Alignak will use the result of the last host or service check and will not execute a new check. If the host or service has not yet been checked, or if the last check falls outside of the cached check horizon timeframe, Alignak will execute a new host or service check by running a plugin.


What it really means
--------------------

Alignak performs on-demand checks because it needs to know the current state of a host or service at that exact moment in time. Using cached checks allows you to make Alignak think that recent check results are “good enough" for knowing the current state of hosts, and that it doesn't need to go out and actually re-check the status of that host or service.

The cached check horizon tells Alignak how recent check results must be in order to reliably reflect the current state of a host or service. For example, with a cached check horizon of 30 seconds, you are telling Alignak that if a host's state was checked sometime in the last 30 seconds, the result of that check should still be considered as the current state of the host.

The number of cached check results that Alignak can use versus the number of on-demand checks it has to actually execute can be considered the cached check “hit" rate. By increasing the cached check horizon to equal the regular check interval of a host, you could theoretically achieve a cache hit rate of 100%. In that case all on-demand checks of that host would use cached check results. What a performance improvement! But is it really? Probably not.

The reliability of cached check result information decreases over time. Higher cache hit rates require that previous check results are considered “valid" for longer periods of time. Things can change quickly in any network scenario, and there's no guarantee that a server that was functioning properly 30 seconds ago isn't on fire right now. There's the tradeoff - reliability versus speed. If you have a large cached check horizon, you risk having unreliable check result values being used in the monitoring logic.

Alignak will eventually determine the correct state of all hosts and services, so even if cached check results prove to unreliably represent their true value, it will only work with incorrect information for a short period of time. Even short periods of unreliable status information can prove to be noisy for admins, as they may receive notifications about problems which no longer exist.

There is no standard cached check horizon or cache hit rate that will be acceptable to every users. Some people will want a short horizon timeframe and a low cache hit rate, while others will want a larger horizon timeframe and a larger cache hit rate (with a low reliability rate).

Some users may even want to disable cached checks altogether to obtain a 100% reliability rate. Testing different horizon timeframes, and their effect on the reliability of status information, is the only want that an individual user will find the “right" value for their situation. More information on this is discussed below.

.. note:: this feature is still in development and not yet fully implemented.

Configuration variables
-----------------------

The following variables determine the time frames in which a previous host or service check result may be used as a cached host or service check result:

  * The ``cached_host_check_horizon`` variable controls cached host checks.
  * The ``cached_service_check_horizon`` variable controls cached service checks.


