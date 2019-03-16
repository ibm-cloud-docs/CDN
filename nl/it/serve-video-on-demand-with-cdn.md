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


# Come servire video on demand con CDN
{: #how-to-serve-video-on-demand-with-cdn}

In questa guida, esploreremo un esempio di come avvalersi di una {{site.data.keyword.cloud}} CDN per riprodurre in streaming del contenuto `.mp4` tramite **HLS** come video on demand, a un browser, da un'origine Linux-Nginx. 

## Introduzione

Per riprodurre in streaming dei video, sono disponibili diversi formati, come HLS, MPEG-DASH e così via. 

Concettualmente, la configurazione che stiamo utilizzando è mostrata nel seguente diagramma.

![IBM Cloud CDN per video on demand](images/ibmcdn-vod-example-model.png)

Useremo anche l'origine come luogo di preparazione. A tale riguardo, avremo anche bisogno di ottenere alcuni pacchetti affinché la cosa funzioni.

Iniziamo pertanto aggiornando l'elenco pacchetti dell'origine.
```
$ sudo apt-get update
```

## Prepara i file video
In questa guida, useremo `ffmpeg` per preparare i file video. È un potente strumento per i file multimediali che può eseguire operazioni di conversione, multiplazione, demultiplazione, filtraggio e così via servendosi della sua gamma di comandi.

Ci procureremo innanzitutto `ffmpeg`.
```
$ sudo apt-get -y install ffmpeg
```

HLS funziona con due tipi di file: `.m3u8` e `.ts`. Puoi pensare al file `.m3u8` come a una "playlist". All'inizio della riproduzione in streaming del video, questo file è il primo che viene recuperato.  La playlist informa quindi il lettore video in merito ai frammenti video che deve recuperare e fornisce altri dati su come riprodurre correttamente il contenuto oggetto dello streaming. I file `.ts` sono i "frammenti" video. Questi frammenti vengono recuperati e riprodotti dal lettore video in base ai dettagli forniti nella "playlist".

Controlliamo e vediamo ora formato, velocità di bit e altre informazioni per i flussi video e audio del nostro video `.mp4` di origine.

```
$ ffprobe test-video.mp4 
```

In questo esempio, prendiamo in considerazione le seguenti informazioni di streaming per `test-video.mp4`:
  * Flusso video 0
    * Formato: h264
    * Profilo formato: Alto
    * Risoluzione: 1920x1080
    * Velocità in bit: 438 kb/s
    * Frequenza fotogrammi: 30,30 fps
  * Flusso audio 0
    * Formato: aac
    * Frequenza di campionamento: 48000
    * Velocità di bit: 128k

Convertiremo ora il nostro file `test-video.mp4` in formati per HLS.

```
$ ffmpeg -i test-video.mp4 -c:a aac -ar 48000 -b:a 128k -c:v h264 -profile:v main -crf 23 -g 61 -keyint_min 61 -sc_threshold 0 -b:v 5300k -maxrate 5300k -bufsize 10600k -hls_time 6 -hls_playlist_type vod test-video.m3u8
```

Qui di seguito viene riportata una suddivisione di quello che questo comando ha fatto:

|Argomento (i)|Effetto|
|:---|:---|
| ffmpeg | Utilizza lo strumento `ffmpeg`. |
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
| test-video.m3u8 | Denomina la play list/il file manifest di output come `test-video.m3u8`.<br/> Di conseguenza, `test-video0.ts`, `test-video1.ts`, `test-video2.ts`, ..., e simili<br/> saranno i nomi del frammento video per impostazione predefinita.|

Nota: per le opzioni `-`, a meno che non venga specificato un flusso, viene scelto il "migliore" per la sua categoria.

Come ad esempio:
  * `-c:a` sceglie il flusso audio con più canali.
  * `-c:v` sceglie il flusso video con la risoluzione più alta.
  * `-c:a:0` sceglie il flusso audio 0.
  * `-c:v:0` sceglie il flusso video 0.

In questa guida, solo 1 flusso audio e 1 flusso video formano il `test-video.mp4` di esempio. Pertanto, la differenza non sarà un problema, andando avanti.

Successivamente, puoi aspettarti di vedere diversi file `.ts`.  Puoi inoltre aspettarti di vedere un file `.m3u8` simile al seguente:

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
Per casi d'uso più complessi, come ad esempio il ridimensionamento della risoluzione video, le operazioni con i sottotitoli, la crittografia HLS AES sui frammenti video per la sicurezza e l'autorizzazione e così via, `ffmpeg` ha molte più opzioni di argomento che gestiscono le funzioni più complesse e specifiche. Puoi trovare le descrizioni di questi argomenti nella [documentazione generale di ffmpeg](https://ffmpeg.org/ffmpeg.html) e nella relativa [documentazione su formati specifici quali HLS](https://ffmpeg.org/ffmpeg-formats.html#hls).

## Prepara l'origine
### Server
Se stai utilizzando questo server come un'origine aggiuntiva sotto un secondo dominio da cui riprodurre in streaming questi HLS, potresti aver bisogno di configurare il server per restituire intestazioni di risposta CORS per un potenziale accesso al browser.

Puoi posizionare i file HLS in qualsiasi directory o sottodirectory tu voglia. Per questo esempio, posizioniamo i file HLS in `/usr/share/nginx/hls/`.

La seguente configurazione Nginx coprirà quanto detto nel senso di base:

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

### Lettore video sulla pagina web

Non tutti i formati video in streaming possono essere riproducibili in modo nativo su tutte le applicazioni. L'esempio in questa guida configura lo streaming utilizzando HLS e CDN.

Ad esempio, Safari supporterà una riproduzione HLS nativa. Pertanto, il lettore video sulla pagina web può essere tanto semplice quanto il seguente esempio con elementi HTML5 `<video>`:

```
<!DOCTYPE html>
<html>
  <!-- Some HTML elements... -->
  
  <video src="https://cdn.example.com/hls/test-video.m3u8"></video>
  
  <!-- Some more HTML elements... -->
</html>
```

Tuttavia, anche altri browser o dispositivi desktop potrebbero aver bisogno di supporto da [Media Source Extensions](https://www.w3.org/TR/media-source/) JavaScript aggiunto, sia sviluppato internamente che da terze parti affidabili, per generare dei flussi di contenuto riproducibili tramite HTML5.

## Configura la CDN
Connettiamo ora l'origine alla CDN per gestire del contenuto in tutto il mondo con una velocità effettiva ottimizzata, una latenza ridotta al minimo e delle prestazioni migliorate.

Innanzitutto, [ordina](/docs/infrastructure/CDN?topic=CDN-order-a-cdn) una CDN.

Procedi quindi [configurando la tua CDN](/docs/infrastructure/CDN?topic=CDN-step-2-name-your-cdn) oppure [aggiungendo un'origine](/docs/infrastructure/CDN?topic=CDN-step-3-configure-your-origin).

Infine, sotto `Optimize For`, seleziona `Video on demand optimization`.

![Ottimizza per Video on Demand](images/optimize-for-video-on-demand.png)
