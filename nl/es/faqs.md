---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-05-21"

keywords: faqs, content delivery network, cname, configuration, status, ports, hotlink protection, error state, file path, cloud object storage, security, console, main page, create

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:faq: data-hd-content-type='faq'}
{:DomainName: data-hd-keyref="DomainName"}

# Preguntas más frecuentes
{: #faqs}

## ¿Qué es una red de entrega de contenido (CDN)?
{: #what-is-a-content-delivery-network-cdn}
{: faq}

Una red de entrega de contenido (CDN) es un conjunto de servidores perimetrales que se distribuyen por diversas regiones del país o del mundo. Su contenido web se proporciona desde un servidor perimetral, que se encuentra en el área geográfica más cercana al cliente que solicita el contenido. Con esta técnica, los usuarios reciben el contenido con menos retraso del que cabría esperar cuando se entrega el contenido desde una ubicación centralizada. Ofrece una mejor experiencia global a sus clientes.

## ¿Cómo funciona una red de entrega de contenido (CDN)?
{: #how-does-a-content-delivery-network-cdn-work}
{: faq}

Una CDN logra su objetivo almacenando contenido en la memoria caché de servidores perimetrales en todo el mundo. Cuando un usuario solicita contenido web, la solicitud se direcciona al servidor perimetral más cercano al usuario desde el punto de vista geográfico. Al reducir la distancia que debe desplazarse el contenido, la CDN ofrece rendimiento optimizado, latencia minimizada y aumento de rendimiento.

## ¿Cómo se crea mi cuenta de servicio de IBM Cloud Content Delivery Network?
{: #how-is-my-ibm-cloud-content-delivery-network-service-account-created}
{: faq}

La cuenta se crea durante el proceso de pedido de la CDN. Si está creando una CDN desde el portal antiguo, cuando pulsa el botón **Pedir CDN**, bajo **Red -> Página CDN**, se crea su cuenta. Si está creando una CDN desde el portal de {{site.data.keyword.cloud}}, al pulsar el botón **Crear**, bajo la página **Catálogo -> Red -> Content Delivery Network**, se crea su cuenta.

## ¿Qué debo hacer cuando mi CDN está en el estado de configuración de CNAME?
{: #what-do-i-do-when-my-cdn-is-in-cname-configuratione-status}
{: faq}

Para CDN HTTP y HTTPS basadas en certificados SAN, actualice el registro de DNS para que el sitio web apunte al `CNAME` asociado con la nueva correlación de CDN. Para CDN HTTPS basadas en certificados comodín, esta actualización de DNS **NO** es necesaria.

**Nota**: puede tardar entre 15 -30 minutos hasta que se active la actualización. Consulte a su proveedor de DNS el momento estimado.

## ¿Cómo añado un registro CNAME para mi dominio CDN en DNS?
{: #how-do-i-add-a-cname-record-for-my-cdn-domain-in-dns}
{: faq}

En la página de configuración de DNS para el dominio de CDN, puede crear un registro CNAME con el nombre de dominio CDN como Host y el CNAME de IBM que ha utilizado para configurar la CDN, como CNAME. El CNAME de IBM termina por `cdnedge.bluemix.net`.

Un registro CNAME típico se parecería a lo siguiente en la página de configuración de DNS:

| **Tipo de recurso** | **Host** | **Apunta a (CNAME)** | **TTL** |
|------------------|---------|-------------|----------------|
| CNAME | www.example.com | example.cdnedge.bluemix.net | 15 minutos |


## ¿Qué se me facturará por mi CDN?
{: #what-will-i-be-billed-for-in-my-cdn}
{: faq}

Solo se le facturará por el ancho de banda utilizado por instancia de IBM Cloud Content Delivery Network. Si la CDN no utiliza ancho de banda, no incurrirá en gastos. Los precios del ancho de banda varían en función de la ubicación regional del servidor perimetral. Puede consultar los precios del ancho de banda por región geográfica en el [documento de precios](/docs/infrastructure/CDN?topic=CDN-pricing) para este servicio.

## ¿Cuándo se me facturará por mi CDN?
{: #when-will-i-be-billed-for-my-cdn}
{: faq}

La facturación de IBM Cloud Content Delivery Network se produce según el periodo de facturación establecido en su cuenta de {{site.data.keyword.BluSoftlayer_notm}}.

## Si selecciono la opción `Suprimir` en el menú de desbordamiento de CDN, ¿se suprime mi cuenta?
{: faq}

No, solo se suprimirá esa CDN. Su cuenta seguirá existiendo y podrá crear CDN adicionales.

## ¿El almacenamiento de contenido en memoria caché utiliza la transferencia o la extracción?
{: #does-content-caching-use-push-or-pull}
{: faq}

El almacenamiento de contenido en memoria caché se realiza utilizando el modelo de _extracción de origen_. La extracción de origen es un método mediante el cual el servidor perimetral "extrae" datos del servidor de origen, en contraposición a la carga manual de contenido al servidor perimetral.

## ¿Se recomienda algún navegador para utilizar la configuración del servicio de la CDN?
{: #is-there-a-recommended-browser-to-use-for-cdn-service-configuration}
{: faq}

Sí, los navegadores recomendados son Firefox y Chrome. Se recomienda utilizar las últimas versiones con IBM Cloud Content Delivery Network.

## ¿Cuál es el objetivo de proporcionar una vía de acceso al crear la CDN?
{: #what-is-the-purpose-of-providing-a-path-when-creating-my-cdn}
{: faq}

Proporcionar una vía de acceso durante la creación de una CDN permite aislar los archivos que se pueden suministrar a través de la CDN desde un servidor de origen determinado.

## El estado de la CDN es Error. ¿Qué hago ahora?
{: faq}

Consulte las páginas [Resolución de problemas](/docs/infrastructure/CDN?topic=CDN-troubleshooting#troubleshooting) u [Obtención de ayuda y soporte](/docs/infrastructure/CDN?topic=CDN-gettinghelp#gettinghelp), o bien abra un tíquet en el [Portal de clientes ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://control.softlayer.com/).

## ¿Dónde puedo encontrar el CNAME de mi CDN si no he proporcionado ninguno?
{: faq}

Pulse sobre su CDN para acceder a la página **Visión general** en el Portal. En la esquina derecha, puede ver una sección **Detalles** con la información de `CName`.

## Mi solicitud de depuración para una vía de acceso de archivo determinada está en curso. ¿Puedo enviar una solicitud nueva para la misma vía de acceso de archivo?

No. Solo puede haber una solicitud de depuración activa a la vez para una vía de acceso de archivo.

## ¿Se admite Internet Protocol versión 6 (IPv6) con el servicio IBM Cloud Content Delivery Network? ¿Cómo funciona?
{: faq}

IPv6 (o soporte de pila dual) está soportada por los servidores Edge de Akamai. Está pensado para ayudar a los clientes con solo origen IPv4 a aceptar las conexiones de los clientes IPv6, convertir de IPv6 a IPv4 en Edge e ir al origen con IPv4.

**NOTA:** No se da soporte a la creación de una CDN de IBM Cloud utilizando una dirección IPv6 como dirección del servidor de origen.

## ¿Hay restricciones sobre qué números de puerto HTTP y HTTPS están permitidos en Akamai?
{: faq}

Sí. Para el proveedor Akamai, solo se admiten los siguientes números de puertos:
72, 80-89, 443, 488, 591, 777, 1080, 1088, 1111, 1443, 2080, 7001, 7070, 7612, 7777, 8000-9001, 9090, 9901-9908, 11080-11110, 12900-12949, 20410 y 45002.

## ¿Qué URL debe utilizarse para acceder a datos en la CDN o la vía de acceso de origen?
{: faq}

La vía de acceso para una correlación de CDN o para el origen se trata como directorio. Por lo tanto, los usuarios que intentan acceder a la vía de acceso de origen deben acceder a un directorio (con una barra inclinada). Por ejemplo, si la CDN `www.example.com` se crea mediante la vía de acceso que incluye el directorio `/images`, el URL para llegar a ella debe ser `www.example.com/images/`

Si omite la barra inclinada, por ejemplo, si utiliza `www.example.com/images` se producirá un error **No se ha encontrado la página**.

## ¿Cómo configuro mi red de entrega de contenido para IBM Cloud Object Storage (COS)?
{: faq}

[Aquí se incluye una guía de aprendizaje](/docs/tutorials?topic=solution-tutorials-static-files-cdn) sobre cómo crear una red de entrega de contenido para IBM Cloud Object Storage.

## He recibido una notificación de que mi certificado de origen caduca. ¿Qué hago ahora?
{: faq}

Siga los pasos indicados en [este artículo ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://community.akamai.com/docs/DOC-7708) de Akamai.

## ¿Qué seguridad se incluye con la solución IBM CDN con Akamai?
{: faq}

Con la plataforma Akamai distribuida, obtendrá una escalabilidad y una capacidad de recuperación sin precedentes, con miles de servidores en más de 50 países. La plataforma Akamai se sitúa entre su infraestructura y sus usuarios finales, y actúa como primer nivel de defensa en caso de aumento repentino del tráfico. Akamai Intelligent Platform es también un proxy inverso que escucha y responde a las solicitudes en puertos 80 y 443, lo que significa que el tráfico en otros puertos se desvía al extremo sin reenviarse a su infraestructura.

## ¿La CDN de Akamai conserva las cookies del servidor de origen?
{: faq}

Para el contenido que no puede colocarse en caché, o cualquier contenido que no se almacene en caché, las cookies se conservan desde el origen. Para el contenido que los servidores perimetrales colocan en caché, las cookies no se conservan.

## ¿Cómo utilizo la consola de IBM Cloud para dar permiso a otros usuarios para crear o gestionar una CDN?
{: faq}

El usuario maestro de la cuenta puede proporcionar a otros usuarios permiso para crear y gestionar una CDN.

Desde la página principal de la consola de IBM Cloud, siga estos pasos para editar los permisos:
 * Seleccione el separador **Gestionar**
 * Seleccione **Acceso (IAM)**
 * Pulse en el separador **Usuarios** del panel izquierdo
 * Pulse en el **Usuario** deseado
 * A continuación, seleccione el separador **Infraestructura clásica**
 * Después, en el separador **Permisos**, expanda la categoría **Servicios**
 * Seleccione **Gestionar cuenta de CDN**
 * Pulse el botón **Guardar**

Desde la página principal de la consola heredada, siga estos pasos para editar los permisos:
 * Seleccione el separador **Cuenta**
 * Seleccione **Usuarios -> Lista de usuarios**
 * Pulse en el **Nombre de usuario** que desee
 * A continuación, seleccione el separador **Permisos de portal**
 * Seleccione el separador **Servicios**
 * Seleccione **Gestionar cuenta de CDN**
 * Pulse el botón **Editar permisos de portal**

## ¿Por qué el botón Crear no aparece o está inhabilitado en la página https://cloud.ibm.com/catalog/infrastructure/cdn-powered-by-akamai?
{: faq}

Si es usted el usuario maestro de la cuenta, debe actualizarla para que aparezca el botón Crear habilitado en esta página. Desde la página de la consola de IBM Cloud, siga estos pasos como usuario maestro de la cuenta:
 * Abra el panel de navegación izquierdo pulsando el icono de la `triple barra` en la esquina superior izquierda de la página
 * Seleccione **Infraestructura clásica**
 * Pulse el botón **Actualizar cuenta** y siga las instrucciones

Si es usted uno de los usuarios secundarios de la cuenta, el usuario maestro debe darle el permiso para `Añadir/Actualizar servicios` para que el botón Crear aparezca habilitado en esta página. Desde la página de la consola de IBM Cloud, el usuario maestro de la cuenta tiene que seguir estos pasos para editar sus permisos:
 * Seleccione el separador **Gestionar**
 * Seleccione **Acceso (IAM)**
 * Pulse en el separador **Usuarios** del panel izquierdo
 * Pulse en el **Usuario** deseado
 * A continuación, seleccione el separador **Infraestructura clásica**
 * Después, en el separador **Permisos**, expanda la categoría **Cuenta**
 * Seleccione **Añadir/Actualizar servicios**
 * Pulse el botón **Guardar**

## ¿Por qué no puedo acceder a mi página web a través de mi CDN después de configurar Hotlink Protection con `protectionType` `ALLOW`?
{: faq}

Veamos un ejemplo en el que el dominio de su sitio web para usuarios finales se ha configurado para que sea el nombre de dominio/host de su CDN: `cdn.example.com`. Cuando alguien intenta acceder a una página web navegando directamente desde la barra de navegación del navegador, el navegador normalmente no envía cabeceras Referer en su solicitud HTTP. Por ejemplo, cuando navega directamente de este modo a `https://cdn.example.com/`, su CDN considera que la solicitud contiene una no coincidencia (non-match) con los `refererValues` especificados. Cuando la CDN evalúa el efecto o la respuesta apropiados a través de Hotlink Protection, determina que no hay coincidencia. Por lo tanto, su CDN denegará el acceso, en lugar de respectar el valor 'ALLOW'.

## ¿Puedo utilizar un punto final privado de Object Storage en los Valores de CDN?
{: faq}

No, CDN sólo se puede conectar a Object Storage en puntos finales públicos.

## ¿Puedo utilizar la característica Brotli en el servicio de CDN?
{: faq}

No, la característica Brotli no está soportada con nuestro servicio de CDN con Akamai.

## ¿Cómo puedo crear un punto final de CDN sin utilizar el dominio?
{: faq}

Puede crear un punto final de CDN sin utilizar el dominio, pero SÓLO para una CDN de tipo **HTTPS Comodín**. Al crear una CDN de tipo **HTTPS Comodín**, su **CNAME** actúa de punto final de la CDN, y el **CNAME** se utiliza para dar servicio al tráfico.
