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

# Límites y valores máximos
{: #limits-and-maximum-values}

## ¿Hay un valor máximo de Tiempo de duración? ¿Y un valor mínimo?

El valor máximo de Tiempo de duración es 2.147.483.647 segundos, que equivale a aproximadamente 68 años. El valor mínimo es de 0 segundos.

## ¿Existe un límite en el número de entradas de TTL y origen?

Sí, el límite combinado es 75 entradas por CDN.

## ¿Cuál es el tamaño de archivo máximo que se puede entregar a través de la CDN de Akamai?

Los intentos de recuperar o entregar archivos de más de 1,8 GB recibirán una respuesta `403 Acceso prohibido` para la configuración de rendimiento predeterminada. Si se habilita la optimización de archivos de gran tamaño, se pueden descargar archivos de hasta 320 GB.
