# Life of an HTTP Request

1. You type a URL into your browser. (Chrome, Firefox, IE, Opera, etc)
2. The browser starts looking cache for the IP of the visited domain, if it exists, skip to step 9. This is called DNS lookup.

> `DNS` - the phonebook of the internet. It translates domain names into numerical IP's needed to locate
> servers anywhere in the world.

The search takes places on several levels:
- Browser cache stores DNS records for a fixed duration
- Operating system cache
- Personal router cache
- ISP cache
- Finally, if it's not cached on the ISP level, the ISP DNS server starts a recursive search starting with the root name server and making its way down to the IP it is looking for.

> There are/were 13 main `Root Name Servers` that handle requests for records and return a list of
> `authoritative name servers` for the TLD you're looking for (.com, .org, etc). With anycast however, that
> number of root name servers has been decentralized to something around 258 in order to bring down response times.

Recursive DNS Search:
- `Root Nameserver`
- `ORG Nameserver`
- `Facebook.com nameserver returns the designated IP`

Since facebook.com has multiple servers all over the world, there is a system for connecting you with the best available option. The options include:

- Round Robin - DNS returns multiple IP addresses
- Load Balancer - piece of hardware that forwards requests to other servers
- Geographic DNS - returning a different IP depending on the client's geographic location. This is how `CDN`'s work.

Once the right IP address is found, browser initiates a TCP connection with the server
- Browser sends `GET HTTP Request` to the server according to HTTP protocol

A GET Request in Chrome To Facebook:

```
:host:www.facebook.com
:method:GET
:path:/ai.php
:scheme:https
:version:HTTP/1.1
accept:text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
accept-encoding:gzip,deflate,sdch
accept-language:en-US,en;q=0.8
cookie:datr=dAgyU-zTspdguQ2-m;
dnt:1
referer:https://www.facebook.com/
user-agent:Mozilla/5.0 (Macintosh; Intel Mac OS X 10_9_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/37.0.2062.122 Safari/537.36
```
`host` - The host site we're looking for
`method` - Method that's taking place (GET)
`User-Agent` - The browser identity
`Accept` - Type of responses its willing to accept
`Accept-Encoding` - The encoding of the responses (gzip, compression)
`Connection` - asks server to keep TCP connection alive for further requests

POST vs GET

GET request sends parameters via URL
POST sends parameters in the request body

```
X-Frame-Options:
DENY
X-Content-Type-Options:
nosniff
P3P:
CP="Facebook does not have a P3P policy. Learn why here: http://fb.me/p3p"
Content-Type:
text/html;charset=utf-8
Pragma:
no-cache
Cache-Control:
private, no-cache, no-store, must-revalidate
Expires:
Sat, 01 Jan 2000 00:00:00 GMT
X-UA-Compatible:
IE=edge,chrome=1
Set-Cookie:
datr=BXcnVBVFPjE-evOAQK4EOlr-; expires=Tue, 27-Sep-2016 02:48:37 GMT; Max-Age=63072000; path=/; domain=.facebook.com; httponly
X-FB-Debug:
/Ho3EmEZISvqRYjJXS//0LpWhj1d2UaA1B7HoI2bXCU+DCsu+4gfDmzbt07HZYhzrriE+SW5RFSCuyVm6V11EQ==
Date:
Sun, 28 Sep 2014 02:48:37 GMT
Connection:
keep-alive
Content-Length:
1651
```

HTTP responses come in a variety of status codes
- 1xx informational message
- 2xx success
- 3xx redirect
- 4xx error on client part
- 5xx server error

The server handles the incoming request using its web server, ie. Apache, Nginx, IIS and the web server passes on the request to the proper request handler that was written in some server side languaage.

Once that request has been processed, the server sends the HTTP Response back to the browser. That response could be your data: JSON, XML, etc or the HTML that's rendering your page.

## Additional Topics:
- Long polling, web sockets

## Resources
- [https://en.wikipedia.org/wiki/Domain_Name_System](https://en.wikipedia.org/wiki/Domain_Name_System)
- [https://en.wikipedia.org/wiki/Name_server#Authoritative_name_server](https://en.wikipedia.org/wiki/Name_server#Authoritative_name_server)
- [http://igoro.com/archive/what-really-happens-when-you-navigate-to-a-url/](http://igoro.com/archive/what-really-happens-when-you-navigate-to-a-url/)
- [http://www.jmarshall.com/easy/http/](http://www.jmarshall.com/easy/http/)
