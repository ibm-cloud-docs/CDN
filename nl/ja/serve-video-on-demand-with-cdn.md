---

copyright:
  years: 2018, 2019
lastupdated: "2019-04-04"

keywords: video, mp4, formats, MPEG, nginx, player, configuration, streaming, stream, files, demand, ffmpeg

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}


# CDN を使用したビデオ・オンデマンドの提供方法
{: #how-to-serve-video-on-demand-with-cdn}

このガイドでは、{{site.data.keyword.cloud}} を活用して、ビデオ・オンデマンドとして **HLS** を介して、`.mp4` コンテンツを Linux-Nginx オリジンからブラウザーにストリーミングする方法の例を探索します。 

## 概要
{: #introduction}

ビデオをストリーミングするために、HLS や MPEG-DASH などの多くのフォーマットが使用可能です。 

概念的に、使用するセットアップを以下の図に示します。

![ビデオ・オンデマンドに対する IBM Cloud CDN](images/ibmcdn-vod-example-model.png)

また、オリジンを準備の場所としても使用します。 その場合、これを機能させるためにいくつかのパッケージの取得も必要になります。

そこで、オリジンのパッケージ・リストの更新から開始しましょう。
```
$ sudo apt-get update
```

## ビデオ・ファイルの準備
{: #prepare-video-files}

このガイドでは、`ffmpeg` を使用してビデオ・ファイルを準備します。 これは、さまざまなコマンドを使用して変換、MUX (マルチプレクサー)、DEMUX (デマルチプレクサー)、フィルター操作、その他を実行できるマルチメディア・ファイル用の強力なツールです。

最初に、`ffmpeg` を取得します。
```
$ sudo apt-get -y install ffmpeg
```

HLS は、`.m3u8` および `.ts` という 2 つのタイプのファイルを処理します。 `.m3u8` ファイルは、「プレイ・リスト」と考えることができます。 ビデオ・ストリームの開始時、このファイルは最初に取り出されるファイルです。  次に、プレイ・リストが、取り出すべきビデオ・フラグメントについてビデオ・プレイヤーに通知し、ストリーミング処理されたコンテンツの正常な再生方法に関するその他のデータを提供します。 `.ts` ファイルは、ビデオ「フラグメント」です。 これらのフラグメントは、「プレイ・リスト」に指定された詳細情報に従って、ビデオ・プレイヤーによって取り出されて再生されます。

次に、ソースである `.mp4` ビデオのビデオ・ストリームとオーディオ・ストリームのフォーマット、ビット・レート、およびその他の情報を確認してみましょう。

```
$ ffprobe test-video.mp4 
```

この例では、`test-video.mp4` の以下のストリーム情報を検討してみましょう。
  * ビデオ・ストリーム 0
    * フォーマット: h264
    * フォーマット・プロファイル: High
    * 解像度: 1920x1080
    * ビット・レート: 438 kb/s
    * フレーム・レート: 30.30 fps
  * オーディオ・ストリーム 0
    * フォーマット: aac
    * サンプル・レート: 48000
    * ビット・レート: 128k

次に、`test-video.mp4` ファイルを HLS 用のフォーマットに変換します。

```
$ ffmpeg -i test-video.mp4 -c:a aac -ar 48000 -b:a 128k -c:v h264 -profile:v main -crf 23 -g 61 -keyint_min 61 -sc_threshold 0 -b:v 5300k -maxrate 5300k -bufsize 10600k -hls_time 6 -hls_playlist_type vod test-video.m3u8
```

このコマンドが実行した内容の明細を以下に示します。

|引数|効果|
|:---|:---|
| ffmpeg | `ffmpeg` ツールを使用する。 |
| -i test-video.mp4 | ソース・ビデオは `test-video.mp4` にある。 |
| -c:a acc | 出力で acc オーディオ・コーデックを使用する。 |
| -ar 48000 | 出力でオーディオ・サンプル・レートを 48000 Hz に設定する。 |
| -b:a 128k | 出力でオーディオ・ビットレートを 128000 ビット/秒に設定する。 |
| -c:v h264 | 出力で `h.264` ビデオ・コーデックを使用する。 |
| -profile:v main | 最も広範なデバイスをサポートするために、選択したコーデックの「メイン」フォーマット・プロファイルを使用する。 |
| -crf 23 | さまざまなファイル・サイズとビットレートでビデオ品質の維持を試行する。<br/>  CRF が低いほど、品質は高くなり、ファイル・サイズは大きくなります。 |
| -g 61 -keyint_min 61 | 最大および最小を設定する。<br/> サンプルのソース・フレーム・レートが 30.30 の場合、キーフレームは<br/> 2 秒ごとに挿入する必要があります (61 フレーム)。 |
| -sc_threshold 0 | `ffmpeg` によるシーン検出を無効にする。<br/> 2 番目のプロセスによって余分なキーフレームが出力に挿入されないようにします。 |
| -b:v 5300k | 出力ビデオ・ストリームのターゲット・ビットレートを 5300000 ビット/秒に設定する。 |
| -maxrate 5300k | 変化する場合に備えて、エンコーダーでの最大出力<br/>ビデオ・ビットレートを 5300000 ビット/秒に制限する。 |
| -bufsize 10600k | `ffmpeg` ビデオ・デコーダーのバッファー・サイズを 10600000 ビットに設定する。<br/>  5300k ビットレートの場合、`ffmpeg` エンコーダーは、2 秒ごとに<br/>ビデオの出力ビットレートをチェックし、ターゲット・ビットレートに再調整しようとします。 |
| -hls_time 6 | 各出力ビデオ・フラグメントの長さを 6 秒に設定する。<br/> ビデオの少なくとも 6 秒間のフレームを蓄積してから、<br/>次のキーフレームが現れたときにビデオ・フラグメントの分割を停止します。 |
| -hls_playlist_type vod | ビデオ・オンデマンド (vod) の出力 `.m3u8` プレイ・リスト・ファイルを準備する。 |
| test-video.m3u8 | 出力プレイ・リスト/マニフェスト・ファイルを `test-video.m3u8` に指定する。<br/> その結果、`test-video0.ts`、`test-video1.ts`、`test-video2.ts`、以下同様<br/>が、デフォルトでビデオ・フラグメントの名前になります。|

`-` オプションについては、ストリームが指定されていない限り、そのカテゴリーの「最良」のものが選択されます。

例えば、次のようになります。
  * `-c:a` は、最も多くのチャネルがあるオーディオ・ストリームを選択します。
  * `-c:v` は、最も高解像度のビデオ・ストリームを選択します。
  * `-c:a:0` は、オーディオ・ストリーム 0 を選択します。
  * `-c:v:0` は、ビデオ・ストリーム 0 を選択します。

このガイドでは、1 つのオーディオ・ストリームと 1 つのビデオ・ストリームのみが例 `test-video.mp4` を形成しています。 したがって、違いは、先に進む際の懸念事項にはなりません。

その後、多くの `.ts` ファイルが表示されます。  さらに、以下のような `.m3u8` ファイルも表示されます。

```
$ cat test-video.m3u8 
#EXTM3U
#EXT-X-VERSION:3
#EXT-X-TARGETDURATION:7
#EXT-X-MEDIA-SEQUENCE:0
#EXT-X-PLAYLIST-TYPE:VOD
#EXTINF:6.039000,
test-video0.ts
#EXTINF:6.039000,
test-video1.ts
#EXTINF:6.039000,
test-video2.ts
#EXTINF:6.039000,
test-video3.ts
#EXTINF:6.039000,
test-video4.ts
#EXTINF:6.039000,
test-video5.ts
#EXTINF:6.039000,
test-video6.ts
#EXTINF:6.039000,
test-video7.ts
#EXTINF:6.039000,
test-video8.ts
#EXTINF:6.039000,
test-video9.ts
#EXTINF:6.039000,
test-video10.ts
#EXTINF:4.620000,
test-video11.ts
#EXT-X-ENDLIST
```
{: screen}

ビデオ解像度のスケーリング、サブタイトルの処理、セキュリティーおよび許可のためのビデオ・フラグメントに対する HLS AES 暗号化など、もっと複雑なユース・ケースの場合、`ffmpeg` には、より複雑で特定の機能を処理するさらに多くの引数オプションがあります。 これらの引数の説明は、[ffmpeg の一般文書](https://ffmpeg.org/ffmpeg.html)および [HLS などの特定のフォーマットに関する文書](https://ffmpeg.org/ffmpeg-formats.html#hls)を参照してください。

## オリジンの準備
{: #prepare-the-origin}

### サーバー
{: #server}

これらの HLS をストリーミングする 2 番目のドメインの追加オリジンとしてこのサーバーを使用する場合は、潜在的なブラウザー・アクセスに対して CORS 応答ヘッダーを返すサーバーを構成する必要がある場合があります。

HLS ファイルは、任意のディレクトリーまたはサブディレクトリーに配置できます。 この例では、HLS ファイルを `/usr/share/nginx/hls/` に配置しましょう。

以下の Nginx 構成は基本的な意味でそれをカバーします。

```
# Some configurations for this main context...

http {

    # Some configurations for this http context...

    server {

        # Some virtual host configurations here...

        location /hls {
            root /usr/share/nginx/;

            # CORS simple requests
            if ($request_method = 'GET') {
                add_header 'Access-Control-Allow-Origin' '*';
                add_header 'Access-Control-Expose-Headers' 'Content-Length, Content-Type';
            }

            # CORS preflight requests
            if ($request_method = 'OPTIONS') {
                add_header 'Access-Control-Allow-Origin' '*';
                add_header 'Access-Control-Allow-Methods' 'GET, OPTIONS';

                # Note: wildcard `Access-Control-Allow-Headers` may not be supported by all browsers, yet.
                add_header 'Access-Control-Allow-Headers' '*';

                add_header 'Access-Control-Max-Age' 1728000;
                add_header 'Content-Type' 'text/plain; charset=utf-8';
                add_header 'Content-Length' 0;
                return 204;
            }

            try_files $uri =404;
        }

        # Some more virtual host configurations...
    }

    # Some more configurations for this http context...
}

# Some more configurations for this main context...
```
{: screen}

### Web ページのビデオ・プレイヤー
{: #video-player-on-the-webpage}

すべてのストリーミング・ビデオ・フォーマットがすべてのアプリケーションでネイティブに再生可能なわけではない可能性があります。 このガイドの例は、HLS および CDN を使用してストリーミングをセットアップします。

例えば、Safari はネイティブな HLS 再生をサポートします。 そのため、Web ページ上のビデオ・プレイヤーは、HTML5 `<video>` 要素を使用する以下の例と同じぐらいシンプルな可能性があります。

```
<!DOCTYPE html>
<html>
  <!-- Some HTML elements... -->
  
  <video src="https://cdn.example.com/hls/test-video.m3u8"></video>
  
  <!-- Some more HTML elements... -->
</html>
```
{: screen}

ただし、デスクトップ・デバイス上のその他のブラウザーは、社内で開発されたか、信頼のおける第三者機関からのものかに関係なく、HTML5 を介して再生可能なコンテンツ・ストリームを生成するために、追加された JavaScript [Media Source Extensions](https://www.w3.org/TR/media-source/) のサポートも必要な可能性があります。

## CDN の構成
{: #configure-the-cdn}

次に、オリジンを CDN に接続して、最適化されたスループット、最小化された待ち時間、および改善されたパフォーマンスでコンテンツを世界中に提供します。

最初に、CDN を[注文](/docs/infrastructure/CDN?topic=CDN-order-a-cdn)します。

次に、[CDN を構成](/docs/infrastructure/CDN?topic=CDN-order-a-cdn#step-2-name-your-cdn)するか、[オリジンを追加](/docs/infrastructure/CDN?topic=CDN-order-a-cdn#step-3-configure-your-origin)します。

最後に、「`Optimize For`」から、「`Video on demand optimization`」を選択します。

![ビデオ・オンデマンドの最適化](images/optimize-for-video-on-demand.png)
