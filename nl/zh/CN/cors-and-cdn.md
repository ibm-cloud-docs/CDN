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

# 通过 CDN 的 CORS 和 CORS 请求
{: #cors-and-cors-requests-through-your-cdn}

跨源资源共享 (CORS) 是浏览器使用的一种机制，主要用于验证从不同源访问内容的权限。

## 什么是 CORS？
{: #what-is-cors}

在浏览器装入 Web 页面时，会强制实施**同源策略**，也就是说，只允许访存同源的内容作为 Web 页面。但在某些情况下，Web 页面需要从信任该 Web 页面的多个源访问资产。此时就会应用 CORS。 

只有在应用程序具有 HTTP 客户机或本身就是 HTTP 客户机，并且该应用程序实现 CORS 时，才存在此安全性机制。几乎所有的现代浏览器（如 Chrome、Firefox 和 Safari）都实现 CORS。

再次重申一下，**针对 CORS 的源并非必须跟 CDN 源相同**。CORS 中的源是由 URI 方案、域以及任意可能的端口号定义的。例如，`https://www.example.com:1443` 是不同于 `http://www.example.com` 的源。此外，CDN 还可以被视为来自浏览器透视图的 CORS 源。

## CORS 的工作原理

CORS 可以处理两种类型的请求：_简单请求_和_预检请求_，这些请求更复杂。

### 简单请求
{: #simple-requessts}

**第一个请求（资源访问）：**

![cors-simple](/images/cors-simple.png)

通过 CORS 的[简单请求 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.w3.org/TR/cors/#simple-cross-origin-request) 是来自某个源的 Web 页面的 `GET` 或 `POST` 请求，该源尝试获取对资源的另一个源的 URL 的访问权。

在发出此类请求时，浏览器会自动设置 CORS 请求头。主要是在 `Origin` HTTP 头中自动设置发出请求的 Web 页面的源。CORS 请求中也可能包含一组[特定的标准 HTTP 头 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.w3.org/TR/cors/#simple-header)，同时在浏览器透视图中将其状态保持为简单 CORS 请求。

收到 CORS 请求的服务器将处理请求，而且可能会（也可能不会）将一组 CORS 响应头以及所请求的内容发送回浏览器。这些 CORS 响应头中包含的值指定是否允许当前 Web 页面访问那些资源，发送的头是不是该请求可接受的，等等。

如果浏览器看到 CORS 响应头无法满足 CORS 请求，就会自动阻止访问和加载内容。否则，就会看到 CORS 源授予使用资源的许可权，并允许访问和加载请求的内容。

### 预检请求
{: #preflighted-requests}

**第一个请求（预检）：**

![cors-preflight](/images/cors-preflight.png)

第二个请求（资源访问）：

![cors-after-preflight](/images/cors-after-preflight.png)

有关浏览器及与请求 Web 页面不同的 CORS 源之间更复杂的 CORS 通信，需要在进行实际资源访问之前先发出[预检请求 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.w3.org/TR/cors/#cross-origin-request-with-preflight-0)。在特定情况下，可能需要预检 CORS 请求，如不是 `GET` 或 `POST` 方法的 HTTP 方法，或将非标准 HTTP 头用于该请求（即使是 `GET` 或 `POST` 请求），等等。

如果需要预检请求，下面是具体操作方法：

* 浏览器使用 HTTP OPTIONS 方法将请求以及所有目标 CORS 请求头发送到服务器。 
* 服务器处理这些 CORS 请求头，并且可能以不包含实际内容数据的 CORS 响应头进行响应。 
* 浏览器检查这些 CORS 响应头，以确保允许该 CORS 请求。 
* 如果浏览器看到服务器应该允许目标资源请求，就会用目标 HTTP 方法向浏览器发出另一个请求（`GET`、`POST`、`PUT` 等），并带有相同的 CORS 请求头。

然后，浏览器与 CORS 源（不同于 Web 页面）之间的通信会继续，就像处理简单 CORS 请求一样。与简单 CORS 请求类似的是，如果第二个 CORS 请求也允许的话，内容和资源可访问并且可装入。

## 如何在源位置设置 CORS
{: #how-to-set-up-cors-at-your-origin}

如上图中所示，CORS 是由请求 HTTP 客户机启动的。但具体影响取决于被请求的源。为了让内容为 CORS 请求做好准备，必须正确配置源，才能以正确的 CORS 响应头和正确的访问权限进行响应。

以下示例显示 Nginx 服务器的基本 CORS 配置：

```nginx
http {
    # 一些 http 上下文配置

    server {
      # 一些服务器上下文配置

        # 到某些内容的 URI 路径
        location /my-static-content {

            # 一些位置上下文配置

            # 处理简单请求
            #
            # 仅考虑“HTTP GET”请求（内容访存）
            if ($request_method = 'GET') {

                # 在 CORS 期间允许浏览器访问此服务器的数据，
                # 仅当请求来自以下源的 Web 页面时
                add_header 'Access-Control-Allow-Origin' 'https://www.example.com';
            }

            # 处理预检请求
            if ($request_method = 'OPTIONS') {

                # 在 CORS 期间允许浏览器访问此服务器的数据，
                # 仅当请求来自以下源的 Web 页面时
                add_header 'Access-Control-Allow-Origin' 'https://www.example.com';

                # 仅允许 GET 请求
                add_header 'Access-Control-Allow-Methods' 'GET';

                # 允许在浏览器的请求头中使用下列头
                # 可能是除浏览器已自动添加的请求头以外，任何对象所添加的请求头
                add_header 'Access-Control-Allow-Headers' 'pragma';

                # 指定浏览器应高速缓存此预检响应多长时间
                add_header 'Access-Control-Max-Age' 1728000;

                # HTTP 204 响应代码表示成功，
                # 但也表示预期此（预检）响应不会有任何内容
                return 204;
            }

            # 其他位置上下文配置

            # 如果不是需要预检响应的复杂 CORS 情况，
            # 那么最后尝试提供路径在此位置块的 URI 路径下匹配的文件
            #
            # 如果找不到这样的文件，那么会显示 HTTP 404 代码（找不到）
            try_files $uri =404;
        }

        # 其他服务器上下文配置
    }

    # 其他 http 上下文配置
}
```
{: screen}

一般情况下，当浏览器在服务器的 CORS 响应头中看到 `Access-Control-Allow-Origin: *` 时，应根据[关于通配符值的 w3 规范 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.w3.org/TR/cors/#security) 自由装入内容。但不是所有的浏览器都支持 `Access-Control-Allow-Origin: *`。

如果服务器必须支持来自多个 Web 页面的访问，那么将从不同的源提供每个页面，`Access-Control-Allow-Origin` 的单个源值应按照请求动态生成。下面是 Nginx 服务器此类用例的基本示例：

```nginx
http {
    # 一些 http 上下文配置

    ######
    # 来自 $http_origin 的输入，          源 HTTP 头中的值
    # 到 $cors_allowed_origin 的输出，   字符串
    #
    # 源头值上的完整字符串匹配
    #
    # 尝试使用左侧列上的正则表达式来查找 $http_origin 中的匹配值。
    # 如果找到了匹配项，那么将 $cors_allowed_origin 求值为与 $http_origin 相同的值
    # 此外，将 $cors_allowed_origin 缺省求值为空字符串，这样浏览器就会禁止 CORS 请求
    ######
    map $http_origin $cors_allowed_origin {
        default '';
        ~^http(s)?://(www|www2|cdn|dev)\.example\.com$ $http_origin;
    }
    
    server {
      # 一些服务器上下文配置

        # 到某些内容的 URI 路径
        location /my-static-content {

            # 一些位置上下文配置

            # 处理简单请求
            if ($request_method = 'GET') {
                add_header 'Access-Control-Allow-Origin' $cors_allowed_origin;
            }

            # 处理预检请求
            if ($request_method = 'OPTIONS') {
                add_header 'Access-Control-Allow-Origin' '$cors_allowed_origin;
                add_header 'Access-Control-Allow-Methods' 'GET';
                add_header 'Access-Control-Allow-Headers' 'pragma';
                add_header 'Access-Control-Max-Age' 1728000;
                
                return 204;
            }

            # 其他位置上下文配置

            try_files $uri =404;
        }

        # 其他服务器上下文配置
    }

    # 其他 http 上下文配置
}
```
{: screen}

在前面的示例中，`map` 伪指令用于避免过度使用 `if` Nginx 语句。现在，向此服务器发出 CORS 请求并且该请求匹配该 URI 路径时，如果是向 `http://www.example.com`、`https://cdn.example.com`、`http://dev.example.com` 等请求内容，服务器会以包含 `http://www.example.com`、`https://cdn.example.com` 或 `http://dev.example.com` 等值的 `Access-Control-Allow-Origin` 头进行响应。

## 如何针对 CDN 设置 CORS
{: #how-to-set-up-cors-for-cdn}

![cors-through-cdn](/images/cors-through-cdn.png)
CDN 对源的 CORS 设置基本是透明的，所以不需要特定的 CDN 配置。如果 CDN 边缘找不到针对某些内容的第一个请求的高速缓存的响应，那么会将请求转发给源主机。如果源主机设置为处理 CORS 请求，并且此请求有 `Origin` 头，那么它应以 CORS 头 `Access-Control-Allow-Origin` 和关联的值来返回响应给边缘服务器。总体响应（包括该头和值）将高速缓存在 CDN 中。在相同 URI 路径中针对该对象的任何后续请求，都会从高速缓存提供服务，并且包含最初从源接收的 `Access-Control-Allow-Origin` 头值。

## 关于 CORS 和 CORS 请求的故障诊断信息
{: #troubleshooting-cors-and-cors-requests}

如果源服务器进行了针对 CORS 的设置，但看不到返回给浏览器请求的 `Access-Control-Allow-Origin` 头，那么高速缓存在 CDN 中的响应头可能会是针对请求中没有源头的请求的。CDN 高速缓存来自源主机的响应头。但高速缓存的头所基于的请求是触发请求到源的请求。在这种情况下，响应头中可能不包含 CORS 头。请使用 CDN 的清除功能清除 CDN 中的针对该路径的高速缓存，然后从客户机重试该请求。

来自源服务器的 `Vary` 响应头也可能导致在通过 CDN 访存内容时发生异常行为。除了一个非常特定的异常以外，如果以 `Vary` 头响应，那么CDN 不会高速缓存来自源服务器的内容（及其关联的响应）。目前我们的服务将 Vary 头从源中除去，那么如果属于以下某种文件类型，就会呈现可高速缓存的对象：`aif, aiff, au, avi, bin, bmp, cab, carb, cct, cdf, class, css, doc, dcr, dtd, exe, flv, gcf, gff, gif, grv, hdml, hqx, ico, ini, jpeg, jpg, js, mov, mp3, nc, pct, pdf, png, ppc, pws, swa, swf, txt, vbs, w32, wav, wbmp, wml, wmlc, wmls, wmlsc, xsd, zip, webp, jxr, hdp, wdp, pict, tif, tiff, mid, midi, ttf, eot, woff, otf, svg, svgz, jar, woff2, json`。如果要高速缓存的对象不属于这些文件类型，请将 `Vary` 头从源服务器的针对该对象的响应中除去，然后在使用 CDN 的清除功能之后重试。

## 更多信息
{: #more-information}

* [https://www.w3.org/TR/cors/ ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.w3.org/TR/cors/)
* [https://w3c.github.io/webappsec-cors-for-developers/ ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://w3c.github.io/webappsec-cors-for-developers/)
