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


# 如何使用 CDN 提供视频点播
{: #how-to-serve-video-on-demand-with-cdn}

在此指南中，我们将探索一个示例，了解如何利用 {{site.data.keyword.cloud}} CDN 通过 **HLS** 将 `.mp4` 内容作为视频点播从 Linux-Nginx 源传送到浏览器。 

## 简介
{: #introduction}

很多格式（如 HLS、MPEG-DASH 等）都可用于流处理视频。 

我们使用的设置以概念化的方式显示在下图中：

![IBM Cloud CDN 用于视频点播](images/ibmcdn-vod-example-model.png)

我们还会使用源作为准备位置。关于这点，我们还需要获取一些软件包才能使其生效。

所以让我们从更新源的软件包列表开始。
```
$ sudo apt-get update
```

## 准备视频文件
{: #prepare-video-files}

在此指南中，我们将使用 `ffmpeg` 来准备视频文件。此工具功能强大，可通过多种命令用于多媒体文件的转换、mux、demux、过滤等。

首先，我们要获取 `ffmpeg`。
```
$ sudo apt-get -y install ffmpeg
```

HLS 可处理两种类型的文件：`.m3u8` 和 `.ts`。您可以将 `.m3u8` 文件看成“播放列表”。在视频流的开始处，此文件是第一个被访存的。播放列表随后会通知视频播放器应访存哪些视频片段，并提供关于如何成功播放流内容的数据。`.ts` 文件是视频的“片段”。视频播放器根据“播放列表”中给定的详细信息来访存和播放这些片段。

现在，我们来查看一下我们源 `.mp4` 视频的视频和音频流的格式、比特率以及其他信息。

```
$ ffprobe test-video.mp4 
```

在此示例中，我们要考虑 `test-video.mp4` 的下列流信息：
  * 视频流 0
    * 格式：h264
    * 格式概要文件：High
    * 分辨率：1920x1080
    * 比特率：438 kb/s
    * 帧速率：30.30 fps
  * 音频流 0
    * 格式：aac
    * 采样率：48000
    * 比特率：128k

现在我们要将 `test-video.mp4` 文件转换为适合 HLS 的格式。

```
$ ffmpeg -i test-video.mp4 -c:a aac -ar 48000 -b:a 128k -c:v h264 -profile:v main -crf 23 -g 61 -keyint_min 61 -sc_threshold 0 -b:v 5300k -maxrate 5300k -bufsize 10600k -hls_time 6 -hls_playlist_type vod test-video.m3u8
```

下面是此命令执行了哪些操作的具体信息：

|参数|影响|
|:---|:---|
| ffmpeg |使用 `ffmpeg` 工具。|
| -i test-video.mp4 |源视频位于 `test-video.mp4`。|
| -c:a acc |将 ACC 音频编码解码器用于输出。|
| -ar 48000 |将输出的音频采样率设置为 48000 赫兹。|
| -b:a 128k |将输出的音频比特率设置为 128000 比特/秒。|
| -c:v h264 |将 `h.264` 视频编码解码器用于输出。|
| -profile:v main |使用所选编码解码器的“主要”格式概要文件，以获得最广泛的设备支持。|
| -crf 23 |尝试针对不同的文件大小和比特率保持视频质量。<br/>CRF 越小，质量越高，文件大小越大。|
| -g 61 -keyint_min 61 |设置最大值和最小值。<br/>对于示例源帧速率 30.30，<br/>应每 2 秒插入一个关键帧（61 个帧）|
| -sc_threshold 0 |禁止 `ffmpeg` 进行场景检测。<br/>阻止可能将无关关键帧插入输出的第二个进程。|
| -b:v 5300k |将输出视频流的目标比特率设置为 5300000 比特/秒。|
| -maxrate 5300k |将编码器的最大输出视频比特率<br/>限制为 5300000 比特/秒，以防它变化。|
| -bufsize 10600k |将 `ffmpeg` 视频解码器缓冲区大小设置为 10600000 比特。<br/>对于 5300000 比特率，`ffmpeg` 编码器应检查并尝试<br/>每经过 2 秒的视频将输出比特率重新调整回目标比特率。|
| -hls_time 6 |尝试将每个输出视频片段的目标长度设定为 6 秒。<br/>累积至少 6 秒视频的帧，然后<br/>在遇到下一个关键帧时停止以中断视频片段。|
| -hls_playlist_type vod |为视频点播 (vod) 准备输出 `.m3u8` 播放列表文件。|
| test-video.m3u8 |将输出播放列表/清单文件命名为 `test-video.m3u8`。<br/>结果是，在缺省情况下，将使用 `test-video0.ts`、`test-video1.ts`、`test-video2.ts`.... 以及类似值<br/>作为视频片段的名称。|

注：对于 `-` 选项，除非指定了流，否则会选择该类别的“最佳”选项。

例如：
  * `-c:a` 选择通道最多的音频流。
  * `-c:v` 选择分辨率最高的视频流。
  * `-c:a:0` 选择音频流 0。
  * `-c:v:0` 选择视频流 0。

在此指南中，构成示例 `test-video.mp4` 的只有 1 个音频流和 1 个视频流。所以存在差异对继续不会有影响。

随后，您会看到很多 `.ts` 文件。此外，您还会看到与以下内容类似的 `.m3u8` 文件。

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

对于更复杂的用例（如缩放视频分辨率、使用子标题、在视频片段上使用 HLS AES 加密来保护安全和进行授权，等等），`ffmpeg` 有很多其他参数选项可用于处理更复杂的功能和特定功能。有关这些参数的说明，您可以在 [ffmpeg 的通用文档](https://ffmpeg.org/ffmpeg.html)中及其[有关特定格式（如 HLS）的文档](https://ffmpeg.org/ffmpeg-formats.html#hls)中找到。

## 准备源
{: #prepare-the-origin}

### 服务器
{: #server}

如果使用此服务器作为用于流处理这些 HLS 的另一个域下的其他源，可能需要配置服务器以针对可能的浏览器访问返回 CORS 响应头。

您可以将 HLS 文件放在您希望的任何目录或子目录下。对于此示例，我们将 HLS 文件放在 `/usr/share/nginx/hls/` 下。

以下 Nginx 配置基本涵盖了这些内容：

```
# 此主要上下文的一些配置...

http {

    # 此 HTTP 上下文的一些配置...

    server {

        # 此处的一些虚拟主机配置...

        location /hls {
            root /usr/share/nginx/;

            # CORS 简单请求
            if ($request_method = 'GET') {
                add_header 'Access-Control-Allow-Origin' '*';
                add_header 'Access-Control-Expose-Headers' 'Content-Length, Content-Type';
            }

            # CORS 预检请求
            if ($request_method = 'OPTIONS') {
                add_header 'Access-Control-Allow-Origin' '*';
                add_header 'Access-Control-Allow-Methods' 'GET, OPTIONS';

                # 注：通配符“Access-Control-Allow-Headers”可能尚未获得所有浏览器的支持。
                add_header 'Access-Control-Allow-Headers' '*';

                add_header 'Access-Control-Max-Age' 1728000;
                add_header 'Content-Type' 'text/plain; charset=utf-8';
                add_header 'Content-Length' 0;
                return 204;
            }

            try_files $uri =404;
        }

        # 其他一些虚拟主机配置...
    }

    # 此 HTTP 上下文的其他一些配置...
}

# 此主要上下文的其他一些配置...
```
{: screen}

### Web 页面上的视频播放器
{: #video-player-on-the-webpage}

并不是所有的流式视频格式都可以在所有应用程序上本机播放。此指南中的示例使用 HLS 和 CDN 来设置流式方法。

例如，Safari 会支持本机 HLS 回放。因此，Web 页面上的视频播放器可能跟使用 HTML5 `<video>` 元素的以下示例一样简单：

```
<!DOCTYPE html>
<html>
  <!-- 一些 HTML 元素... -->

  <video src="https://cdn.example.com/hls/test-video.m3u8"></video>
  
  <!-- 其他一些 HTML 元素... -->
</html>
```
{: screen}

但是，台式机设备上的其他浏览器可能也需要来自已添加的 JavaScript [介质源扩展名](https://www.w3.org/TR/media-source/)的支持（无论是内部开发的还是来自受信任的第三方），这样才能生成可通过 HTML5 播放的内容流。

## 配置 CDN
{: #configure-the-cdn}

现在，让我们将源连接到 CDN 以在全世界范围内提供内容，并实现优化吞吐量、将等待时间降至最低，并提高性能。

首先，[订购](/docs/infrastructure/CDN?topic=CDN-order-a-cdn) CDN。

接下来，[配置 CDN](/docs/infrastructure/CDN?topic=CDN-order-a-cdn#step-2-name-your-cdn) 或者[添加源](/docs/infrastructure/CDN?topic=CDN-order-a-cdn#step-3-configure-your-origin)。

最后，在`优化目标`下选择`视频点播优化`。

![针对视频点播进行优化](images/optimize-for-video-on-demand.png)
