.. _configuration/ssl_certificate:

===============
SSL certificate
===============


Why use SSL certificate
-----------------------

By default the communication between daemons (arbiter, scheduler...) is in HTTP.

This documentation part explain how to configure to have this communication in HTTPS


Official certificate
--------------------

You can buy a SSL certificate from an official certificate authority.



Self-signed certificate
-----------------------

You can generate yourself your certificate, the commands are::

    openssl dhparam -out dhparams.pem 2048
    openssl genrsa -passout pass:the_password_you_want -out certificate_test.key 2048
    openssl req -new -x509 -days 3650 -key certificate_test.key -out certificate_test.csr

When generate the certificate_test.csr (last command), if your Alignak run inly in local, you can put in *Common Name*
the value: *localhost*, otherwise put the server name where the daemon is running.


Configuration in daemons
------------------------

In *.ini files in etc/alignak/daemons/ define the complete path of your certificates::

    server_cert=/usr/local/etc/alignak/certs/certificate_test.csr
    server_key=/usr/local/etc/alignak/certs/certificate_test.key
    server_dh=/usr/local/etc/alignak/certs/dhparams.pem

In case you use a certificate from an official certificate authority, define too the intermediate certificate of the authority::

    ca_cert=/usr/local/etc/alignak/certs/ca.pem


At the end, enable SSL in daemons configuration (it's the client in satellite/daemons to communicate with other daemons).
So in etc/alignak/arbiter/daemons/*.cfg define::

    use_ssl 1

For more security, active::

    hard_ssl_name_check 1


