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

# Solicitudes CORS y CORS a través de su CDN
{: #cors-and-cors-requests-through-your-cdn}

Cross Origin Resource Sharing (CORS) es un mecanismo que utilizan los navegadores, principalmente para validar permisos de acceso a contenido procedente un otro diferente.

## ¿Qué es CORS?
{: #what-is-cors}

Cuando un navegador carga una página web, aplica la **política de mismo origen**, lo que significa que solo permite que se capte contenido del mismo origen que la página web. Sin embargo, en algunos casos una página web puede tener que acceder a activos de diversos orígenes que confían en ese sitio web. Aquí es donde entra CORS. 

Este mecanismo de seguridad solo existe si una aplicación tiene o es un cliente HTTP y si dicha aplicación implementa CORS. Casi todos los navegadores modernos, como Chrome, Firefox y Safari, implementan CORS.

Para que quede claro, **un origen con respecto a CORS no tiene que ser el mismo que un origen de CDN**. Un origen en CORS se define mediante un esquema de URI, un dominio y cualquier número de puerto posible. Por ejemplo, `https://www.example.com:1443` es un origen distinto de `http://www.example.com`. Por lo tanto, una CDN también se puede considerar como un origen de CORS desde la perspectiva del navegador.

## Cómo funciona CORS

CORS puede manejar dos tipos de solicitudes: _solicitudes simples_ y _solicitudes previas_, que son más complejas.

### Solicitudes simples
{: #simple-requessts}

**Primera solicitud (acceso a recurso):**

![cors-simple](/images/cors-simple.png)

[Las solicitudes simples ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.w3.org/TR/cors/#simple-cross-origin-request) a través de CORS son solicitudes `GET` o `POST` procedentes de la página web de un origen que está intentando acceder al URL de otro origen para obtener recursos.

Cuando se realiza este tipo de solicitud, el navegador establece automáticamente las cabeceras de solicitud de CORS. En primer lugar, establece el origen de la página web que realiza la solicitud en la cabecera HTTP `Origin` de forma automática. La solicitud de CORS también puede contener un conjunto de [cabeceras HTTP determinadas y estándar ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.w3.org/TR/cors/#simple-header), mientras mantiene su estado como solicitud CORS simple, desde la perspectiva del navegador.

El servidor que recibe la solicitud de CORS procesará la solicitud y puede (o no) enviar un conjunto de cabeceras de respuesta de CORS al navegador, con el contenido solicitado. Estas cabeceras de respuesta de CORS contienen valores que especifican si la página web actual tiene permiso de acceso a dichos recursos, si las cabeceras enviadas son aceptables para dicha solicitud, etc.

Si el navegador no ve su solicitud de CORS satisfecha por parte de las cabeceras de respuesta de CORS, impide automáticamente el acceso al contenido e impide también que se cargue. De lo contrario, ve que el origen de CORS da permiso para utilizar el recurso, y permite el acceso y la carga del contenido solicitado.

### Solicitudes previas
{: #preflighted-requests}

**Primera solicitud (previa):**

![cors-preflight](/images/cors-preflight.png)

Segunda solicitud (acceso a recursos):

![cors-after-preflight](/images/cors-after-preflight.png)

En el caso de una comunicación CORS más compleja entre el navegador y un origen de CORS distinto de la página web de la solicitud, se necesita una [solicitud previa ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.w3.org/TR/cors/#cross-origin-request-with-preflight-0) antes de un acceso al recurso real. En algunas situaciones se pueden requerir solicitudes CORS previas, como métodos HTTP que no son métodos `GET` o `POST`,
o el uso de cabeceras HTTP no estándares con la solicitud, incluso si es una solicitud `GET` o `POST`, etc.

Si se necesita una solicitud previa, así es como se desarrollan los sucesos:

* El navegador envía una solicitud utilizando el método OPTIONS de HTTP al servidor con todas las cabeceras de solicitud CORS previstas. 
* El servidor procesa estas cabeceras de solicitud CORS y puede responder con cabeceras de respuesta CORS que no contengan datos de contenido real. 
* El navegador comprueba estas cabeceras de respuesta CORS para asegurarse de que se permite la solicitud de CORS. 
* Si el navegador ve que el servidor debe permitir la solicitud de recurso prevista, realiza una segunda solicitud al navegador con el método HTTP previsto, ya sea `GET`, `POST`, `PUT`, etc., con las mismas cabeceras de solicitud de CORS.

Después, la comunicación entre el navegador y el origen de CORS (distinto del de la página web) continúa como si se tratara de una solicitud CORS simple. Al igual que sucede en una solicitud CORS simple, el contenido y los recursos resultan accesibles y se pueden cargar si también se permite esta segunda solicitud CORS.

## Cómo configurar CORS en el origen
{: #how-to-set-up-cors-at-your-origin}

Tal como se ha mostrado en los diagramas anteriores, el cliente HTTP solicitante inicia CORS. Sin embargo, los efectos dependen del origen solicitado. Para que su contenido esté preparado para solicitudes CORS, el origen debe estar correctamente configurado de modo que responsa con las cabeceras de respuesta CORS correctas y los permisos de acceso correctos.

El ejemplo siguiente muestra una configuración básica de CORS para un servidor Nginx:

```nginx
http {
    # configuraciones de contexto http

    server {
        # configuraciones de contexto de servidor

        # vía de acceso de URI a contenido
        location /my-static-content {
        
            # configuraciones de contexto de ubicación
        
            # Manejar solicitudes simples
            #
            # Solo se tienen en cuenta solicitudes "HTTP GET" (captación previa de contenido)
            if ($request_method = 'GET') {
                
                # Permite al navegador acceder a datos procedentes de este servidor durante CORS,
                # solo si la solicitud procede de una página web con el siguiente origen
                add_header 'Access-Control-Allow-Origin' 'https://www.example.com';
            }

            # Manejar solicitudes previas
            if ($request_method = 'OPTIONS') {
                
                # Permite al navegador acceder a datos procedentes de este servidor durante CORS,
                # solo si la solicitud procede de una página web con el siguiente origen
                add_header 'Access-Control-Allow-Origin' 'https://www.example.com';
                
                # Solo permite solicitudes GET
                add_header 'Access-Control-Allow-Methods' 'GET';
                
                # Permite las siguientes cabeceras en las cabeceras de solicitud del navegador que
                # se puedan haber añadido que no sean lo que el navegador ha añadido automáticamente
                add_header 'Access-Control-Allow-Headers' 'pragma';
                
                # Indica al navegador el tiempo que debe mantener en caché esta respuesta previa
                add_header 'Access-Control-Max-Age' 1728000;
                
                # La respuesta 204 de HTTP significa éxito,
                # pero también que no se espera ningún contenido de esta respuesta (previa)
                return 204;
            }

            # más configuraciones de contexto de ubicación
            
            # Si no es una situación CORS compleja que requiera una respuesta previa,
            # finalmente se intenta servir el archivo cuya vía de acceso está dentro de la coincidencia de vía de acceso de URI del bloque de esta ubicación
            #
            # Si no se encuentra dicho archivo, se presenta un código 404 de HTTP (no encontrado)
            try_files $uri =404;
        }

        # más configuraciones de contexto de servidor
    }

    # más configuraciones de contexto de http
}
```
{: screen}

En general, el navegador carga libremente el contenido cuando ve `Access-Control-Allow-Origin: *` en las cabeceras de respuesta de CORS del servidor, de acuerdo con la [especificación w3 sobre este valor de carácter comodín ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.w3.org/TR/cors/#security). Sin embargo, no todos los navegadores dan soporte a `Access-Control-Allow-Origin: *`.

Si el servidor debe dar soporte al acceso procedente de varias páginas web, cada una servida desde un origen diferente, se debe generar de forma dinámica un solo valor de origen para `Access-Control-Allow-Origin` por solicitud. A continuación se muestra un ejemplo básico de un caso de uso de este tipo para un servidor Nginx:

```nginx
http {
    # configuración de contexto de http

    ######
    # Entrada de $http_origin,          el valor de la cabecera HTTP de origen
    # Salida a $cors_allowed_origin,   una serie
    #
    # Coincidencia de serie completa en el valor de cabecera Origin
    #
    # Intento de encontrar una coincidencia para el valor procedente de $http_origin utilizando regex en la columna izquierda.
    # Si se encuentra una coincidencia, evaluar $cors_allowed_origin con el mismo valor que $http_origin
    # Si no, el valor predeterminado es evaluar $cors_allowed_origin en una serie vacía para que los navegadores no permitan la solicitud CORS
    ######
    map $http_origin $cors_allowed_origin {
        default '';
        ~^http(s)?://(www|www2|cdn|dev)\.example\.com$ $http_origin;
    }
    
    server {
        # configuraciones de contexto de servidor

        # vía de acceso de URI a contenido
        location /my-static-content {
        
            # configuraciones de contexto de ubicación
        
            # Manejar solicitudes simples
            if ($request_method = 'GET') {
                add_header 'Access-Control-Allow-Origin' $cors_allowed_origin;
            }

            # Manejar solicitudes previas
            if ($request_method = 'OPTIONS') {
                add_header 'Access-Control-Allow-Origin' '$cors_allowed_origin;
                add_header 'Access-Control-Allow-Methods' 'GET';
                add_header 'Access-Control-Allow-Headers' 'pragma';
                add_header 'Access-Control-Max-Age' 1728000;
                
                return 204;
            }

            # más configuraciones de contexto de ubicación

            try_files $uri =404;
        }

        # más configuraciones de contexto de servidor
    }

    # más configuraciones de contexto de http
}
```
{: screen}

En el ejemplo anterior se utiliza la directriz `map` para evitar un uso excesivo de la sentencia Nginx `if`. Ahora, cuando se realiza una solicitud CORS a este servidor y coincide con esta vía de acceso de URI, el servidor responde con la cabecera `Access-Control-Allow-Origin` que contiene el valor `http://www.example.com`, `https://cdn.example.com` o `http://dev.example.com`, etc. cuando el contenido se solicita desde `http://www.example.com`, `https://cdn.example.com`, `http://dev.example.com`, etc.

## Cómo configurar CORS para CDN
{: #how-to-set-up-cors-for-cdn}

![cors-through-cdn](/images/cors-through-cdn.png)
CDN resulta en gran medida transparente para la configuración de CORS del origen, de modo que no requiere ninguna configuración especial de CDN. Si el servidor perimetral de CDN no encuentra una respuesta en caché para la primera solicitud de contenido, reenvía la solicitud al host de origen. Si el host de origen está configurado para manejar las solicitudes de CORS y esta solicitud tiene la cabecera `Origin`,
debería responder de nuevo al servidor perimetral con la cabecera CORS `Access-Control-Allow-Origin` y el valor asociado. La respuesta global, incluida la cabecera y el valor, se colocará en memoria caché en la CDN. Las siguientes solicitudes del objeto en la misma vía de acceso de URI se sirven desde la memoria caché e incluyen el valor de cabecera `Access-Control-Allow-Origin` que se ha recibido originalmente desde el origen.

## Resolución de problemas de CORS y de las solicitudes CORS
{: #troubleshooting-cors-and-cors-requests}

Si el servidor de origen está configurado para CORS y no ve que se devuelva la cabecera `Access-Control-Allow-Origin` a la solicitud del navegador, es posible que la cabecera de respuesta almacenada en memoria caché en CDN fuera para una solicitud que no tenía la cabecera Origin en la solicitud. CDN almacena en memoria caché las cabeceras de respuesta procedentes del host de origen. Sin embargo, las cabeceras almacenadas en memoria caché se basan en la solicitud que ha desencadenado la solicitud a Origin. En ese caso, es posible que las cabeceras de respuesta no incluyan las cabeceras CORS. Borre la memoria caché en la CDN para esa vía de acceso utilizando la función Purge de CDN y vuelva a intentar la solicitud procedente del cliente.

La cabecera de respuesta `Vary` desde el servidor de origen también puede provocar un comportamiento inesperado cuando se capta contenido a través de su CDN. Aparte de una excepción muy específica, la CDN no almacena en memoria caché contenido (y su cabecera de respuesta asociada) procedente de su servidor de origen, si el servidor responde con una cabecera `Vary`. Actualmente, nuestro servicio elimina la cabecera Vary del origen para mostrar el objeto de la memoria caché, si el objeto es uno de los siguientes tipos de archivos: `aif, aiff, au, avi, bin, bmp, cab, carb, cct, cdf, class, css, doc, dcr, dtd, exe, flv, gcf, gff, gif, grv, hdml, hqx, ico, ini, jpeg, jpg, js, mov, mp3, nc, pct, pdf, png, ppc, pws, swa, swf, txt, vbs, w32, wav, wbmp, wml, wmlc, wmls, wmlsc, xsd, zip, webp, jxr, hdp, wdp, pict, tif, tiff, mid, midi, ttf, eot, woff, otf, svg, svgz, jar, woff2, json`. Si el objeto que hay que almacenar en caché no tiene uno de estos tipos de archivo, elimine la cabecera `Vary` de la respuesta del servidor de origen para dicho objeto y vuélvalo a intentar después de utilizar la función Purge de CDN.

## Más información
{: #more-information}

* [https://www.w3.org/TR/cors/ ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.w3.org/TR/cors/)
* [https://w3c.github.io/webappsec-cors-for-developers/ ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://w3c.github.io/webappsec-cors-for-developers/)
