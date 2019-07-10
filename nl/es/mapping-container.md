---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-04"

keywords: mapping, container, class, collection, object, API

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}  

# Contenedor de correlaciones
{: #mapping-container}

La recopilación `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` contiene los atributos utilizados por nuestras API de correlación. Cada API de correlación devuelve una recopilación de este tipo.

**clase** `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`:

* `vendorName`: el nombre de un proveedor válido de {{site.data.keyword.cloud}} CDN.
* `uniqueId`: un identificador de 10 dígitos generado por el sistema, que es exclusivo para cada correlación. Se genera cuando se crea una correlación.
* `domain`: nombre de la CDN primaria. También denominado `nombre de host`.
* `protocol`: protocolo utilizado para configurar servicios. Puede ser HTTP, HTTPS o una combinación de ambos, HTTP_AND_HTTPS.
* `originType`: tipo del host de origen, actualmente 'HOST_SERVER' u 'OBJECT_STORAGE'.
* `originHost`: dirección del servidor de origen (nombre de host o la dirección IPv4 del servidor de origen), que es el punto final desde el que captar contenido, o el nombre del grupo donde se almacena el contenido. Debe ser inferior a 511 caracteres.
* `httpPort`: número del puerto utilizado para el protocolo HTTP. Akamai tiene ciertas limitaciones en los números de puerto para puertos HTTP y HTTPS. Consulte las [preguntas más frecuentes](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) para conocer los números de puerto permitidos.
* `httpsPort`: número del puerto utilizado para el protocolo HTTPS. Akamai tiene ciertas limitaciones en los números de puerto para puertos HTTP y HTTPS. Consulte las [preguntas más frecuentes](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) para conocer los números de puerto permitidos.
* `certificateType`: tipo de certificado utilizado por una correlación. Puede ser `WILDCARD_CERT` o `SHARED_SAN_CERT`. El valor será 0 para correlaciones HTTP.
* `cname`: registro de nombre canónico que da alias al nombre de host. Puede ser proporcionado por el usuario o generado por el sistema. Si es proporcionado por el usuario, debe tener menos de 64 caracteres alfanuméricos y ser exclusivo. Si no se proporciona, se generará uno cuando se crea la correlación.
* `respectHeaders`: un valor booleano que, si se establece en 'true', provocará que los valores de TTL en el origen sustituyan a los valores de TTL de CDN.
* `header`: especifica la información de cabecera de host utilizada por el servidor de origen.
* `status`: el estado de la correlación. El estado puede ser CNAME_CONFIGURATION, SSL_CONFIGURATION, RUNNING, STOPPED, DELETED o ERROR.
* `bucketName`: Nombre de grupo para S3 Object Storage.
* `fileExtension`: extensiones de archivo que se pueden almacenar en memoria caché.
* `path`: la vía de acceso de S3 Object Storage. Normalmente, se encuentra en la sección API o URL del almacén de objetos del servicio.
* `cacheKeyQueryRule`: están disponibles las siguientes opciones para configurar el comportamiento de la clave de caché:
  * `include-all`: incluye todos los argumentos de consulta **predeterminados**
  * `ignore-all`: ignora todos los argumentos de consulta
  * `ignore: space separated query-args`: ignora argumentos de consulta específicos. Por ejemplo, "ignore: query1 query2"
  * `include: space separated query-args`: incluye argumentos de consulta específicos. Por ejemplo, "include: query1 query2"

Cabe destacar `uniqueId`, que se genera cuando se crea una correlación y se utiliza como parámetro para las llamadas de API posteriores.

Por ejemplo, esta llamada a `listDomainMappingByUniqueid`  
```php  
$cdnMapping = $client->listDomainMappingByUniqueid('750352919747xxx');  
```

devolverá un objeto similar a este:

```php  
[0] => SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping Object
                (
            [cacheKeyQueryRule] => include-all
            [certificateType] => NO_CERT
            [cname] => api-testing.cdnedge.bluemix.net
            [domain] => api-testing.cdntesting.net
            [header] => origin.cdntesting.net
            [httpPort] => 80
            [httpsPort] =>
            [originHost] => origin.cdntesting.net
            [originType] => HOST_SERVER
            [path] => /media/
            [performanceConfiguration] => General web delivery
            [protocol] => HTTP
            [respectHeaders] => 1
            [serveStale] => 1
            [status] => RUNNING
            [uniqueId] => 750352919747xxx
            [vendorName] => akamai
        )

```
{: codeblock}

Las llamadas a `stopDomainMapping` y `startDomainMapping` devolverán el mismo objeto de correlación, siendo el `status` la diferencia.

```php  
[0] => SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping Object
                (
          ...

            [status] => STOPPED
            [uniqueId] => 750352919747xxx

          ...
        )

```
{: codeblock}

```php  
[0] => SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping Object
                (
          ...

            [status] => RUNNING
            [uniqueId] => 750352919747xxx

          ...
        )

```
{: codeblock}
