.. _configuration/ssl_certificate:

==============================
SSL inter-daemon communication
==============================


Why using SSL communication?
----------------------------

By default the Alignak inter-daemons communication uses the HTTP protocol.

This documentation part explain how to configure the Alignak daemons to use an encrypted SSL (HTTPS) communication.


Official certificate
--------------------

You can buy a SSL certificate from an official certificate authority.



Self-signed certificate
-----------------------

You can generate your own self-signed certificate. The commands are::

   $ openssl dhparam -out dhparams.pem 2048
   $ cat dhparams.pem
      -----BEGIN DH PARAMETERS-----
      MIIBCAKCAQEArKTgemTGBjUAyHM3piDjNhBU4KGJ9JVS8n7+n8+vMdN/2XMOjvWS
      lrfpUJ51gcYw17JwfiwtKqQFvCOYw+XAo2jhfrMixuke1ggCQbnuMiDQfRZROj6T
      gelAzbDJ0LCHOEWl8gs16mj36KJeNSRqyy3V916SzboiiLCst+QGSR0RCekrq8no
      74/uLRSQfivSnCl33hs4LXpmZG+YvMQsH9ylqVt86NADPWTEVEl4gXQFnEHuC3Jd
      1Dm/bbUxGm4pCod1h76Ljy3j72osWHDOpARMV85QFLrXhmUvegt7fZxSA/TVETbT
      7FFXNaCKtGZ5wEfT49716NlNqLodwCRvSwIBAg==
      -----END DH PARAMETERS-----

   $ openssl genrsa -passout pass:the_password_you_want -out certificate_test.key 2048
   $ cat certificate_test.key
      -----BEGIN RSA PRIVATE KEY-----
      MIIEpAIBAAKCAQEA4e3i+ZetHrvSGmp/Jr3YefAHOnSoB6iWAkRbH3yLcxxM6DDP
      7uA8LzPMkiye6CTipVSL1huBUZMrz06fwOavCAQCZJjMPR7dOQJshxhIkU/oU5ZM
      UzZFw+njy/R0JgJjL77qO/TZWF9qkInXt71GokJHeFjfmWSTJXamjZjB5ypV9EmD
      xnGRyIJ7Jlq0uSZDRqWBo9xB9bxn+qXeIzOBb3ktap2XjDFXejyy9Hpn+oGJZxMA
      dbDjl20LcwD8gRpAHdWucIyOcIchM+8YFAJYgtYbtbiZdxgLd7hnoDuymrrHkctN
      0Z7KPyyVPvibf10oxtVEx50T8O6PFBReRn+gGwIDAQABAoIBAQDFZ+TtrtDOTNAc
      0ra89B5lFQxL0EhNQMmpu25fSaRS9QRh0NyuXPFZUQpLIn/KWQhL616vuqK40z3x
      SkKd+zIub8pjeXrjYMdtG6gWNmqZxVc7SdTw1DgLIZ8vwy2FVIqz2j2yG5OY+u4S
      0s5Qtio0dnMaPZVJ4y4LCuwmRrYOMzQdTl43WH0INgxRW3gHooDkTTlqsE5HKl1e
      8jlXzLQDyMESZi7eff3GIyKqDcyKtGT7A+9fclLTv1yzL9hlAsCSAvqxvQajtPNC
      LkHRg/vhHpetgByHKqCSxZN5PzqmuvQlmKr1KJy/KhWrii7dSbzJd6sbqtTZBuKx
      SASQvzKhAoGBAPiNl71r+tN4fWr9BmmqEAFDJoAeTX8eY+QWX/DD9q5wIEkXZp6i
      ffNq02EybnpiEWeoBl2UjBXMhEjXb9N4Ujlf14H2G2DMn6xiJqOcotAMjg4I2vm0
      F2Lnzcq9rKOInie9n4YJ1PEoMuMdRAcXgp9iL+Kp2QQm4Zro6kNoRkLLAoGBAOiy
      xLt3/3mcF1YqLfzDzLWsZjxQoads5LcecSOGOJUaseb2R1w/InGgvfm9bk0O9DQm
      YPRavbFWQKVODsuDsyn2bWGZZvqpxp6zEC+MbRYN738G5CIVPRNHMUmW7TE58Axr
      9AVFFDeEzizehHOu6UIPMD8Z3QFc/vducHo0ZV3xAoGAVXBWuMZlckv40M4pZikP
      V1+93EyOVyQbMkx+rkSuh0gD0Rw6Kk2w/fu6ra6oS2lqkjcv+PsXLGchEej8h7TU
      juRjMElpH903Bgq3PYaacOnf6vMgUrWVVGpaU1bgAVb1BrQoIes/R6aJ14g32jg6
      ro8R5th7wPGcm6N047b0cAECgYEAvYs4ktfJAr7xh18uPHElI2q9kC3Br4YUu1CR
      qfUfy9yFwvMi53IJ1XKwrGfwG9atdnk4inILiBMQ71Wo2X96hhjTuidhaZa3Uffb
      nE+PX+KUDe2IEHcqW7Sm4iGNLYbbENMyXsSJFjwYURYj37M/D28dxpiDnCOrD9Mm
      zXQ2iZECgYAL7HqDFPnXI/uNSLX03Eqwqo4rQo8SHYv2JDPczJJbG3ktTV7Nr1wG
      sY2BzMWKElvWrNjyZidY4gQpxe4+uD/FdtmcWBUc3XBbC2USUrcZbhPp5udIxxaQ
      VhTO0p+GvL1Uh5NuCeUTMe10oCe8u8iB2N3H02NdoVL43I0bh3xrCw==
      -----END RSA PRIVATE KEY-----

   $ openssl req -new -x509 -days 365 -key certificate_test.key -out certificate_test.csr
      You are about to be asked to enter information that will be incorporated
      into your certificate request.
      What you are about to enter is what is called a Distinguished Name or a DN.
      There are quite a few fields but you can leave some blank
      For some fields there will be a default value,
      If you enter '.', the field will be left blank.
      -----
      Country Name (2 letter code) [AU]:FR
      State or Province Name (full name) [Some-State]:
      Locality Name (eg, city) []:Valence
      Organization Name (eg, company) [Internet Widgits Pty Ltd]:Fred
      Organizational Unit Name (eg, section) []:Alignak
      Common Name (e.g. server FQDN or YOUR name) []:Fred
      Email Address []:frederic.mohier@alignak.net

   $ cat certificate_test.csr
      -----BEGIN CERTIFICATE-----
      MIID9TCCAt2gAwIBAgIJAMYvKhJ1pjGUMA0GCSqGSIb3DQEBCwUAMIGQMQswCQYD
      VQQGEwJGUjETMBEGA1UECAwKU29tZS1TdGF0ZTEQMA4GA1UEBwwHVmFsZW5jZTEN
      MAsGA1UECgwERnJlZDEQMA4GA1UECwwHQWxpZ25hazENMAsGA1UEAwwERnJlZDEq
      MCgGCSqGSIb3DQEJARYbZnJlZGVyaWMubW9oaWVyQGFsaWduYWsubmV0MB4XDTE4
      MDcxNTE0MzA1M1oXDTI4MDcxMjE0MzA1M1owgZAxCzAJBgNVBAYTAkZSMRMwEQYD
      VQQIDApTb21lLVN0YXRlMRAwDgYDVQQHDAdWYWxlbmNlMQ0wCwYDVQQKDARGcmVk
      MRAwDgYDVQQLDAdBbGlnbmFrMQ0wCwYDVQQDDARGcmVkMSowKAYJKoZIhvcNAQkB
      FhtmcmVkZXJpYy5tb2hpZXJAYWxpZ25hay5uZXQwggEiMA0GCSqGSIb3DQEBAQUA
      A4IBDwAwggEKAoIBAQDh7eL5l60eu9Iaan8mvdh58Ac6dKgHqJYCRFsffItzHEzo
      MM/u4DwvM8ySLJ7oJOKlVIvWG4FRkyvPTp/A5q8IBAJkmMw9Ht05AmyHGEiRT+hT
      lkxTNkXD6ePL9HQmAmMvvuo79NlYX2qQide3vUaiQkd4WN+ZZJMldqaNmMHnKlX0
      SYPGcZHIgnsmWrS5JkNGpYGj3EH1vGf6pd4jM4FveS1qnZeMMVd6PLL0emf6gYln
      EwB1sOOXbQtzAPyBGkAd1a5wjI5whyEz7xgUAliC1hu1uJl3GAt3uGegO7KauseR
      y03Rnso/LJU++Jt/XSjG1UTHnRPw7o8UFF5Gf6AbAgMBAAGjUDBOMB0GA1UdDgQW
      BBShPcP02G8+ZSNEgR+ImxpPSGuyETAfBgNVHSMEGDAWgBShPcP02G8+ZSNEgR+I
      mxpPSGuyETAMBgNVHRMEBTADAQH/MA0GCSqGSIb3DQEBCwUAA4IBAQCa7Kx3wWZn
      eOMYxzyXEH7eYmFJO5gZ2YpDbHpDb5i2sZ34M/xT2DfM1CiDFinX0kL4hDnrGQ8k
      UWR0H1ibd+ESUPiM3QLsRfftDzPeRsUAZg+32waRunPdyMr+sm8/gHnhzpXPnR+5
      AXkJLj+SdhzwFnx4oIQ+UZ9K9AE7m5OdYElXbbxReWFWGG+zqZG5EdFxqZi5gjjU
      GjsOcMXWdOodnrzuPFq0PVyUa1yB/5wiW6BaV3n71OpoliPqf3RAT++L4ZoiWJWA
      LtAavPyl7wG1KTmCOw/KabTmS8NHaHaCzrsyb2Ig1BH9RGe6424sv8j7JioiLI+u
      6bVtvmVAot/A
      -----END CERTIFICATE-----


