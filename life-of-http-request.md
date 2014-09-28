# Life of an HTTP Request

#### 1. You type a URL into your browser. (Chrome, Firefox, IE, Opera, etc)
#### 2. The browser starts looking cache for the IP of the visited domain, if it exists, skip to step 4.

> `DNS` - the phonebook of the internet. It translates domain names into numerical IP's needed to locate
> servers anywhere in the world.

#### 3. The search takes places on several levels:

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
- `TLD Nameserver`
- `Facebook.com nameserver returns the designated IP`

Since facebook.com has multiple servers all over the world, they have their own nameservers that connect you with the best available option. There are many ways other providers can do this, here are a few:

> - Round Robin - DNS returns multiple IP addresses
> - Load Balancer - piece of hardware that forwards requests to other servers
> - Geographic DNS - returning a different IP depending on the client's geographic location. This is how `CDN`'s work.

#### 4. Once the correct IP is found, the browser sends an `HTTP GET` request to the web server.

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

The more important of these fields:

- `host` - The host site we're looking for
- `method` - Method that's taking place (GET)
- `User-Agent` - The browser identity
- `Accept` - Type of responses its willing to accept
- `Accept-Encoding` - The encoding of the responses it can accept (gzip, compression)
-  - Your response can be compressed over the wire, so the browser needs to know to uncompress it before doing anything with it
- `Connection` - asks server to keep TCP connection alive for further requests
- `Cookies` - Cookies track the state of a website between different page requests. It's an important feature for website authentication. The server needs to know that you're authenticated even when you leave the login page. These are sent with every request

POST vs GET

- GET request sends parameters via URL (http://facebook.com?q=name)
- POST sends parameters in the request body

#### 5. The server receives the request and now needs to process it before sending back a response

Web servers use software like `Apache`, `Nginx` or `IIS` in order to handle HTTP requests and decide what needs to happen. This part is usually a black box when it comes to the browser. The server can store or process information but all the browser cares about is the response it will receive back. There is a `request hander` that will read the request parameters and the cookies and will decide what to do with the information. It will then generate an HTML response.

#### 6. The server sends back a response

Here is an example of a server response header:

```
cache-control:private, no-cache, no-store, must-revalidate
content-encoding:gzip
content-security-policy:default-src *;script-src https://*.facebook.com http://*.facebook.com https://*.fbcdn.net http://*.fbcdn.net *.facebook.net *.google-analytics.com *.virtualearth.net *.google.com 127.0.0.1:* *.spotilocal.com:* 'unsafe-inline' 'unsafe-eval' https://*.akamaihd.net http://*.akamaihd.net *.atlassolutions.com chrome-extension://lifbcibllhkdhoafpjfnlhfpfgnpldfl;style-src * 'unsafe-inline';connect-src https://*.facebook.com http://*.facebook.com https://*.fbcdn.net http://*.fbcdn.net *.facebook.net *.spotilocal.com:* https://*.akamaihd.net wss://*.facebook.com:* ws://*.facebook.com:* http://*.akamaihd.net https://fb.scanandcleanlocal.com:* *.atlassolutions.com http://attachment.fbsbx.com https://attachment.fbsbx.com;
content-type:text/html; charset=utf-8
date:Sun, 28 Sep 2014 19:20:54 GMT
pragma:no-cache
status:200 OK
strict-transport-security:max-age=15552000; preload
version:HTTP/1.1
x-content-type-options:nosniff
x-frame-options:SAMEORIGIN
x-xss-protection:0
```

Definitiosn of some of the headers returned:

- `content-type` - The type of content that will be sent in the response body. We can see that the server responded with HTML
- `content-encoding` - Tells the browser that the content being sent back is gzipped and it needs to do something with it
- `date` - The date of the request
- `status` - The status of the response. 200 is the computers response followed by a human readable response of "OK"
- `x-frame-options` - Indicates whether or not the browser should be allowed to render a page in an iframe, frame or object. Prevents the content from being embedded in other sites.
- `strict-transport-security` - The web browser is only allowed to interact with a secure HTTPS connection
- `x-xss-protection` - Used to protect some forms of XSS attacks

There are a wide variety of HTTP headers that a server can return, this is just a limited list of those headers.

HTTP Responses like `200 OK` can come in a wide variety as well:
- 1xx informational message
- 2xx success
- 3xx redirect
- 4xx error on client part
- 5xx server error

#### 7. Even though not all of the HTML has been received, the browser begins to render the website for the sake of a better user experience.

The browser will now start the HTML parsing process and look for other resources that include a URL. Most of the time this includes external `images`, `css`, `javascript` files that are required to display on a page. The process of the HTTP request is very similar for these static files as well.

The beauty of all this is that the browser is able to cache these static files in order to optimize performance. A great example is the Google CDN or Google Fonts. There's a high chance that the stylesheet for a particular font has already been cached on your machine since the library is so popular.

There is also an `expires` header that will let the browser know how long its allowed to cache certain static files. There are times when you would need to clear your cache in order to see changes, that is because the server response had a longer expiration than the actual updated content.

#### 8. AJAX Requests

One of the greatest features of javascript is `AJAX` - Asynchronous Javascript and XML. The asynchronous nature of Javascript allows your browser to execute javascript to either `POST` or `GET` information without a full page reload.

## Additional Topics:
- Long polling, web sockets

## Resources
- [https://en.wikipedia.org/wiki/Domain_Name_System](https://en.wikipedia.org/wiki/Domain_Name_System)
- [https://en.wikipedia.org/wiki/Name_server#Authoritative_name_server](https://en.wikipedia.org/wiki/Name_server#Authoritative_name_server)
- [http://igoro.com/archive/what-really-happens-when-you-navigate-to-a-url/](http://igoro.com/archive/what-really-happens-when-you-navigate-to-a-url/)
- [http://www.jmarshall.com/easy/http/](http://www.jmarshall.com/easy/http/)
- [https://www.mobify.com/blog/beginners-guide-to-http-cache-headers/](https://www.mobify.com/blog/beginners-guide-to-http-cache-headers/)
