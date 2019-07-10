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

# CORS および CDN からの CORS 要求
{: #cors-and-cors-requests-through-your-cdn}

Cross Origin Resource Sharing (CORS) は、主として、異なるオリジンからコンテンツへのアクセスの許可を検証するためにブラウザーによって使用されるメカニズムです。

## CORS とは何ですか?
{: #what-is-cors}

ブラウザーは Web ページをロードするときに、**同一生成元ポリシー**を適用します。これは、Web ページと同じオリジンからのコンテンツの取り出しのみを許可することを意味します。 しかし、場合によっては、Web ページは、その Web サイトを信頼する複数のオリジンからのアセットにアクセスする必要があることがあります。 この場合に、CORS が使用されます。 

このセキュリティー・メカニズムは、アプリケーションが HTTP クライアントであるか、HTTP クライアントを持っている場合で、かつそのアプリケーションが CORS を実装している場合にのみ存在します。 Chrome、Firefox、および Safari など、ほとんどすべての最新ブラウザーは CORS を実装しています。

明確にしておくと、**CORS に関するオリジンは、CDN オリジンと同じである必要はありません**。 CORS のオリジンは、URI スキーム、ドメイン、および指定可能な任意のポート番号によって定義されます。 例えば、`https://www.example.com:1443` は、`http://www.example.com` とは異なるオリジンです。 したがって、CDN は、ブラウザーの観点から、CORS オリジンと見なすこともできます。

## CORS はどのように機能するか

CORS は、_単純要求_と、より複雑な _プリフライト要求_という 2 つのタイプの要求を処理できます。

### 単純要求
{: #simple-requessts}

**最初の要求 (リソース・アクセス):**

![cors-simple](/images/cors-simple.png)

CORS を介した[単純要求 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.w3.org/TR/cors/#simple-cross-origin-request)は、リソースの別のオリジンの URL にアクセスしようとしているあるオリジンの Web ページからの `GET` 要求または `POST` 要求のいずれかになります。

そのような要求を行っているとき、ブラウザーは自動的に CORS 要求ヘッダーを設定します。 主として、要求を行っている Web ページのオリジンを `Origin` HTTP ヘッダーに自動的に設定します。 また、CORS 要求は、ブラウザーの観点から、単純 CORS 要求としての状況を維持しながら、[特定の標準 HTTP ヘッダー ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.w3.org/TR/cors/#simple-header)のセットを含む場合もあります。

CORS 要求を受信したサーバーは、その要求を処理し、要求されたコンテンツとともに CORS 応答ヘッダーのセットをブラウザーに送信する (または送信しない) 可能性があります。 これらの CORS 応答ヘッダーは、現在の Web ページがそれらのリソースへのアクセスを許可されるかどうか、送信されたヘッダーがその要求に対して受け入れ可能かどうかなどを指定する値を含んでいます。

ブラウザーは、CORS 応答ヘッダーによってその CORS 要求が満たされていることを確認できないと、コンテンツへのアクセス、およびコンテンツのロードを自動的に阻止します。 そうでない場合は、CORS オリジンがリソースの使用許可を与えていることを確認して、要求されたコンテンツへのアクセスおよびコンテンツのロードを許可します。

### プリフライト要求
{: #preflighted-requests}

**最初の要求 (プリフライト):**

![cors-preflight](/images/cors-preflight.png)

2 番目の要求 (リソース・アクセス):

![cors-after-preflight](/images/cors-after-preflight.png)

ブラウザーと、要求している Web ページとは異なる CORS オリジンの間の、より複雑な CORS 通信の場合は、実際のリソース・アクセスの前に[プリフライト要求 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.w3.org/TR/cors/#cross-origin-request-with-preflight-0) が必要になります。 特定の状況では、`GET` メソッドでも `POST` メソッドでもない HTTP メソッドなどの CORS 要求をプリフライトしたり、`GET` 要求または `POST` 要求であったとしても、要求に標準以外の HTTP ヘッダーを使用したりすることが必要になる可能性があります。

プリフライト要求が必要な場合に、どのようにイベントが展開されていくのかを以下に示します。

* ブラウザーが、HTTP OPTIONS メソッドを使用し、意図したすべての CORS 要求ヘッダーを含めた要求をサーバーに送信します。 
* サーバーは、それらの CORS 要求ヘッダーを処理し、実際のコンテンツ・データを含まない CORS 応答ヘッダーで応答する可能性があります。 
* ブラウザーはそれらの CORS 応答ヘッダーを検査して、CORS 要求が許可されることを確認します。 
* ブラウザーは、意図したリソース要求がサーバーによって許可されることを確認すると、同じ CORS 要求ヘッダーを使用して、意図した HTTP メソッド (`GET`、`POST`、`PUT` などのいずれでも) でブラウザーに 2 番目の要求を行います。

その後、ブラウザーと CORS オリジン (Web ページのオリジンとは異なる) の間の通信は、単純 CORS 要求であるかのように処理されます。 この 2 番目の CORS 要求も許可される場合、単純 CORS 要求と同様に、コンテンツとリソースはアクセス可能であり、ロードできます。

## オリジンでの CORS のセットアップ方法
{: #how-to-set-up-cors-at-your-origin}

前の図に示されているように、CORS は、要求側の HTTP クライアントによって開始されます。 ただし、その効果は、要求されたオリジンによって決まります。 コンテンツを CORS 要求対応にするには、正しい CORS 応答ヘッダーと正しいアクセス許可を使用して応答するようにオリジンが正しく構成されている必要があります。

Nginx サーバーの基本 CORS 構成の例を以下に示します。

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

[w3 specification regarding that wildcard value ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.w3.org/TR/cors/#security) によると、通常、ブラウザーは、サーバーの CORS 応答ヘッダーで `Access-Control-Allow-Origin: *` を確認すると、自由にコンテンツをロードできるはずです。 ただし、必ずしもすべてのブラウザーが `Access-Control-Allow-Origin: *` をサポートしているわけではありません。

サーバーが、それぞれ異なるオリジンから提供される複数の Web ページからのアクセスをサポートする必要がある場合、`Access-Control-Allow-Origin` の単一オリジン値は要求ごとに動的に生成される必要があります。 Nginx サーバーでのそのようなユース・ケースの基本例を以下に示します。

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

前の例では、`if` Nginx ステートメントの過剰使用を避けるために、`map` ディレクティブが使用されています。CORS 要求がこのサーバーに対して行われ、その URI パスに一致すると、`http://www.example.com`、`https://cdn.example.com`、または `http://dev.example.com` などからコンテンツが要求されたとき、サーバーは、`http://www.example.com`、`https://cdn.example.com`、`http://dev.example.com` などの値を含む `Access-Control-Allow-Origin` ヘッダーで応答します。

## CDN 用の CORS のセットアップ方法
{: #how-to-set-up-cors-for-cdn}

![cors-through-cdn](/images/cors-through-cdn.png)
CDN はオリジンの CORS セットアップからほとんど認識されないため、特定の CDN 構成は必要ありません。 CDN エッジは、あるコンテンツの最初の要求に対して、キャッシュされた応答を見つけられない場合、要求をオリジン・ホストに転送します。 オリジン・ホストが CORS 要求を処理するようにセットアップされており、この要求に `Origin` ヘッダーがある場合、オリジン・ホストは `Access-Control-Allow-Origin` の CORS ヘッダーと関連値を使用してエッジに応答するはずです。 ヘッダーと値を含めて応答全体が CDN にキャッシュされます。 同じ URI パスのオブジェクトに対する後続の要求は、このキャッシュから提供され、もともとオリジンから受信された `Access-Control-Allow-Origin` ヘッダー値を含みます。

## CORS および CORS 要求のトラブルシューティング
{: #troubleshooting-cors-and-cors-requests}

オリジン・サーバーが CORS 用にセットアップされており、ブラウザーの要求に対して `Access-Control-Allow-Origin` ヘッダーが返されない場合、CDN にキャッシュされている応答ヘッダーは、要求内に Origin ヘッダーを持っていなかった要求用のものだった可能性があります。 CDN は、オリジン・ホストからの応答ヘッダーをキャッシュに入れます。 しかし、キャッシュされたヘッダーは、オリジンへの要求をトリガーした要求に基づきます。 その場合、応答ヘッダーに、CORS ヘッダーが含まれていない可能性があります。 CDN のパージ機能を使用してそのパスの CDN 内のキャッシュをクリアし、クライアントから要求を再試行してください。

オリジン・サーバーからの `Vary` 応答ヘッダーも、CDN からコンテンツを取得するときに、予期しない動作を引き起こす可能性があります。 サーバーが `Vary` ヘッダーを使用して応答した場合、CDN は、1 つの非常に特定の例外を除き、オリジン・サーバーからのコンテンツ (およびその関連応答ヘッダー) をキャッシュに入れません。 現在、オブジェクトが次のいずれかのファイル・タイプの場合、サービスは、Vary ヘッダーをオリジンから削除して、オブジェクトをキャッシング可能にします: `aif, aiff, au, avi, bin, bmp, cab, carb, cct, cdf, class, css, doc, dcr, dtd, exe, flv, gcf, gff, gif, grv, hdml, hqx, ico, ini, jpeg, jpg, js, mov, mp3, nc, pct, pdf, png, ppc, pws, swa, swf, txt, vbs, w32, wav, wbmp, wml, wmlc, wmls, wmlsc, xsd, zip, webp, jxr, hdp, wdp, pict, tif, tiff, mid, midi, ttf, eot, woff, otf, svg, svgz, jar, woff2, json`。 キャッシュするオブジェクトがこれらのどのファイル・タイプでもない場合は、そのオブジェクトに対するオリジン・サーバーの応答から `Vary` ヘッダーを削除して、CDN のパージ機能を使用した後に再試行してください。

## 詳細情報
{: #more-information}

* [https://www.w3.org/TR/cors/ ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.w3.org/TR/cors/)
* [https://w3c.github.io/webappsec-cors-for-developers/ ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://w3c.github.io/webappsec-cors-for-developers/)
