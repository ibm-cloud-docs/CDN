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

# CDN을 통한 CORS 및 CORS 요청
{: #cors-and-cors-requests-through-your-cdn}

CORS(Cross Origin Resource Sharing)는 주로 다른 원본으로부터 컨텐츠에 대한 액세스 권한을 검증하기 위해 브라우저에서 사용되는 메커니즘입니다.

## CORS의 개념
{: #what-is-cors}

브라우저가 웹 페이지를 로드할 때 **동일 출처 정책**을 적용합니다. 즉, 웹 페이지와 동일한 원본에서만 컨텐츠가 페치되도록 허용합니다. 그러나 경우에 따라서 웹 페이지가 해당 웹 사이트를 신뢰하는 다중 원본에서 자산에 대한 액세스를 필요로 할 수 있습니다. 이 경우, CORS가 사용됩니다. 

애플리케이션이 보유하고 있거나 HTTP 클라이언트이며 해당 애플리케이션에 CORS가 구현된 경우에만 이 보안 메커니즘이 존재합니다. Chrome, Firefox 및 Safari 등의 거의 모든 현대 브라우저는 CORS를 구현합니다.

명확히 하자면, **CORS와 관련하여 원본이 CDN 원본과 동일할 필요는 없습니다**. CORS의 원본은 URI 스킴, 도메인 및 가능한 임의의 포트 번호에 의해 정의됩니다. 예를 들어, `https://www.example.com:1443`은 `http://www.example.com`과 원본이 다릅니다. 따라서 CDN 또한 브라우저 퍼스펙티브에서 CORS 원본으로 간주될 수 있습니다.

## CORS가 작동하는 방법

CORS는 _단순 요청_ 및 _사전 요청_이라는 두 가지 유형의 요청을 처리할 수 있으며 후자가 더 복잡합니다.

### 단순 요청
{: #simple-requessts}

**첫 번째 요청(자원 액세스):**

![cors-simple](/images/cors-simple.png)

[CORS를 통한 단순 요청 ![E외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.w3.org/TR/cors/#simple-cross-origin-request)은 자원에 대한 또 다른 원본의 URL에 대한 액세스를 얻으려고 시도하는 한 원본의 웹 페이지로부터의 `GET` 또는 `POST` 요청입니다.

해당 요청을 만들 때, 브라우저는 자동으로 CORS 요청 헤더를 설정합니다. 주로, 자동으로 `Origin` HTTP 헤더에서 요청을 만들어 웹 페이지의 원본을 설정합니다. 또한 CORS 요청은 브라우저의 퍼스펙티브로부터 상태를 단순 CORS 요청으로 유지하면서 일련의 [특정한 표준 HTTP 헤더 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.w3.org/TR/cors/#simple-header)를 포함할 수 있습니다.

CORS 요청을 수신하는 서버는 요청을 처리하고 요청된 컨텐츠와 함께 일련의 CORS 응답 헤더를 다시 브라우저로 전송하거나 전송하지 않습니다. 이러한 CORS 응답 헤더는 현재 웹 페이지가 해당 리소스에 액세스하도록 허용되는지 여부, 전송된 헤더가 해당 요청에 대해 허용되는지 여부 등을 지정하는 값을 포함합니다.

브라우저가 CORS 응답 헤더에 의해 충족된 CORS 요청을 파악할 수 없으면 자동으로 컨텐츠에 대한 액세스 및 컨텐츠 로드를 금지합니다. 그렇지 않으면 CORS 원본이 리소스를 사용할 수 있는 지정된 권한이 있는지 파악하고 요청된 컨텐츠에 대한 액세스 및 컨텐츠 로드를 허용합니다.

### 사전 요청
{: #preflighted-requests}

**첫 번째 요청(사전):**

![cors-preflight](/images/cors-preflight.png)

두 번째 요청(자원 액세스):

![cors-after-preflight](/images/cors-after-preflight.png)

브라우저와 요청 웹 페이지와 다른 CORS 원본 사이의 복합 CORS 통신의 경우, 실제 리소스 액세스 전에 [사전 요청![외부 링크 아이콘](../../icons/launch-glyph.svg "External link icon")](https://www.w3.org/TR/cors/#cross-origin-request-with-preflight-0)이 필요합니다. 특정한 상황에서는 `GET` 또는 `POST` 메소드가 아닌 HTTP 메소드 등의 사전 CORS 요청이 필요하거나 `GET` 또는 `POST` 요청이라도 요청과 함께 비표준 HTTP 헤더를 사용해야 할 수 있습니다.

사전 요청이 필요한 경우, 이벤트가 전개되는 방식은 다음과 같습니다.

* 브라우저가 모든 대상 CORS 요청 헤더가 있는 서버에 HTTP OPTIONS 메소드를 사용하여 요청을 전송합니다. 
* 서버가 해당 CORS 요청 헤더를 처리하고 실제 컨텐츠 데이터를 포함하지 않는 CORS 요청 헤더를 사용하여 응답할 수 있습니다. 
* 브라우저가 해당 CORS 요청 헤더를 검사하여 CORS 요청이 허용되는지 여부를 확인합니다. 
* 브라우저가 대상 리소스 요청이 서버에 의해 허용되어야 하는 것으로 파악하면 동일한 CORS 요청 헤더를 사용하여 `GET`, `POST`, `PUT` 등의 대상 HTTP 메소드가 있는 브라우저에 두 번째 요청을 작성합니다.

그런 다음, 브라우저 및 (웹 페이지와 다른) CORS 원본 사이의 통신이 단순 CORS 요청인 것처럼 진행됩니다. 단순 CORS 요청과 유사하게 이 두 번째 CORS 요청도 허용되면 컨텐츠 및 리소스에 액세스할 수 있으며 로드할 수 있습니다.

## 원본에서 CORS를 설정하는 방법
{: #how-to-set-up-cors-at-your-origin}

이전 다이어그램에서 보듯이 CORS는 요청 HTTP 클라이언트에 의해 시작됩니다. 그러나 영향은 요청된 원본에 의해 결정됩니다. 컨텐츠가 CORS 요청에 대해 준비되려면 원본이 올바르게 구성되어야 하며 올바른 CORS 응답 헤더 및 올바른 액세스 권한을 사용하여 응답해야 합니다.

다음 예에서는 Nginx 서버에 대한 기본 CORS 구성을 보여줍니다.

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

일반적으로 [와일드카드 값에 관한 w3 스펙 ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.w3.org/TR/cors/#security)에 따라 브라우저가 서버의 CORS 응답 헤더에서 `Access-Control-Allow-Origin: *`를 발견하면 컨텐츠를 자유롭게 로드할 수 있어야 합니다. 그러나 모든 브라우저가 `Access-Control-Allow-Origin: *`를 지원하지는 않습니다.

서버가 각기 다른 원본에서 수행되는 다중 웹 페이지에서의 액세스를 지원해야 하는 경우, `Access-Control-Allow-Origin`에 대한 단일 원본 값이 요청마다 동적으로 생성되어야 합니다. 다음은 Nginx 서버에 대한 해당 유스 케이스의 기본 예입니다.

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

이전 예에서는 `if` Nginx 명령문의 남용을 피하기 위해 `map` 지시문을 사용합니다. 이제, CORS 요청이 이 서버에 작성되고 해당 URI 경로가 일치하면 `http://www.example.com`, `https://cdn.example.com`, `http://dev.example.com` 등에서 컨텐츠가 요청될 때 `http://www.example.com`, `https://cdn.example.com` 또는 `http://dev.example.com` 등의 값을 포함하는 `Access-Control-Allow-Origin` 헤더를 사용하여 응답합니다.

## CDN용 CORS를 설정하는 방법
{: #how-to-set-up-cors-for-cdn}

![cors-through-cdn](/images/cors-through-cdn.png)
CDN은 대개 원본의 CORS 설정에 대해 투명하므로 특정 CDN 구성이 필요하지 않습니다. CDN 에지가 일부 컨텐츠의 첫 번째 요청에 대해 캐싱된 응답을 찾지 못하는 경우, 요청을 원본 호스트로 전달합니다. 원본 호스트가 CORS 요청을 처리하도록 설정되고 이 요청에 `Origin` 헤더가 있는 경우, `Access-Control-Allow-Origin`의 CORS 헤더 및 연관된 값을 사용하여 에지로 다시 응답해야 합니다. 해당 헤더 및 값을 포함하는 전체 응답이 CDN에서 캐싱됩니다. 동일한 URI 경로에 있는 오브젝트에 대한 모든 후속 요청도 캐시에서 수행되고 원래 원본에서 수신된 `Access-Control-Allow-Origin` 헤더 값이 포함됩니다.

## CORS 및 CORS 요청 문제점 해결
{: #troubleshooting-cors-and-cors-requests}

원본 서버가 CORS용으로 설정되었으나 `Access-Control-Allow-Origin` 헤더가 브라우저의 요청으로 리턴되지 않는 경우, CDN에서 캐시된 응답 헤더가 요청에 원본 헤더가 없는 요청에 대한 것일 수 있습니다. CDN은 원본 호스트로부터 응답 헤더를 캐싱합니다. 그러나 캐싱된 헤더는 원본에 대한 요청을 트리거한 요청을 기반으로 합니다. 이런 경우, 응답 헤더가 CORS 헤더를 포함하지 않을 수 있습니다. CDN의 제거 기능을 사용하여 해당 경로에 대한 CDN에서 캐시를 지우고 클라이언트로부터 요청을 재시도하십시오.

CDN을 통해 컨텐츠를 페치할 때 원본 서버의 `Vary` 응답 헤더가 예상치 못한 동작을 발생시킬 수 있습니다. 한 가지 매우 특수한 예외를 제외하고는 서버가 `Vary` 헤더를 사용하여 응답할 때 CDN이 원본 서버에서 컨텐츠 및 연관된 응답 헤더를 캐싱하지 않습니다. 현재, 이 서비스에 관해 오브젝트가 다음 파일 유형 중 하나인 경우, 오브젝트를 캐싱할 수 있도록 렌더링하기 위해 원본에서 Vary 헤더를 제거하고 있습니다. `aif, aiff, au, avi, bin, bmp, cab, carb, cct, cdf, class, css, doc, dcr, dtd, exe, flv, gcf, gff, gif, grv, hdml, hqx, ico, ini, jpeg, jpg, js, mov, mp3, nc, pct, pdf, png, ppc, pws, swa, swf, txt, vbs, w32, wav, wbmp, wml, wmlc, wmls, wmlsc, xsd, zip, webp, jxr, hdp, wdp, pict, tif, tiff, mid, midi, ttf, eot, woff, otf, svg, svgz, jar, woff2, json`. 캐싱할 오브젝트가 해당 파일 유형 중 하나가 아닌 경우, 해당 오브젝트에 대한 원본 서버의 응답에서 `Vary` 헤더를 제거하고 CDN의 제거 기능을 사용한 후에 다시 시도하십시오.

## 자세한 정보
{: #more-information}

* [https://www.w3.org/TR/cors/ ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.w3.org/TR/cors/)
* [https://w3c.github.io/webappsec-cors-for-developers/ ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://w3c.github.io/webappsec-cors-for-developers/)
