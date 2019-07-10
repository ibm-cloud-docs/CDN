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


# Vorgehensweise zum Bereitstellen von Video-on-Demand mit CDN
{: #how-to-serve-video-on-demand-with-cdn}

In dieser Anleitung wird anhand eines Beispiels erläutert, wie mit {{site.data.keyword.cloud}} CDN `.mp4`-Inhalte über **HLS** als Video-on-Demand von einem Linux-Nginx-Ursprung an einen Browser gestreamt werden. 

## Einführung
{: #introduction}

Für das Streaming von Video stehen eine Reihe von Formaten zur Verfügung, wie HLS, MPEG-DASH usw. 

Konzeptionell wird die Konfiguration, die wir verwenden, in dem folgenden Diagramm dargestellt:

![IBM Cloud CDN für Video-on-Demand](images/ibmcdn-vod-example-model.png)

Die Vorbereitungen führen wir am Ursprung durch. Hier müssen wir einige Pakete abrufen, damit das Vorhaben gelingt.

Beginnen wir also mit der Aktualisierung der Paketliste des Ursprungs.
```
$ sudo apt-get update
```

## Videodateien vorbereiten
{: #prepare-video-files}

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
| -i test-video.mp4 | Das Quellenvideo befindet sich unter `test-video.mp4`. |
| -c:a acc | Verwenden Sie den acc-Audio-Codec für die Ausgabe. |
| -ar 48000 | Setzen Sie die Audio-Abtastrate auf 48000 Hz für die Ausgabe fest. |
| -b:a 128k | Setzen Sie die Audio-Bitübertragungsrate auf 128000 Bit/Sekunde für die Ausgabe fest. |
| -c:v h264 | Verwenden Sie den Video-Codec `h.264` für die Ausgabe. |
| -profile:v main | Verwenden Sie das "main"-Formatprofil des ausgewählten Codec für die umfassendste Einheitenunterstützung. |
| -crf 23 | Versuchen Sie, die Videoqualität mit variierender Dateigröße und Bitübertragungsrate zu erhalten.<br/> Je niedriger der CRF, desto höher die Qualität und Dateigröße. |
| -g 61 -keyint_min 61 | Legen Sie ein Maximum und ein Minimum fest.<br/> Bei einer Beispiel-Quellvollbildrate von 30:30 sollte alle<br/> 2 Sekunden ein Schlüsselvollbild eingefügt werden (61 Vollbilder). |
| -sc_threshold 0 | Inaktivieren der Szenenerkennung durch `ffmpeg`.<br/> Verhindert einen zweiten Prozess, der überflüssige Schlüsselvollbilder in die Ausgabe einfügen kann. |
| -b:v 5300k | Legt die Ziel-Bitübertragungsrate des Ausgabevideodatenstroms auf 5300000 Bit/Sekunde fest. |
| -maxrate 5300k | Begrenzt die maximale Ausgabevideobitübertragungsrate <br/> des Encoders auf 5300000 Bit/Sekunde, falls sie variiert. |
| -bufsize 10600k | Legt die Puffergröße für den Video-Decoder `ffmpeg` auf 10600000 Bit fest.<br/>  Bei einer Bitübertragungsrate von 5300K sollte der `ffmpeg`-Encoder überprüfen und <br/> versuchen, die Ausgabe-Bitübertragungsrate alle 2 Videosekunden auf die Ziel-Bitübertragungsrate zurückzusetzen. |
| -hls_time 6 | Versuchen Sie, die Länge jedes ausgegebenen Videofragments auf 6 Sekunden zu beschränken.<br/> Sammelt Vollbilder für mindestens 6 Sekunden Video und stoppt<br/> dann, um ein Videofragment zu unterbrechen, wenn es auf das nächste Schlüsselvollbild trifft. |
| -hls_playlist_type vod | Bereitet die Ausgabe-Playlist-Datei `.m3u8` für Video-on-Demand (VoD) vor. |
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
{: screen}

Für komplexere Anwendungsfälle wie die Skalierung der Videoauflösung, das Arbeiten mit Untertiteln, die HLS-AES-Verschlüsselung der Videofragmente für Sicherheit und Autorisierung usw. verfügt `ffmpeg` über viele weitere Argumentoptionen für komplexere und spezifischere Funktionen. Beschreibungen dieser Argumente finden Sie in der [allgemeinen Dokumentation zu ffmpeg](https://ffmpeg.org/ffmpeg.html) und in der [ffmpeg-Dokumentation zu bestimmten Formaten wie HLS](https://ffmpeg.org/ffmpeg-formats.html#hls).

## Ursprung vorbereiten
{: #prepare-the-origin}

### Server
{: #server}

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
{: screen}

### Videoplayer auf der Webseite
{: #video-player-on-the-webpage}

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
{: screen}

Für andere Browser auf Desktopgeräten sind jedoch möglicherweise zusätzliche JavaScript [Media Source Extensions](https://www.w3.org/TR/media-source/) erforderlich, die entweder intern entwickelt oder von einem vertrauenswürdigen Dritten bezogen wurden, um Inhaltsströme zu generieren, die über HTML5 abspielbar sind.

## CDN konfigurieren
{: #configure-the-cdn}

Jetzt verbinden wir den Ursprung mit dem CDN, um Inhalte weltweit mit optimiertem Durchsatz, minimaler Latenz und hoher Leistung bereitzustellen.

Zuerst [bestellen](/docs/infrastructure/CDN?topic=CDN-order-a-cdn) Sie ein CDN.

Anschließend [konfigurieren Sie Ihr CDN](/docs/infrastructure/CDN?topic=CDN-order-a-cdn#step-2-name-your-cdn) oder [fügen Sie einen Ursprung hinzu](/docs/infrastructure/CDN?topic=CDN-order-a-cdn#step-3-configure-your-origin).

Schließlich wählen Sie unter `Optimieren für` die Option `Optimierung für Video-on-Demand` aus.

![Optimieren für Video-on-Demand](images/optimize-for-video-on-demand.png)
