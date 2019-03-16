---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-19"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}


# Vorgehensweise zum Bereitstellen von Video-on-Demand mit CDN
{: #how-to-serve-video-on-demand-with-cdn}

In dieser Anleitung wird anhand eines Beispiels erläutert, wie mit {{site.data.keyword.cloud}} CDN `.mp4`-Inhalte über **HLS** als Video-on-Demand von einem Linux-Nginx-Ursprung an einen Browser gestreamt werden.  

## Einführung

Für das Streaming von Video stehen eine Reihe von Formaten zur Verfügung, wie HLS, MPEG-DASH usw. 

Konzeptionell wird die Konfiguration, die wir verwenden, in dem folgenden Diagramm dargestellt:

![IBM Cloud CDN für Video-on-Demand](images/ibmcdn-vod-example-model.png)

Die Vorbereitungen führen wir am Ursprung durch. Hier müssen wir einige Pakete abrufen, damit das Vorhaben gelingt.

Beginnen wir also mit der Aktualisierung der Paketliste des Ursprungs.
```
$ sudo apt-get update
```

## Videodateien vorbereiten
In diesem Abschnitt verwenden wir `ffmpeg`, um die Videodateien vorzubereiten. Mit den verschiedenen Befehlen dieses leistungsfähigen Tools für Multimediadateien können Sie Dateien konvertieren, muxen, demuxen, filtern usw.

Zunächst installieren wir `ffmpeg`.
```
$ sudo apt-get -y install ffmpeg
```

HLS funktioniert mit zwei Typen von Dateien: `.m3u8` und `.ts`. Die Datei `.m3u8` können Sie sich wie eine Wiedergabeliste vorstellen. Am Anfang des Videodatenstroms ist diese Datei die erste, die abgerufen wird.  Die "Wiedergabeliste" informiert den Videoplayer dann über die Videofragmente, die er abrufen soll, und stellt weitere Daten zur erfolgreichen Wiedergabe der gestreamten Inhalte bereit. Die `.ts`-Dateien sind die "Fragmente" des Videos. Diese Fragmente werden vom Videoplayer gemäß den in der Wiedergabeliste angegebenen Details abgerufen und wiedergegeben.

Lassen Sie uns jetzt das Format, die Bitübertragungsrate und andere Informationen für die Video- und Audiodatenströme unseres Quellenvideos `.mp4` überprüfen.

```
$ ffprobe test-video.mp4 
```

In diesem Beispiel nehmen wir die folgenden Datenstrominformationen für `test-video.mp4` an:
  * Videodatenstrom 0
    * Format: h264
    * Formatprofil: Hoch
    * Auflösung: 1920x1080
    * Bitübertragungsrate: 438 Kb/s
    * Vollbildrate: 30,30 FPS
  * Audiodatenstrom 0
    * Format: aac
    * Abtastrate: 48000
    * Bitübertragungsrate: 128 k

Nun konvertieren wir unsere Datei `test-video.mp4` in die Formate für HLS.

```
$ ffmpeg -i test-video.mp4 -c:a aac -ar 48000 -b:a 128k -c:v h264 -profile:v main -crf 23 -g 61 -keyint_min 61 -sc_threshold 0 -b:v 5300k -maxrate 5300k -bufsize 10600k -hls_time 6 -hls_playlist_type vod test-video.m3u8
```

Die folgende Aufgliederung enthält die Aktionen, die durch diesen Befehl ausgeführt wurden:

|Argument(e)|Auswirkung|
|:---|:---|
| ffmpeg | Tool `ffmpeg` verwenden. |
| -i test-video.mp4 | The source video is located at `test-video.mp4`. |
| -c:a acc | Use the acc audio codec for the output. |
| -ar 48000 | Set the audio sample rate to 48000 Hz for the output. |
| -b:a 128k | Set the audio bitrate to 128000 bits/second for the output. |
| -c:v h264 | Use the `h.264` video codec for the output. |
| -profile:v main | Use the "main" format profile of the selected codec for widest device support. |
| -crf 23 | Attempt to maintain the video quality with varying file size and bitrate.<br/>  The lower the CRF, the higher the quality and file size. |
| -g 61 -keyint_min 61 | Set a maximum and minimum.<br/> With the example source frame rate as 30.30, a keyframe should be <br/> inserted every 2 seconds (61 frames). |
| -sc_threshold 0 | Disable scene detection by `ffmpeg`.<br/> Prevents a second process that may insert extraneous keyframes into the output. |
| -b:v 5300k | Sets the output video stream's target bitrate to 5300000 bits/second. |
| -maxrate 5300k | Limits the maximum output video bitrate at<br/> the encoder to 5300000 bits/second, in case it varies. |
| -bufsize 10600k | Sets the `ffmpeg` video decoder buffer size to 10600000 bits.<br/>  With 5300k bitrate, the `ffmpeg` encoder should check and <br/> attempt to re-adjust the output bitrate back to the target bitrate for every 2 seconds of video. |
| -hls_time 6 | Attempt to target each output video fragment length to 6 seconds.<br/> Accumulates frames for at least 6 seconds of video, and then<br/> stops to break off a video fragment when it encounters the next keyframe. |
| -hls_playlist_type vod | Prepares the output `.m3u8` playlist file for video-on-demand (vod). |
| test-video.m3u8 | Der Name der ausgegebenen Wiedergabelisten-/Manifestdatei lautet `test-video.m3u8`.<br/> Die Folge davon ist, dass die Namen der Videofragmente standardmäßig `test-video0.ts`, `test-video1.ts`, `test-video2.ts` ... und ähnlich<br/> lauten.|

Hinweis: Für die Optionen, die mit `-` beginnen, wird der beste Wert für die jeweilige Kategorie ausgewählt, wenn kein Datenstrom angegeben wird.

Beispiele:
  * Mit `-c:a` wird der Audiodatenstrom mit den meisten Kanälen ausgewählt.
  * Mit `-c:v` wird der Videodatenstrom mit der höchsten Auflösung ausgewählt.
  * Mit `-c:a:0` wird Audiodatenstrom 0 ausgewählt.
  * Mit `-c:v:0` wird Videodatenstrom 0 ausgewählt.

Bei dem in diesem Abschnitt beschriebenen Beispiel besteht die Datei `test-video.mp4` aus nur einem Audiodatenstrom und einem Videodatenstrom. Daher ist der Unterschied im Folgenden nicht relevant.

Nach der Ausführung des Befehls sind eine Reihe von `.ts`-Dateien vorhanden.  Außerdem ist eine `.m3u8`-Datei vorhanden, die etwa wie folgt aussieht:

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
Für komplexere Anwendungsfälle wie die Skalierung der Videoauflösung, das Arbeiten mit Untertiteln, die HLS-AES-Verschlüsselung der Videofragmente für Sicherheit und Autorisierung usw. verfügt `ffmpeg` über viele weitere Argumentoptionen für komplexere und spezifischere Funktionen. Beschreibungen dieser Argumente finden Sie in der [allgemeinen Dokumentation zu ffmpeg](https://ffmpeg.org/ffmpeg.html) und in der [ffmpeg-Dokumentation zu bestimmten Formaten wie HLS](https://ffmpeg.org/ffmpeg-formats.html#hls).

## Ursprung vorbereiten
### Server
Wenn Sie diesen Server als zusätzlichen Ursprung unter einer zweiten Domäne verwenden, von der diese HLS-Dateien gestreamt werden sollen, müssen Sie möglicherweise den Server so konfigurieren, dass er CORS-Antwortheader für den potenziellen Browserzugriff zurückgibt.

Sie können die HLS-Dateien in ein beliebiges Verzeichnis oder Unterverzeichnis stellen. In diesem Beispiel stellen wir die HLS-Dateien in das Verzeichnis `/usr/share/nginx/hls/`.

Die folgende grundlegende Nginx-Konfiguration reicht in diesem Fall aus:

```
# Einige Konfigurationen für diesen Hauptkontext...

http {

    # Einige Konfigurationen für diesen HTTP-Kontext...

    server {

        # Einige Konfigurationen für den virtuellen Host hier...

        location /hls {
            root /usr/share/nginx/;

            # Einfache CORS-Anforderungen
            if ($request_method = 'GET') {
                add_header 'Access-Control-Allow-Origin' '*';
                add_header 'Access-Control-Expose-Headers' 'Content-Length, Content-Type';
            }

            # CORS-Preflight-Anforderungen
            if ($request_method = 'OPTIONS') {
                add_header 'Access-Control-Allow-Origin' '*';
                add_header 'Access-Control-Allow-Methods' 'GET, OPTIONS';

                # Hinweis: Platzhalter bei `Access-Control-Allow-Headers` wird möglicherweise noch nicht von allen Browsern unterstützt.
                add_header 'Access-Control-Allow-Headers' '*';

                add_header 'Access-Control-Max-Age' 1728000;
                add_header 'Content-Type' 'text/plain; charset=utf-8';
                add_header 'Content-Length' 0;
                return 204;
            }

            try_files $uri =404;
        }

        # Einige weitere Konfigurationen für den virtuellen Host...
    }

    # Einige weitere Konfigurationen für diesen HTTP-Kontext...
}

# Einige weitere Konfigurationen für diesen Hauptkontext...
```

### Videoplayer auf der Webseite

Nicht alle Streaming-Videoformate können mit allen Anwendungen nativ abgespielt werden. Mit dem Beispiel in diesem Abschnitt wird Streaming mit HLS und CDN konfiguriert.

Safari unterstützt beispielsweise die native HLS-Wiedergabe. Daher kann der Videoplayer auf der Webseite so einfach sein wie im folgenden Beispiel mit HTML5-`<video>`-Elementen:

```
<!DOCTYPE html>
<html>
  <!-- Einige HTML-Elemente... -->
  
  <video src="https://cdn.example.com/hls/test-video.m3u8"></video>
  
  <!-- Einige weitere HTML-Elemente... -->
</html>
```

Für andere Browser auf Desktopgeräten sind jedoch möglicherweise zusätzliche JavaScript [Media Source Extensions](https://www.w3.org/TR/media-source/) erforderlich, die entweder intern entwickelt oder von einem vertrauenswürdigen Dritten bezogen wurden, um Inhaltsströme zu generieren, die über HTML5 abspielbar sind.

## CDN konfigurieren
Jetzt verbinden wir den Ursprung mit dem CDN, um Inhalte weltweit mit optimiertem Durchsatz, minimaler Latenz und hoher Leistung bereitzustellen.

Zuerst [bestellen](/docs/infrastructure/CDN?topic=CDN-order-a-cdn) Sie ein CDN.

Anschließend [konfigurieren Sie Ihr CDN](/docs/infrastructure/CDN?topic=CDN-step-2-name-your-cdn) oder [fügen Sie einen Ursprung hinzu](/docs/infrastructure/CDN?topic=CDN-step-3-configure-your-origin).

Schließlich wählen Sie unter `Optimieren für` die Option `Optimierung für Video-on-Demand` aus.

![Optimieren für Video-on-Demand](images/optimize-for-video-on-demand.png)
