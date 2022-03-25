# OASpy Client

[![Documentation Status](https://readthedocs.org/projects/oaspy-client/badge/?version=latest)](https://oaspy-client.readthedocs.io)
[![CI builds](https://b11c.semaphoreci.com/badges/oaspy-client.svg?style=shields)](https://b11c.semaphoreci.com/projects/oaspy-client)

OASpy Client allows a user to call an `operationId` as if it was a Python method.

Basic Usage
-----------

OASpy Client is a class that takes an OpenAPI specification in the form of a Python dictionary; there is also a helper class method that will load the spec from a provided file:

```python
from oaspy.client import Client
client = Client.from_file("path/to/openapi.yaml")
```

On instantiation, the client adds a number of methods to itself, each using a snake-case version of an `operationId` as a name. To make a request to the API, call the corresponding method; e.g. if the spec contains an `operationId` named `someEndpointId`, it can be called as:

```python
result = client.some_endpoint_id("foo", "bar", query_var="example")
```

If the corresponding API endpoint accepts any path variables (e.g. `/root/{id}/{name}`), they can be passed in the form of positional arguments to the method call. Any keyword arguments will be added to the request as the URL query, while the special keyword arguments `body_` and `headers_` will populate body and headers of the request, respectively.

OASPy Client will validate both an outgoing request before sending it, as well as the response after receiving, to confirm that they conform to the API specification.

Advanced Usage
--------------

The `Client` class accepts four optional, keyword-only arguments on instantiation:

* `server_url`: A server hostname that will be used to make the actual requests. If it is not present in the `servers` list of the specification it will be appended, and if none is specified the first from the `servers` list will be used. 
* `client`: The HTTP client implementation used to make actual requests. By default OASpy Client uses [`httpx`](https://www.encode.io/httpx/), but it can be replaced by any object compatible with the `Requests` client.
* `request_class`: The class an outgoing request will be wrapped into, before validation. By default it is the built-in `ClientOpenAPIRequest`; if additional functionality is needed it can be substituted by its subclass.
* `response_factory`: A callable used to construct the instance of `OpenAPIResponse` an incoming response will be wrapped into, before validation. By default it is the built-in `ClientOpenAPIResponse`; if additional functionality is needed it can be substituted by its subclass.


Why OASpy?
----------

The main advantage of OASpy -- both as a server and as a client -- is that it uses the OpenAPI specification to both prepare and validate requests and responses. 

Specifically, when `oaspy.server` receives a request it validates it against the URLs defined in the specification
, raising an error in case of an invalid request. On a valid request it looks for an "endpoint function" matching the
 request URL's `operationId`; this function is supposed to process a request and return a response, which is then
  also validated based on the specification rules before being sent back to the client.
  
On the other hand, `oaspy.client` uses the `operationId` to generate a request based on the rules defined in the
 specification. The user needs to call the `operationId` as if it was a method of a client instance
 , optionally passing any specified arguments. 
