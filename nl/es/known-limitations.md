---

copyright:
  years: 2017, 2018
lastupdated: "2018-06-19"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Limitaciones conocidas

Las limitaciones siguientes corresponden al nuevo servicio de CDN para el proveedor Akamai:
* Actualmente no se admite la depuración de contenido a nivel de directorio o de varios archivos.
* Actualmente se permite un límite de 10 CDN en una cuenta de {{site.data.keyword.BluSoftlayer_notm}} determinada.
* El límite del número total de orígenes y entradas de TTL es de 75 por cada CDN.
* La opción Proporcionar contenido obsoleto siempre estará **Activa**.
* El tipo de certificado HTTPS no se puede cambiar una vez que se ha creado una correlación (por ejemplo, de comodín a SAN DV).
* HTTPS con el certificado SAN DV sólo está disponible para los nuevos CDN.
* Una CDN creada con un certificado SAN DV no se puede suprimir a menos que esté en un estado RUNNING, CNAME_Configuration, CREATE_ERROR o DELETE_ERROR.
