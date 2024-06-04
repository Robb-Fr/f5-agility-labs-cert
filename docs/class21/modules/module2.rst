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

https://docs.nginx.com/nginx/admin-guide/load-balancer/http-health-check/#passive-health-checks

https://nginx.org/en/docs/http/load_balancing.html

https://nginx.org/en/docs/http/ngx_http_upstream_module.html#server

**Manually remove a server from a pool**

Let us say you have an upstream pool such as the one showed in the following
configuration file:

.. code-block:: NGINX
    :linenos:

    upstream backend {
        server backend1.example.com;
        server backend2.example.com:8080;
        server backend2.example.com:8081;
        }

You may want to perform some maintenance on the server ``backend1.example.com``
and therefore, temporarily remove it from the pool. Removing the line 2 and
reloading the configuration file with ``nginx -s reload`` will make NGINX not
choose this upstream server for any new incoming connection. Another cleaner
possibility would be to use the ``down`` option such as:

.. code-block:: NGINX
    :linenos:
    :emphasize-lines: 2

    upstream backend {
        server backend1.example.com down;
        server backend2.example.com:8080;
        server backend2.example.com:8081;
        }

Where you perform a minimal alteration on your file. Note that this may lead to
connection loss for clients that were proxied to the backend1 server when you
run the configuration reload command.

.. _health check:

**Automatic removal with passive health checks**

NGINX also manages automatic removal of pool members using the passive health
checks. If the response from a particular server fails with an error, nginx
will mark this server as failed, and will try to avoid selecting this server
for subsequent inbound requests for a while.

The max_fails directive sets the number of consecutive unsuccessful attempts to
communicate with the server that should happen during fail_timeout. By default,
max_fails is set to 1. When it is set to 0, health checks are disabled for this
server. The fail_timeout parameter also defines how long the server will be
marked as failed. After fail_timeout interval following the server failure,
nginx will start to gracefully probe the server with the live client's
requests. If the probes have been successful, the server is marked as a live
one.

|

**1.1 - Describe what happens when a pool server goes down**

This aspect is covered in the previous part on `health check`_.

|

**1.1 - Explain what is unique to NGINX as a load balancer**

https://www.f5.com/company/events/webinars/nginx-plus-for-load-balancing-30-min
(from 6:40 to 10:20 notably)

**What are the other load balancing methods**

DNS Rounds Robin
    This method is quite simple and can be easily and cheaply configured: to
    load balance between 3 servers with 3 different IPs, the DNS record for the
    service (example.com for example) is configured to one element among an
    array of 3 IP addresses. Clients, receiving these, will contact the server
    with the received IP address, allowing to distribute load among clients as
    long as the DNS server returns different results to different clients.

    However, this lacks on the update speed: updating DNS records can take time
    and a server that is down may be served to some client for a long time.
    Also, this method does not scale well as it requires managing every growing
    DNS records which can be complicated.

Hardware L4 load balancer
    These are advanced network switches that do not handle a full TCP stack but
    stream TCP packets and track the TCP sessions using the attributes they
    find in the TCP header. They deliver great performances but are limited in
    terms of available features: out of order and broken TCP packets are not
    easy to handle and lead to a reduced flexibility.

Cloud solutions
    Cloud providers often provide their own load balancing systems (Amazon's
    Elastic Load Balancer for example). However, these totally depend on the
    exposed interface from the Cloud provider's system, potentially giving a
    lower flexibility.

**Where NGINX stands and what challenges it can overcome**

NGINX is in the category of the Software load balancer. This refers to reverse
proxy systems: these are software applications running on machines having their
own full TCP stack (Linux or FreeBSD machines for example). The particularity
is that it terminates the TCP connection and handles it. It afterward processes
the content of the connection as desired, and reopen a TCP connection to the
upstream server, using any implementable software method to load balance
between different servers. This gives the maximum degree of flexibility to
control the received connection and stream and apply logic to ensure
performance and security.

For example, with NGINX, one can perform load balancing depending on HTTP
content (session cookies, request URIs, ...) as the reverse proxy terminates
the TCP connection, it has the ability to use any L4-L7 information to perform
load balancing decision.

Also, NGINX being implemented using low level performant C code, it benefits
from excellent performances despite being software based, which is a key aspect
to efficient load balancing.

The following diagrams picture the different ideologies between the different
types of load balancers.

.. image:: /_static/n1-n4/load-balancers-dns.excalidraw.svg
    :width: 1200px
    :align: center
    :alt: Diagram load balancer DNS

.. image:: /_static/n1-n4/load-balancers-l4.excalidraw.svg
    :width: 1200px
    :align: center
    :alt: Diagram load balancer L4

.. image:: /_static/n1-n4/load-balancers-software.excalidraw.svg
    :width: 1200px
    :align: center
    :alt: Diagram software load balancer

|

**1.1 -Describe how to configure security**

https://docs.nginx.com/nginx/admin-guide/security-controls/

https://docs.nginx.com/nginx/admin-guide/monitoring/logging/

**L4-L7 security**

This given objective may sound quite vague and it is not clear why it stands in
this section about load balancing as it could be a section in itself.
Considering this, the reader is advised to be familiar with all the NGINX
security controls available in NGINX OSS that we will list here and are
detailed in the linked documentation.

- NGINX as an HTTPS/SSL server: NGINX can handle and terminate TLS/SSL
  communications. The simple default but customizable at will principle also
  applies here: 3 directives allow setting up NGINX as an HTTPS reverse proxy
  load balancer, but other options can be enabled (mTLS, OCSP, SNI
  validation...). Note these are available in ``http {}`` and ``stream {}``
  blocks.
