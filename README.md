OASpy Client
============

HTTP API client for consuming APIs based on OpenAPI specification.

[![Documentation Status](https://readthedocs.org/projects/oaspy-client/badge/?version=latest)](https://oaspy-client.readthedocs.io)
[![CI builds](https://b11c.semaphoreci.com/badges/oaspy-client.svg?style=shields)](https://b11c.semaphoreci.com/projects/oaspy-client)

**OASpy Client** is a Python library that enables automated creation of a client for consuming HTTP services based on the [OpenAPI Specification](https://swagger.io/resources/open-api/) standard.


Quick Start
-----------

```python
from oaspy.client import Client

client = Client.from_file("path/to/openapi.yaml")
result = client.some_endpoint_id("path", "variables", "query_var" = "example")
```