# DNS in detail

- Translates website names to IP addresses. 

## Domain heirarchy
- Take admin.tryhackme.com as an example. Always, the right most part of a domain name is the top-level domain (TLD). 

    Second level domain is to the left of the TLD. 

    Subdomains are to the left of second level domains and separated by periods. **Even "www" is a subdomain!** 

    In our example, 

    - TLD: .com

    - Second level domain: tryhackme

    - Subdomain: admin

    ![alt text](images/DNS.png)

## Record types

- A record: These records resolve to IPv4 addresses, for example 104.26.10.229.

- AAAA record: These records resolve to IPv6 addresses, for example 2606:4700:20::681a:be5.

- CNAME record: Alias records for domains. 

## What happens when you make a DNS request?

![process of dns request](images/process-of-dns-req.png)

Client <-> Recursive DNS server (usually provided by ISP) -> Root DNS server (.) -> TLD (.com) -> Authoritative server (OR name server) which has accurate DNS records for the requested domain.

# HTTP in detail

## Requests and responses

- URL: 

    A URL is an instruction to the browser on how to access a resource on a web server. 

    ![URL structure](images/url-structure.png)

    - Path: The file name or location of the resource you are trying to access.

    - Query String: Extra bits of information that can be sent to the requested path. For example, /blog?id=1 would tell the blog path that you wish to receive the blog article with the id of 1.

    - Fragment: This is a reference to a location on the actual page requested. This is commonly used for pages with long content and can have a certain part of the page directly linked to it, so it is viewable to the user as soon as they access the page.

- Requests

    ![Example request](images/ex-request.png)

    1. Line 1 indicates that GET method is used to get home page (/) and we tell the web server that HTTP protocol version 1.1 is being used. 

## HTTP methods

