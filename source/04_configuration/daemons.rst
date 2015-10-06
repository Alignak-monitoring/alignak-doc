
=======
Daemons
=======

Ini files
=========

When run a daemon, it start with an ini file.
This ini file define options like IP address, port, SSL...

The format is same for all and defined in this style::

    [daemon]
        name = value
        name = value
    }


Scheduler
---------

==================================== ======= ============================== ============================================================
Variable name                        Type    default                        Short description
==================================== ======= ============================== ============================================================
workdir                              string  /var/run/alignak               daemon will chdir into this directory when launched
logdir                               string  /var/log/alignak               daemon will write logs into this directory
pidfile                              string  %(workdir)s/schedulerd.pid     PID file of the daemon
port                                 integer 7768                           port used by the daemon
host                                 string  0.0.0.0                        Set IP daemon listen
user                                 string  alignak                        username to run daemon
group                                string  alignak                        group to run daemon
idontcareaboutsecurity               boolean 0                              set 1 to disable security in daemon (not recommanded)
modulesdir                           string  modules                        define modules directory installation
daemon_enabled                       boolean 1                              set to 0 if want this daemon not run
use_ssl                              boolean 0                              set 1 to use ssl for communications
ca_cert                              string  /etc/alignak/certs/ca.pem      full path for certificate
server_cert                          string  /etc/alignak/certs/server.cert full path for certificate
server_key                           string  /etc/alignak/certs/server.key  full path for certificate key
hard_ssl_name_check                  boolean 0                              set 1 if require a valid certificate
use_local_log                        boolean 1                              set to 1 to ease troubleshooting
local_log                            string  %(logdir)s/schedulerd.log      file to write logs
log_level                            string  WARNING                        log level in DEBUG,INFO,WARNING,ERROR,CRITICAL
modules_dir                          string  /var/lib/alignak/modules       folder where find module code (python files)
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
pidfile                              string  %(workdir)s/pollerd.pid        PID file of the daemon
host                                 string  0.0.0.0                        Set IP daemon listen
port                                 integer 7771                           port used by the daemon
idontcareaboutsecurity               boolean 0                              set 1 to disable security in daemon (not recommanded)
use_ssl                              boolean 0                              set 1 to use ssl for communications
ca_cert                              string  /etc/alignak/certs/ca.pem      full path for certificate
server_cert                          string  /etc/alignak/certs/server.cert full path for certificate
server_key                           string  /etc/alignak/certs/server.key  full path for certificate key
hard_ssl_name_check                  boolean 0                              set 1 if require a valid certificate
use_local_log                        boolean 1                              set to 1 to ease troubleshooting
local_log                            string  %(logdir)s/pollerd.log         file to write logs
log_level                            string  WARNING                        log level in DEBUG,INFO,WARNING,ERROR,CRITICAL
modules_dir                          string  /var/lib/alignak/modules       folder where find module code (python files)
==================================== ======= ============================== ============================================================

Receiver
--------

==================================== ======= ============================== ============================================================
Variable name                        Type    default                        Short description
==================================== ======= ============================== ============================================================
workdir                              string  /var/run/alignak               daemon will chdir into this directory when launched
logdir                               string  /var/log/alignak               daemon will write logs into this directory
pidfile                              string  %(workdir)s/receiverd.pid      PID file of the daemon
port                                 integer 7773                           port used by the daemon
host                                 string  0.0.0.0                        Set IP daemon listen
user                                 string  alignak                        username to run daemon
group                                string  alignak                        group to run daemon
idontcareaboutsecurity               boolean 0                              set 1 to disable security in daemon (not recommanded)
daemon_enabled                       boolean 1                              set to 0 if want this daemon not run
use_ssl                              boolean 0                              set 1 to use ssl for communications
ca_cert                              string  /etc/alignak/certs/ca.pem      full path for certificate
server_cert                          string  /etc/alignak/certs/server.cert full path for certificate
server_key                           string  /etc/alignak/certs/server.key  full path for certificate key
hard_ssl_name_check                  boolean 0                              set 1 if require a valid certificate
use_local_log                        boolean 1                              set to 1 to ease troubleshooting
local_log                            string  %(logdir)s/receiverd.log       file to write logs
log_level                            string  WARNING                        log level in DEBUG,INFO,WARNING,ERROR,CRITICAL
modules_dir                          string  /var/lib/alignak/modules       folder where find module code (python files)
==================================== ======= ============================== ============================================================

Broker
------

==================================== ======= ============================== ============================================================
Variable name                        Type    default                        Short description
==================================== ======= ============================== ============================================================
workdir                              string  /var/run/alignak               daemon will chdir into this directory when launched
logdir                               string  /var/log/alignak               daemon will write logs into this directory
pidfile                              string  %(workdir)s/brokerd.pid        PID file of the daemon
user                                 string  alignak                        username to run daemon
group                                string  alignak                        group to run daemon
host                                 string  0.0.0.0                        Set IP daemon listen
port                                 integer 7772                           port used by the daemon
idontcareaboutsecurity               boolean 0                              set 1 to disable security in daemon (not recommanded)
daemon_enabled                       boolean 1                              set to 0 if want this daemon not run
use_ssl                              boolean 0                              set 1 to use ssl for communications
ca_cert                              string  /etc/alignak/certs/ca.pem      full path for certificate
server_cert                          string  /etc/alignak/certs/server.cert full path for certificate
server_key                           string  /etc/alignak/certs/server.key  full path for certificate key
hard_ssl_name_check                  boolean 0                              set 1 if require a valid certificate
use_local_log                        boolean 1                              set to 1 to ease troubleshooting
local_log                            string  %(logdir)s/brokerd.log         file to write logs
log_level                            string  WARNING                        log level in DEBUG,INFO,WARNING,ERROR,CRITICAL
max_queue_size                       integer 100000                         restart an external module if queue to high. 0 to disable
modules_dir                          string  /var/lib/alignak/modules       folder where find module code (python files)
==================================== ======= ============================== ============================================================

Reactionner
-----------

==================================== ======= ============================== ============================================================
Variable name                        Type    default                        Short description
==================================== ======= ============================== ============================================================
workdir                              string  /var/run/alignak               daemon will chdir into this directory when launched
logdir                               string  /var/log/alignak               daemon will write logs into this directory
pidfile                              string  %(workdir)s/reactionnerd.pid   PID file of the daemon
port                                 integer 7769                           port used by the daemon
host                                 string  0.0.0.0                        Set IP daemon listen
user                                 string  alignak                        username to run daemon
group                                string  alignak                        group to run daemon
idontcareaboutsecurity               boolean 0                              set 1 to disable security in daemon (not recommanded)
daemon_enabled                       boolean 1                              set to 0 if want this daemon not run
use_ssl                              boolean 0                              set 1 to use ssl for communications
ca_cert                              string  /etc/alignak/certs/ca.pem      full path for certificate
server_cert                          string  /etc/alignak/certs/server.cert full path for certificate
server_key                           string  /etc/alignak/certs/server.key  full path for certificate key
hard_ssl_name_check                  boolean 0                              set 1 if require a valid certificate
use_local_log                        boolean 1                              set to 1 to ease troubleshooting
local_log                            string  %(logdir)s/reactionnerd.log    file to write logs
log_level                            string  WARNING                        log level in DEBUG,INFO,WARNING,ERROR,CRITICAL
modules_dir                          string  /var/lib/alignak/modules       folder where find module code (python files)
==================================== ======= ============================== ============================================================