- NGINX as a perimeter authentication system: NGINX supports authentication
  protocols (limited in NGINX OSS) to ensure the desired ``server {}`` or
  ``location {}`` blocks are protected and authenticated.
- Rate/bandwidth control: NGINX can be configured to limit the request
  rate/amount or the served bandwidth to some clients to prevent abuses.
- IP based restrictions: NGINX can restrict access to some routes or some
  servers based on the client's IP.
- NGINX as an HTTPS/SSL client: NGINX can finally handle secured connections to
  upstream servers with again, simple defaults and some granular control to
  enable options.

Also, considering observability as a security property, take note of the
logging configuration of NGINX, notably its centralisation capabilities with
easy to configure log sending to a syslog server.

|

**1.1 - Modify or tune a memory zone configuration**

http://nginx.org/en/docs/http/ngx_http_limit_conn_module.html#limit_conn_zone

http://nginx.org/en/docs/http/ngx_http_limit_req_module.html#limit_req_zone

http://nginx.org/en/docs/http/ngx_http_js_module.html#js_shared_dict_zone

http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_path

http://nginx.org/en/docs/http/ngx_http_upstream_module.html#zone

**Memory zones in NGINX**

When configuring memory zones in NGINX, we generally refer to shared memory
zones, as seen and explained in the previous part. To modify or tune these, we
must first identify where they appear in our NGINX configurations. In NGINX
OSS, shared memory zones can be configured in the following contexts:

- The connection limiting: sharing across worker the state of clients
  regarding the amount of connection requests.
- The request limiting: sharing across worker the state of clients regarding
  the amount and nature of HTTP requests.
- The JavaScript shared dictionary: sharing across workers JS structure in the
  form of dictionary.
- The proxy caching: sharing across workers the key/value pairs associating
  requests parameters with cached content location on the disk.
- The upstream pools: sharing across workers the state of upstream services of
  pools for updating their status (alive, down, served X times, ...)

**What can be configured and tuned**

In each of the aforementioned contexts, different directives allow to configure
the shared memory zones corresponding. For most of these, this zone has only 2
parameters: a name (used to identify a same zone multiple times in the config),
and a size in bytes.

The size parameter can be tuned and engineered to correspond to the nature of
the application and the server's resources. For example, knowing that a shared
JS dictionary should only have a few small entries, on can allocate only a few
kilo bytes preventing the allocation of mega bytes of memory and not using it.

For details on the different syntaxes, the reader should refer to the mentioned
links to the documentation.

|

**1.1 - Describe how to configure NGINX as mirroring server**

https://alex.dzyoba.com/blog/nginx-mirror/

http://nginx.org/en/docs/http/ngx_http_mirror_module.html

https://thelinuxnotes.com/index.php/mirroring-requests-to-another-server-with-nginx/

**The concept of mirroring requests in NGINX**

In the context of reverse proxying, request mirroring refers to making the
reverse proxy, proxy requests to a mirroring server, "as if" it was an actual
backend upstream server. However, the specificity lies in the fact that NGINX
does not actually forward the mirror server's response back to the client. This
for example allows to test a new, off-production backend server with real
clients' requests and assess its functionalities before pushing it to
production.

The following diagram from `Alex Dzyoba's
blog <https://alex.dzyoba.com/blog/nginx-mirror/>`_ provides a visual
representation of a mirroring setup where NGINX would both, proxy the actual
client's request to the real backend server, as well as mirroring this request
to a test server.

.. image:: /_static/n1-n4/nginx-mirror-mirror-setup.png
    :height: 400px
    :alt: Diagram of a mirroring server setup with NGINX

**Configure NGINX to mirror requests**

NGINX uses the directives from the ``ngx_http_mirror_module`` to implement the
mirroring.

The following configuration defines 2 locations: the first where NGINX should:

1. mirror the client's request to its ``/mirror`` URI
2. proxy the request to the real backend, picked from the upstream pool named
   ``backend``.

The second location is internal (meaning it can only be reached by NGINX
itself, not from the outside), and defines what should happen to requests made
to the ``/mirror`` endpoint. They should be proxied to another backend, picked
from the ``test_backend`` pool.

.. code-block:: NGINX

    location / {
        mirror /mirror;
        proxy_pass http://backend;
    }

    location = /mirror {
        internal;
        proxy_pass http://test_backend$request_uri;
    }

|

**1.1 - Describe how to configure NGINX as a layer 4 load balancer**

https://docs.nginx.com/nginx/admin-guide/load-balancer/tcp-udp-load-balancer/

**TCP/UDP load balancing**

In the same fashion as NGINX can be configured as a Layer 7 (HTTP) load
balancer, the same can be done at the Layer 4 with a similar syntax: one must
configure an upstream servers group with the ``upstream`` directive and can
afterward use the ``proxy_pass`` directive to proxy the requests at layer 4 to
the upstream pool.

The following configuration defines an upstream pool composed of 3 servers: the
first 3 are active while the last 2 are backup (they receive requests only when
one of the active server is down). The first server is preferred in the load
balancing algorithm by a factor of 5. The load balancing algorithm uses the
hash algorithm by taking the remote client's address as a key.

.. code-block:: NGINX

    upstream backend {
        hash $remote_addr;

        server backend1.example.com:12345  weight=5;
        server backend2.example.com:12345;
        server unix:/tmp/backend3;

        server backup1.example.com:12345   backup;
        server backup2.example.com:12345   backup;
    }

    server {
        listen 12346;
        proxy_pass backend;
    }

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
