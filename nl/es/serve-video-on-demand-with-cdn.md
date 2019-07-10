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


# Cómo ofrecer vídeo bajo demanda con CDN
{: #how-to-serve-video-on-demand-with-cdn}

En esta guía, estudiaremos un ejemplo de cómo aprovechar la CDN de {{site.data.keyword.cloud}} para transmitir contenido `.mp4` a través de **HLS** como vídeo bajo demanda a un navegador desde un origen Linux-Nginx. 

## Introducción
{: #introduction}

Para transmitir vídeo existen varios formatos, como HLS, MPEG-DASH, etc. 

En el siguiente diagrama se muestra el concepto de la configuración que vamos a utilizar:

![CDN de IBM Cloud para vídeo bajo demanda](images/ibmcdn-vod-example-model.png)

También usaremos el origen como el lugar para la preparación. Tendremos que obtener unos cuantos paquetes para que esto funcione.

Comenzaremos por actualizar la lista de paquetes del origen.
```
$ sudo apt-get update
```

## Preparar archivos de vídeo
{: #prepare-video-files}

En esta guía, utilizaremos `ffmpeg` para preparar los archivos de vídeo. Es una potente herramienta para archivos multimedia que puede convertir, realizar procesos mux y demux, filtrar, etc., mediante sus distintos mandatos.

En primer lugar vamos a obtener `ffmpeg`.
```
$ sudo apt-get -y install ffmpeg
```

HLS funciona con dos tipos de archivos: `.m3u8` y `.ts`. El archivo `.m3u8` se puede considerar como una "lista de reproducción". Al principio de la secuencia de vídeo, este archivo es el primero que se recupera.  A continuación, la lista de reproducción informa al reproductor de vídeo sobre los fragmentos de vídeo que debe captar, y proporciona otros datos sobre cómo reproducir correctamente el contenido en streaming. Los archivos `.ts` son los "fragmentos" del vídeo. El reproductor de vídeo capta y reproduce estos fragmentos según los detalles especificados en la "lista de reproducción".

Ahora vamos a ver el formato, la velocidad de bits y otra información de las secuencias de vídeo y de audio de nuestro vídeo `.mp4` de origen.

```
$ ffprobe test-video.mp4 
```

En este ejemplo se utiliza la siguiente información de transmisión para `test-video.mp4`:
  * Secuencia de vídeo 0
    * Formato: h264
    * Perfil de formato: Alto
    * Resolución: 1920x1080
    * Velocidad de bits: 438 kb/s
    * Velocidad de tramas: 30,30 fps
  * Secuencia de audio 0
    * Formato: aac
    * Velocidad de muestra: 48000
    * Velocidad de bits: 128k

Ahora convertiremos nuestro archivo `test-video.mp4` a los formatos para HLS.

```
$ ffmpeg -i test-video.mp4 -c:a aac -ar 48000 -b:a 128k -c:v h264 -profile:v main -crf 23 -g 61 -keyint_min 61 -sc_threshold 0 -b:v 5300k -maxrate 5300k -bufsize 10600k -hls_time 6 -hls_playlist_type vod test-video.m3u8
```

Este es el desglose de lo que ha hecho este mandato:

|Argumento(s)|Efecto|
|:---|:---|
| ffmpeg | Usar la herramienta `ffmpeg`. |
| -i test-video.mp4 | El vídeo de origen se encuentra en `test-video.mp4`. |
| -c:a acc | Usar el códec de audio acc para la salida. |
| -ar 48000 | Establecer la velocidad de muestra del audio a 48000 Hz para la salida. |
| -b:a 128k | Establecer la velocidad de bits del audio a 128000 bits/segundo para la salida. |
| -c:v h264 | Usar el códec de vídeo `h.264` para la salida. |
| -profile:v main | Usar el perfil de formato "principal" del códec seleccionado para un soporte de dispositivo más amplio. |
| -crf 23 | Intentar mantener la calidad de vídeo cuando varía el tamaño de archivo y la velocidad de bits. <br/>  Como menor sea el CRF, mayor será la calidad y el tamaño de archivo. |
| -g 61 -keyint_min 61 | Establecer un máximo y un mínimo.<br/> Con la velocidad de tramas de origen de ejemplo de 30.30, se debería insertar un keyframe <br/> cada 2 segundos (61 tramas). |
| -sc_threshold 0 | Inhabilitar la detección de escena mediante `ffmpeg`.<br/> Impide un segundo proceso que pudiera insertar keyframes superfluos en la salida. |
| -b:v 5300k | Establece la velocidad de bits de la corriente de vídeo de salida a 5300000 bits/segundo. |
| -maxrate 5300k | Limita la velocidad de bits máxima de vídeo de salida en<br/> el codificador a 5300000 bits/segundo, en caso de que varíe. |
| -bufsize 10600k | Establece el tamaño del almacenamiento intermedio del decodificador de vídeo `ffmpeg` a 10600000 bits.<br/>  Con una velocidad de bits 5300k, el codificador `ffmpeg` debería comprobar e <br/> intentar reajustar la velocidad de bits de salida otra vez a la velocidad de bits de destino para cada 2 segundos de vídeo. |
| -hls_time 6 | Intentar que la longitud de cada fragmento del vídeo de salida sea de 6 segundos.<br/> Acumula tramas de al menos 6 segundos de vídeo y <br/> se detiene para cortar un fragmento de vídeo cuando encuentra el siguiente keyframe. |
| -hls_playlist_type vod | Prepara el archivo de lista de reproducción de salida `.m3u8` para vídeo bajo demanda (vod). |
| test-video.m3u8 | Asignar al archivo de lista de reproducción/manifiesto de salida el nombre `test-video.m3u8`.<br/> Como resultado, `test-video0.ts`, `test-video1.ts`, `test-video2.ts`, ..., y similares,<br/> serán los nombres de los fragmentos del vídeo de forma predeterminada.|

Observe que para las opciones `-`, a menos que se especifique una secuencia, se elige la "mejor" para su categoría.

Por ejemplo:
  * `-c:a` elige la secuencia de audio con más canales.
  * `-c:v` elige la secuencia de vídeo con la resolución más alta.
  * `-c:a:0` elige la secuencia de audio 0.
  * `-c:v:0` elige la secuencia de vídeo 0.

En esta guía, solo la secuencia de audio 1 y la secuencia de vídeo 1 conforman el ejemplo `test-video.mp4`. Por lo tanto, la diferencia no representa un problema para continuar.

Luego puede ver varios archivos `.ts`.  También puede ver un archivo `.m3u8` parecido al siguiente:

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

Para usos más complejos, como escalar la resolución de vídeo, trabajar con subtítulos, utilizar el cifrado HLS AES en fragmentos de vídeo por motivos de seguridad y autorización, etc., `ffmpeg` tiene muchas más opciones de argumentos que manejan características más complejas y específicas. Encontrará descripciones de estos argumentos en la [documentación general de ffmpeg](https://ffmpeg.org/ffmpeg.html) y en su [documentación sobre formatos específicos, como HLS](https://ffmpeg.org/ffmpeg-formats.html#hls).

## Preparar el origen
{: #prepare-the-origin}

### Servidor
{: #server}

Si utiliza este servidor como origen adicional bajo un segundo dominio desde el que se transmiten estos HLS, es posible que tenga que configurar el servidor para que devuelva las cabeceras de respuesta de CORS para el posible acceso del navegador.

Puede colocar los archivos HLS en el directorio o subdirectorio que desee. En este ejemplo, coloquemos los archivos HLS bajo `/usr/share/nginx/hls/`.

La siguiente configuración de Nginx cubriría esto en el sentido básico:

```
# Configuraciones de este contexto principal...

http {

    # Configuraciones de este contexto http...

    server {

        # Configuraciones de host virtual...

        location /hls {
            root /usr/share/nginx/;

            # Solicitudes simples de CORS
            if ($request_method = 'GET') {
                add_header 'Access-Control-Allow-Origin' '*';
                add_header 'Access-Control-Expose-Headers' 'Content-Length, Content-Type';
            }

            # Solicitudes previas de CORS
            if ($request_method = 'OPTIONS') {
                add_header 'Access-Control-Allow-Origin' '*';
                add_header 'Access-Control-Allow-Methods' 'GET, OPTIONS';

                # Nota: es posible que no todos los navegadores den soporte al comodín `Access-Control-Allow-Headers`.
                add_header 'Access-Control-Allow-Headers' '*';

                add_header 'Access-Control-Max-Age' 1728000;
                add_header 'Content-Type' 'text/plain; charset=utf-8';
                add_header 'Content-Length' 0;
                return 204;
            }

            try_files $uri =404;
        }

        # Más configuraciones de host virtual...
    }

    # Más configuraciones de este contexto http...
}

# Más configuraciones de este contexto principal...
```
{: screen}

### Reproductor de vídeo en la página web
{: #video-player-on-the-webpage}

No todos los formatos de transferencia de vídeo se pueden reproducir de forma nativa en todas las aplicaciones. El ejemplo de esta guía configura la transferencia mediante HLS y CDN.

Por ejemplo, Safari da soporte a la reproducción HLS nativa. Por lo tanto, el reproductor de vídeo de la página web puede ser tan sencillo como el del ejemplo siguiente que utiliza elementos `<video>` HTML5:

```
<!DOCTYPE html>
<html>
  <!-- Algunos elementos HTML... -->
  
  <video src="https://cdn.example.com/hls/test-video.m3u8"></video>
  
  <!-- Más elementos HTML... -->
</html>
```
{: screen}

Sin embargo, otros navegadores de dispositivos de escritorio también pueden necesitar el soporte de [extensiones de origen de soporte](https://www.w3.org/TR/media-source/) de JavaScript añadidas, ya sea desarrolladas internamente o por un tercero de confianza, para generar secuencias de contenido que se puedan reproducir a través de HTML5.

## Configurar la CDN
{: #configure-the-cdn}

Ahora vamos a conectar el origen a la CDN para que sirva contenido en todo el mundo con rendimiento optimizado, latencia minimizada y mayor calidad.

En primer lugar, [solicite](/docs/infrastructure/CDN?topic=CDN-order-a-cdn) una CDN.

A continuación, [configure su CDN](/docs/infrastructure/CDN?topic=CDN-order-a-cdn#step-2-name-your-cdn) o [añada un origen](/docs/infrastructure/CDN?topic=CDN-order-a-cdn#step-3-configure-your-origin).

Finalmente, bajo `Optimizar para`, seleccione `Optimización de vídeo bajo demanda`.

![Optimizar para vídeo bajo demanda](images/optimize-for-video-on-demand.png)
