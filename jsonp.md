# JSONP
## The G**d, The Bad, The Ugly

XMLHttpRequest will not work across domains in order to prevent XSS. A way around this is wrapping regular JSON in a function.

Now instead of using a reuglar XMLHttpRequest to retrieve the document, you are dynamically attaching a script tag to the page pointing to the JSON document. This evokes a function call passing the JSON object to whatever JSONP is sent to.

Issues With This:

- Need ABSOLUTE faith in the site that you are using. Code is being excuted by the browser with no way to stop any malicious code from screwing things up.
- Still needs to be protected from CSRF (Cross-site Request Forgery)
- Mixed Content Warnings
- Authentication is ugly
- Other languages cannot parse JSONP. There's no reason to use it anywhere but Javascript
- Mixing functions and data - You're doing it wrong.

An alternative to JSONP is work all this out server-side instead where you have much more control and security.

## References
[https://security.stackexchange.com/questions/23438/security-risks-with-jsonp](https://security.stackexchange.com/questions/23438/security-risks-with-jsonp)