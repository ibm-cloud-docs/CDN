---

copyright:
  years: 2017, 2018
lastupdated: "2018-08-30"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Utilización de `max-age` de Cache-Control para controlar la duración en caché del navegador

Cuando se utiliza una CDN, hay un tipo de almacenamiento en caché para cada ubicación en la que se puede almacenar en caché:
  * **Caché a nivel de navegador**: se da cuando el navegador coloca en la caché un elemento de contenido durante un periodo de tiempo específico.
  * **Caché a nivel perimetral**: se da cuando un servidor perimetral de la CDN coloca en caché un elemento de contenido del origen durante un periodo de tiempo, que no tiene que ser necesariamente el mismo.

Hay varios métodos para controlar el tiempo durante el que se almacena el contenido en caché a nivel de navegador. El método que se utilizará depende de los factores siguientes:
  * Si ha actualizado o [actualizará los detalles de configuración de la CDN](https://console.bluemix.net/docs/infrastructure/CDN/how-to.html#updating-cdn-configuration-details) con “Respect Header: On”.
  * Si el servidor de origen proporciona un valor `max-age` con la cabecera Cache-Control para un elemento dado de contenido. 

Independientemente de cómo cambien estos factores, su origen tiene que proporcionar una cabecera Cache-Control no vacía para el contenido de destino. Si su origen no proporciona una cabecera Cache-Control no vacía para el servidor perimetral, el servidor perimetral no proporcionará su propia Cache-Control al navegador.

Cuando el servidor perimetral envía una cabecera Cache-Control al navegador, el valor `max-age` para esta cabecera Cache-Control corresponde a la cantidad de tiempo restante antes que el contenido en el servidor perimetral sea inválido y necesite una nueva validación desde el origen. 

## Respect Header: Off
Cuando Respect Header se establece en **Off**, puede controlar el tiempo de caché a nivel de navegador configurando sus valores TTL de CDN. Para cada archivo o directorio de archivos, necesita crear una regla TTL para su vía de acceso y duración de caché deseada.

Esta configuración hace dos cosas:
  * La regla TTL indica al servidor perimetral que coloque en la caché el contenido durante el periodo de tiempo especificado.
  * Cuando el servidor perimetral sirve su contenido, envía al navegador su propia cabecera Cache-Control con un valor `max-age`, basado en la duración de TTL.

## Respect Header: On
Cuando **Respect Header** se establece en **On**, puede elegir no configurar una regla TTL para controlar el tiempo en caché a nivel de navegador para su contenido. En este caso, debe configurar el servidor de origen para que proporcione una cabecera Cache-Control con un valor `max-age` para un elemento de contenido dado. Puesto que Respect Header está activado, el servidor perimetral utiliza el valor `max-age` de Cache-Control del origen como la duración de TTL para ese contenido específico. Si ya tiene una entrada para la regla TTL para su CDN para el contenido, el servidor perimetral realmente todavía utilizará el valor `max-age` y sobrescribirá la duración en caché existente a nivel perimetral.

Esta configuración hace dos cosas:
  * El valor de la directiva `max-age` de Cache-Control desde el origen indica al servidor perimetral que coloque en la caché el contenido durante el periodo de tiempo especificado.
  * Cuando el contenido se sirve desde el servidor perimetral, este envía su propia cabecera Cache-Control al navegador con un valor `max-age` basado en el valor `max-age` desde el origen.

Ningún otro contenido para la regla TTL se ve afectado si está utilizando el método elegido mostrado en el ejemplo.

Cuando **Respect Header** se establece en **On**, la duración en la caché en el navegador se puede controlar con una regla TTL siempre que el servidor de origen no envíe la directiva `max-age` en la cabecera Cache-Control. Si especifica un valor de `max-age`, dicho valor podría sobrescribir accidentalmente el valor de tiempo de la regla TTL.

## Resumen

|Respetar cabecera|Origen proporciona Cache-Control|Duración en caché de contenido específico en servidor perimetral|Servidor perimetral proporciona Cache-Control|
|---|---|---|---|
|Activado|Si, especifica un `max-age`|Sobrescrito con el valor `max-age` del origen|Si, `max-age` es el tiempo antes de que sea necesario volver a validar en el contenido del perímetro desde el origen|
|Activado|Si, no especifica un `max-age`|Se basa en la configuración TTL de la CDN|Si, `max-age` es el tiempo antes de que sea necesario volver a validar en el contenido del perímetro desde el origen|
|Activado|No|Se basa en la configuración TTL de la CDN|No|
|Desactivado|Si, especifica un `max-age`|Se basa en la configuración TTL de la CDN|Si, `max-age` es el tiempo antes de que sea necesario volver a validar en el contenido del perímetro desde el origen|
|Desactivado|Si, no especifica un `max-age`|Se basa en la configuración TTL de la CDN|Si, `max-age` es el tiempo antes de que sea necesario volver a validar en el contenido del perímetro desde el origen|
|Desactivado|No|Se basa en la configuración TTL de la CDN|No|

## Más información
* Cómo [gestionar su CDN](https://console.bluemix.net/docs/infrastructure/CDN/how-to.html)
* Cache-Control tal como se define en la sección 14.9 del [RFC 2616](https://www.ietf.org/rfc/rfc2616.txt)
