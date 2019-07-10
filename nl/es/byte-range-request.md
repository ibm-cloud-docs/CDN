---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-25"

keywords: byte range request, byte-range request, origin server, range HTTP request, transfer-encoding

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}


# Cómo trabajar con solicitudes de rango de bytes
{: #working-with-byte-range-requests}

Una solicitud de rango de bytes se utiliza para recuperar contenido parcial de un servidor de origen. Este documento puede ayudarle a comprender los códigos de estado de respuesta que puede ver.

Cuando se envía una **solicitud de rango de bytes** utilizando {{site.data.keyword.cloud}} CDN con Akamai, el usuario puede recibir un código de respuesta `200 (OK)` para la primera solicitud, y un código de respuesta `206` para todas las solicitudes posteriores, porque los servidores perimetrales de Akamai solicitan contenido del origen en formato comprimido. Por lo tanto, cuando un servidor perimetral no tiene un objeto en su memoria caché, ni tiene ninguna información sobre la longitud del contenido del objeto, va directamente al origen y solicita todo el objeto. A cambio, el origen proporciona el objeto sin la cabecera de longitud de contenido a Akamai, y el usuario final recibe el objeto completo aunque sea una solicitud de rango de bytes. Por lo tanto, el código de estado es `200`. En solicitudes posteriores, el servidor perimetral tiene el objeto en su memoria caché y proporcionará el código de estado `206`.

La cabecera de **solicitud HTTP de rango** indica qué parte del contenido debe devolver el servidor. Se pueden solicitar varias partes con una sola cabecera de rango a la vez, y el servidor puede devolver estos rangos en una respuesta en varias partes. Si el servidor devuelve los rangos, responde con un estado 206 (contenido parcial).

Un modo de garantizar una respuesta 206, incluso para la primera solicitud de rango de bytes, es inhabilitar `Transfer-Encoding: chunked` en el servidor de origen.
