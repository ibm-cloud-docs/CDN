---

copyright:
  years: 2018
lastupdated: "2018-05-03"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Métricas

## ¿Cómo visualizo las métricas y el uso?

Puede ver las métricas y el uso en la página **Visión general**, a la que puede navegar seleccionando la CDN en la página **Redes de entrega de contenido**. **NOTA**: después de crear la CDN, puede tardar 48 horas hasta que aparezcan las métricas.

## He creado una CDN y había tráfico de datos a través de la CDN. ¿Por qué no aparecen mis métricas?

Después de crear una CDN, las métricas tardan 48 horas en aparecer.


## ¿Existe un número mínimo de días para los que puedo ver las métricas? ¿Hay un máximo?

Existe un número mínimo y máximo de días durante el cual puede visualizar las métricas utilizando IBM Cloud Content Delivery Network con Akamai. Se pueden recopilar métricas para un periodo mínimo de 7 días. Se pueden visualizar métricas para un periodo máximo de 90 días. A los que utilizan la API, se les recomienda usar 89 días como máximo para que se tengan en cuenta las diferencias de huso horario.

## ¿Por qué la proporción de coincidencias es distinta a cero cuando el total de coincidencias es cero?
La proporción de coincidencias representa el porcentaje de veces que el contenido se ha suministrado desde la memoria caché del servidor Edge, en lugar de desde el servidor de origen. Se calcula de la siguiente forma:

> ((Edge hits - Ingress hits)/Edge hits) * 100

donde:
Aciertos de Edge se define como "Todos los aciertos en los servidores de Edge de los usuarios finales"  
Aciertos de Ingress se define como "Aciertos de Origen o Ingress para el tráfico desde el origen a los servidores Edge de Akamai"

Como la proporción de coincidencias se calcula a nivel de cuenta y no por CDN, la proporción de coincidencias será la misma para todas las CDN de su cuenta. Esto también explica por qué la proporción de coincidencias pueda ser distinta a cero cuando el número de coincidencias Edge para una CDN determinada sea cero.

