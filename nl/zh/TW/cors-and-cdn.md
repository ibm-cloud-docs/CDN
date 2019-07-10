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

# 透過 CDN 的 CORS 及 CORS 要求
{: #cors-and-cors-requests-through-your-cdn}

「跨原點資源共用 (CORS)」是瀏覽器所使用的機制，主要用來驗證從不同原點存取內容的許可權。

## 何謂 CORS？
{: #what-is-cors}

當瀏覽器載入網頁時，會施行**同源政策**，這表示其只容許從網頁的相同原點提取內容。然而，在一些情況下，網頁可能需要從信任該網站的多個原點，存取資產。這時便需要 CORS。 

只有在應用程式具有 HTTP 用戶端或者本身是 HTTP 用戶端，並且該應用程式實作 CORS 時，這個安全機制才存在。幾乎所有現代瀏覽器（例如，Chrome、Firefox 以及 Safari）都實作 CORS。

要澄清的一點是，**關於 CORS 的原點不必同於 CDN 原點**。CORS 中的原點由 URI 架構、網域以及任何可能的埠號所定義。例如，`https://www.example.com:1443` 這個原點與 `http://www.example.com` 不同。因此，從瀏覽器視景來看，CDN 也可以視為 CORS 原點。

## CORS 如何運作

CORS 可以處理兩種類型的要求：_簡單要求_ 以及較複雜的_預檢要求_。

### 簡單要求
{: #simple-requessts}

**第一個要求（資源存取）：**

![cors-simple](/images/cors-simple.png)

透過 CORS 的[簡單要求 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.w3.org/TR/cors/#simple-cross-origin-request)，是來自某原點之網頁的 `GET` 或 `POST` 要求，而該網頁嘗試取得另一個原點之 URL 的存取權，以便取得資源。

提出這類要求時，瀏覽器會自動設定 CORS 要求標頭。根本上，它會自動設定網頁的原點，在 `Origin` HTTP 標頭中提出要求。從瀏覽器的角度來看，CORS 要求也可包含一組[特定的標準 HTTP 標頭 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.w3.org/TR/cors/#simple-header)，同時將其狀態保持為簡單 CORS 要求。

收到 CORS 要求的伺服器會處理該要求，而且不一定會將一組 CORS 回應標頭以及所要求的內容傳回瀏覽器。這些 CORS 回應標頭所包含的值會指定是否容許目前的網頁存取那些資源、該要求是否接受所傳送的標頭等等。

如果瀏覽器無法看到 CORS 回應標頭滿足其 CORS 要求，則會自動防止存取及載入內容。否則，會看到 CORS 原點提供資源的使用許可權，並容許存取及載入所要求的內容。

### 預檢要求
{: #preflighted-requests}

**第一個要求（預檢）：**

![cors-preflight](/images/cors-preflight.png)

第二個要求（資源存取）：

![cors-after-preflight](/images/cors-after-preflight.png)

對於瀏覽器與 CORS 原點（不是發出要求的網頁）之間的較複雜 CORS 通訊，在實際資源存取之前，會要求[預檢要求 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.w3.org/TR/cors/#cross-origin-request-with-preflight-0)。特定狀況可能需要預檢 CORS 要求，例如，不是 `GET` 或 `POST` 方法的 HTTP 方法，或者非標準 HTTP 標頭與要求搭配使用 - 即使其為 `GET` 或 `POST` 要求，等等。

如果需要預檢要求，事件的發展如下所示：

* 瀏覽器使用 HTTP OPTIONS 方法，將含有所有想要之 CORS 要求標頭的要求傳送至伺服器。 
* 伺服器處理那些 CORS 要求標頭，並且可能以不含實際內容資料的 CORS 回應標頭進行回應。 
* 瀏覽器檢查那些 CORS 回應標頭，確定該 CORS 要求已被接受。 
* 如果瀏覽器認為伺服器應該接受想要的資源要求，便會使用想要的 HTTP 方法（`GET`、`POST`、`PUT` 等等），提出含有相同 CORS 要求標頭的第二個要求給瀏覽器。

之後，瀏覽器與 CORS 原點之間的通訊（不同於網頁的通訊）會繼續進行，就像是簡單 CORS 要求一樣。類似簡單 CORS 要求，如果這個第二個 CORS 要求也被接受，則可存取及載入內容與資源。

## 如何在您的原點設定 CORS
{: #how-to-set-up-cors-at-your-origin}

如之前圖表中所示，CORS 由發出要求的 HTTP 用戶端所起始。但是，結果取決於所要求的原點。為了讓您的內容準備好以用於 CORS 要求，您的原點必須正確配置，才能以正確的 CORS 回應標頭以及正確的存取許可權進行回應。

下列範例顯示 Nginx 伺服器的基本 CORS 配置：

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

一般而言，根據[關於萬用字元值的 w3 規格 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.w3.org/TR/cors/#security)，瀏覽器在伺服器的 CORS 回應標頭中看到 `Access-Control-Allow-Origin: *` 時，應可自由載入內容。然而，並非所有瀏覽器都支援 `Access-Control-Allow-Origin: *`。

如果伺服器必須支援從多個網頁中存取，且每個都從不同的原點提供，則應該針對每一個要求，以動態方式產生單一個 `Access-Control-Allow-Origin` 原點值。下列為 Nginx 伺服器的這類使用案例的基本範例：

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

前一個範例使用 `map` 指引來避免過度使用 `if` Nginx 陳述式。現在，對此伺服器提出一個 CORS 要求，並符合該 URI 路徑時，當內容要求自 `http://www.example.com`、`https://cdn.example.com`、`http://dev.example.com` 等等時，伺服器會以 `Access-Control-Allow-Origin` 標頭回應，該標頭包含 `http://www.example.com`、`https://cdn.example.com` 或 `http://dev.example.com` 等等的值。

## 如何設定 CDN 的 CORS
{: #how-to-set-up-cors-for-cdn}

![cors-through-cdn](/images/cors-through-cdn.png)
對「原點」的 CORS 設定而言，CDN 大多是透通的，因此它不需要特定的 CDN 配置。如果 CDN 邊緣找不到一些內容第一個要求的已快取回應，則會將要求轉遞給「原點主機」。如果「原點主機」設定為處理 CORS 要求，而且此要求具有 `Origin` 標頭，則原點主機應該以 CORS 標頭 `Access-Control-Allow-Origin` 及相關聯的值回應邊緣。整體回應（包括該標頭及值）都會快取在 CDN 中。該物件在相同 URI 路徑中旳所有後續要求都會從快取中提供，且會包含原本從「原點」接收到的 `Access-Control-Allow-Origin` 標頭值。

## 疑難排解 CORS 及 CORS 要求
{: #troubleshooting-cors-and-cors-requests}

如果您的「原點」伺服器已針對 CORS 進行設定，且您沒有看到 `Access-Control-Allow-Origin` 標頭傳回到瀏覽器的要求，則可能是因為在 CDN 中快取的回應標頭適用於在要求中沒有 Origin 標頭的要求。CDN 從「原點主機」快取回應標頭。然而，快取的標頭取決於向「原點」觸發要求的要求。在該案例中，回應標頭可能不包含 CORS 標頭。請使用 CDN 的「清除」功能來清除 CDN 中該路徑的快取，然後再從用戶端重試一次要求。

透過 CDN 提取內容時，您的「原點」伺服器中的 `Vary` 回應標頭也可能會造成非預期的行為。除了一個非常特定的異常狀況之外，如果伺服器以 `Vary` 標頭進行回應，則 CDN 不會從您的「原點」伺服器快取內容（及其相關聯回應標頭）。目前，我們的服務從「原點」移除 Vary 標頭，以在物件是下列其中一種檔案類型時，呈現該物件是可快取的：`aif、aiff、au、avi、bin、bmp、cab、carb、cct、cdf、class、css、doc、dcr、dtd、exe、flv、gcf、gff、gif、grv、hdml、hqx、ico、ini、jpeg、jpg、js、mov、mp3、nc、pct、pdf、png、ppc、pws、swa、swf、txt、vbs、w32、wav、wbmp、wml、wmlc、wmls、wmlsc、xsd、zip、webp、jxr、hdp、wdp、pict、tif、tiff、mid、midi、ttf、eot, woff、otf、svg、svgz、jar、woff2、json`。如果要快取的物件不是這些檔案類型中的其中一種，請從「原點」伺服器對於該物件的回應中移除 `Vary` 標頭，然後在使用 CDN 的「清除」功能之後，再試一次。

## 相關資訊
{: #more-information}

* [https://www.w3.org/TR/cors/ ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.w3.org/TR/cors/)
* [https://w3c.github.io/webappsec-cors-for-developers/ ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://w3c.github.io/webappsec-cors-for-developers/)
