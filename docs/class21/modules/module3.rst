F5N3 - Configuration: Demonstrate
=================================

|
|

*TODO*

|
|

Objective - 1.1 Demonstrate how to manage connections and bandwidth
-------------------------------------------------------------------

|
|

1.1 - Describe the difference between rate limiting and bandwidth throttling
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*TODO*

|

1.1 - Demonstrate how to limit the amount of connections that are made to the NGINX server and its upstreams
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*TODO*

|

1.1 - Demonstrate how to set a bandwidth limit
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*TODO*

|

1.1 - Understand how to enable and optimize keep-alives for the NGINX server and its upstreams
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*TODO*

|
|

Objective - 1.2 Demonstrate how to restrict access
--------------------------------------------------

|
|

.. _module3 restrict ip:

1.2 - Demonstrate how to restrict access to NGINX based on IP address
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*TODO*

|

1.2 - Demonstrate how to restrict access to NGINX based on HTTP method
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*TODO*

|

.. _module3 demonstrate authenticate:

1.2 - Demonstrate how to authenticate (auth basic / auth request)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*TODO*

.. code-block:: NGINX

  location / {
    auth_basic "Private site";
    auth_basic_user_file conf.d/passwd;
  }

This example shows the ``/`` location being protected thanks to two directives.
``auth_basic`` enables authentication by defining an HTTP Basic authentication
realm name (see `RFC 2617
<https://datatracker.ietf.org/doc/html/rfc2617#page-3>`_ for more details).
``auth_basic_user_file`` points to a user file (containing username and
password pairs) as defined `here
<http://nginx.org/en/docs/http/ngx_http_auth_basic_module.html#auth_basic_user_file>`_.
This allows to make sure that users accessing endpoints matching the ``location
/ {}`` must present valid authentication credentials following the HTTP Basic
authentication specification. Other users will be returned 401 or 403 error
codes.

On another hand, NGINX proposes another,

|

1.2 - Demonstrate how to restrict URIs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*TODO*

|
|

Objective - 1.3 Demonstrate how to configure logging
----------------------------------------------------

|
|

1.3 - Demonstrate how to customize the format of log files
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*TODO*

|

1.3 - Demonstrate how to customize the location of log files
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*TODO*

|

1.3 - Demonstrate how to set log levels (severity)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*TODO*

|

1.3 - Describe the difference between an error log and an access log
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*TODO*

|
|

.. _module3 configure certificates:

Objective - 1.4 Demonstrate how to configure certificates
---------------------------------------------------------

|
|

1.4 - Define the difference between a server certificate and a client certificate
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*TODO*

|

1.4 - Describe the components necessary to use an SSL certificate
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*TODO*

|

1.4 - Configure encryption
~~~~~~~~~~~~~~~~~~~~~~~~~~

*TODO*

|

1.4 - Describe how to protect the SSL certificate and key
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*TODO*

|
|

Objective - 1.5 Demonstrate how to enable HTTPS and associated security settings
--------------------------------------------------------------------------------

|
|

1.5 - Compare the advantages of TLS termination, end to end encryption, and TLS passthrough
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*TODO*

|

1.5 - Demonstrate how to enable TLS encryption
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*TODO*

|

1.5 - Enable/Disable ciphers and TLS version
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*TODO*

|

1.5 - Describe how force all traffic to redirect to HTTPS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*TODO*

|
|
