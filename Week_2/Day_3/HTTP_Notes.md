# HTTP Notes

## HTTP `Requests`

![](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview/http_request.png)

* Will consist of the following:
  * An HTTP **method**, usually a verb like *GET*, *POST* or a noun like *OPTIONS* or *HEAD* that defines the operation the client wants to perform. Typically, a client wants to fetch a resource (using GET) or post the value of an HTML form (using POST), though more operations may be needed in other cases.
  * The **path** of the resource to fetch; the URL of the resource stripped from elements that are obvious from the context, for example without the protocol (http://), the domain (here, developer.mozilla.org), or the TCP port (here, 80).
  * The **version** of the HTTP protocol.
  * *Optional*: **headers** that convey additional information for the servers.
  * *Or*: a **body**, for some methods like POST, similar to those in responses, which contain the resource sent.

## HTTP `Responses`

![](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview/http_response.png)

* Will consist of the following:
  * The **version** of the HTTP protocol they follow.
  * A **status code**, indicating if the request was successful, or not, and why.
  * A status message, a non-authoritative short description of the status code.
  * HTTP headers, like those for requests.
  * *Optionally*: a body containing the fetched resource.

## `npm request`

* Node has a package called `request` that makes HTTP requests much simpler for us.
  * Behind the scenes, it uses `http`, which in turn uses `net` that we have already used before