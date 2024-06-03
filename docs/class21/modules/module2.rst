F5N2 - Configuration: Knowledge
===============================

|
|

*TODO*

|
|

Objective - 1.1 Configure NGINX as a load balancer
--------------------------------------------------

|
|

**1.1 - Define the load balancing pools/systems**

http://nginx.org/en/docs/http/load_balancing.html

https://docs.nginx.com/nginx/admin-guide/load-balancer/tcp-udp-load-balancer/

**``upstream`` directive**

Defining a load balancing pool is as simple as writing the ``upstream``
directive in an ``http`` or ``stream`` block. Please refer to the referenced
pages for details.

 The simplest configuration for load balancing with nginx may look like the
 following:

.. code-block:: NGINX

    http {
        upstream myapp1 {
            server srv1.example.com;
            server srv2.example.com;
            server srv3.example.com;
        }

        server {
            listen 80;

            location / {
                proxy_pass http://myapp1;
            }
        }
    }

In the example above, there are 3 instances of the same application running on
srv1-srv3. When the load balancing method is not specifically configured, it
defaults to round-robin. All requests are proxied to the server group myapp1,
and nginx applies HTTP load balancing to distribute the requests.

|

**1.1 - Explain the different load balancing algorithms**

https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/#choosing-a-load-balancing-method

**Different algorithms for different needs**

The referenced resource explains in details each algorithm. The algorithms
available in NGINX OSS are the following:

- default: round-robin
- least connections with ``least_conn``
- ip hash with ``ip_hash``
- generic hash with ``hash``
- random with ``random``

Note that for these methods, one can also define a ``weight`` parameter, that
influence some server over others when making the upstream server choice with a
defined weight.

|

**1.1 - Describe the process used to remove a server from the pool**

*TODO*

|

**1.1 - Describe what happens when a pool server goes down**

*TODO*

|

**1.1 - Explain what is unique to NGINX as a load balancer**

*TODO*

|

**1.1 -Describe how to configure security**

*TODO*

|

**1.1 - Modify or tune a memory zone configuration**

*TODO*

|

**1.1 - Describe how to configure NGINX as mirroring server**

*TODO*

|

**1.1 - Describe how to configure NGINX as a layer 4 load balancer**

*TODO*

|

**1.1 - Describe how to configure NGINX as an API Gateway**

*TODO*

|
|

Objective - 1.2 Configure NGINX as a content cache server
---------------------------------------------------------

|
|

**1.2 - Define a minimum retention policy**

*TODO*

|

**1.2 - Describe how to configure path regex routing**

*TODO*

|

**1.2 - Describe the why and how of caching in NGINX**

*TODO*

|

**1.2 - Define the cache in the http context**

*TODO*

|

**1.2 - Enable the cache**

*TODO*

|

**1.2 - Specify the content that should be cached**

*TODO*

|

**1.2 - Describe different types of caching**

*TODO*

|

**1.2 - Explain what is unique to NGINX as a cache server**

*TODO*

|
|

Objective - 1.3 Configure NGINX as a web server
-----------------------------------------------

|
|

**1.3 - Demonstrate how to securely serve content (HTTP/HTTPS)**

*TODO*

|

**1.3 - Describe the difference between serving static content and dynamic
content. (REGEX, and variables)**

*TODO*

|

**1.3 - Describe how server and location work**

*TODO*

|

**1.3 - Explain what is unique to NGINX as a web server**

*TODO*

|
|

Objective - 1.4 Manage shared memory zones
------------------------------------------

|
|

**1.4 - Explain how traffic routing is handled in NGINX as a reverse proxy**

*TODO*

|

**1.4 - Explain what is unique to NGINX as a reverse proxy**

*TODO*

|

**1.4 - Configure encryption**

*TODO*

|

**1.4 - Demonstrate how to manipulate headers**

*TODO*

|

**1.4 - Describe the difference between proxy_set_header and add_header**

*TODO*

|

**1.4 - Modify or tune a memory zone configuration**

*TODO*

|

**1.4 - Describe how to configure NGINX as socket reserve proxy**

*TODO*

|

**1.4 - Describe how open source NGINX handles health checks in different
situations**

*TODO*

|
|
