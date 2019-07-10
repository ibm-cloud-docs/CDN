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

# CORS et requêtes CORS via votre CDN
{: #cors-and-cors-requests-through-your-cdn}

Le partage de ressources d'origine croisée (CORS) est un mécanisme qu'utilisent les navigateurs, principalement pour valider des droits d'accès à des contenus d'une origine différente.

## Qu'est-ce que CORS ?
{: #what-is-cors}

Lorsqu'un navigateur charge une page Web, il impose la **règle same-origin**, ce qui signifie que seul le contenu ayant la même origine que la page Web peut être extrait. Toutefois, dans certains cas, une page Web peut nécessiter un accès à des actifs d'origines multiples qui font confiance à ce site Web. C'est là qu'intervient CORS. 

Ce mécanisme de sécurité existe uniquement si une application a ou est un client HTTP, et si l'application implémente CORS. La plupart des navigateurs modernes, tels que Chrome, Firefox et Safari implémentent CORS.

Pour plus de clarté, **une origine en ce qui concerne CORS n'a pas à être identique à une origine CDN**. Dans CORS, une origine est définie par un schéma URI, un domaine et tout numéro de port possible. Par exemple l'origine `https://www.example.com:1443` est différente de `http://www.example.com`. Par conséquent, un CDN peut également être considéré comme une origine CORS du point de vue du navigateur.

## Fonctionnement de CORS

CORS peut gérer deux types de requête : les _requêtes simples_ et les _requêtes préliminaires_, qui sont plus complexes.

### Requêtes simples
{: #simple-requessts}

**Première requête (accès aux ressources) :**

![cors-simple](/images/cors-simple.png)

Les [requêtes simples ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.w3.org/TR/cors/#simple-cross-origin-request) via CORS sont des requêtes `GET` ou `POST` émises depuis la page Web d'une origine qui tente d'accéder à l'URL d'une autre origine pour atteindre des ressources.

Avec une requête de ce type, le navigateur définit automatiquement des en-têtes de requête CORS. Il définit principalement l'origine de la page Web qui émet la requête dans l'en-tête HTTP `Origin` automatiquement. La requête CORS peut également contenir un ensemble de [certains en-têtes HTTP standard![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.w3.org/TR/cors/#simple-header), tout en conservant sont statut de requête CORS simple, du point de vu du navigateur.

Le serveur qui reçoit la requête CORS la traite et peut (ou non) renvoyer un ensemble d'en-têtes de réponse CORS au navigateur, avec le contenu demandé. Ces en-têtes de réponse CORS contiennent des valeurs indiquant si la page Web en cours est autorisée à accéder à ces ressources, si les en-têtes envoyés sont acceptables pour la requête, etc.

Si les en-têtes de réponse CORS n'indiquent pas au navigateur que la requête CORS est satisfaite, celui-ci empêche automatiquement l'accès au contenu et son chargement. S'il constate que l'origine CORS a le droit d'utiliser la ressource, il autorise l'accès au contenu demandé et son chargement.

### Requêtes préliminaires
{: #preflighted-requests}

**Première requête (préliminaire) :**

![cors-preflight](/images/cors-preflight.png)

Seconde requête (accès aux ressources) :

![cors-after-preflight](/images/cors-after-preflight.png)

Pour une communication CORS plus complexe entre le navigateur et une origine CORS différente de celle de la page Web qui émet la demande, une [requête préliminaire![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.w3.org/TR/cors/#cross-origin-request-with-preflight-0) est nécessaire avant l'accès effectif aux ressources. Certaines situations peuvent exiger des requêtes CORS préliminaires, telles que des méthodes HTTP autres que `GET` ou `POST`, ou l'utilisation d'en-têtes HTTP non standard avec la requête même s'il s'agit d'une requête `GET` ou `POST`, etc.

Lorsqu'une requête préliminaire est nécessaire, voici comment se déroulent les événements :

* Le navigateur envoie une requête au serveur en utilisant la méthode HTTP OPTIONS, avec tous les en-têtes de requête CORS prévus. 
* Le serveur traite ces en-têtes de requête CORS et peut éventuellement renvoyer des en-têtes de réponse CORS ne contenant aucune donnée de contenu effective. 
* Le navigateur vérifie ces en-têtes de réponse CORS pour s'assurer que la requête CORS est autorisée. 
* Si le navigateur constate que le serveur doit autoriser la demande de ressource escomptée, il envoie une seconde requête au navigateur avec la méthode HTTP prévue, à savoir `GET`, `POST`, `PUT`, etc., avec les mêmes en-têtes de requête CORS.

Par la suite, la communication entre le navigateur et l'origine CORS (différente de celle de la page Web) s'effectuera comme s'il s'agissait d'une requête CORS simple. Comme avec une requête CORS simple, le contenu et les ressources sont accessibles et peuvent être chargés si la seconde requête CORS est également autorisée.

## Configuration de CORS à votre origine
{: #how-to-set-up-cors-at-your-origin}

Comme illustré dans les diagrammes précédents, CORS est initié par le client HTTP demandeur. Cependant, les répercussions dépendent de l'origine demandeuse. Pour que votre contenu soit prêt pour des requêtes CORS, votre origine doit être correctement configurée de manière à répondre avec les en-têtes de réponse CORS adéquats et les droits d'accès appropriés.

Exemple de configuration CORS de base pour un serveur Nginx :

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

En général, le navigateur doit charger librement du contenu lorsqu'il détecte un en-tête `Access-Control-Allow-Origin: *` dans les en-têtes de réponse CORS du serveur conformément à la [spécification w3 concernant cette valeur de caractère de remplacement![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.w3.org/TR/cors/#security). Cependant, les navigateurs ne prennent pas tous en charge `Access-Control-Allow-Origin: *`.

Si le serveur doit prendre en charge l'accès à de multiples pages Web, chacune provenant d'une origine différente, une seule valeur d'origine pour `Access-Control-Allow-Origin` doit être générée dynamiquement pour chaque requête. Exemple basique de ce type d'utilisation pour un serveur Nginx :

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

L'exemple précédent utilise la directive `map` pour éviter une surutilisation de l'instruction Nginx `if`. Désormais, lorsqu'une requête CORS qui correspond à ce chemin d'URI est envoyée à ce serveur, ce dernier répond avec l'en-tête `Access-Control-Allow-Origin` qui contient la valeur `http://www.example.com`, `https://cdn.example.com` ou `http://dev.example.com`, etc. lorsque le contenu est demandé depuis `http://www.example.com`, `https://cdn.example.com`, `http://dev.example.com`, etc.

## Configuration de CORS pour CDN
{: #how-to-set-up-cors-for-cdn}

![cors-through-cdn](/images/cors-through-cdn.png)
CDN est grandement transparent pour la configuration CORS de l'origine et ne nécessite donc pas une configuration CDN spécifique. Si le serveur d'équilibrage des charges CDN ne parvient pas à trouver une réponse en cache pour la première requête de contenu, il transmet la requête à l'hôte d'origine. Si l'hôte d'origine est configuré de manière à gérer des requêtes CORS et que la requête contient l'en-tête `Origin`, l'hôte doit répondre au serveur d'équilibrage des charges avec l'en-tête CORS `Access-Control-Allow-Origin` et la valeur associée. La réponse globale, y compris l'en-tête et la valeur, seront mis en cache dans le CDN. Toutes les requêtes ultérieures de l'objet au même chemin d'URI seront traitées depuis le cache et incluent la valeur d'en-tête `Access-Control-Allow-Origin` reçu au départ depuis l'origine.

## Traitement des incidents liés à CORS et aux requêtes CORS
{: #troubleshooting-cors-and-cors-requests}

Si votre serveur d'origine est configuré pour CORS et que l'en-tête `Access-Control-Allow-Origin` n'est pas renvoyé à la requête du navigateur, il est possible que l'en-tête de réponse mis en cache dans CDN concerne une requête qui ne contenait pas l'en-tête Origin. CDN met en cache les en-têtes de réponse depuis l'hôte d'origine. Toutefois, les en-têtes mis en cache sont basé sur la requête qui a déclenché la requête envoyée à l'origine. Dans ce cas, les en-têtes de réponse peuvent ne pas inclure les en-têtes CORS. Videz le cache dans CDN pour ce chemin à l'aide de la fonction Purge de CDN, puis envoyez de nouveau la requête depuis le client.

L'en-tête de réponse `Vary` de votre serveur d'origine peut également engendrer un comportement inattendu lors de l'extraction de contenu via votre CDN. A l'exception d'une exception très précise, le CDN ne met pas en cache un contenu (et son en-tête de réponse associé) depuis votre serveur d'origine si ce dernier répond avec un en-tête `Vary`. Actuellement, notre service supprime l'en-tête Vary de l'origine pour que l'objet puisse être mis en cache, si l'objet est l'un des types de fichier suivants : `aif, aiff, au, avi, bin, bmp, cab, carb, cct, cdf, class, css, doc, dcr, dtd, exe, flv, gcf, gff, gif, grv, hdml, hqx, ico, ini, jpeg, jpg, js, mov, mp3, nc, pct, pdf, png, ppc, pws, swa, swf, txt, vbs, w32, wav, wbmp, wml, wmlc, wmls, wmlsc, xsd, zip, webp, jxr, hdp, wdp, pict, tif, tiff, mid, midi, ttf, eot, woff, otf, svg, svgz, jar, woff2, json`. Si l'objet à mettre en cache n'est pas l'un de ces types de fichier, supprimez l'en-tête `Vary` de la réponse du serveur d'origine pour cet objet et faites une nouvelle tentative après avoir exécuté la commande Purge de CDN.

## Informations complémentaires
{: #more-information}

* [https://www.w3.org/TR/cors/ ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.w3.org/TR/cors/)
* [https://w3c.github.io/webappsec-cors-for-developers/ ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://w3c.github.io/webappsec-cors-for-developers/)
