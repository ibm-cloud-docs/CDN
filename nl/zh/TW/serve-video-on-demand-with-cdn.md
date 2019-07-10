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


# 如何提供隨選視訊與 CDN
{: #how-to-serve-video-on-demand-with-cdn}

在本手冊中將探索一個範例，說明如何利用 {{site.data.keyword.cloud}} CDN，將 `.mp4` 內容透過 **HLS**（作為隨選視訊），從 Linux-Nginx 原點串流至瀏覽器。 

## 簡介
{: #introduction}

串流視訊可使用許多格式，例如，HLS、MPEG-DASH 等等。 

在概念上，我們使用的設定顯示在下圖中：

![IBM Cloud CDN for Video On Demand](images/ibmcdn-vod-example-model.png)

我們也將原點當作準備工作區使用。在其上，我們也需要取得一些套件才能使其運作。

因此，讓我們從更新原點的套件清單開始吧。
```
$ sudo apt-get update
```

## 準備視訊檔案
{: #prepare-video-files}

在本手冊中，將使用 `ffmpeg` 來準備視訊檔案。其為適用於多媒體檔案的強大工具，可透過其各種指令來進行轉換、多工、解多工、過濾等等。

首先，取得 `ffmpeg`。
```
$ sudo apt-get -y install ffmpeg
```

HLS 搭配使用兩種類型的檔案：`.m3u8` 及 `.ts`。您可以將 `.m3u8` 檔案視為「播放清單」。在視訊串流的一開始，第一個提取的便是這個檔案。然後，播放清單會向視訊播放程式通知其應提取的視訊片段，並提供有關如何順利播放已串流內容的其他資料。`.ts` 檔案為視訊「片段」。視訊播放程式會根據「播放清單」中所給定的詳細資料，來提取及播放這些片段。

現在，針對來源 `.mp4` 視訊的視訊及音訊串流，檢查並查看格式、位元速率及其他資訊。

```
$ ffprobe test-video.mp4 
```

在此範例中，假設 `test-video.mp4` 有下列串流資訊：
  * 視訊串流 0
    * 格式：h264
    * 格式設定檔：高
    * 解析度：1920x1080
    * 位元速率：438 kb/s
    * 訊框率：30.30 fps
  * 音訊串流 0
    * 格式：aac
    * 取樣率：48000
    * 位元速率：128k

現在，將 `test-video.mp4` 檔案轉換成 HLS 的格式。

```
$ ffmpeg -i test-video.mp4 -c:a aac -ar 48000 -b:a 128k -c:v h264 -profile:v main -crf 23 -g 61 -keyint_min 61 -sc_threshold 0 -b:v 5300k -maxrate 5300k -bufsize 10600k -hls_time 6 -hls_playlist_type vod test-video.m3u8
```

這個指令的分解如下：

|引數|效果|
|:---|:---|
| ffmpeg | 使用 `ffmpeg` 工具。|
| -i test-video.mp4 | 來源視訊位於 `test-video.mp4`。|
| -c:a acc | 使用 acc 音訊轉碼器進行輸出。|
| -ar 48000 | 將輸出的音訊取樣率設為 48000 Hz。|
| -b:a 128k | 將輸出的音訊位元速率設為 128000 個位元/秒。|
| -c:v h264 | 使用 `h.264` 視訊轉碼器進行輸出。|
| -profile:v main | 使用所選轉碼器的「主要」格式設定檔，以取得最多裝置支援。|
| -crf 23 | 試圖以各種檔案大小及位元速率維護視訊品質。<br/>  CRF 越低，品質越好，檔案越大。|
| -g 61 -keyint_min 61 | 設定上限及下限。<br/> 以來源訊框率 30.30 為例，主要畫面格應<br/> 每 2 秒插入一次（61 個訊框）。|
| -sc_threshold 0 | 停用依照 `ffmpeg` 進行場景偵測。<br/> 防止第二個處理程序可能將無關的主要畫面格插入輸出。|
| -b:v 5300k | 將輸出視訊串流的目標位元速率設為 5300000 個位元/秒。|
| -maxrate 5300k | 將編碼器的輸出視訊位元速率上限<br/> 限制為 5300000 個位元/秒，以防其變更。|
| -bufsize 10600k | 將 `ffmpeg` 視訊解碼器緩衝區大小設為 10600000 個位元。<br/>  位元速率若為 5300k，`ffmpeg` 編碼器應檢查並<br/> 試圖將輸出位元速率重新調整回目標位元速率（每 2 秒視訊進行一次）。|
| -hls_time 6 | 試圖達成每個輸出視訊片段長度為 6 秒的目標。<br/> 累計至少 6 秒視訊的訊框，然後<br/> 在發現下一個主要畫面格時，停止中斷視訊片段。|
| -hls_playlist_type vod | 準備隨選視訊 (vod) 的輸出 `.m3u8` 播放清單檔案。|
| test-video.m3u8 | 將輸出播放清單/資訊清單檔命名為 `test-video.m3u8`。<br/> 因此，`test-video0.ts`、`test-video1.ts`、`test-video2.ts`，.以此類推，<br/> 依預設，會成為視訊片段的名稱。|

請注意，對於 `-` 選項，除非已指定某個串流，否則會選擇「最適合」其種類的串流。

例如：
  * `-c:a` 會選擇具有最多頻道的音訊串流。
  * `-c:v` 會選擇具有最高解析度的視訊串流。
  * `-c:a:0` 會選擇音訊串流 0。
  * `-c:v:0` 會選擇視訊串流 0。

在本手冊中，只有 1 個音訊串流以及 1 個視訊串流構成 `test-video.mp4` 範例。因此，差異在後續操作中並不重要。

之後，您可以預期會看到許多 `.ts` 檔。此外，您也可以預期會看到一個 `.m3u8` 檔，其類似下列：

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

如需更複雜的使用案例（例如，調整視訊解析度、使用字幕、基於安全及授權，在視訊片段上新增 HLS AES 加密，等等），`ffmpeg` 有許多其他引數選項可處理更複雜及特定的特性。您可以在 [ffmpeg 的一般文件](https://ffmpeg.org/ffmpeg.html)以及在其[特定格式（例如，HLS）的文件](https://ffmpeg.org/ffmpeg-formats.html#hls)中，找到這些引數的說明。

## 準備原點
{: #prepare-the-origin}

### 伺服器
{: #server}

如果您將這個伺服器當作從中串流這些 HLS 之第二個網域下的其他原點使用，則可能需要將伺服器配置成傳回 CORS 回應標頭，以進行可能的瀏覽器存取。

您可以將 HLS 檔案放在您喜歡的任何目錄或子目錄之下。此範例將 HLS 檔案放在 `/usr/share/nginx/hls/` 之下。

一般情況下，下列 Nginx 配置會包含該資訊：

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

### 網頁上的視訊播放程式
{: #video-player-on-the-webpage}

並非所有多媒體串流視訊格式都可以在所有應用程式上原生播放。本手冊中的範例使用 HLS 及 CDN 來設定多媒體串流。

例如，Safari 支援原生 HLS 播放。因此，網頁上的視訊播放程式可能和下列使用 HTML5 `<video>` 元素的範例一樣簡單：

```
<!DOCTYPE html>
<html>
  <!-- Some HTML elements... -->
  
  <video src="https://cdn.example.com/hls/test-video.m3u8"></video>
  
  <!-- Some more HTML elements... -->
</html>
```
{: screen}

然而，桌面裝置上的其他瀏覽器可能也需要附加 JavaScript [媒體源擴充](https://www.w3.org/TR/media-source/)的支援（不論是內部開發或來自具公信力的第三者），才能產生可透過 HTML5 播放的內容串流。

## 配置 CDN
{: #configure-the-cdn}

現在，將原點連接至 CDN，以最佳傳輸量、最小延遲以及提高的效能，向全球提供內容。

首先，[訂購](/docs/infrastructure/CDN?topic=CDN-order-a-cdn)一個 CDN。

接下來，[配置您的 CDN](/docs/infrastructure/CDN?topic=CDN-order-a-cdn#step-2-name-your-cdn)，或者[新增原點](/docs/infrastructure/CDN?topic=CDN-order-a-cdn#step-3-configure-your-origin)。

最後，在`最佳化`之下，選取`隨選視訊最佳化`。

![最佳化隨選視訊](images/optimize-for-video-on-demand.png)
