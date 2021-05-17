# Domain Name System

* DNS is a way for us to find websites easily and without using IP addresses to find these websites.

* When you type a URL, the browser asks the operating system (OS) where the URL is.

* If it's not stored in cache, the OS will query the resolving name server (RNS).

* The RNS will ask the root name servers, which only knows where to locate the **root**.

* They will tell the RNS to query the top-level domain servers (TDS). So if you're asking about `www.example.com`, the RNS will point you to the `com` TLD servers.

* The TLD will point to the authoritative name servers. This is through the help of the domain's *registrar*.
  * The registrar is told which authoritative name servers to use and tells them to update the TLD nameservers.

* The authoritative name server will tell the RNS where to go.

* The RNS will store this in cache, will report back to the OS, and the OS will send this answer back to the browser.