---

copyright:
  years: 2018, 2019
lastupdated: "2019-04-04"

keywords: rules, naming, conventions, hostname, cname, RFC, 1033, 1035, bucket, path, origin, purge, alphanumeric, top-level domain, valid, string

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:download: .download}

# Convenios y reglas de nomenclatura
{: #rules-and-naming-conventions}

## ¿Cuáles son las reglas para el nombre de host de CDN?
{: #what-are-the-rules-for-the-cdn-hostname}

La entrada para la serie del `Nombre de host` de CDN **debe**:
  * constar de caracteres alfanuméricos
  * ser inferior a 254 caracteres
  * finalizar por un nombre de dominio de nivel superior válido
  * **no** debe contener más de 10 etiquetas
  * **no** debe terminar en `cdnedge.bluemix.net` (ese sufijo se utiliza para los CNAME y está reservado)

Consulte RFC 1035, sección 2.3.4, para obtener más detalles. 

Además, le recomendamos encarecidamente que utilice un nombre de dominio completo como nombre de host de CDN. Elija un nombre con el formato `www.example.com` en lugar de un nombre de dominio raíz (también denominado como dominio Naked o de zona Apex), con el formato `example.com`. Necesitará crear un registro CNAME para el nombre de host de CDN que utilice. El DNS RFC 1033 requiere que el registro de dominio raíz sea un registro A, no un CNAME. Consulte RFC 2181, sección 10.1 para obtener más información.

## ¿Cuáles son las convenciones de nomenclatura de CNAME personalizado?
{: #what-are-the-custom-cname-naming-conventions}

La serie de entrada `CNAME` debe cumplir las siguientes reglas:
  * **debe** ser exclusivo (no puede estar en uso por ninguna otra CDN de IBM Cloud)
  * tener menos de 64 caracteres alfanuméricos
  * los caracteres especiales `! @ # $ % ^ & *` **no** están permitidos
  * los guiones bajos `_` **no** están permitidos
  * los puntos `.` **no** están permitidos
  * los guiones `-` están permitidos, pero CNAME no puede empezar ni terminar con un guión

## ¿Cuáles son las reglas para los nombres de grupo?
{: #what-are-the-rules-for-bucket-names}

Nombres de grupo:
  * **deben** tener al menos 1 carácter
  * deben tener menos de 200 caracteres
  * pueden contener letras (se permiten mayúsculas y minúsculas), números y guiones
  * **no** deben tener formato de dirección IP (por ejemplo, 127.0.0.1)
  * **no** pueden comenzar con un punto (.)
  * pueden finalizar con un punto (.)
  * Sólo se permite un punto (.) entre etiquetas (por ejemplo, my..ibmcloud.bucket no está permitido).

Aunque las letras mayúsculas y los puntos pueden pasar la validación, recomendamos utilizar siempre nombres de grupo compatibles con DNS.
{: note}

## ¿Cuáles son las reglas para las series de la vía de acceso para el origen?
{: #what-are-the-rules-for-the-path-string-for-the-origin}

La vía de acceso es opcional al crear la CDN. Sin embargo, si se proporciona, la vía de acceso **debe**:
  * ser inferior a 1000 caracteres
  * empezar por `/`

## ¿Cuáles son las reglas para la serie de vía de acceso para Depurar?
{: #what-are-the-rules-for-the-path-string-for-purge}

La vía de acceso de depuración:
  * debe tener menos de 1000 caracteres de longitud
  * debe empezar por `/`
  * no puede finalizar con `/`
  * no puede finalizar con un punto (.)
  * no puede contener `*`

La depuración sólo se permite para archivos individuales. La depuración a nivel de directorio no está soportada en este momento.
{; nota}

## En el mandato **Añadir origen**, ¿hay algunas reglas a seguir para la serie de la vía de acceso?
{: #for-the-add-origin-command-are-there-any-rules-to-follow-for-the-path-string}

En **Añadir origen**, la vía de acceso es **obligatoria**. También si la CDN se ha creado con una vía de acceso, la vía de acceso para **Añadir origen** debe empezar por la vía de acceso de la CDN como prefijo. Por ejemplo, si la vía de acceso de la CDN se ha especificado como `/storage`, para invocar **Añadir origen** con una vía de acceso llamada `/examplePath`, la vía de acceso proporcionada será `/storage/examplePath`.

## ¿Cuáles son las reglas para proporcionar extensiones de archivo?
{: #what-are-the-rules-for-providing-file-extensions}

Al crear un origen con Object Storage, las extensiones de archivo deben estar separadas por comas. Por ejemplo, `txt, jpg, xml` es una lista válida. Cualquier extensión determinada no puede tener una longitud de más de 10 caracteres.
