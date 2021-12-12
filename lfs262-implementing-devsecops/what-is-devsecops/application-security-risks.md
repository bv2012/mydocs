# Application Security Risks

## What Are the Risks with Modern Applications?

Traditionally, when applications were built as monoliths and deployed in data centers, limiting physical access and setting up firewalls would provide sufficient security. With the advent of cloud computing, security measures have been expanded to further implement policies associated with virtual private clouds (VPCs), network access control (NAC), and identity and access management (IAM).

Running modern applications on the cloud—which are exposed as APIs, designed as microservices, packaged with containers, and deployed with Kubernetes—introduces new dimensions of risk. Microservices open many perimeters (for many services), have a flexible flow and are constantly/rapidly deployed, which make it even more challenging to address security issues associated with them.

Next, we will present some tales of caution that should help you put things in perspective.

## API/Application Vulnerabilities

* A researcher found a way to take over a user account of any Uber driver ([Issue 49: Uber account takeover and the leaky Get API - API Security News](https://apisecurity.io/issue-49-uber-account-takeover-leaky-get-api/))
* Researchers detected vulnerabilities with electric vehicle (EV) charging stations, using which they could obtain users' personal information and take control of the charging, possibly causing spikes in charge or mass attacks ([Issue 145: APIs and electric car charging stations, The Nuts and Bolts of OAuth 2.0 - API Security News](https://apisecurity.io/issue-145-apis-electric-car-charging-stations-nuts-bolts-oauth-2-0/))
* A vulnerability in the iPhone’s automatic call recorder application could expose users’ call recordings and sensitive information ([Issue 125: iPhone call recorder API flaw, Burp and OpenAPI, GraphQL pentesting, FAPI - API Security News](https://apisecurity.io/issue-125/)).

You can read about many such vulnerabilities at [API Security Articles, News, Vulnerabilities & Best Practices](https://apisecurity.io).

## Platform Vulnerabilities

In the 2017 Equifax breach, private information such as credit ratings, of 147 million Americans was leaked due to a known vulnerability in a third-party software, i.e. Apache Struts. The root cause for the breach was attributed to this software not being updated for a few months, despite a fix being available ([2017 Equifax data breach - Wikipedia](https://en.wikipedia.org/wiki/2017\_Equifax\_data\_breach)).

A vulnerability was discovered with runC that would allow containers to break out and gain root access on the underlying host in some cases. This affected most of the container environments running Docker and Kubernetes, with runC being the most common low-level container runtime.

Certain Kubernetes vulnerabilities were discovered in the past that could allow attacks such as denial of service (read [Top 5 Kubernetes Vulnerabilities of 2019 — the Year in Review | StackRox Community](https://www.stackrox.io/blog/top-5-kubernetes-vulnerabilities-of-2019-the-year-in-review/). Also find out the [recent Kubernetes CVEs](https://www.cvedetails.com/vulnerability-list/vendor\_id-15867/product\_id-34016/Kubernetes-Kubernetes.html)).

For more information about API security, see [API Security? Yep, everything you needed to know about that in 6,252 words | Ping Identity](https://www.pingidentity.com/en/company/blog/posts/2020/everything-need-know-api-security-2020.html).

The point here is that modern applications need a holistic and layered approach toward security and need everyone’s involvement, including that of the application developers.

From the web application security perspective, one important resource is the [Open Web Application Security Project (OWASP)](https://owasp.org), which is an online community that produces resources such as articles, methodologies, even tools and technologies in the field of application security.

