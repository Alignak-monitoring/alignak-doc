.. _configuration/core:

==================
Core configuration
==================

The core configuration part describes the Alignak framework infrastructure (which daemons are used and how they are). 

For historical reasons, the configuration is splitted in two kinds of files. The ini file contains the parameters used by the daemon to communicate with the other daemons in the Alignak framework where as the .cfg file contains parameters dedicated to its role in the whole stuff

In a feature paradise, one .cfg file per daemon should be enough to configure all settings of the concerned daemon. Currently each daemon has a .cfg and a .ini file for its configuration.

Cfg files
=========

Arbiter, main role
------------------

The arbiter is the main Alignak daemon. As of it, its configuration part is the most important one.

This part of the configuration is stored by default in the folder */usr/local/etc/alignak/arbiter/* and it contains:

    * the configuration of the daemons and their modules
    * the configuration of the monitored objects
    * the configuration of monitoring resources

Each sub-part is stored by default in its own sub-directory:

    * *arbiter/daemons/* for the daemons cfg files. Each daemon has its own cfg file.
    * *arbiter/modules/* for the modules configuration
    * *arbiter/templates/* for the objects templates
    * *arbiter/packs/* for the checks packs
    * *arbiter/objects/* for the objects. Described in  :ref:`the monitored objects dedicated chapter <monitoring_configuration/index>`.
    * *arbiter/resource.d/* for the resources and global macros

Arbiter configuration
---------------------

Definition Format
~~~~~~~~~~~~~~~~~

The configuration is defined in this style::

    define arbiter {
        name    value
        name    value
    }

==================================== ======= ======= ======== ============================================================
Variable name                        Type    default Required Short description
==================================== ======= ======= ======== ============================================================
arbiter_name                         string          yes      Name of arbiter
host_name                            string          yes      hostname
address                              string          yes      dns name or ip address
port                                 integer  7770   no       port of the daemon
spare                                boolean  0      no       set 1 if it's a spare
modules                              string          no       modules list separed by comma
use_ssl                              boolean  0      no       use ssl for communications
hard_ssl_name_check                  boolean  0      no       set 1 if require a valid certificate
timeout                              integer  3      no       number of seconds to block the arbiter waiting for an answer
data_timeout                         integer  120    no       seconds to wait when sending data to another daemon
max_check_attempts                   integer  3      no       number of time before node declared failed
check_interval                       integer  60     no       seconds to wait before issuing a new check
accept_passive_unknown_check_results integer  0      no       set 1 to allow passive check for unknown host
==================================== ======= ======= ======== ============================================================

Example Definition:
~~~~~~~~~~~~~~~~~~~

::

  define arbiter{
      arbiter_name       Main-arbiter
      address            node1.mydomain
      host_name          node1
      port               7770
      spare              0
      modules            module1,module2
  }


Variable Descriptions
~~~~~~~~~~~~~~~~~~~~~

arbiter_name
  This variable is used to identify the *short name* of the arbiter with which the data will be associated with.

host_name
  This variable is used by the arbiters daemons to define which 'arbiter' object they are: all theses daemons on different servers use the same configuration, so the only difference is their server name. This value must be equal to the name of the server (like with the hostname command). If none is defined, the arbiter daemon will put the name of the server where it's launched, but this will not be tolerated with more than one arbiter (because each daemons will think it's the master).

address
  This directive is used to define the address from where the main arbiter can reach this arbiter (that can be itself). This can be a DNS name or an IP address.

port
  This directive is used to define the TCP port used by the daemon. The default value is *7770*.

spare
  This variable is used to define if the daemon matching this arbiter definition is a spare one or not. The default value is *0* (non-spare).

modules
  This variable is used to define all the modules that the arbiter daemon matching this definition will load.

use_ssl
  This variable is used to allow inter-daemons communication in SSL mode (HTTPS)

hard_ssl_name_check
  This variable is used to force the checking of the SSL certificate

timeout
  This variable defines how much time the arbiter will wait for the response of an inter-process ping (default is 3 seconds). This operation is non blocking.

data_timeout
  Data send timeout. When sending data to another process. 120 seconds by default.

max_check_attempts
  If ping fails N or more, then the node is considered dead. 3 attempts by default.

check_interval
  Ping node every N seconds. 60 seconds by default.

accept_passive_unknown_check_results
  If this is enabled, the arbiter will accept passive check results for unconfigured hosts and will generate unknown host/service check result broks.


Scheduler configuration
-----------------------

Definition Format
~~~~~~~~~~~~~~~~~

The configuration is defined in this style::

    define scheduler {
        name    value
        name    value
    }

==================================== ======= ======= ======== =============================================================
Variable name                        Type    default Required Short description
==================================== ======= ======= ======== =============================================================
scheduler_name                       string          yes      Name of scheduler
address                              string          yes      dns name or ip address
port                                 integer  7768   no       port of the daemon
spare                                boolean  0      no       set 1 if it's a spare
weight                               integer  1      no       some schedulers can manage more hosts than other
timeout                              integer  3      no       number of seconds to block the arbiter waiting for an answer
data_timeout                         integer  120    no       seconds to wait when sending data to another daemon
max_check_attempts                   integer  3      no       number of time before node declared failed
check_interval                       integer  60     no       seconds to wait before issuing a new check
modules                              string          no       modules list separed by comma
realm                                string   All    no       it's for multi-datacenter
skip_initial_broks                   boolean  0      no       set to 1 to skip initial broks creation
satellitemap                         string          no       define other daemons separated by comma, format: name=ip:port
use_ssl                              boolean  0      no       use ssl for communications
hard_ssl_name_check                  boolean  0      no       set 1 if require a valid certificate
accept_passive_unknown_check_results integer  0      no       set 1 to allow passive check for unknown host
==================================== ======= ======= ======== =============================================================


Example Definition:
~~~~~~~~~~~~~~~~~~~

::

  define scheduler{
      scheduler_name         Europe-scheduler
      address                node1.mydomain
      port                   7770
      spare                  0
      realm                  Europe

      # Optional parameters
      spare                  0   ; 1 = is a spare, 0 = is not a spare
      weight                 1   ; Some schedulers can manage more hosts than others
      timeout                3   ; Ping timeout
      data_timeout           120 ; Data send timeout
      max_check_attempts     3   ; If ping fails N or more, then the node is dead
      check_interval         60  ; Ping node every minutes
      modules                PickleRetention

      # Skip initial broks creation for faster boot time. Experimental feature
      # which is not stable.
      skip_initial_broks    0

      # In NATted environments, you declare each satellite ip[:port] as seen by
      # *this* scheduler (if port not set, the port declared by satellite itself
      # is used)
      satellitemap          poller-1=1.2.3.4:1772, reactionner-1=1.2.3.5:1773, ...
  }


Variable Descriptions
~~~~~~~~~~~~~~~~~~~~~


== TODO UPDATE THIS PART ==

scheduler_name
  This variable is used to identify the *short name* of the scheduler which the data is associated with.

address
  This directive is used to define the address from where the main arbier can reach this scheduler. This can be a DNS name or a IP address.

port
  This directive is used to define the TCP port used bu the daemon. The default value is *7768*.

spare
  This variable is used to define if the scheduler must be managed as a spare one (will take the conf only if a master failed). The default value is *0* (master).

realm
  This variable is used to define the realm where the scheduler will be put. If none is selected, it will be assigned to the default one.

modules
  This variable is used to define all modules that the scheduler will load.

accept_passive_unknown_check_results
  If this is enabled, the scheduler will accept passive check results for unconfigured hosts and will generate unknown host/service check result broks.


Broker configuration
--------------------

Definition Format
~~~~~~~~~~~~~~~~~

The configuration is defined in this style::

    define broker {
        name    value
        name    value
    }

==================================== ======= ======= ======== =============================================================
Variable name                        Type    default Required Short description
==================================== ======= ======= ======== =============================================================
broker_name                          string          yes      Name of broker
address                              string          yes      dns name or ip address
port                                 integer  7772   no       port of the daemon
spare                                boolean  0      no       set 1 if it's a spare
manage_arbiters                      boolean  1      no       set 1 to take data from Arbiter
manage_sub_realms                    boolean  1      no       set 1 to take jobs from schedulers of sub-realms
timeout                              integer  3      no       number of seconds to block the arbiter waiting for an answer
data_timeout                         integer  120    no       seconds to wait when sending data to another daemon
max_check_attempts                   integer  3      no       number of time before node declared failed
check_interval                       integer  60     no       seconds to wait before issuing a new check
modules                              string          no       modules list separed by comma
use_ssl                              boolean  0      no       use ssl for communications
hard_ssl_name_check                  boolean  0      no       set 1 if require a valid certificate
realm                                string   All    no       it's for multi-datacenter
==================================== ======= ======= ======== =============================================================


Example Definition:
~~~~~~~~~~~~~~~~~~~


::

  define broker{
      broker_name        broker-1
      address            node1.mydomain
      port               7772
      spare              0
      realm              All
      ## Optional
      manage_arbiters     1
      manage_sub_realms   1
      timeout             3   ; Ping timeout
      data_timeout        120 ; Data send timeout
      max_check_attempts  3   ; If ping fails N or more, then the node is dead
      check_interval      60  ; Ping node every minutes  	       manage_sub_realms  1
      modules             livestatus,simple-log,webui
  }


Variable Descriptions
~~~~~~~~~~~~~~~~~~~~~

== TODO UPDATE THIS PART ==

broker_name
  This variable is used to identify the *short name* of the broker which the data is associated with.

address
  This directive is used to define the address from where the main arbier can reach this broker. This can be a DNS name or a IP address.

port
  This directive is used to define the TCP port used bu the daemon. The default value is *7772*.

spare
  This variable is used to define if the broker must be managed as a spare one (will take the conf only if a master failed). The default value is *0* (master).

realm
  This variable is used to define the realm where the broker will be put. If none is selected, it will be assigned to the default one.

manage_arbiters
  Take data from Arbiter. There should be only one broker for the arbiter.

manage_sub_realms
  This variable is used to define if the broker will take jobs from scheduler from the sub-realms of it's realm. The default value is *1*.

modules
  This variable is used to define all modules that the broker will load. The main goal of the Broker is to give status to theses modules.


Poller configuration
--------------------

Definition Format
~~~~~~~~~~~~~~~~~

The configuration is defined in this style::

    define poller {
        name    value
        name    value
    }

==================================== ======= ======= ======== =============================================================
Variable name                        Type    default Required Short description
==================================== ======= ======= ======== =============================================================
poller_name                          string          yes      Name of poller
address                              string          yes      dns name or ip address
port                                 integer  7771   no       port of the daemon
spare                                boolean  0      no       set 1 if it's a spare
manage_sub_realms                    boolean  0      no       set 1 to take jobs from schedulers of sub-realms
min_workers                          integer  0      no       starts with N processes (0 = 1 per CPU)
max_workers                          integer  0      no       no more than N processes (0 = 1 per CPU)
processes_by_worker                  integer  256    no       each worker manages N checks
polling_interval                     integer  1      no       get jobs from schedulers each N seconds
timeout                              integer  3      no       number of seconds to block the arbiter waiting for an answer
data_timeout                         integer  120    no       seconds to wait when sending data to another daemon
max_check_attempts                   integer  3      no       number of time before node declared failed
check_interval                       integer  60     no       seconds to wait before issuing a new check
modules                              string          no       modules list separed by comma
passive                              boolean  0      no       set 1 to inverse the connections, so scheduler -> poller
poller_tags                          string   None   no       tags separed by comma. Use None to manage untagged checks
use_ssl                              boolean  0      no       use ssl for communications
hard_ssl_name_check                  boolean  0      no       set 1 if require a valid certificate
realm                                string   All    no       it's for multi-datacenter
==================================== ======= ======= ======== =============================================================


Example Definition:
~~~~~~~~~~~~~~~~~~~


::

  define poller{
      poller_name          Europe-poller
      address              node1.mydomain
      port                 7771
      spare                0

      # Optional parameters
      manage_sub_realms    0
      poller_tags          DMZ, Another-DMZ
      modules              module1,module2
      realm                Europe
      min_workers          0    ; Starts with N processes (0 = 1 per CPU)
      max_workers          0    ; No more than N processes (0 = 1 per CPU)
      processes_by_worker  256  ; Each worker manages N checks
      polling_interval     1    ; Get jobs from schedulers each N seconds
  }


Variable Descriptions
~~~~~~~~~~~~~~~~~~~~~

== TODO UPDATE THIS PART ==

poller_name
  This variable is used to identify the *short name* of the poller which the data is associated with.

address
  This directive is used to define the address from where the main arbier can reach this poller. This can be a DNS name or a IP address.

port
  This directive is used to define the TCP port used bu the daemon. The default value is *7771*.

spare
  This variable is used to define if the poller must be managed as a spare one (will take the conf only if a master failed). The default value is *0* (master).

realm
  This variable is used to define the realm where the poller will be put. If none is selected, it will be assigned to the default one.

manage_sub_realms
  This variable is used to define if the poller will take jobs from scheduler from the sub-realms of it's realm. The default value is *0*.

poller_tags
  This variable is used to define the checks the poller can take. If no poller_tags is defined, poller will take all untagged checks. If at least one tag is defined, it will take only the checks that are also taggued like it.
  By default, there is no poller_tag, so poller can take all untagged checks (default).

modules
  This variable is used to define all modules that the scheduler will load.


Reactionner configuration
-------------------------

Definition Format
~~~~~~~~~~~~~~~~~

The configuration is defined in this style::

    define reactionner {
        name    value
        name    value
    }

==================================== ======= ======= ======== =============================================================
Variable name                        Type    default Required Short description
==================================== ======= ======= ======== =============================================================
reactionner_name                     string          yes      Name of reactionner
address                              string          yes      dns name or ip address
port                                 integer  7769   no       port of the daemon
spare                                boolean  0      no       set 1 if it's a spare
manage_sub_realms                    boolean  0      no       set 1 to take jobs from schedulers of sub-realms
min_workers                          integer  1      no       starts with N processes (0 = 1 per CPU)
max_workers                          integer  15     no       no more than N processes (0 = 1 per CPU)
polling_interval                     integer  1      no       get jobs from schedulers each N seconds
timeout                              integer  3      no       number of seconds to block the arbiter waiting for an answer
data_timeout                         integer  120    no       seconds to wait when sending data to another daemon
max_check_attempts                   integer  3      no       number of time before node declared failed
check_interval                       integer  60     no       seconds to wait before issuing a new check
modules                              string          no       modules list separed by comma
reactionner_tags                     string   None   no       tags separed by comma. Use None to manage untagged handlers
use_ssl                              boolean  0      no       use ssl for communications
hard_ssl_name_check                  boolean  0      no       set 1 if require a valid certificate
realm                                string   All    no       it's for multi-datacenter
==================================== ======= ======= ======== =============================================================


Example Definition:
~~~~~~~~~~~~~~~~~~~

::

  define reactionner{
      reactionner_name      Main-reactionner
      address               node1.mydomain
      port                  7769
      spare                 0
      realm                 All

      # Optional parameters
      manage_sub_realms     0   ; Does it take jobs from schedulers of sub-Realms?
      min_workers           1   ; Starts with N processes (0 = 1 per CPU)
      max_workers           15  ; No more than N processes (0 = 1 per CPU)
      polling_interval      1   ; Get jobs from schedulers each 1 second
      timeout               3   ; Ping timeout
      data_timeout          120 ; Data send timeout
      max_check_attempts    3   ; If ping fails N or more, then the node is dead
      check_interval        60  ; Ping node every minutes
      reactionner_tags      tag1
      modules               module1,module2
  }


Variable Descriptions
~~~~~~~~~~~~~~~~~~~~~

== TODO UPDATE THIS PART ==

reactionner_name
  This variable is used to identify the *short name* of the reactionner which the data is associated with.

address
  This directive is used to define the address from where the main arbier can reach this reactionner. This can be a DNS name or a IP address.

port
  This directive is used to define the TCP port used bu the daemon. The default value is *7772*.

spare
  This variable is used to define if the reactionner must be managed as a spare one (will take the conf only if a master failed). The default value is *0* (master).

realm
  This variable is used to define the realm where the reactionner will be put. If none is selected, it will be assigned to the default one.

manage_sub_realms
  This variable is used to define if the poller will take jobs from scheduler from the sub-realms of it's realm. The default value is *1*.

modules
  This variable is used to define all modules that the reactionner will load.

reactionner_tags
  This variable is used to define the checks the reactionner can take. If no reactionner_tags is defined, reactionner  will take all untagged notifications and event handlers. If at least one tag is defined, it will take only the checks that are also taggued like it.

By default, there is no reactionner_tag, so reactionner can take all untagged notification/event handlers (default).

Reaceiver configuration
-----------------------

Definition Format
~~~~~~~~~~~~~~~~~

The configuration is defined in this style::

    define receiver {
        name    value
        name    value
    }

==================================== ======= ======= ======== =============================================================
Variable name                        Type    default Required Short description
==================================== ======= ======= ======== =============================================================
receiver_name                        string          yes      Name of receiver
address                              string          yes      dns name or ip address
port                                 integer  7773   no       port of the daemon
spare                                boolean  0      no       set 1 if it's a spare
timeout                              integer  3      no       number of seconds to block the arbiter waiting for an answer
data_timeout                         integer  120    no       seconds to wait when sending data to another daemon
max_check_attempts                   integer  3      no       number of time before node declared failed
check_interval                       integer  60     no       seconds to wait before issuing a new check
modules                              string          no       modules list separed by comma
use_ssl                              boolean  0      no       use ssl for communications
hard_ssl_name_check                  boolean  0      no       set 1 if require a valid certificate
direct_routing                       boolean  0      no       set 1 to allow scheduler to send command instead me
realm                                string   All    no       it's for multi-datacenter
==================================== ======= ======= ======== =============================================================

Example Definition:
~~~~~~~~~~~~~~~~~~~

== TODO UPDATE THIS PART ==


Variable Descriptions
~~~~~~~~~~~~~~~~~~~~~

== TODO UPDATE THIS PART ==

Ini files
=========

To run a daemon, it must start with a ini file.
This ini file defines options like IP address, port, SSL...

The format is the same for all the daemons and is defined in this style::

    [daemon]
    variable=value
    variable=value

When Alignak is installed, some variables in those configuration files are modified to match at 
best with your system. The changed variables are:

    - workdir
    - logdir
    - pidfile
    - user
    - group
    - ca_cert
    - server_cert
    - server_key

Their values are set according to the current plaform (Linux, BSD, ...) and configuration defined
in the default alignak configuration.

Arbiter
-------

==================================== ======= ============================== ============================================================
Variable name                        Type    default                        Short description
==================================== ======= ============================== ============================================================
workdir                              string  /var/run/alignak               daemon will chdir into this directory when launched
logdir                               string  /var/log/alignak               daemon will write logs into this directory
etcdir                               string  /etc/alignak                   daemon will get configuration into this directory
pidfile                              string  /var/run/schedulerd.pid        PID file of the daemon
port                                 integer 7768                           port used by the daemon
host                                 string  0.0.0.0                        Set IP daemon listen
user                                 string  alignak                        username to run daemon
group                                string  alignak                        group to run daemon
idontcareaboutsecurity               boolean 0                              set 1 to disable security in daemon (not recommended)
modulesdir                           string  modules                        define modules directory installation
daemon_enabled                       boolean 1                              set to 0 if want this daemon not run
use_ssl                              boolean 0                              set 1 to use ssl for communications
ca_cert                              string  /etc/alignak/certs/ca.pem      full path for certificate
server_cert                          string  /etc/alignak/certs/server.cert full path for certificate
server_key                           string  /etc/alignak/certs/server.key  full path for certificate key
hard_ssl_name_check                  boolean 0                              set 1 if require a valid certificate
use_local_log                        boolean 1                              set to 1 to ease troubleshooting
local_log                            string  /var/log/schedulerd.log        file to write logs
log_level                            string  WARNING                        log level in DEBUG,INFO,WARNING,ERROR,CRITICAL
==================================== ======= ============================== ============================================================

Scheduler
---------

==================================== ======= ============================== ============================================================
Variable name                        Type    default                        Short description
==================================== ======= ============================== ============================================================
workdir                              string  /var/run/alignak               daemon will chdir into this directory when launched
logdir                               string  /var/log/alignak               daemon will write logs into this directory
pidfile                              string  /var/run/schedulerd.pid        PID file of the daemon
port                                 integer 7768                           port used by the daemon
host                                 string  0.0.0.0                        Set IP daemon listen
user                                 string  alignak                        username to run daemon
group                                string  alignak                        group to run daemon
idontcareaboutsecurity               boolean 0                              set 1 to disable security in daemon (not recommended)
modulesdir                           string  modules                        define modules directory installation
daemon_enabled                       boolean 1                              set to 0 if want this daemon not run
use_ssl                              boolean 0                              set 1 to use ssl for communications
ca_cert                              string  /etc/alignak/certs/ca.pem      full path for certificate
server_cert                          string  /etc/alignak/certs/server.cert full path for certificate
server_key                           string  /etc/alignak/certs/server.key  full path for certificate key
hard_ssl_name_check                  boolean 0                              set 1 if require a valid certificate
use_local_log                        boolean 1                              set to 1 to ease troubleshooting
local_log                            string  /var/log/schedulerd.log        file to write logs
log_level                            string  WARNING                        log level in DEBUG,INFO,WARNING,ERROR,CRITICAL
==================================== ======= ============================== ============================================================

Poller
------

==================================== ======= ============================== ============================================================
Variable name                        Type    default                        Short description
==================================== ======= ============================== ============================================================
user                                 string  alignak                        username to run daemon
group                                string  alignak                        group to run daemon
daemon_enabled                       boolean 1                              set to 0 if want this daemon not run
workdir                              string  /var/run/alignak               daemon will chdir into this directory when launched
logdir                               string  /var/log/alignak               daemon will write logs into this directory
pidfile                              string  /var/run/pollerd.pid           PID file of the daemon
host                                 string  0.0.0.0                        Set IP daemon listen
port                                 integer 7771                           port used by the daemon
idontcareaboutsecurity               boolean 0                              set 1 to disable security in daemon (not recommended)
use_ssl                              boolean 0                              set 1 to use ssl for communications
ca_cert                              string  /etc/alignak/certs/ca.pem      full path for certificate
server_cert                          string  /etc/alignak/certs/server.cert full path for certificate
server_key                           string  /etc/alignak/certs/server.key  full path for certificate key
hard_ssl_name_check                  boolean 0                              set 1 if require a valid certificate
use_local_log                        boolean 1                              set to 1 to ease troubleshooting
local_log                            string  /var/log/pollerd.log           file to write logs
log_level                            string  WARNING                        log level in DEBUG,INFO,WARNING,ERROR,CRITICAL
==================================== ======= ============================== ============================================================

Receiver
--------

==================================== ======= ============================== ============================================================
Variable name                        Type    default                        Short description
==================================== ======= ============================== ============================================================
workdir                              string  /var/run/alignak               daemon will chdir into this directory when launched
logdir                               string  /var/log/alignak               daemon will write logs into this directory
pidfile                              string  /var/run/receiverd.pid         PID file of the daemon
port                                 integer 7773                           port used by the daemon
host                                 string  0.0.0.0                        Set IP daemon listen
user                                 string  alignak                        username to run daemon
group                                string  alignak                        group to run daemon
idontcareaboutsecurity               boolean 0                              set 1 to disable security in daemon (not recommended)
daemon_enabled                       boolean 1                              set to 0 if want this daemon not run
use_ssl                              boolean 0                              set 1 to use ssl for communications
ca_cert                              string  /etc/alignak/certs/ca.pem      full path for certificate
server_cert                          string  /etc/alignak/certs/server.cert full path for certificate
server_key                           string  /etc/alignak/certs/server.key  full path for certificate key
hard_ssl_name_check                  boolean 0                              set 1 if require a valid certificate
use_local_log                        boolean 1                              set to 1 to ease troubleshooting
local_log                            string  /var/log/receiverd.log         file to write logs
log_level                            string  WARNING                        log level in DEBUG,INFO,WARNING,ERROR,CRITICAL
==================================== ======= ============================== ============================================================

Broker
------

==================================== ======= ============================== ============================================================
Variable name                        Type    default                        Short description
==================================== ======= ============================== ============================================================
workdir                              string  /var/run/alignak               daemon will chdir into this directory when launched
logdir                               string  /var/log/alignak               daemon will write logs into this directory
pidfile                              string  /var/run/brokerd.pid           PID file of the daemon
user                                 string  alignak                        username to run daemon
group                                string  alignak                        group to run daemon
host                                 string  0.0.0.0                        Set IP daemon listen
port                                 integer 7772                           port used by the daemon
idontcareaboutsecurity               boolean 0                              set 1 to disable security in daemon (not recommended)
daemon_enabled                       boolean 1                              set to 0 if want this daemon not run
use_ssl                              boolean 0                              set 1 to use ssl for communications
ca_cert                              string  /etc/alignak/certs/ca.pem      full path for certificate
server_cert                          string  /etc/alignak/certs/server.cert full path for certificate
server_key                           string  /etc/alignak/certs/server.key  full path for certificate key
hard_ssl_name_check                  boolean 0                              set 1 if require a valid certificate
use_local_log                        boolean 1                              set to 1 to ease troubleshooting
local_log                            string  /var/log/brokerd.log           file to write logs
log_level                            string  WARNING                        log level in DEBUG,INFO,WARNING,ERROR,CRITICAL
max_queue_size                       integer 100000                         restart an external module if queue to high. 0 to disable
==================================== ======= ============================== ============================================================

Reactionner
-----------

==================================== ======= ============================== ============================================================
Variable name                        Type    default                        Short description
==================================== ======= ============================== ============================================================
workdir                              string  /var/run/alignak               daemon will chdir into this directory when launched
logdir                               string  /var/log/alignak               daemon will write logs into this directory
pidfile                              string  /var/run/reactionnerd.pid      PID file of the daemon
port                                 integer 7769                           port used by the daemon
host                                 string  0.0.0.0                        Set IP daemon listen
user                                 string  alignak                        username to run daemon
group                                string  alignak                        group to run daemon
idontcareaboutsecurity               boolean 0                              set 1 to disable security in daemon (not recommended)
daemon_enabled                       boolean 1                              set to 0 if want this daemon not run
use_ssl                              boolean 0                              set 1 to use ssl for communications
ca_cert                              string  /etc/alignak/certs/ca.pem      full path for certificate
server_cert                          string  /etc/alignak/certs/server.cert full path for certificate
server_key                           string  /etc/alignak/certs/server.key  full path for certificate key
hard_ssl_name_check                  boolean 0                              set 1 if require a valid certificate
use_local_log                        boolean 1                              set to 1 to ease troubleshooting
local_log                            string  /var/log/reactionnerd.log      file to write logs
log_level                            string  WARNING                        log level in DEBUG,INFO,WARNING,ERROR,CRITICAL
==================================== ======= ============================== ============================================================
