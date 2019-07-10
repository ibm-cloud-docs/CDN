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


# Como entregar vídeo sob demanda com o CDN
{: #how-to-serve-video-on-demand-with-cdn}

Neste guia, exploraremos um exemplo de como alavancar o {{site.data.keyword.cloud}} CDN para transmitir o conteúdo `.mp4` por meio do **HLS** como vídeo sob demanda, para um navegador de uma origem Linux-Nginx. 

## Introdução
{: #introduction}

Vários formatos, como HLS, MPEG-DASH e assim por diante, estão disponíveis para transmitir vídeo. 

Conceitualmente, a configuração que estamos usando é mostrada no diagrama a seguir:

![IBM Cloud CDN para vídeo sob demanda](images/ibmcdn-vod-example-model.png)

Também usaremos a origem como o local para a preparação. Quanto a isso, também precisaremos obter alguns pacotes para fazer isso funcionar.

E assim, vamos começar atualizando a lista de pacotes da origem.
```
$ sudo apt-get update
```

## Preparar arquivos de vídeo
{: #prepare-video-files}

Neste guia, usaremos `ffmpeg` para preparar os arquivos de vídeo. É uma ferramenta poderosa para arquivos multimídia que pode converter, multiplexar, demultiplexar, filtrar, e assim por diante, por meio de seus vários comandos.

Primeiro, obteremos `ffmpeg`.
```
$ sudo apt-get -y install ffmpeg
```

O HLS funciona com dois tipos de arquivos: `.m3u8` e `.ts`. É possível pensar no arquivo `.m3u8` como a "lista de execução". No início das conexões de vídeo, esse arquivo é o primeiro buscado.  Em seguida, a lista de execução informa ao reprodutor de vídeo sobre os fragmentos de vídeo que ele deve buscar e fornece outros dados sobre como reproduzir o conteúdo transmitido com êxito. Os arquivos `.ts` são os "fragmentos" de vídeo. Esses fragmentos são buscados e reproduzidos pelo reprodutor de vídeo de acordo com os detalhes fornecidos na "lista de execução".

Agora, vamos verificar e ver o formato, a taxa de bits e outras informações para os fluxos de vídeo e áudio do vídeo de origem `.mp4`.

```
$ ffprobe test-video.mp4 
```

Neste exemplo, vamos considerar as informações de fluxo a seguir para `test-video.mp4`:
  * Conexões de vídeo 0
    * Formato: h264
    * Perfil do formato: alto
    * Resolução: 1920 x 1080
    * Taxa de bits: 438 kb/s
    * Taxa de quadros: 30,30 fps
  * Fluxo de áudio 0
    * Formato: aac
    * Taxa de amostra: 48.000
    * Taxa de bits: 128 k

Agora, converteremos nosso arquivo `test-video.mp4` nos formatos para HLS.

```
$ ffmpeg -i test-video.mp4 -c:a aac -ar 48000 -b:a 128k -c:v h264 -profile:v main -crf 23 -g 61 -keyint_min 61 -sc_threshold 0 -b:v 5300k -maxrate 5300k -bufsize 10600k -hls_time 6 -hls_playlist_type vod test-video.m3u8
```

Aqui está o detalhamento do que esse comando fez:

|Argumento(s)|Efeito|
|:---|:---|
| ffmpeg | Use a ferramenta `ffmpeg`. |
| -i test-video.mp4 | O vídeo de origem está localizado em `test-video.mp4`. |
| -c:a acc | Use o codec de áudio acc para a saída. |
| -ar 48000 | Configure a taxa de amostra de áudio para 48000 Hz para a saída. |
| -b:a 128k | Configure a taxa de bits de áudio para 128000 bits/segundo para a saída. |
| -c:v h264 | Use o codec de vídeo `h.264` para a saída. |
| -profile:v main | Use o perfil de formato "principal" do codec selecionado para o suporte de dispositivo mais amplo. |
| -crf 23 | Tente manter a qualidade do vídeo com tamanho de arquivo e taxa de bits variáveis.<br/>  Quanto menor o CRF, maior a qualidade e o tamanho do arquivo. |
| -g 61 -keyint_min 61 | Configure um máximo e mínimo.<br/> Com a taxa de quadros de origem de amostra como 30,30, um quadro chave seria <br/> inserido a cada dois segundos (61 quadros). |
| -sc_threshold 0 | Desativar detecção de cena por `ffmpeg`.<br/> Evita um segundo processo que pode inserir quadros chaves externos na saída. |
| -b:v 5300k | Configura a taxa de bits de destino do fluxo de vídeo de saída para 5300000 bits/segundo. |
| -maxrate 5300k | Limita a taxa de bits máxima de vídeo de vídeo de saída no<br/> codificador para 5300000 bits/segundo, no caso de variar. |
| -bufsize 10600k | Configura o tamanho do buffer do decodificador de vídeo `ffmpeg` para 10600000 bits.<br/>  Com taxa de bits de 5300 k, o codificador `ffmpeg` deve verificar e <br/> tentar reajustar a taxa de bits de saída de volta para a taxa de bits de destino para cada dois segundos de vídeo. |
| -hls_time 6 | Tente destinar seis segundos para cada comprimento de fragmento de vídeo de saída.<br/> Acumula quadros por pelo menos seis segundos de vídeo e, em seguida,<br/> para interrompendo um fragmento de vídeo quando encontra o próximo quadro chave. |
| -hls_playlist_type vod | Prepara o arquivo de lista de execução `.m3u8` de saída para vídeo sob demanda (vod). |
| test-video.m3u8 | Nomeie o arquivo manifest/lista de execução de saída para `test-video.m3u8`.<br/> Como resultado, `test-video0.ts`, `test-video1.ts`, `test-video2.ts`, ... e semelhantes,<br/> serão os nomes do fragmento de vídeo por padrão.|

Observe que, para as opções `-`, a menos que um fluxo seja especificado, o "melhor" para sua categoria será escolhido.

Como:
  * `-c:a` escolhe o fluxo de áudio com a maioria dos canais.
  * `-c:v` escolhe as conexões de vídeo com a resolução mais alta.
  * `-c:a:0` escolhe o fluxo de áudio 0.
  * `-c:v:0` escolhe as conexões de vídeo 0.

Neste guia, apenas 1 fluxo de áudio e 1 uma conexão de vídeo compõem o exemplo `test-video.mp4`. E assim, a diferença não seria uma preocupação mais adiante.

Depois disso, é possível esperar ver vários arquivos `.ts`.  Além disso, é possível esperar ver um arquivo `.m3u8` que se pareça com o seguinte:

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

Para casos de uso mais complexos -- como o ajuste de escala de resolução de vídeo, o trabalho com subtítulos, a criptografia HLS AES em fragmentos de vídeo para segurança e autorização, e assim por diante -- `ffmpeg` tem muito mais opções de argumentos que manipulam os recursos mais complexos e específicos. É possível localizar descrições desses argumentos na [documentação geral do ffmpeg](https://ffmpeg.org/ffmpeg.html) e em sua [documentação sobre formatos específicos, como HLS](https://ffmpeg.org/ffmpeg-formats.html#hls).

## Preparar a origem
{: #prepare-the-origin}

### Servidor
{: #server}

Se você estiver usando esse servidor como uma origem adicional em um segundo domínio do qual transmitir esse HLS, poderá ser necessário configurar o servidor para retornar cabeçalhos de resposta do CORS para acesso potencial ao navegador.

É possível colocar os arquivos HLS em qualquer diretório ou subdiretório que você desejar. Para esse exemplo, vamos colocar os arquivos HLS em `/usr/share/nginx/hls/`.

A configuração Nginx a seguir cobriria isso no sentido básico:

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

### Reprodutor de vídeo na página da web
{: #video-player-on-the-webpage}

Nem todos os formatos de vídeo de fluxo podem ser nativamente reproduzíveis em todos os aplicativos. O exemplo neste guia configura o fluxo usando HLS e CDN.

Por exemplo, o Safari suportaria reprodução HLS nativa. E assim, o reprodutor de vídeo na página da web pode ser tão simples quanto o exemplo a seguir usando HTML5 `<video>`:

```
<!DOCTYPE html>
<html>
  <!-- Some HTML elements... -->
  
  <video src="https://cdn.example.com/hls/test-video.m3u8"></video>
  
  <!-- Some more HTML elements... -->
</html>
```
{: screen}

No entanto, outros navegadores em dispositivos de área de trabalho também poderão precisar do suporte de [Extensões de origem das mídias](https://www.w3.org/TR/media-source/) JavaScript incluídas, sejam elas desenvolvidas internamente ou por meio de um terceiro confiável, para gerar fluxos de conteúdo reproduzíveis por meio de HTML5.

## Configurar o CDN
{: #configure-the-cdn}

Agora, vamos conectar a origem ao CDN para servir entregar em todo o mundo com rendimento otimizado, latência minimizada e maior desempenho.

Primeiro, [peça](/docs/infrastructure/CDN?topic=CDN-order-a-cdn) um CDN.

Em seguida, [configure seu CDN](/docs/infrastructure/CDN?topic=CDN-order-a-cdn#step-2-name-your-cdn) ou [inclua uma origem](/docs/infrastructure/CDN?topic=CDN-order-a-cdn#step-3-configure-your-origin).

Finalmente, em `Optimize for`, selecione `Video on demand optimization`.

![Otimizar para vídeo sob demanda](images/optimize-for-video-on-demand.png)
