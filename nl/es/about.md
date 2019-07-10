---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-03"

keywords: content delivery network, web content, caching, edge servers, streaming content

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Acerca de las redes de entrega de contenidos (CDN)
{: #about-content-delivery-networks-cdn-}

Una red de entrega de contenido (CDN) es un conjunto de servidores perimetrales que se distribuyen por diversas regiones del país o del mundo. El contenido web se proporciona desde un servidor perimetral, que se encuentra en el área geográfica más cercana al cliente que solicita el contenido. Esta técnica permite que los usuarios finales reciban el contenido con menos retraso, además de ofrecer una mejor experiencia a sus clientes.

## ¿Cómo funciona una CDN?
{: #how-does-a-cdn-work}

Una CDN logra su objetivo almacenando contenido en la memoria caché de servidores perimetrales en todo el mundo. Cuando un cliente solicita contenido web, la solicitud se direcciona al servidor perimetral más cercano al cliente desde el punto de vista geográfico. Al reducir la distancia que debe desplazarse el contenido, la CDN ofrece rendimiento optimizado, latencia minimizada y aumento de rendimiento. Al utilizar {{site.data.keyword.cloud}} Content Delivery Network con Akamai, los proveedores de contenidos garantizan la entrega eficiente del contenido solicitado en todo el mundo, con una configuración mínima.

![Diagrama CDN de alto nivel](images/high-level-cdn-diagram.png)

## Características
{: #features}

Las características principales del servicio {{site.data.keyword.cloud}} Content Delivery Network incluyen:
  * Soporte al origen de servidor de host
  * Soporte al origen de Object Storage
  * Soporte para múltiples orígenes con distintas vías de acceso
  * Correlaciones de CDN basadas en vías de acceso
  * Depuración de contenido almacenado en memoria caché
  * Tiempo de duración (TTL)
  * Métricas con vistas gráficas
  * Soporte de protocolo HTTPS con certificado comodín y SAN DV
  * Soporte a la cabecera de host
  * Proporcionar contenido obsoleto
  * Argumentos de consulta de claves de memoria caché
  * Compresión de contenido
  * Optimización de archivos de gran tamaño
  * Vídeo on Demand
  * Control de acceso geográfico
  * Hotlink Protection

Consulte [el documento de descripción de características](/docs/infrastructure/CDN?topic=CDN-feature-descriptions) para ver la descripción completa de las características.

## Diagrama arquitectónico
{: #architectural-diagram}

El siguiente diagrama ofrece una visión general esquemática de la arquitectura de tres niveles de la CDN de IBM Cloud.

![Diagrama arquitectural](images/3-tier-architecture.png)
