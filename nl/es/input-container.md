---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-04"

keywords: input, container, class, API, mapping, origin, path, provider, hotlink

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Contenedor de entradas
{: #input-container}

El contenedor de entradas es una recopilación que utilizan las clases de Correlación y Vía de acceso (de origen). Proporciona un conjunto coherente de atributos de entrada para ambas clases.

* `vendorName`: el nombre de un proveedor válido de {{site.data.keyword.cloud}} CDN.
* `oldPath`: utilizado por updateOriginPath(). Esta propiedad almacena el nombre de la vía de acceso actual, o antigua.

Los siguientes atributos son comunes en las clases de Correlación y Vía de acceso (de origen):
* `originType`: tipo del host de origen, actualmente 'HOST_SERVER' u 'OBJECT_STORAGE'.
* `origin`: dirección del servidor de origen (nombre de host o la dirección IPv4 del servidor de origen), el punto final desde el que captar contenido, o el nombre del grupo donde se almacena el contenido. Debe ser inferior a 511 caracteres.
* `httpPort`: número del puerto utilizado para el protocolo HTTP. Akamai tiene ciertas limitaciones en los números de puerto para puertos HTTP y HTTPS. Consulte las [preguntas más frecuentes](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) para obtener una lista de los números de puerto permitidos.
* `httpsPort`: número del puerto utilizado para el protocolo HTTPS. Akamai tiene ciertas limitaciones en los números de puerto para puertos HTTP y HTTPS. Consulte las [preguntas más frecuentes](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) para obtener una lista de los números de puerto permitidos.
* `status`: el estado de la correlación o vía de acceso. El estado puede ser CNAME_CONFIGURATION, SSL_CONFIGURATION, RUNNING, STOPPED, DELETED o ERROR.
* `path`: vía de acceso desde la cual se servirá el contenido almacenado en memoria caché. La vía de acceso predeterminada es /\* Cuando la utiliza la API `updateOriginPath`, este atributo se refiere a la nueva vía de acceso que se añadirá.
* `performanceConfiguration`: especificaciones para la configuración del rendimiento de la correlación.
* `cacheKeyQueryRule`: están disponibles las siguientes opciones para configurar el comportamiento de la clave de caché:
  * `include-all`: incluye todos los argumentos de consulta
  * `ignore-all`: ignora todos los argumentos de consulta
  * `ignore: space separated query-args`: ignora argumentos de consulta específicos. Por ejemplo, `ignore: query1 query2`
  * `include: space separated query-args`: incluye argumentos de consulta específicos. Por ejemplo, `include: query1 query2`
* `geoblockingRule`

Los siguientes atributos son específicos de la clase Correlación:

* `uniqueId`: un identificador de 10 dígitos generado por el sistema, que es exclusivo para cada correlación. Se genera cuando se crea una correlación.
* `domain`: nombre de la CDN primaria. También denominado nombre de host.
* `protocol`: protocolo utilizado para configurar servicios. Puede ser HTTP, HTTPS o una combinación de ambos, HTTP_AND_HTTPS.
* `cname`: registro de nombre canónico da alias al nombre de host. Puede ser proporcionado por el usuario o generado por el sistema. Si es proporcionado por el usuario, debe tener menos de 64 caracteres alfanuméricos y ser exclusivo. Si no se proporciona, se generará uno cuando se crea la correlación.
* `certificateType`: tipo de certificado utilizado por una correlación. Puede ser `WILDCARD_CERT` o `SHARED_SAN_CERT`. El valor será 0 para correlaciones HTTP.
* `respectHeaders`: un valor booleano que, si se establece en 'true', provocará que los valores de TTL en el origen sustituyan a los valores de TTL de CDN.
* `header`: Especifica la información de cabecera de host utilizada por el origen.

Los siguientes atributos están relacionados con Cloud Object Storage (COS):  
* `bucketName`: nombre exclusivo del grupo para S3 Object Storage.  
* `fileExtension`: extensiones de archivo permitidas.

El atributo siguiente está relacionado con la configuración de Hotlink Protection:
* `hotlinkProtection`: consulte la [clase hotlink protection](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class) para ver más información.
