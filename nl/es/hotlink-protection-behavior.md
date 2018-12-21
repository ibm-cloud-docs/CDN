---

copyright:
  years: 2017, 2018
lastupdated: "2018-11-01"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Clase Hotlink Protection

La `clase SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking` contiene los atributos que utilizan nuestras API de Hotlink Protection. Este objeto se utiliza para definir el comportamiento de Hotlink Protection para una CDN mediante una llamada a la API.  También lo devuelven las API de Hotlink Protection después de una llamada correcta a la API.

Clase `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`:

* `protectionType`: especifica si se debe permitir o denegar el acceso a contenido cuando una solicitud HTTP tiene un valor de Referer Header que coincide con uno de los términos de `refererValues`. Sucede lo contrario cuando no hay coincidencia.
  * Valor posible para protectionType:
    * `ALLOW`
    * `DENY`
* `refererValues`: es una lista de URL de referencia separados por un solo espacio, que también puede tener coincidencia de carácter comodín, en formato de serie de caracteres.
  * Estos son algunos ejemplos de serie **válida** para `refererValues`:
    * `alternate-domain.example.com`
    * `www1.example.com www2.example.com www3.example.com`
    * `www.example.com www.example.com/ www.example.com/path`
    * `*.example.com`
    * `www.example.*`
    * `*.example.com *.example.net`
    * `https*www.example.com`
  * Estos son algunos ejemplos de serie **no válida** para `refererValues`:
    * `www1.example.com www2.example.com www3.example.com www4.example.com www5.example.com`...
      * Contiene más de 2100 caracteres
    * `domain1.example.com domain1.example.com`
      * Contiene duplicados en la lista
    * ` `
      * refererValues vacío
    * `domain1.exa}mple.com domain1.example.com`
      * Utiliza uno o varios caracteres no especificados en [RFC-3986](https://tools.ietf.org/html/rfc3986#section-2)
    * `www.example.com/path&`
      * No se da soporte al carácter `&`
    * `www.example.org http://www.example.com`
      * No se da soporte a una serie `refererValues` que tenga al menos un término de coincidencia de URL que tenga un conjunto de caracteres antes del primer `.` que contiene `://`
