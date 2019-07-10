---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-31"

keywords: limits, maximum, values, time to live, entries, large file, size, optimization, downloads, years

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Límites y valores máximos
{: #limits-and-maximum-values}

## ¿Hay un valor máximo de Tiempo de duración? ¿Y un valor mínimo?
{: #is-there-a-maximum-value-for-time-to-live}

El valor máximo de Tiempo de duración es 2.147.483.647 segundos, que equivale a aproximadamente 68 años. El valor mínimo es de 0 segundos.

## ¿Existe un límite en el número de entradas de TTL y origen?
{: #is-there-a-limit-on-the-number-of-origin-and-ttl-entries}

Sí, el límite combinado es 75 entradas por CDN.

## ¿Cuál es el tamaño de archivo máximo que se puede entregar a través de la CDN de Akamai?
{: #what-is-the-largest-file-size-that-can-be-delivered-through-akamai-cdn}

Los intentos de recuperar o entregar archivos de más de 1,8 GB recibirán una respuesta `403 Acceso prohibido` para la configuración de rendimiento predeterminada. Si se habilita la optimización de archivos de gran tamaño, se pueden descargar archivos de hasta 320 GB.

## ¿Cuál es el tamaño máximo de las cabeceras de respuesta de origen?
{: #what-is-the-maximum-size-for-the-orignin-response-headers}

El tamaño máximo de las cabeceras de respuesta es 16 KB. En caso de que algunos orígenes intenten establecer cookies que sean demasiado grandes para que el cliente las devuelva en solicitudes subsiguientes, Akamai establece esta límite en los servidores perimetrales. Si las cabeceras de respuesta tiene más de 16 KB, la solicitud obtendrá una respuesta `502 Bad Gateway`.

## ¿Cuál es el tamaño máximo de las cabeceras de solicitud de cliente?
{: #what-is-the-maximum-size-for-the-client-request-headers}

El tamaño máximo de las cabeceras de solicitud es 32 KB. Si las cabeceras de solicitud tienen más de 32 KB, el servidor perimetral de Akamai devolverá una respuesta `400 Bad Request`.
