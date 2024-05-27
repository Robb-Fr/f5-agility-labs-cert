F5N1 - Management
=========================

|
|

*TODO*

|
|

Objective - 1.1 Given a scenario identify when to use NGINX
-----------------------------------------------------------

|
|

**1.1 - Describe NGINX as a web server**

https://nginx.org/en/

https://docs.nginx.com/nginx/admin-guide/web-server/web-server/

Slide 11 https://f5u.csod.com/ui/lms-learning-details/app/course/f62cdc17-e0df-4117-820c-6270100981a2

**NGINX handling web protocols**

One of NGINX's key feature is its web server functionality. NGINX software
indeed implements many features related to the HTTP (web) protocol and can be
configured to serve web content, making it describable as a web server. At a
high level, configuring NGINX Plus as a web server is a matter of defining
which URLs it handles and how it processes HTTP requests for resources at those
URLs. At a lower level, the configuration defines a set of virtual servers that
control the processing of requests for particular domains or IP addresses.

The most basic web server feature that can be handled by NGINX is serving
static content. The following snippet shows a basic example of NGINX
configuration file for serving static web content:

.. code-block:: NGINX

    http {
        server {
            root /www/data;

            location / {
            }

        }
    }

Receiving this configuration file, NGINX would listen to the HTTP port 80 on
the host machine for incoming connections. Upon receiving HTTP request, it
would serve content from the host's `/www/data` directory (for example, a
`/www/data/index.html` file).

|

**1.1 - Describe NGINX as a reverse proxy**

https://www.cloudflare.com/learning/cdn/glossary/reverse-proxy/

https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/

https://nginx.org/en/docs/http/ngx_http_proxy_module.html

Slide 8 and 13 https://f5u.csod.com/ui/lms-learning-details/app/course/f62cdc17-e0df-4117-820c-6270100981a2

**What is reverse proxying and why do so**

A reverse proxy is a server that sits in front of web servers and forwards
client (e.g. web browser) requests to those web servers. Reverse proxies are
typically implemented to help increase security, performance, and reliability.

Proxying is typically used to distribute the load among several servers,
seamlessly show content from different websites, or pass requests for
processing to application servers over protocols other than HTTP.

.. image:: /_static/n1-n4/reverse_proxy_flow.jpg
    :alt: Reverse proxy flow

