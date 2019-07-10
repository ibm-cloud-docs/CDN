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


# Comment diffuser de la vidéo à la demande avec CDN
{: #how-to-serve-video-on-demand-with-cdn}

Dans ce guide, nous examinerons un exemple d'optimisation d'{{site.data.keyword.cloud}} CDN afin de diffuser du contenu `.mp4` via **HLS** en vidéo à la demande sur un navigateur à partir d'un serveur d'origine Linux-Nginx. 

## Introduction
{: #introduction}

De nombreux formats, tels que HLS, MPEG-DASH, etc., sont disponibles pour diffuser de la vidéo. 

Sur le plan conceptuel, nous utilisons la configuration illustrée dans le diagramme suivant :

![IBM Cloud CDN pour la vidéo à la demande](images/ibmcdn-vod-example-model.png)

Nous utiliserons également le serveur d'origine comme lieu de préparation. Il nous faudra également quelques modules pour effectuer ce travail.

Commençons par mettre à jour la liste de modules du serveur d'origine.
```
$ sudo apt-get update
```

## Préparation des fichiers vidéo
{: #prepare-video-files}

Dans ce guide, nous utiliserons `ffmpeg` pour préparer les fichiers vidéo. C'est un outil puissant dont les diverses commandes permettent de convertir, mutiplexer, démultiplexer, filtrer, etc., des fichiers multimédia.

Procurons-nous d'abord `ffmpeg`.
```
$ sudo apt-get -y install ffmpeg
```

HLS travaille avec deux types de fichier : `.m3u8` et `.ts`. Vous pouvez considérer le fichier `.m3u8` comme la "liste de lecture". Lorsque commence la diffusion vidéo, ce fichier est le premier extrait.  La liste de lecture indique ensuite au lecteur vidéo les fragments vidéo qu'il doit extraire et fournit d'autres données sur la manière de lire avec succès le contenu diffusé. Les fichiers `.ts` sont les "fragments" vidéo. Ces fragments sont extraits et lus par le lecteur vidéo selon les informations fournies dans la "liste de lecture".

A présent, vérifions et examinons le format, le débit binaire et d'autres informations, concernant les flux vidéo et audio de votre vidéo `.mp4` source.

```
$ ffprobe test-video.mp4 
```

Dans cet exemple, examinons les informations de flux suivantes pour `test-video.mp4` :
  * Flux vidéo 0
    * Format : h264
    * Profil de format : High
    * Résolution : 1920x1080
    * Débit binaire : 438 kb/s
    * Fréquence des images : 30,30 trames par seconde
  * Flux audio 0
    * Format : aac
    * Taux d'échantillonnage : 48000
    * Débit binaire : 128k

Nous allons maintenant convertir votre fichier `test-video.mp4` en formats pour HLS.

```
$ ffmpeg -i test-video.mp4 -c:a aac -ar 48000 -b:a 128k -c:v h264 -profile:v main -crf 23 -g 61 -keyint_min 61 -sc_threshold 0 -b:v 5300k -maxrate 5300k -bufsize 10600k -hls_time 6 -hls_playlist_type vod test-video.m3u8
```

Voici le détail de ce que cette commande a effectué :

|Argument(s)|Effet|
|:---|:---|
| ffmpeg | Utilisation de l'outil `ffmpeg`. |
| -i test-video.mp4 | La vidéo source se trouve dans `test-video.mp4`. |
| -c:a acc | Utilisation du codec audio acc pour la sortie. |
| -ar 48000 | Définition du taux d'échantillonnage audio sur 48000 Hz pour la sortie. |
| -b:a 128k | Définition du débit binaire audio sur 128000 octets par seconde pour la sortie. |
| -c:v h264 | Utilisation du codec vidéo `h.264` pour la sortie. |
| -profile:v main | Utilisation du profil de format "main" du codec sélectionné pour la plus large prise en charge de périphériques. |
| -crf 23 | Tentative de conservation de la qualité vidéo avec des tailles de fichier et débits binaires variables.<br/> Plus la valeur de CRF est élevée, plus la qualité est bonne et la taille de fichier élevée. |
| -g 61 -keyint_min 61 | Définition d'un maximum et d'un minimum.<br/> Avec une fréquence des images source dans l'exemple de 30,30, une image clé devrait être<br/> insérée toutes les 2 secondes (61 images). |
| -sc_threshold 0 | Désactivation de la détection des scènes par `ffmpeg`.<br/> Evite un second traitement qui pourrait insérer des images clés superflues dans la sortie. |
| -b:v 5300k | Définition du débit binaire cible du flux vidéo de sortie à 5300000 octets/seconde. |
| -maxrate 5300k | Limitation du débit binaire vidéo de sortie maximum au niveau<br/> du décodeur à 5300000 octets/seconde, au cas où il varie. |
| -bufsize 10600k | Définition de la taille de la mémoire tampon du décodeur vidéo `ffmpeg` sur 10600000 octets.<br/> Avec un débit binaire de 5300k, le décodeur `ffmpeg` devrait vérifier et tenter de réajuster <br/> le débit binaire de sortie au niveau du débit binaire cible toutes les 2 secondes de vidéo. |
| -hls_time 6 | Tentative de ciblage de la longueur de chaque fragment vidéo en sortie à 6 secondes.<br/> Accumule des trames pour au moins 6 secondes de vidéo, puis<br/> arrête de séparer un fragment vidéo lorsqu'il arrive à l'image clé suivante. |
| -hls_playlist_type vod | Préparation du fichier de liste de lecture `.m3u8` de sortie pour la vidéo à la demande (VOD). |
| test-video.m3u8 | Nom du fichier de liste de lecture/manifeste de sortie `test-video.m3u8`.<br/> Par conséquent, `test-video0.ts`, `test-video1.ts`, `test-video2.ts`, ..., et similaires<br/> seront les noms de fragment vidéo par défaut.|

Pour les options `-`, à moins qu'un flux soit spécifié, le "meilleur" de sa catégorie est sélectionné.

Par exemple :
  * `-c:a` sélectionne le flux audio ayant le plus de canaux.
  * `-c:v` sélectionne le flux vidéo ayant la résolution la plus élevée.
  * `-c:a:0` sélectionne le flux audio 0.
  * `-c:v:0` sélectionne le flux vidéo 0.

Dans ce guide, seul le flux audio 1 et le flux vidéo 1 sont indiquées dans l'exemple `test-video.mp4`. Par conséquent, la diversité n'est pas actuellement une préoccupation.

Dans l'avenir, vous pouvez vous attendre à voir un certain nombre de fichiers `.ts`.  En outre, vous verrez des fichiers `.m3u8` qui ressembleront sensiblement aux suivants :

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

Pour les cas d'utilisation plus complexes (par exemple, la mise à l'échelle de résolution vidéo, l'utilisation de sous-titres, le chiffrement AES HLS sur des fragments vidéo pour la sécurité et les autorisations, etc.) `ffmpeg` dispose de nombreuses autres options d'argument à même de prendre en charge les fonctionnalités les plus complexes et spécifiques. Vous trouverez la description de ces arguments dans la [documentation générale de ffmpeg](https://ffmpeg.org/ffmpeg.html) et dans sa [documentation sur les formats spécifiques tels que HLS](https://ffmpeg.org/ffmpeg-formats.html#hls).

## Préparation du serveur d'origine
{: #prepare-the-origin}

### Serveur
{: #server}

Si vous utilisez ce serveur en tant que serveur d'origine supplémentaire sous un second domaine à partir duquel diffuser ces fichiers HLS, vous devrez sans doute configurer le serveur de manière à renvoyer des en-têtes de réponse CORS pour les accès à des navigateurs potentiels.

Vous pouvez placer les fichiers HLS dans n'importe quel répertoire ou sous-répertoire qui vous convient. Pour cet exemple, plaçons les fichiers HLS dans `/usr/share/nginx/hls/`.

La configuration Nginx suivante répond fondamentalement à ce choix :

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

### Lecteur vidéo sur la page Web
{: #video-player-on-the-webpage}

Il est possible que les formats de flux vidéo ne soient pas tous lisibles en natif sur toutes les applications. L'exemple donné dans ce guide configure le flux à l'aide de HLS et CDN.

Par exemple, Safari prend en charge la relecture HLS native. Ainsi, le lecteur vidéo sur la page Web peut être aussi simple qu'indiqué dans l'exemple suivant qui utilise des éléments HTML5 `<video>` :

```
<!DOCTYPE html>
<html>
  <!-- Some HTML elements... -->
  
  <video src="https://cdn.example.com/hls/test-video.m3u8"></video>
  
  <!-- Some more HTML elements... -->
</html>
```
{: screen}

Toutefois, d'autres navigateurs sur des périphériques de bureau peuvent également nécessiter une prise en charge depuis un JavaScript ajouté d'[extensions de source média](https://www.w3.org/TR/media-source/), développé en interne ou par un tiers de confiance, afin de générer des flux de contenu lisibles via HTML5.

## Configuration du CDN
{: #configure-the-cdn}

Connectons maintenant le serveur d'origine au CDN afin de diffuser du contenu dans le monde entier avec débit optimal, une latence réduite et de meilleures performances.

Tout d'abord, [commandez](/docs/infrastructure/CDN?topic=CDN-order-a-cdn) un CDN.

Ensuite, [configurez votre CDN](/docs/infrastructure/CDN?topic=CDN-order-a-cdn#step-2-name-your-cdn) ou [ajoutez un serveur d'origine](/docs/infrastructure/CDN?topic=CDN-order-a-cdn#step-3-configure-your-origin).

Enfin, sous `Optimize For`, sélectionnez `Video on demand optimization`.

![Optimiser pour la vidéo à la demande](images/optimize-for-video-on-demand.png)
