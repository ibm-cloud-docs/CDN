---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-21"

keywords: cross origin resource sharing, CORS, CORS request, same-origin policy, simple request, preflighted request

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# CORS and CORS Requests Through Your CDN
{: #cors-and-cors-requests-through-your-cdn}

Cross Origin Resource Sharing (CORS) is a mechanism used by browsers, primarily to validate permissions for access to content from a different origin.

## What is CORS?
{: #what-is-cors}

When a browser loads a webpage, it enforces the **Same-origin Policy**, which means that it only allows content to be fetched from the same origin as the webpage. However, in some cases a webpage may need access to assets from multiple origins that trust that website. This is where CORS comes in. 

This security mechanism exists only if an application has or is an HTTP client, and if that application implements CORS. Nearly all modern browsers, such as Chrome, Firefox, and Safari,  implement CORS.

To clarify, **an origin with respect to CORS does not have to be the same as a CDN origin**. An origin in CORS is defined by a URI scheme, domain, and any possible port number. For example `https://www.example.com:1443` is different origin than `http://www.example.com`. And so, a CDN can also be considered as a CORS origin from a browser's perspective.

## How CORS works

CORS can handle two types of requests: _simple requests_ and _preflighted requests_, which are more complex.

### Simple requests
{: #simple-requessts}

**First request (resource access):**

![cors-simple](/images/cors-simple.png)

[Simple requests ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.w3.org/TR/cors/#simple-cross-origin-request) through CORS are either `GET` or `POST` requests from the webpage of one origin that's attempting to gain access to the URL of another origin for resources.

When making such a request, the browser automatically sets CORS request headers. Primarily, it sets the origin of the webpage making the request in the `Origin` HTTP header, automatically. The CORS request also may contain a set of [certain, standard HTTP headers ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.w3.org/TR/cors/#simple-header), while maintaining its status as a simple CORS request, from the browser's perspective.

The server that receives the CORS request will process the request, and may (or may not) send a set of CORS response headers back to the browser, with the requested content. These CORS response headers contain values specifying whether the current webpage is allowed access to those resources, whether the headers sent are acceptable for that request, and so forth.

If the browser is unable to see its CORS request satisfied by the CORS response headers, it automatically prevents the access to and loading of the content. Otherwise, it sees that the CORS origin is giving permission to use the resource, and it allows the access to and loading of the requested content.

### Preflighted requests
{: #preflighted-requests}

**First request (preflight):**

![cors-preflight](/images/cors-preflight.png)

Second request (resource access):

![cors-after-preflight](/images/cors-after-preflight.png)

For more complex CORS communication between the browser and a CORS origin that's different than the requesting webpage, a [preflight request ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.w3.org/TR/cors/#cross-origin-request-with-preflight-0) would be required before an actual resource access. Certain situations may require preflighting CORS requests, such as HTTP methods that are not `GET` or `POST` methods, or using non-standard HTTP headers with the request - even if it is a `GET` or `POST` request, and so forth.

If a preflight request is needed, here's how the events unfold:

* The browser sends a request using the HTTP OPTIONS method to the server with all of the intended CORS request headers. 
* The server processes those CORS request headers, and may respond with CORS response headers containing no actual content data. 
* The browser checks those CORS response headers to make sure that the CORS request is allowed. 
* If the browser sees that the intended resource request should be allowed by the server, it makes a second request to the browser with the intended HTTP method, whether `GET`, `POST`, `PUT`, etc., with the same CORS request headers.

Afterward, the communication between the browser and CORS origin (different than that of the webpage) will proceed as if it was a simple CORS request. Similar to a simple CORS request, content and resources are accessible and can be loaded if this second CORS request also is allowed.

## How to set up CORS at your origin
{: #how-to-set-up-cors-at-your-origin}

As shown in the previous diagrams, CORS is initiated by the requesting HTTP client. However, the effects depend on the requested origin. For your content to be ready for CORS requests, your origin must be configured correctly, to respond with the correct CORS response headers and the correct access permissions.

The following example shows a basic CORS configuration for an Nginx server:

```nginx
http {
    # some http context configs

    server {
        # some server context configs

        # URI path to some content
        location /my-static-content {
        
            # some location context configs
        
            # Handle simple requests
            #
            # Consider only "HTTP GET" requests (content fetching)
            if ($request_method = 'GET') {
                
                # Allows the browser to access data from this server during CORS,
                # only if the request comes from a webpage from the following origin
                add_header 'Access-Control-Allow-Origin' 'https://www.example.com';
            }
            
            # Handle preflight requests
            if ($request_method = 'OPTIONS') {
                
                # Allows the browser to access data from this server during CORS,
                # only if the request comes from a webpage from the following origin
                add_header 'Access-Control-Allow-Origin' 'https://www.example.com';
                
                # Allows only GET requests 
                add_header 'Access-Control-Allow-Methods' 'GET';
                
                # Allows the following headers in the browser's request headers
                # that may have been added by anything other than what the browser had added automatically
                add_header 'Access-Control-Allow-Headers' 'pragma';
                
                # Specifies to the browser how long it should cache this preflight response
                add_header 'Access-Control-Max-Age' 1728000;
                
                # HTTP 204 response code means success,
                # but also that it should expect no content from this (preflight) response
                return 204;
            }
            
            # more location context configs
            
            # If it is not a complex CORS situation requiring a preflight response,
            # then finally attempt to serve the file whose path falls under this location block's URI path match
            #
            # If no such file is found, then present an HTTP 404 code (not found)
            try_files $uri =404;
        }

        # more server context configs
    }

    # more http context configs
}
```
{: screen}

Generally, the browser should freely load content when it sees a `Access-Control-Allow-Origin: *` in the server's CORS response headers according to [w3 specification regarding that wildcard value ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.w3.org/TR/cors/#security). However, not all browsers support `Access-Control-Allow-Origin: *`.

If the server must support access from multiple webpages, each served from a different origin, a single origin value for `Access-Control-Allow-Origin` should be dynamically generated per request. The following is a basic example of such a use case for an Nginx server:

```nginx
http {
    # some http context config

    ######
    # Input from $http_origin,          the value in Origin HTTP header
    # Output to $cors_allowed_origin,   a string
    #
    # Full string match on the Origin header value
    #
    # Attempt to find a match for value from $http_origin using regex on left column.
    # If a match is found, then evaluate $cors_allowed_origin to the same value as $http_origin
    # Else, default evaluate $cors_allowed_origin to an empty string, so browsers will disallow the CORS request
    ######
    map $http_origin $cors_allowed_origin {
        default '';
        ~^http(s)?://(www|www2|cdn|dev)\.example\.com$ $http_origin;
    }
    
    server {
        # some server context configs

        # URI path to some content
        location /my-static-content {
        
            # some location context configs
        
            # Handle simple requests
            if ($request_method = 'GET') {
                add_header 'Access-Control-Allow-Origin' $cors_allowed_origin;
            }
            
            # Handle preflight requests
            if ($request_method = 'OPTIONS') {
                add_header 'Access-Control-Allow-Origin' '$cors_allowed_origin;
                add_header 'Access-Control-Allow-Methods' 'GET';
                add_header 'Access-Control-Allow-Headers' 'pragma';
                add_header 'Access-Control-Max-Age' 1728000;
                
                return 204;
            }
            
            # more location context configs

            try_files $uri =404;
        }

        # more server context configs
    }

    # more http context configs
}
```
{: screen}

The previous example uses the `map` directive, to avoid overusing the `if` Nginx statement. Now, when a CORS request is made to this server and matches that URI path, the server responds with the `Access-Control-Allow-Origin` header containing the value `http://www.example.com`, `https://cdn.example.com`, or `http://dev.example.com`, etc. when the content is requested from `http://www.example.com`, `https://cdn.example.com`, `http://dev.example.com`, and so forth.

## How to set up CORS for CDN
{: #how-to-set-up-cors-for-cdn}

![cors-through-cdn](/images/cors-through-cdn.png)
CDN is largely transparent to the CORS setup of the Origin, so it does not require a specific CDN configuration. If the CDN edge cannot find a cached response for the first request for some content, it forwards the request to the Origin Host. If the Origin Host is set up to handle CORS requests and this request has the `Origin` header, then it should respond back to the edge with a CORS header of `Access-Control-Allow-Origin` and the associated value. The overall response, including that header and value, will be cached in the CDN. Any subsequent request for the object at the same URI path is served from the cache and includes the `Access-Control-Allow-Origin` header value that was originally received from the Origin.

## Troubleshooting CORS and CORS requests
{: #troubleshooting-cors-and-cors-requests}

If your Origin server is set up for CORS and you do not see the `Access-Control-Allow-Origin` header returned to the browser’s request, it is possible that the response header cached in CDN was for a request that did not have the Origin header in the request. CDN caches response headers from the Origin Host. However, the cached headers are based on the request that triggered the request to Origin. In that case, the response headers may not include the CORS headers. Please clear the cache in CDN for that path using CDN’s Purge functionality, and re-try the request from the client.

The `Vary` response header from your Origin server may also cause unexpected behavior when fetching content through your CDN. Other than one very specific exception, the CDN does not cache content (and its associated response header) from your Origin server, if the server responds with a `Vary` header. Currently, our service removes the Vary header from the Origin to render the object cacheable, if the object is one of the following file types: `aif, aiff, au, avi, bin, bmp, cab, carb, cct, cdf, class, css, doc, dcr, dtd, exe, flv, gcf, gff, gif, grv, hdml, hqx, ico, ini, jpeg, jpg, js, mov, mp3, nc, pct, pdf, png, ppc, pws, swa, swf, txt, vbs, w32, wav, wbmp, wml, wmlc, wmls, wmlsc, xsd, zip, webp, jxr, hdp, wdp, pict, tif, tiff, mid, midi, ttf, eot, woff, otf, svg, svgz, jar, woff2, json`. If the object to cache is not one of these file types, please remove the `Vary` header from the Origin server's response for that object and try again after using CDN's Purge functionality.

## More information
{: #more-information}

* [https://www.w3.org/TR/cors/ ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.w3.org/TR/cors/)
* [https://w3c.github.io/webappsec-cors-for-developers/ ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://w3c.github.io/webappsec-cors-for-developers/)