Reverse proxy flow illustrated. A reverse proxy sits just before the
application servers in the classic client-server network architecture. (from
https://www.cloudflare.com/learning/cdn/glossary/reverse-proxy/).

**How can NGINX be a reverse proxy**

When NGINX proxies a request, it sends the request to a specified proxied
server, fetches the response, and sends it back to the client. It is possible
to proxy requests to an HTTP server (another NGINX server or any other server)
or a non-HTTP server (which can run an application developed with a specific
framework, such as PHP or Python) using a specified protocol. Supported
protocols include FastCGI, uwsgi, SCGI, and memcached.

To pass a request to an HTTP proxied server, the proxy_pass directive is
specified inside a location.

.. code-block:: NGINX

    location /some/path/ {
        proxy_pass http://www.example.com/link/;
    }

This example configuration results in passing all requests processed in this
location to the proxied server at the specified address. This address can be
specified as a domain name or an IP address.

|

**1.1 - Describe NGINX as a load balancer**

https://www.cloudflare.com/learning/performance/what-is-load-balancing/

https://nginx.org/en/docs/http/load_balancing.html

https://docs.nginx.com/nginx/admin-guide/load-balancer/

Slide 14 https://f5u.csod.com/ui/lms-learning-details/app/course/f62cdc17-e0df-4117-820c-6270100981a2

**What is load balancing in the context of networking and web server**

Load balancing is the practice of distributing computational workloads between
two or more computers. On the Internet, load balancing is often employed to
divide network traffic among several servers. This reduces the strain on each
server and makes the servers more efficient, speeding up performance and
reducing latency. Load balancing is essential for most Internet applications to
function properly.

.. image:: /_static/n1-n4/without_load_balancing_diagram.png
    :alt: Without load balancing diagram
    :height: 500px

.. image:: /_static/n1-n4/with_load_balancing_diagram.png
    :alt: With load balancing diagram
    :height: 500px

**How can NGINX be seen as a load balancer**

It is possible to use nginx as a very efficient HTTP load balancer to
distribute traffic to several application servers and to improve performance,
scalability and reliability of web applications with nginx.

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

*TODO*

|

**1.1 - Describe NGINX as a caching solution**

https://aws.amazon.com/caching/

https://docs.nginx.com/nginx/admin-guide/content-cache/content-caching/

https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache

https://www.f5.com/company/events/webinars/content-caching-nginx-plus

**Caching in the context of the web**

Caching is a general concept referring to storing data in a high-speed storage
so that this data can be retrieved faster than possible when reaching its
original location. In the context of web, caching is crucial and is present at
many layers between the actual client software (e.g. web browser) and the
backend server or database, storing the content.

.. image:: /_static/n1-n4/diagram_caching.png
    :alt: Caching on the web diagram
    :height: 250px

+---------------+--------------------------------------+---------------+----------------------------------------------------------------------------+--------------------------------------+------------------------------------------+
| Layer         | Client-Side                          | DNS           | Web                                                                        | App                                  | Database                                 |
+===============+======================================+===============+============================================================================+======================================+==========================================+
|| Use Case     || Accelerate retrieval of web content || Domain to IP || Accelerate retrieval of web content from web/app servers. Manage Web      || Accelerate application performance  || Reduce latency associated with database |
||              || from websites (browser or device)   || Resolution   || Sessions (server side) web/app servers. Manage Web Sessions (server side) || and data access                     || query requests                          |
+---------------+--------------------------------------+---------------+----------------------------------------------------------------------------+--------------------------------------+------------------------------------------+
|| Technologies || HTTP Cache Headers, Browsers        || DNS Servers  || HTTP Cache Headers, CDNs, Reverse Proxies, Web Accelerators,              || Key/Value data stores, Local caches || Database buffers, Key/Value data stores |
||              ||                                     ||              || Key/Value Stores                                                          ||                                     ||                                         |
+---------------+--------------------------------------+---------------+----------------------------------------------------------------------------+--------------------------------------+------------------------------------------+

The above representation comes from the AWS article about caching
(https://aws.amazon.com/caching/) and depicts various levels at which caching
has influence in the context of web content serving.

**How NGINX can be seen as a caching solution**

NGINX leverages the caching capabilities of the web in multiple ways. Indeed,
NGINX as a reverse proxy or web server sits in a crucial place of web content
retrieval pipeline:

- Client-Side caching: NGINX can set and add HTTP headers helping the client
  side to use caching.
- Web and App content: NGINX can, through its `proxy_cache` directive, cache
  the results of requests to the backend servers. These cached results can be
  directly served to new clients, preventing making additional requests to the
  backend servers. NGINX content caching can be fine-tuned, allowing to best
  match the application logic. For example, highly dynamic pages may not be
  cached to make sure the most up-to-date content is served to the clients,
  while static files can be cached for longer time to prevent requesting the
  backend server multiple times for identical files.
- Web and App connections: another useful caching technique sits at the
  transport layer. Thanks to its `keepalive` directives, NGINX can prevent
  closing TCP sockets too early and reuse existing sockets instead of
  re-negotiating a new connection. For the Transport Layer Security, NGINX also
  has directives to cache SSL sessions and prevent re-negotiating keys and
  certificates and reuse existing parameters with clients.

These put together allow NGINX to vastly influence the caching of the web
content served by an application when it acts as a reverse proxy.

|

**1.1 - Describe NGINX as an API gateway**

https://www.redhat.com/en/topics/api/what-does-an-api-gateway-do

https://www.f5.com/company/blog/nginx/deploying-nginx-plus-as-an-api-gateway-part-1

**What is an API gateway**

An API gateway is an API management tool that sits between a client and a
collection of backend services. In this case, a client is the application on a
user's device and the backend services are those on an enterprise's servers.
API stands for application programming interface, which is a set of definitions
and protocols for building and integrating application software.

An API gateway is a component of application delivery (the combination of
services that serve an application to users) and acts as a reverse proxy to
accept all application programming interface (API) calls, aggregate the various
services required to fulfil them, and return the appropriate result. In
simpler terms, an API gateway is a piece of software that intercepts API calls
from a user and routes them to the appropriate backend service.

**How can NGINX be used as an API gateway**

NGINX can be configured as a perfect API gateway through various configuration
aspects:

- Load balancing and upstream definition. As seen before, NGINX can be
  configured with an upstream servers pool and perform load balancing between
  the different API servers. This allows to ensure that the API service can
  scale and match different needs.
- API routes definition. NGINX can be used to define the different API routes
  available in the same way it defines static web content serving routes. This
  allows to define broad (using REGEX matching URIs) and precise (using exact
  matching URIs) endpoints, and control the private or restricted endpoint's
  access.
- Request interception and rewriting for handling breaking changes. NGINX can,
  for example, handle the redirection from legacy API routes to the new ones
  through HTTP redirect and content rewriting.
- Handling errors. NGINX can interpret HTTP errors received from the backend
  and display the desired error pages or messages.
- Authenticating endpoints. NGINX can handle restricting access to some
  endpoints using authentication methods such as API keys or JSON Web Tokens.
- Rate limiting and logging. NGINX can help to secure and enforcing policies on
  endpoints by using its rate limiting or advanced logging features.

All these aspects make NGINX an efficient and complete solution for placing it
as an API gateway.

|
|

Objective - 1.2 Explain the NGINX configuration directory structure
-------------------------------------------------------------------

|
|

**1.2 - Identify the default NGINX core config file**

*TODO*

|

**1.2 - Identify the included directories/files**

*TODO*

|

**1.2 - Describe the order of how the included files will be 'merged' into the
running configuration**

*TODO*

|

**1.2 - Describe directive inheritance and overriding properties**

*TODO*

|
|

Objective - 1.3 Demonstrate how to manage user permissions
----------------------------------------------------------

|
|

**1.3 - Identify user context (i.e. using the configuration file)**

*TODO*

|

**1.3 - Describe how and when to give read/write/execute access**

*TODO*

|

**1.3 - Describe how to run NGINX as a specific user type**

*TODO*

|

**1.3 - Describe the relationship between NGINX processes and users**

*TODO*

|
|

Objective - 1.4 Manage shared memory zones
------------------------------------------

|
|

**1.4 - Describe how and why NGINX uses shared memory zones**

*TODO*

|

**1.4 - Describe why directives use a shared memory zone**

*TODO*

|
|
