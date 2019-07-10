---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-25"

keywords: metrics, bandwidth, overview, hit ratio, edge server, cache, ingress, hits

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Métricas
{: #metrics}

Cuando selecciona por primera vez la CDN de la lista, se abre la página Visión general. Aquí puede ver el ancho de banda total, el total de coincidencias y la proporción de coincidencias para el periodo de tiempo seleccionado (el valor predeterminado es de 30 días).

  ![Visión general de las métricas](images/metrics-overview.png)

Directamente por debajo de la visión general, verá una representación gráfica del ancho de banda total, el ancho de banda por región, el total de coincidencias y las coincidencias por tipo.

  ![Gráficas de métricas](images/metrics-graphs.png)

**NOTA**: después de crear la CDN, puede tardar 48 horas hasta que aparezcan las métricas.

## ¿Existe un número mínimo de días para los que puedo ver las métricas? ¿Hay un máximo?

Existe un número mínimo y máximo de días durante el cual puede visualizar las métricas utilizando {{site.data.keyword.cloud}} Content Delivery Network con Akamai. Se pueden recopilar métricas para un periodo mínimo de 7 días. Se pueden visualizar métricas para un periodo máximo de 90 días. A los que utilizan la API, se les recomienda usar 89 días como máximo para que se tengan en cuenta las diferencias de huso horario.

## ¿Por qué la proporción de coincidencias es distinta a cero cuando el total de coincidencias es cero?
La proporción de coincidencias representa el porcentaje de veces que el contenido se ha suministrado desde la memoria caché del servidor Edge, en lugar de desde el servidor de origen. Se calcula de la siguiente forma:

`((coincidencias Edge - coincidencias Ingress)/coincidencias Edge) * 100`

donde:

Las _coincidencias Edge_ se definen como "Todas las coincidencias a los servidores perimetrales desde los usuarios finales".  
Las _coincidencias Ingress_ se definen como "Las coincidencias de origen o Ingress son para el tráfico desde su origen a los servidores perimetrales de Edge".

Como la proporción de coincidencias se calcula a nivel de cuenta y no por CDN, la proporción de coincidencias será la misma para todas las CDN de su cuenta. Esto también explica por qué la proporción de coincidencias pueda ser distinta a cero cuando el número de coincidencias Edge para una CDN determinada sea cero.

## ¿Se actualizan las métricas en tiempo real?

No. Las métricas se actualizan cada 24 horas.
