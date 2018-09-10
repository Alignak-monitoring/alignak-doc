.. _annexes/plugin_api:

=========================
Alignak check plugins API
=========================

Alignak aims at maintaining compatibilty with Nagios. As such, all Nagios available plugins are fully compatible and can be used with Alignak.


Some resources
==============

If you're looking at writing your own plugins for Alignak or Nagios, please make sure to visit:

  * The official `Monitoring plugins project website`_
  * The official `Monitoring plugins development guidelines`_


Plugin Overview
================

Scripts and executables must do two things (at minimum) in order to function as Alignak plugins:

   * Exit with one of several possible return values
   * Print at least one line of text output to the standard stdout

Alignak do not matter about the inner workings of a plugin. Only the interface between Alignak and the plugin is of any concern.

If you are interested in having a plugin that is very performant to use with Alignak, you should consider making it an Alignak Worker or Module and it will be daemonized by the Alignak poller or receiver daemon. You can look at the existing modules...


Return code
===========

Alignak determines the status of an host or service by evaluating the return code from its check plugin.
The following table shows a list of valid return codes, along with their corresponding service or host states.

================== ============= =======================
Plugin Return Code Service State Host State
0                  OK            UP
1                  WARNING       DOWN/UNREACHABLE*
2                  CRITICAL      DOWN/UNREACHABLE
3                  UNKNOWN       DOWN/UNREACHABLE
================== ============= =======================

The process by which Alignak determines whether or not a host is DOWN or UNREACHABLE is discussed :ref:`here <monitoring_features/network_reachability>`.


Plugin output
=============

At minimum, plugins should return at least one of text output. Plugins can optionally return multiple lines of output. Plugins may also return optional performance data that can be processed by external applications. The basic format for plugin output is shown below::

  TEXT OUTPUT | OPTIONAL PERFDATA
  LONG TEXT LINE 1
  LONG TEXT LINE 2
  ...
  LONG TEXT LINE N | PERFDATA LINE 2
  PERFDATA LINE 3
  ...
  PERFDATA LINE N

The performance data are optional.

If a plugin returns performance data in its output, it must separate the performance data from the other text output using a pipe (|) symbol.

Additional lines of long text output are also optional.


Some examples
-------------

Some examples of possible plugin output...

**Case 1: One line of output (text only)**

Assume we have a plugin that returns one line of output that looks like this::

  DISK OK - free space: / 3326 MB (56%);

If this plugin was used to perform a service check, the entire line of output will be stored in the :ref:`$SERVICEOUTPUT$ <$SERVICEOUTPUT$>` macro.

**Case 2: One line of output (text and perfdata)**

A plugin can return optional performance data for use by external applications. To do this, the performance data must be separated from the text output with a pipe (|) symbol like such as::

  DISK OK - free space: / 3326 MB (56%)|/=2643MB;5948;5958;0;5968

If this plugin was used to perform a service check, the left part of the output (before the pipe separator) will be stored in the :ref:`$SERVICEOUTPUT$ <$SERVICEOUTPUT$>` macro and the right part of the output (after the pipe separator) will be stored in the :ref:`$SERVICEPERFDATA$ <thebasics/macrolist#serviceperfdata>` macro.

**Case 3: Multiple lines of output (text and perfdata)**

A plugin may optionally return multiple lines of both text output and perfdata, like such as::

  DISK OK - free space: / 3326 MB (56%);|/=2643MB;5948;5958;0;5968
  / 15272 MB (77%);
  /boot 68 MB (69%);
  /home 69357 MB (27%);
  /var/log 819 MB (84%);|/boot=68MB;88;93;0;98
  /home=69357MB;253404;253409;0;253414
  /var/log=818MB;970;975;0;980

If this plugin was used to perform a service check, the left part of the first line of output (before the pipe separator) will be stored in the :ref:`$SERVICEOUTPUT$ <$SERVICEOUTPUT$>` macro. The right part of the first line and the subsequent lines are concatenated (with spaces) and stored in the :ref:`$SERVICEPERFDATA$ <thebasics/macrolist#serviceperfdata>` macro. The blue portions of the 2nd _ 5th lines of output will be concatenated (with escaped newlines) and stored in :ref:`$LONGSERVICEOUTPUT$ <thebasics/macrolist#longserviceoutput>` the macro.

The final content of each macro is listed below:

=================== =================================================================================================================
Macro               Value
$SERVICEOUTPUT$     DISK OK - free space: / 3326 MB (56%);
$SERVICEPERFDATA$   /=2643MB;5948;5958;0;5968"/boot=68MB;88;93;0;98"/home=69357MB;253404;253409;0;253414"/var/log=818MB;970;975;0;980
$LONGSERVICEOUTPUT$ / 15272 MB (77%);\n/boot 68 MB (69%);\n/var/log 819 MB (84%);
=================== =================================================================================================================

With regards to multiple lines of output, you have the following options for returning performance data:

  * You can choose to return no performance data whatsoever
  * You can return performance data on the first line only
  * You can return performance data only in subsequent lines (after the first)
  * You can return performance data in both the first line and subsequent lines (as shown above)


Plugin output length restrictions
=================================

Alignak will only read the first 8 KB of data that a plugin returns. This is done in order to prevent runaway plugins from dumping megs or gigs of data back to Alignak. This 8 KB output limit is easy to change if you need. Simply edit the value of the ``max_plugins_output_length`` variable in the configuration file.
