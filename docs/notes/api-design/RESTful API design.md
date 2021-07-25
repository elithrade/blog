# RESTful web API design

[RESTful web API design](https://docs.microsoft.com/en-us/azure/architecture/best-practices/api-design)

* Representational State Transfer (REST) as an architectural approach to designing web services.
* An API should be platform independent, meaning any client can call the API regardless of how it is implemented internally.
* It should also be backwards compatible. Additional functionality can be added without breaking the existing ones.

## REST design principles
* REST APIs are designed around resources.
* Each resource has a unique identified, a URI identified the resource, e.g. ```https://adventure-works.com/orders/1```.
* Client interact with it using representations of resources, e.g. `json`.
* For REST APIs built on HTTP, the uniform interface includes using standard HTTP verbs to perform operations on resources. The most common operations are GET, POST, PUT, PATCH, and DELETE.
* REST APIs use a stateless request model. 
* Organise API design around resources. When possible, resource URIs should be based on nouns (the resource) and not verbs (the operations on the resource).
* Avoid creating APIs that simply mirror the internal structure of a database. The purpose of REST is to model entities and the operations that an application can perform on those entities. A client should not be exposed to the internal implementation.
* Adopt a consistent naming convention in URIs.
* In more complex systems, it can be tempting to provide URIs that enable a client to navigate through several levels of relationships, such as `/customers/1/orders/99/products`. However, this level of complexity can be difficult to maintain and is inflexible if the relationships between resources change in the future. Instead, try to keep URIs relatively simple. Once an application has a reference to a resource, it should be possible to use this reference to find items related to that resource. The preceding query can be replaced with the URI `/customers/1/orders` to find all the orders for customer 1, and then `/orders/99/products` to find the products in this order.
* Avoid sending too many requests, instead denormalize the data and combine related information into bigger resources that can be retrieved with a single request. 

## The differences between POST, PUT, and PATCH
* A POST request creates new a resource.
* A PUT create a new or updates an existing one.
* A PATCH request performs a partial update to an existing resource. The client specifies the URI for the resource. The request body specifies a set of changes to apply to the resource. This can be more efficient than using PUT, because the client only sends the changes, not the entire representation of the resource.

## Asynchronous operations
* Status code 202 (Accepted) should returned together with the ***Location*** header, so the client can poll the status endpoint for status.
  ```http
  HTTP/1.1 202 Accepted
  Location: /api/status/12345
  ```
  If the client sends a GET request to the status endpoint, it should return the current progress potentially with URI to cancel the operation.
  ```http
  HTTP/1.1 200 OK

  Content-Type: application/json
  {
    "status":"In progress",
    "link": { "rel":"cancel", "method":"delete", "href":"/api/status/12345" }
  }
  ```
* If an async operation creates a resource that might take a long time, the status endpoint should return code 303 (See Other) after the operation completes. The header should contain ***Location** that gives the URI of the new resource.
  ```http
  HTTP/1.1 303 See Other
  Location: /api/orders/12345
  ```

## Filter and pagination
* Allow passing filter in the query string of the URI such as ***/order?minCost=n*** and return filtered result.
* Also considering imposing upper limit on the number of items returned, to help prevent Denial of Service attack.
* Query strings can also be used for sorting, however this might not be idea for caching as many caching methods use URI as the key.

## Use HATEOAS to enable navigation to related resources
* This principle is known as HATEOAS, or Hypertext as the Engine of Application State.
* Each HTTP GET request should return the information necessary to find the resources related directly to the requested object through hyperlinks included in the response.
* It should also be provided with information that describes the operations available on each of these resources. For example, to handle the relationship between an order and a customer, the representation of an order could include links that identify the available operations for the customer of the order. Here is a possible representation:
    ```json
    {
    "orderID":3,
    "productID":2,
    "quantity":4,
    "orderValue":16.60,
    "links":[
        {
        "rel":"customer",
        "href":"https://adventure-works.com/customers/3",
        "action":"GET",
        "types":["text/xml","application/json"]
        },
        {
        "rel":"customer",
        "href":"https://adventure-works.com/customers/3",
        "action":"PUT",
        "types":["application/x-www-form-urlencoded"]
        },
        {
        "rel":"customer",
        "href":"https://adventure-works.com/customers/3",
        "action":"DELETE",
        "types":[]
        },
        {
        "rel":"self",
        "href":"https://adventure-works.com/orders/3",
        "action":"GET",
        "types":["text/xml","application/json"]
        },
        {
        "rel":"self",
        "href":"https://adventure-works.com/orders/3",
        "action":"PUT",
        "types":["application/x-www-form-urlencoded"]
        },
        {
        "rel":"self",
        "href":"https://adventure-works.com/orders/3",
        "action":"DELETE",
        "types":[]
        }]
    }
  ```

## Versioning a RESTful web API
### No versioning
* Adding content to existing resources might not present a breaking change as client applications that are not expecting to see this content will ignore it. However, if more radical changes to the schema of resources occur (such as removing or renaming fields) or the relationships between resources change then these may constitute breaking changes that prevent existing client applications from functioning correctly.

### URI versioning
* The version of the resource could be exposed through a URI containing a version number, such as ```https://adventure-works.com/v2/customers/3```.
* This versioning mechanism is very simple but depends on the server routing the request to the appropriate endpoint. However, it can become unwieldy as the web API matures through several iterations and the server has to support a number of different versions. 
* HATEOAS can be complicated as all links need to reference the new version.

### Query string versioning
* Include version as query string of URL, ```https://adventure-works.com/customers/3?version=2```.
* This approach also suffers from the same complications for implementing HATEOAS as the URI versioning mechanism.

### Header versioning
Rather than appending the version number as a query string parameter, you could implement a custom header that indicates the version of the resource. This approach requires that the client application adds the appropriate header to any requests, although the code handling the client request could use a default value (version 1) if the version header is omitted. 

[Microsoft REST API Design Guideline](https://github.com/Microsoft/api-guidelines/blob/master/Guidelines.md) should be used as a reference.