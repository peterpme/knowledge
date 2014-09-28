# Life Of An HTTP Request

You type in a URL into your browser (Chrome, Firefox, IE, Opera, etc), several things can happen at this point depending on whether or not the information is cached.

Say it isn't:
- DNS look up to find the IP of the server
- DNS is considered to be the phone book of the internet. It translates domain names into numerical IP addresses needed to locate servers all over the world. The way this works is through `authoritative name servers` that'll give the request an answer in response to names in a zone.

Caching Takes Place on Several Levels:
- Your Browser
- Operating System
- Your Network Router
- ISP Cache

In the event that no cache exists, a DNS recursive search takes place in order to find the appropriate IP address.

The way the DNS recursive search because is by contacting the `root nameserer` and working with the TLD (top level domain) like .com first. The .com nameserver will then do its own search and call on the facebook.com nameserver. At that point, facebook.com will return an IP address to connect to.

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
