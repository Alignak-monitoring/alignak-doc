.. _configuration/ssl_certificate:

===============
SSL certificate
===============


Why using SSL certificate
-------------------------

By default the Alignak inter-daemons communication uses the HTTP protocol.

This documentation part explain how to configure the Alignak daemons to use an encrypted SSL (HTTPS) communication.


Official certificate
--------------------

You can buy a SSL certificate from an official certificate authority.



Self-signed certificate
-----------------------

You can generate your own self-signed certificate. The commands are::

    openssl dhparam -out dhparams.pem 2048
    openssl genrsa -passout pass:the_password_you_want -out certificate_test.key 2048
    openssl req -new -x509 -days 3650 -key certificate_test.key -out certificate_test.csr

When generating the `certificate_test.csr` (last command), if your run Alignak locally, you can use *Common Name* and the *localhost* value for the server name, otherwise enter the server fully qualified domain name where the daemon is running.


Daemons configuration
---------------------

In the daemons ini configuration files (*etc/alignak/daemons/*) define the full path of your certificate files and uncomment::

    server_cert=%(etcdir)s/certs/certificate_test.csr
    server_key=%(etcdir)s/certs/certificate_test.key
    server_dh=%(etcdir)s/certs/dhparams.pem

If you are using a certificate from an official certification authority, you must also define the intermediate certificate of the authority and uncomment::

    ca_cert=%(etcdir)s/certs/ca.pem


At last, enable SSL in the daemons configuration (it's the client in satellite/daemons to communicate with other daemons).
So in etc/alignak/arbiter/daemons/*.cfg define::

    use_ssl 1

For the highest security, uncomment and activate::

    hard_ssl_name_check 1


