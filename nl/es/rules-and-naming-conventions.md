---

copyright:
  years: 2018
lastupdated: "2018-05-07"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Convenios y reglas de nomenclatura

## ¿Cuáles son las reglas para el nombre de host?
La serie de entrada `Hostname` **debe**:
  * constar de caracteres alfanuméricos
  * ser inferior a 254 caracteres
  * finalizar por un nombre de dominio de nivel superior válido
  * **no** debe contener más de 10 etiquetas
  * **no** debe terminar en `cdnedge.bluemix.net` (ese sufijo se utiliza para los CNAME y está reservado)

Consulte RFC 1035, sección 2.3.4, para obtener más detalles.

## ¿Cuáles son las convenciones de nomenclatura de CNAME personalizado?
La serie de entrada `CNAME` debe cumplir las siguientes reglas:
  * **debe** ser exclusivo (no puede estar en uso por ninguna otra CDN de IBM Cloud)
  * tener menos de 64 caracteres alfanuméricos
  * los caracteres especiales `! @ # $ % ^ & *` **no** están permitidos
  * los guiones bajos `_` **no** están permitidos
  * los puntos `.` **no** están permitidos
  * los guiones `-` están permitidos, pero CNAME no puede empezar ni terminar con un guión

## ¿Cuáles son las reglas para los nombres de grupo?
Nombres de grupo:
  * **deben** tener al menos 1 carácter
  * deben tener menos de 200 caracteres
  * pueden contener letras (se permiten mayúsculas y minúsculas), números y guiones
  * **no** deben tener formato de dirección IP (por ejemplo, 127.0.0.1)
  * **no** pueden comenzar con un punto (.)
  * pueden finalizar con un punto (.)
  * Sólo se permite un punto (.) entre etiquetas (por ejemplo, my..ibmcloud.bucket no está permitido).

**NOTA**: aunque las letras mayúsculas y los puntos pueden pasar la validación, recomendamos utilizar siempre nombres de grupo compatibles con DNS.

## ¿Cuáles son las reglas para las series de la vía de acceso para el origen?
La vía de acceso es opcional al crear la CDN. Sin embargo, si se proporciona, la vía de acceso **debe**:
  * ser inferior a 1000 caracteres
  * empezar por `/`

## ¿Cuáles son las reglas para la serie de vía de acceso para Depurar?
La vía de acceso de depuración:
  * debe tener menos de 1000 caracteres de longitud
  * debe empezar por `/`
  * no puede finalizar con `/`
  * no puede finalizar con un punto (.)
  * no puede contener `*`

**NOTA**: La depuración sólo se permite para archivos individuales. La depuración a nivel de directorio no está soportada en este momento.

## En el mandato **Añadir origen**, ¿hay algunas reglas a seguir para la serie de la vía de acceso?
En **Añadir origen**, la vía de acceso es **obligatoria**. También si la CDN se ha creado con una vía de acceso, la vía de acceso para **Añadir origen** debe empezar por la vía de acceso de la CDN como prefijo. Por ejemplo, si la vía de acceso de la CDN se ha especificado como `/storage`, para invocar **Añadir origen** con una vía de acceso llamada `/examplePath`, la vía de acceso proporcionada será `/storage/examplePath`.

## ¿Cuáles son las reglas para proporcionar extensiones de archivo?
Al crear un origen con Object Storage, las extensiones de archivo deben estar separadas por comas. Por ejemplo, `txt, jpg, xml` es una lista válida. Cualquier extensión determinada no puede tener una longitud de más de 10 caracteres.
