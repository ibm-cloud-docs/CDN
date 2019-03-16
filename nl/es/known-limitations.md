---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-02-19"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Limitaciones conocidas
{: #known-limitations}

Las limitaciones siguientes corresponden al servicio de CDN para el proveedor Akamai:
* Actualmente no se admite la depuración de contenido a nivel de directorio o de varios archivos.
* Existe un límite de un número total de 75 orígenes por CDN.
* HTTPS con el certificado SAN DV sólo está disponible para los nuevos CDN.
* Hotlink Protection no da soporte a los términos de coincidencia de URL `refererValues` si el carácter definido frente al primer carácter `.` contiene el grupo de caracteres `://`, como sucede en `http://www.example.com` o `www.example.com http://www.example.com`
