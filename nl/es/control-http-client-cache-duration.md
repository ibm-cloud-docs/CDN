---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-04"

keywords: cache control, cache-control, cache duration, max-age,  edge server, edge-level, respect header, HTTP client

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Utilización de Cache Control para controlar la duración en caché de un cliente HTTP
{: #using-cache-control-to-control-an-http-client-s-cache-duration}

Cuando se utiliza una CDN, están disponibles dos niveles de almacenamiento en memoria caché:

  * La **colocación en caché perimetral** se produce cuando un servidor perimetral de la CDN coloca en caché un elemento de contenido procedente del origen.
  * La **colocación en caché descendente** desde la red perimetral de servidores se produce cuando un usuario final o un cliente HTTP, como un navegador solicitante, coloca en caché un elemento de contenido procedente de un servidor perimetral.

El método que elija para controlar el tiempo que el contenido se almacena en la memoria en el solicitante, como por ejemplo un navegador, depende de los factores siguientes:

  * Si el [valor Respect Header](/docs/infrastructure/CDN?topic=CDN-manage-your-cdn#updating-cdn-configuration-details) está activado (ON) o desactivado (OFF). De forma predeterminada está activado.
  * Si el servidor de origen proporciona un valor `max-age` con la cabecera Cache-Control para un determinado elemento de contenido. 

Independientemente de cómo cambien estos factores, el origen debe proporcionar una cabecera Cache-Control para el contenido deseado al servidor perimetral, si desea que los servidores perimetrales envíen respuestas HTTP con la cabecera Cache-Control para ese contenido.

Básicamente, las cabeceras Cache-Control que se envían desde un servidor perimetral en sentido descendente piden al solicitante que almacene en memoria caché el contenido asociado de acuerdo con las directivas de almacenamiento en memoria caché o con los valores especificados por el servidor perimetral.

## Respect Header: Off
{: #respect-header-off}

Si su origen proporciona una cabecera Cache-Control con una directriz `max-age` y un valor para un elemento específico de contenido, entonces la duración en caché del elemento de contenido específico almacenado en la caché del servidor perimetral todavía deriva de los valores de TTL de su CDN. Además, el servidor perimetral responde al solicitante en sentido descendente con un valor de `max-age` de Cache-Control igual al menor de los siguientes:
  * El valor de `max-age` de Cache-Control del origen.
  * El tiempo restante hasta que el contenido quede obsoleto en el servidor perimetral.

Sin embargo, si su origen no proporciona una cabecera Cache-Control al servidor perimetral, los servidores perimetrales no proporcionarán una cabecera Cache-Control al solicitante. La duración en memoria caché en el servidor perimetral del contenido todavía se deriva de los valores de TTL de su CDN.

## Respect Header: On
{: #respect-header-on}

Si su origen proporciona una cabecera Cache-Control con una directriz `max-age` para un elemento de contenido determinado, el valor `max-age` se convierte en la duración en caché para dicho elemento de contenido específico en caché en el servidor perimetral, lo que sobrescribe cualquier valor de TTL aplicable para dicho elemento de contenido. Además, el servidor perimetral responde al solicitante con un valor de `max-age` de Cache-Control igual al tiempo restante hasta que el contenido queda obsoleto en el servidor perimetral.

Sin embargo, si su origen no proporciona una cabecera Cache-Control al servidor perimetral, el servidor perimetral no proporcionará una cabecera Cache-Control al solicitante. La duración en memoria caché en el servidor perimetral del contenido todavía se deriva de los valores de TTL de su CDN.

## Resumen
{: #summary}

|Respect Header|Origen proporciona Cache-Control|Duración en caché de contenido específico en servidor perimetral|Servidor perimetral proporciona Cache-Control|
|---|---|---|---|
|Activado|Si, el origen especifica un `max-age`|La duración en caché en servidor perimetral se sobrescribe con el valor `max-age` del origen|Sí, el servidor perimetral también proporciona un `max-age` con un valor que es el tiempo (sobrescrito) antes de que el servidor perimetral tenga que renovar el contenido desde el origen|
|Activado|Si, pero el origen no especifica un `max-age`|Duración en caché en servidor perimetral basada en la configuración de TTL de la CDN|Sí, el servidor perimetral también proporciona un `max-age` con un valor que es el tiempo antes de que el servidor perimetral tenga que renovar el contenido desde el origen|
|Activado|No|Duración en caché en servidor perimetral basada en la configuración de TTL de la CDN|No|
|Desactivado|Si, el origen especifica un `max-age`|Duración en caché en servidor perimetral basada en la configuración de TTL de la CDN|Sí, el servidor perimetral también proporciona un `max-age` que es el menor entre el valor `max-age` y el tiempo antes de que el servidor perimetral tenga que renovar el contenido desde el origen|
|Desactivado|Si, pero el origen no especifica un `max-age`|Duración en caché en servidor perimetral basada en la configuración de TTL de la CDN|Sí, el servidor perimetral también proporciona un `max-age` con un valor que es el tiempo antes de que el servidor perimetral tenga que renovar el contenido desde el origen|
|Desactivado|No|Duración en caché en servidor perimetral basada en la configuración de TTL de la CDN|No|

## Más información sobre el control de caché
{: #more-information-on-cache-control}

* Cómo [gestionar su CDN](/docs/infrastructure/CDN?topic=CDN-manage-your-cdn)
* Cache-Control tal como se define en la sección 14.9 de [RFC 2616 ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ietf.org/rfc/rfc2616.txt)
