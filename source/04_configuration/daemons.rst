.. _configuration/daemons:

=======
Daemons
=======

Ini files
=========

To run a daemon, it must start with a configuration file (ini file).
This configuration file defines options like IP address, port, SSL...

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