When generating the `certificate_test.csr` (last command), if you run Alignak locally, you can use *Common Name* and the *localhost* value for the server name, otherwise enter the server fully qualified domain name where the daemon is running.

Copy the 3 generated files into the Alignak default configuration directory::

   sudo cp dhparams.pem /usr/local/share/alignak/etc/certs/
   sudo cp certificate_test.csr /usr/local/share/alignak/etc/certs/
   sudo cp certificate_test.key /usr/local/share/alignak/etc/certs/

Daemons configuration
---------------------

In the concerned daemons section of the Alignak configuration, define the full path of your certificate files and uncomment::

   $ sudo vi /usr/local/share/alignak/etc/alignak.ini

      ; SSL configuration
      ; -----
      ; Configure this part if you are using SSL for communication between the Alignak daemons
      use_ssl=true
      ; Paths for certificate files
      server_cert=./certs/certificate_test.csr
      server_key=./certs/certificate_test.key
      ca_cert=./certs/dhparams.pem



    server_cert=%(etcdir)s/certs/certificate_test.csr
    server_key=%(etcdir)s/certs/certificate_test.key
    server_dh=%(etcdir)s/certs/dhparams.pem

If you are using a certificate from an official certification authority, you must also define the intermediate certificate of the authority and uncomment::

    ca_cert=%(etcdir)s/certs/ca.pem


At last, enable SSL in the daemons configuration::

    use_ssl=1
