---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-03"

keywords: application programming interface, api, slapi, reference, development interface

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


# Referencia de la API de la CDN
{: #cdn-api-reference}

La interfaz de programación de aplicaciones de la infraestructura de {{site.data.keyword.cloud}} (habitualmente llamada SLAPI), proporcionada por {{site.data.keyword.cloud}}, es la interfaz de desarrollo que da a los desarrolladores y administradores del sistema una interacción directa con el sistema de fondo de la infraestructura de {{site.data.keyword.cloud_notm}}.

La SLAPI implementa muchas funciones en el Portal de clientes: si es posible una interacción en el Portal de cliente, también puede cumplirse en la SLAPI. Como puede interactuar con todas las porciones del entorno de la infraestructura de {{site.data.keyword.cloud_notm}} mediante programación, dentro de la SLAPI, puede utilizar la API para automatizar tareas.

La SLAPI es un sistema de llamada a procedimiento remoto (RPC). Cada llamada implica el envío de datos a un punto final de API y la recepción de datos estructurados. El formato utilizado para enviar y recibir datos con la SLAPI depende de qué implementación de la API utilice. Actualmente la SLAPI utiliza SOAP, XML-RPC o REST para la transmisión de datos.

Para obtener más información sobre la SLAPI, o sobre las API de servicio de {{site.data.keyword.cloud_notm}} Content Delivery Network (CDN), consulte los siguientes recursos en {{site.data.keyword.cloud_notm}} Development Network:

* [Descripción general de SLAPI](https://softlayer.github.io/ )
* [Iniciación a SLAPI](https://softlayer.github.io/article/getting-started/ )
* [SoftLayer_Product_Package API](https://softlayer.github.io/reference/services/SoftLayer_Product_Package/ )
* [Guía de API Soap PHP](https://softlayer.github.io/article/php/ )

----

Para empezar, aquí tiene una secuencia de llamada de API recomendada para seguir:
* `listVendors` - Proporciona la lista de proveedores soportados
* `verifyOrder` - Verifica si se puede realizar el pedido
* `placeOrder` - Crea la cuenta de la CDN con un determinado proveedor. Se pueden crear hasta 10 correlaciones de CDN después de una llamada placeOrder satisfactoria.
* `createDomainMapping` - Crea la correlación de CDN
* `verifyDomainMapping` - Cambia el estado de la CDN a _RUNNING_

Puede utilizar las otras API una vez ha seguido la secuencia anterior.

[El código de ejemplo está disponible para cada paso en esta secuencia de llamada.](/docs/infrastructure/CDN?topic=CDN-code-examples-using-the-cdn-api)

**Debe** utilizar el nombre de usuario de la API y la clave de API de un usuario con permiso `CDN_ACCOUNT_MANAGE` para la mayoría de las llamadas de API que se muestran en este documento. Compruebe con el usuario maestro de su cuenta si necesita permiso para habilitarlas (cada cuenta de cliente de IBM Cloud se proporciona con un usuario maestro).
{: note}

----
## API para proveedor
{: #api-for-vendor}

### listVendors
Esta API permite al usuario enumerar los proveedores de CDN soportados. `vendorName` es necesario para crear una cuenta de CDN y empezar con el pedido de la CDN.

* **Parámetros necesarios**: Ninguno
* **Devuelve**: una recopilación de tipo `SoftLayer_Container_Network_CdnMarketplace_Vendor`

  Aquí puede verse el contenedor de proveedor y un ejemplo de uso: [Proveedor de contenedor](/docs/infrastructure/CDN?topic=CDN-vendor-container)

----
## API para cuenta
{: #api-for-account}

### verifyCdnAccountExists
Comprueba si existe una cuenta CDN del usuario que llama a la API para el `vendorName` dado.

* **Parámetros necesarios**: `vendorName`: Proporcione el nombre de un proveedor de CDN válido.
* **Devuelve**: `true` si existe una cuenta, de lo contrario `false`.

----
## API para la correlación de dominios
{: #api-for-domain-mapping}

### createDomainMapping
Usando las entradas proporcionadas, esta función crea una correlación de dominios para el proveedor dado y la asocia con el ID de cuenta de la infraestructura de {{site.data.keyword.cloud_notm}} del usuario. La cuenta de CDN debe crearse primero utilizando `placeOrder` para que esta API funcione (vea un ejemplo de la llamada de API `placeOrder` en los [Ejemplos de código](/docs/infrastructure/CDN?topic=CDN-code-examples-using-the-cdn-api). Después de crear correctamente la CDN, se crea `defaultTTL` con un valor de 3600 segundos.

* **Parámetros**: una recopilación de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
  Aquí puede ver todos los atributos del Contenedor de entrada:

  [Ver el contenedor de entrada](/docs/infrastructure/CDN?topic=CDN-input-container)

  Los siguientes atributos forman parte del contenedor de entradas y pueden proporcionarse al crearse una correlación de dominios (los atributos son opcionales a menos que se indique lo contrario):
    * `vendorName`: **necesario** Proporcione el nombre de un proveedor de IBM Cloud CDN válido.
    * `origin`: **necesario** Proporcione una dirección de servidor de origen como una serie.
    * `originType`: **necesario** El tipo de origen puede ser `HOST_SERVER` u `OBJECT_STORAGE`.
    * `domain`: **necesario** Proporcione el nombre de host como una serie.
    * `protocol`: **necesario** Los protocolos soportados son `HTTP`, `HTTPS` o `HTTP_AND_HTTPS`.
    * `certificateType`: **obligatorio** para el protocolo HTTPS. `SHARED_SAN_CERT` o `WILDCARD_CERT`
    * `path`: vía de acceso desde la cual se servirá el contenido almacenado en memoria caché. La vía de acceso predeterminada es `/*`
    * `httpPort` y/o `httpsPort`: (**necesario** para el servidor de host) Estas dos opciones deben corresponder al protocolo deseado. Si el protocolo es `HTTP`, debe establecerse `httpPort` y `httpsPort` _no_ debe establecerse. Del mismo modo, si el protocolo es `HTTPS`, debe establecerse `httpsPort` y `httpPort` _no_ debe establecerse. Si el protocolo es `HTTP_AND_HTTPS`, _ambos atributos_ `httpPort` y `httpsPort` _deben_ estar establecidos. Akamai tiene ciertas limitaciones en los números de puerto. Consulte las [preguntas más frecuentes](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) para conocer los números de puerto permitidos.
    * `header`: especifica la información de cabecera de host utilizada por el servidor de origen.
    * `respectHeader`: un valor booleano que, si se establece en `true`, provocará que los valores de TTL en el origen sustituyan a los valores de TTL de CDN.
    * `cname`: proporcione un alias para el nombre de host. Se generará si no se proporciona ninguno.
    * `bucketName`: (**necesario** solo para Object Storage) Nombre de grupo para S3 Object Storage.
    * `fileExtension`: (opcional para Object Storage) Extensiones de archivo que se pueden almacenar en memoria caché.
    * `cacheKeyQueryRule`: están disponibles las siguientes opciones para configurar el comportamiento de la clave de caché. Si no se suministran argumentos `cacheKeyQueryRule`, se establecerá el valor predeterminado "include-all"
      * `include-all` - incluye todos los argumentos de consulta **predeterminados**
      * `ignore-all` - ignora todos los argumentos de consulta
      * `ignore: space separated query-args` - ignora argumentos de consulta específicos. Por ejemplo, `ignore: query1 query2`
      * `include: space separated query-args`: incluye argumentos de consulta específicos. Por ejemplo, `include: query1 query2`

* **Devuelve**: una recopilación de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`.

  **NOTA**: La recopilación proporciona un valor `uniqueId` que debe enviarse como entrada para las llamadas API posteriores relacionadas con la vía de acceso de origen y la correlación.

  [Ver el contenedor de correlaciones](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### deleteDomainMapping
Suprime la correlación de dominios basada en `uniqueId`. El estado de correlación de dominios debe ser uno de los siguientes: _RUNNING_, _STOPPED_, _DELETED_, _ERROR_, _CNAME_CONFIGURATION_ o _SSL_CONFIGURATION_.

* **Parámetros necesarios**: `uniqueId`: el ID exclusivo de la correlación que se va a suprimir
* **Devuelve**: una recopilación de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`
  [Ver contenedor de correlaciones](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### verifyDomainMapping
Verifica el estado de la CDN y actualiza el `estado` de la correlación de CDN si ha cambiado. Cuando se crea una correlación de CDN inicial, su estado se muestra como _CNAME_CONFIGURATION_. En este punto, debe actualizar el registro DNS para que la correlación de CDN apunte el nombre de host a CNAME. Consulte a su proveedor de DNS si tiene dudas sobre cómo se realiza la actualización y cuánto puede durar la propagación del cambio en Internet. En general, dura de 15 a 30 minutos. Una vez transcurrido este tiempo, esta API `verifyDomainMapping` debe llamarse para verificar si la cadena de CNAME está completa. Si la cadena de CNAME está completa, el estado de la correlación de CDN cambia al estado _RUNNING_.

Puede llamar a esta API en cualquier momento para obtener el estado más reciente de la correlación de la CDN. El estado de correlación de dominios debe ser uno de los siguientes: _RUNNING_ o _CNAME_CONFIGURATION_.

* **Parámetros necesarios**: `uniqueId`: ID exclusivo de la correlación que desea verificar
* **Devuelve**: una recopilación de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`

  [Ver el contenedor de correlaciones](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### startDomainMapping
Inicia una correlación de dominios de CDN basada en `uniqueId`. Para que se inicie, el estado de la correlación de dominios debe ser _STOPPED_.

* **Parámetros necesarios**: `uniqueId`: ID exclusivo de la correlación que se va a iniciar
* **Devuelve**: una recopilación de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`

  [Ver el contenedor de correlaciones](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### stopDomainMapping
Detiene una correlación de dominios de CDN basada en `uniqueId`. Para que se inicie la detención, el estado de la correlación de dominios debe ser _RUNNING_.

* **Parámetros necesarios**: `uniqueId`: ID exclusivo de la correlación que se va a detener
* **Devuelve**: una recopilación de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`

  [Ver el contenedor de correlaciones](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### updateDomainMapping
Habilita al usuario para que actualice las propiedades de la correlación identificadas con `uniqueId`. Pueden modificarse los campos siguientes: `originHost`, `httpPort`, `httpsPort`, `respectHeader`, `header`, argumentos `cacheKeyQueryRule`, y si el tipo de origen es Object Storage, también pueden cambiarse `bucketName` y `fileExtension`. Para que se produzca una actualización, el estado de la correlación de dominios debe ser _RUNNING_.

* **Parámetros**: una recopilación de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
  Puede ver todos los atributos del contenedor de entradas aquí: [Ver el contenedor de entradas](/docs/infrastructure/CDN?topic=CDN-input-container)

  Los siguientes atributos forman parte del contenedor de entradas y es **necesario** proporcionarlos al actualizar una correlación de dominios:
    * `vendorName`: Proporcione el nombre del proveedor de CDN para esta correlación.
    * `path`: Proporcione la vía de acceso actual para esta correlación
    * `origin`: Proporcione una dirección de servidor de origen como una serie.
    * `originType`: El tipo de origen puede ser `HOST_SERVER` u `OBJECT_STORAGE`.
    * `domain`: Proporcione el nombre de host.
    * `protocol`: Los protocolos soportados son `HTTP`, `HTTPS` o `HTTP_AND_HTTPS`.
    * `httpPort` y/o `httpsPort`: Estas dos opciones deben corresponder al protocolo deseado. Si el protocolo es `HTTP`, debe establecerse `httpPort` y `httpsPort` _no_ debe establecerse. Del mismo modo, si el protocolo es `HTTPS`, debe establecerse `httpsPort` y `httpPort` _no_ debe establecerse. Si el protocolo es `HTTP_AND_HTTPS`, _ambos atributos_ `httpPort` y `httpsPort` _deben_ estar establecidos. Akamai tiene ciertas limitaciones en los números de puerto. Consulte las [preguntas más frecuentes](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) para conocer los números de puerto permitidos.
    * `header`: especifica la información de cabecera de host utilizada por el servidor de origen.
    * `respectHeader`: un valor booleano que, si se establece en `true`, provocará que los valores de TTL en el origen sustituyan a los valores de TTL de CDN.
    * `uniqueId`: generado después de crear la correlación.
    * `cname`: proporcione el cname. Se genera uno cuando se crea la correlación si no ha proporcionado uno.
    * `bucketName`: (**necesario** solo para Object Storage) Nombre de grupo para S3 Object Storage.
    * `fileExtension`: (**necesario** solo para Object Storage) Extensiones de archivo que se pueden almacenar en memoria caché.
    * `cacheKeyQueryRule`: las reglas de comportamiento de clave de caché solo pueden actualizarse para las correlaciones de CDN creadas _después del_ 16/11/17. Las siguientes opciones están disponibles para configurar el comportamiento de clave de caché:
      * `include-all` - incluye todos los argumentos de consulta **predeterminados**
      * `ignore-all` - ignora todos los argumentos de consulta
      * `ignore: space separated query-args` - ignora argumentos de consulta específicos. Por ejemplo, `ignore: query1 query2`
      * `include: space separated query-args`: incluye argumentos de consulta específicos. Por ejemplo, `include: query1 query2`
* **Devuelve** una recopilación de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`

  [Ver el contenedor de correlaciones](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### listDomainMappings
Devuelve una recopilación de todas las correlaciones de dominio para el cliente actual.

* **Parámetros necesarios**: Ninguno
* **Devuelve**: una recopilación de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`

  [Ver el contenedor de correlaciones](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### listDomainMappingByUniqueId
Devuelve una recopilación con un objeto de dominio único basado en el `uniqueId` de una CDN.

* **Parámetros necesarios**: `uniqueId`: ID exclusivo de la correlación que se va a devolver
* **Devuelve**: una recopilación de un solo objeto de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`

  [Ver el contenedor de correlaciones](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
## API para origen
{: #apis-for-origin}

### createOriginPath
Crea una vía de acceso de origen para una CDN existente y un cliente determinado. La vía de acceso de origen puede basarse en un servidor de Host o en Object Storage. Para crear la vía de acceso de origen, el estado de la correlación de dominios debe ser _RUNNING_ o _CNAME_CONFIGURATION_.

* **Parámetros**: una recopilación de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
  Aquí puede ver todos los atributos del Contenedor de entrada:

  [Ver el contenedor de entrada](/docs/infrastructure/CDN?topic=CDN-input-container)

  Los siguientes atributos forman parte del contenedor de entradas y pueden proporcionarse al crearse una vía de acceso de origen (los atributos son opcionales a menos que se indique lo contrario):
    * `vendorName`: **necesario** Proporcione el nombre de un proveedor de IBM Cloud CDN válido.
    * `origin`: **necesario** Proporcione una dirección de servidor de origen como una serie.
    * `originType`: **necesario** El tipo de origen puede ser `HOST_SERVER` u `OBJECT_STORAGE`.
    * `domain`: **necesario** Proporcione el nombre de host como una serie.
    * `protocol`: **necesario** Los protocolos soportados son `HTTP`, `HTTPS` o `HTTP_AND_HTTPS`.
    * `path`: vía de acceso desde la cual se servirá el contenido almacenado en memoria caché. Debe empezar por la vía de acceso de correlación. Por ejemplo, si la vía de acceso de correlación es `/test`, la vía de acceso de origen debe ser `/test/media`
    * `httpPort` y/o `httpsPort`: **necesario** Estas dos opciones deben corresponder al protocolo deseado. Si el protocolo es `HTTP`, debe establecerse `httpPort` y `httpsPort` _no_ debe establecerse. Del mismo modo, si el protocolo es `HTTPS`, debe establecerse `httpsPort` y `httpPort` _no_ debe establecerse. Si el protocolo es `HTTP_AND_HTTPS`, _ambos atributos_ `httpPort` y `httpsPort` _deben_ estar establecidos. Akamai tiene ciertas limitaciones en los números de puerto. Consulte las [preguntas más frecuentes](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) para conocer los números de puerto permitidos.
    * `header`: especifica la información de cabecera de host utilizada por el servidor de origen.
    * `uniqueId`: **necesario** generado después de crear la correlación.
    * `cname`: proporcione un alias para el nombre de host. Si no proporciona un cname exclusivo, uno que se haya generado para usted al crear la correlación.
    * `bucketName`: (**necesario** para Object Storage) Nombre de grupo para S3 Object Storage.
    * `fileExtension`: (opcional para Object Storage) Extensiones de archivo que se pueden almacenar en memoria caché.
    * `cacheKeyQueryRule`: están disponibles las siguientes opciones para configurar el comportamiento de la clave de caché:
      * `include-all` - incluye todos los argumentos de consulta **predeterminados**
      * `ignore-all` - ignora todos los argumentos de consulta
      * `ignore: space separated query-args` - ignora argumentos de consulta específicos. Por ejemplo, `ignore: query1 query2`
      * `include: space separated query-args`: incluye argumentos de consulta específicos. Por ejemplo, `include: query1 query2`

* **Devuelve**: una recopilación de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`

  [Ver contenedor de vías de acceso de origen](/docs/infrastructure/CDN?topic=CDN-path-origin-container)

----
### updateOriginPath
Actualiza una vía de acceso de origen para una correlación existente y un cliente determinado. El tipo de origen no puede cambiarse con esta API. Se pueden cambiar las siguientes propiedades: `path`, `origin`, `httpPort` y `httpsPort`, `header` y argumentos `cacheKeyQueryRule`. Para que se actualice la correlación de dominios debe estar en estado _RUNNING_ o _CNAME_CONFIGURATION_.

* **Parámetros**: una recopilación de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
  Aquí puede ver todos los atributos del Contenedor de entrada:

  [Ver el contenedor de entrada](/docs/infrastructure/CDN?topic=CDN-input-container)

  Los siguientes atributos forman parte del contenedor de entradas y pueden proporcionarse al actualizar una vía de acceso de origen (los atributos son opcionales a menos que se indique lo contrario):
    * `oldPath`: **necesario** vía de acceso actual que se cambiará
    * `origin`: (**necesario** si se está actualizando) Proporcione una dirección de servidor de origen como una serie.
    * `originType`: **necesario** El tipo de origen puede ser `HOST_SERVER` u `OBJECT_STORAGE`.
    * `path`: **necesario** Nueva vía de acceso que se va a añadir. Relativa a la vía de acceso de correlación.
    * `httpPort` y/o `httpsPort`: (**necesario** para el servidor de host, si se está actualizando) Estas dos opciones deben corresponder al protocolo deseado. Si el protocolo es `HTTP`, debe establecerse `httpPort` y `httpsPort` _no_ debe establecerse. Del mismo modo, si el protocolo es `HTTPS`, debe establecerse `httpsPort` y `httpPort` _no_ debe establecerse. Si el protocolo es `HTTP_AND_HTTPS`, _ambos atributos_ `httpPort` y `httpsPort` _deben_ estar establecidos. Akamai tiene ciertas limitaciones en los números de puerto. Consulte las [preguntas más frecuentes](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) para conocer los números de puerto permitidos.
    * `uniqueId`: **necesario** ID exclusivo de la correlación a la que pertenece este origen
    * `bucketName`: (**necesario** solo para Object Storage) Nombre de grupo para S3 Object Storage.
    * `fileExtension`: (**necesario** solo para Object Storage) Extensiones de archivo que se pueden almacenar en memoria caché.
    * `cacheKeyQueryRule`: (**necesario** si se está actualizando) Las reglas de comportamiento de la clave de caché solo pueden actualizarse para las vías de acceso de origen creadas _después del_ 16/11/17. Las siguientes opciones están disponibles para configurar el comportamiento de clave de caché:
      * `include-all` - incluye todos los argumentos de consulta **predeterminados**
      * `ignore-all` - ignora todos los argumentos de consulta
      * `ignore: space separated query-args` - ignora argumentos de consulta específicos. Por ejemplo, `ignore: query1 query2`
      * `include: space separated query-args`: incluye argumentos de consulta específicos. Por ejemplo, `include: query1 query2`

* **Devuelve**: una recopilación de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`

  [Ver contenedor de vías de acceso de origen](/docs/infrastructure/CDN?topic=CDN-path-origin-container)

----
### deleteOriginPath
Suprime una vía de acceso de origen para una CDN existente y un cliente determinado. Para que se suprima la correlación de dominios debe estar en estado _RUNNING_ o _CNAME_CONFIGURATION_.

* **Parámetros necesarios**:
  * `uniqueId`: ID exclusivo de la correlación a la cual pertenece esta vía de acceso de origen
  * `path`: la vía de acceso que se va a suprimir

* **Devuelve**: un mensaje de estado si la supresión se ha realizado correctamente, de lo contrario se emite una excepción.

----
### listOriginPath
Lista las vías de acceso de origen para una correlación existente basándose en `uniqueId`.

* **Parámetros necesarios**:
  * `uniqueId`: proporcione el ID exclusivo de la correlación para la que desea listar las vías de acceso de origen.
* **Devuelve**: una recopilación de objetos de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`

  [Ver contenedor de vías de acceso de origen](/docs/infrastructure/CDN?topic=CDN-path-origin-container)

----
## API para la depuración
{: #api-for-purge}

### Clase de contenedor para la depuración:
```
class SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge
{
    /**
     * @var string
     */
    public $path;

    /**
     * @var string
     */
    public $status;

    /**
     * @var string
     */
    public $saved;

    /**
     * @var string
     */
    public $date;
}  
```

### createPurge
Crea un registro de depuración y lo inserta en la base de datos.

* **Parámetros**:
  * `uniqueId`: ID exclusivo de la correlación a la cual se va a crear la depuración
  * `path`: vía de acceso de la depuración que se va a crear

* **Devuelve**: una recopilación de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`

----
### getPurgeHistoryPerMapping
Devuelve el historial de depuraciones de una CDN basado en los estados `uniqueId` y `saved`. (De forma predeterminada, el valor de `saved` se establece en _UNSAVED_).

* **Parámetros**:
  * `uniqueId`: ID exclusivo de la correlación para la cual se recupera el historial de depuración
  * `saved`: devuelve depuraciones con estado _SAVED_ o _UNSAVED_

* **Devuelve**: una recopilación de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`

----
### saveOrUnsavePurgePath
Actualiza el estado de la entrada de vía de acceso de depuración para indicar si debe guardarse esta vía de acceso. Crea una nueva depuración `saved` si se guarda la vía de acceso de depuración. Suprime un registro de depuración guardado si la vía de acceso es `unsaved`.

* **Parámetros**:
  * `uniqueId`: ID exclusivo de la correlación a la que pertenece la depuración
  * `path`: vía de acceso de la depuración que se va a guardar o dejar de guardar
  * `saved`: _SAVED_ o _UNSAVED_

* **Devuelve**: una recopilación de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`

----
## API para el tiempo de duración
{: #api-for-time-to-live}

### Variables de la clase TimeToLive:  
```  
class SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive  
{
    /**
     * @var string
     */
    public $path;

    /**
     * @var int
     */
    public $timeToLive;

    /**
     * @var timestamp
     */
    public $createDate;
}
```  
### createTimeToLive
Crea un nuevo objeto `TimeToLive` y lo inserta en la base de datos.

 * **Parámetros**: `string` `uniqueId`, `string` `path`, `int` `ttl`
 * **Devuelve**: un objeto de tipo `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive`
___
### updateTtl
Actualiza un objeto `TimeToLive` existente. Si las entradas de _oldTtl_ y _newTtl_ son iguales, sale pronto.

 * **Parámetros**: `string` `uniqueId`, `string` `oldPath`, `string` `newPath`, `int` `oldTtl`, `int` `newTtl`
 * **Devuelve**: un objeto de tipo `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive`
___
### deleteTtl
Suprime un objeto `TimeToLive` existente de la base de datos.

 * **Parámetros**: `string` `uniqueId`, `string` `pathName`
 * **Devuelve**: una cadena con el estado de la supresión
___
### listTtl
Lista los objetos `TimeToLive` existentes según un valor de `uniqueId` de la CDN.

 * **Parámetros**: `string` `uniqueId`
 * **Devuelve**: una matriz de objetos de tipo `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive`

 ----
## API para métricas
{: #api-for-metrics}

[Visualizar el contenedor de métricas](/docs/infrastructure/CDN?topic=CDN-container-class-for-metrics)

### getCustomerUsageMetrics
Devuelve el número total de estadísticas predeterminadas para su visualización directa (sin gráficos), correspondientes a la cuenta de un cliente durante un periodo de tiempo determinado.

 * **Parámetros**:
   * `vendorName`
   * `startDate`
   * `endDate`
   * `frequency`

 * **Devuelve**: una recopilación de objetos de tipo `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingUsageMetrics
Devuelve el número total de estadísticas predeterminadas para su visualización directa, correspondientes a la correlación determinada. El valor de `frequency` es "aggregate" de forma predeterminada.

 * **Parámetros**:
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **Devuelve**: una recopilación de objetos de tipo `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingHitsMetrics
Devuelve el número total de aciertos en una frecuencia determinada durante un intervalo de tiempo dado, por correlación de dominios. La frecuencia puede ser diaria, semanal o mensual, en la que cada intervalo es un punto de gráfico. Los datos devueltos se ordenan según los valores de `startDate`, `endDate` y `frequency`. El valor de `frequency` es "aggregate" de forma predeterminada.

 * **Parámetros**:
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **Devuelve**: una recopilación de objetos de tipo `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingHitsByTypeMetrics
Devuelve el número total de aciertos en una frecuencia determinada durante un intervalo de tiempo dado. La frecuencia puede ser diaria, semanal o mensual, en la que cada intervalo es un punto de gráfico. Los datos devueltos deben ordenarse según los valores de `startDate`, `endDate` y `frequency`. El valor de `frequency` es "aggregate" de forma predeterminada.

 * **Parámetros**:
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **Devuelve**: una recopilación de objetos de tipo `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingBandwidthMetrics
Devuelve el número de aciertos en el perímetro en una CDN individual. Las regiones pueden diferir según el proveedor. Por correlación.

 * **Parámetros**:  
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **Devuelve**: una recopilación de objetos de tipo `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingBandwidthByRegionMetrics
Devuelve el número total de estadísticas predeterminadas para su visualización directa (sin gráficos), correspondientes a la correlación determinada durante un periodo de tiempo determinado. El valor de `frequency` es "aggregate" de forma predeterminada.

 * **Parámetros**:
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **Devuelve**: una recopilación de objetos de tipo `SoftLayer_Container_Network_CdnMarketplace_Metrics`

----
## API de Control de acceso geográfico
{: #api-for-geographical-access-control}

### createGeoblocking
Crea una nueva regla de control de acceso geográfico, y devuelve la regla recién creada.

  * **Parámetros**: una recopilación de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    Aquí puede ver todos los atributos del Contenedor de entrada:

    [Ver el contenedor de entrada](/docs/infrastructure/CDN?topic=CDN-input-container)

    Los siguientes atributos forman parte del contenedor de entrada y son **necesarios** cuando se crea una nueva regla de control de acceso geográfico:
    * `uniqueId`: ID exclusivo de la correlación para asignar a la regla
    * `accessType`: especifica si la regla permitirá (`ALLOW`) o denegará (`DENY` ) el tráfico a una región dada
    * `regionType`: tipo de región que se aplicará a la regla de control de acceso geográfico, `CONTINENT` o `COUNTRY_OR_REGION`
    * `regions`: una matriz con una lista de ubicaciones a las que se aplicará el `accessType`

      Consulte la página [`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`](/docs/infrastructure/CDN?topic=CDN-geoblocking-class) para ver una lista de las posibles regiones.

  * **Devuelve**: objeto del tipo `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`

    [Ver la clase de bloqueo geográfico](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)

----
### updateGeoblocking
Actualiza una regla de control de acceso geográfico existente para una correlación de dominio existente y devuelve la regla actualizada.

  * **Parámetros**: una recopilación de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    Aquí puede ver todos los atributos del Contenedor de entrada:

    [Ver el contenedor de entrada](/docs/infrastructure/CDN?topic=CDN-input-container)

    Los siguientes atributos forman parte del Contenedor de entrada y se pueden proporcionar al actualizar una regla de control de acceso geográfico (los parámetros son opcionales a menos que se indique lo contrario):
    * `uniqueId`: ID exclusivo **necesario** de la correlación a la que pertenece la regla a actualizar
    * `accessType`: especifica si la regla permitirá (`ALLOW`) o denegará (`DENY` ) el tráfico a una región dada.
    * `regionType`: tipo de región que se aplicará a la regla, `CONTINENT` o `COUNTRY_OR_REGION`
    * `regions`: una matriz con una lista de ubicaciones a las que se aplicará el `accessType`

      Consulte la página [`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`](/docs/infrastructure/CDN?topic=CDN-geoblocking-class) para ver una lista de las posibles regiones.

  * **Devuelve**: objeto del tipo `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`

    [Ver la clase de bloqueo geográfico](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)

----
### deleteGeoblocking
Elimina una regla de control de acceso geográfico existente de una correlación de dominio existente.

  * **Parámetros**: una recopilación de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    Aquí puede ver todos los atributos del Contenedor de entrada:

    [Ver el contenedor de entrada](/docs/infrastructure/CDN?topic=CDN-input-container)

    El siguiente atributo forma parte del contenedor de entrada y es **necesario** al suprimir una regla de control de acceso geográfico:
    * `uniqueId`: proporcione el ID exclusivo de la correlación a la que pertenece la regla a suprimir.

  * **Devuelve**: objeto del tipo `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`

    [Ver la clase de bloqueo geográfico](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)

----
### getGeoblocking
Recupera de la base de datos un comportamiento del control de acceso geográfico de una correlación.

  * **Parámetros**:
    * `uniqueId`: identificador exclusivo de la correlación a la que pertenece la regla.

  * **Devuelve**: un objeto del tipo `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`

    [Ver la clase de bloqueo geográfico](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)

----
### getGeoblockingAllowedTypesAndRegions
Devuelve una lista de los tipos y regiones que están permitidos para crear reglas de control de acceso geográfico.

  * **Parámetros**:
    * `uniqueId`: ID exclusivo de su correlación de dominio.

  * **Devuelve**: un objeto del tipo `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking_Type`

    [Ver la clase de bloqueo geográfico](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)
----
## API para protección de hotlink
{: #api-for-hotlink-protection}

### createHotlinkProtection
Crea una nueva protección de hotlink y devuelve el nuevo comportamiento creado.

  * **Parámetros**: una recopilación de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    Aquí puede ver todos los atributos del Contenedor de entrada:

    [Ver el contenedor de entrada](/docs/infrastructure/CDN?topic=CDN-input-container)

    Los siguientes atributos forman parte del contenedor de entrada y son **necesarios** cuando se crea una nueva protección de hotlink:
    * `uniqueId`: ID exclusivo de la correlación a la que asignar el comportamiento
    * `protectionType`: Especifica si permitir ("ALLOW") o denegar ("DENY") el acceso al contenido cuando una página web realiza una solicitud de contenido con una cabecera Referer que coincide con uno de los términos especificados en refererValues
    * `refererValues`: Una lista separada por un único espacio de términos coincidentes con el URL de Referer para los que tendrá efecto el comportamiento `protectionType`

      Consulte la página [`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class) para ver una lista de los valores válidos para la protección de hotlink.

  * **Devuelve**: un objeto del tipo `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`

    [Ver la clase de protección de hotlink](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class)

----
### updateHotlinkProtection
Actualiza un comportamiento de protección de hotlink existente para una correlación de dominio existente y devuelve el comportamiento actualizado.

  * **Parámetros**: una recopilación de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    Aquí puede ver todos los atributos del Contenedor de entrada:

    [Ver el contenedor de entrada](/docs/infrastructure/CDN?topic=CDN-input-container)

    Los siguientes atributos forman parte del contenedor de entrada y son **necesarios** cuando se actualiza una protección de hotlink existente:
    * `uniqueId`: ID exclusivo de la correlación a la que pertenece el comportamiento existente
    * `protectionType`: Especifica si permitir ("ALLOW") o denegar ("DENY") el acceso al contenido cuando una página web realiza una solicitud de contenido con una cabecera Referer que coincide con uno de los términos especificados en refererValues 
    * `refererValues`: Una lista separada por un único espacio de términos coincidentes con el URL de Referer para los que tendrá efecto el comportamiento `protectionType`

      Consulte la página [`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class) para ver una lista de los valores válidos para la protección de hotlink.

  * **Devuelve**: un objeto del tipo `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`

    [Ver la clase de protección de hotlink](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class)

----
### deleteHotlinkProtection
Elimina un comportamiento de protección de hotlink existente de una correlación de dominio existente.

  * **Parámetros**: una recopilación de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    Aquí puede ver todos los atributos del Contenedor de entrada:

    [Ver el contenedor de entrada](/docs/infrastructure/CDN?topic=CDN-input-container)

    Los siguientes atributos forman parte del contenedor de entrada y son **necesarios** cuando se crea una nueva protección de hotlink:
    * `uniqueId`: ID exclusivo de la correlación de la que eliminar el comportamiento

  * **Devuelve**: null

----
### getHotlinkProtection
Recupera el comportamiento de protección de hotlink actual de una correlación.

  * **Parámetros**:
    * `uniqueId`: ID exclusivo de la correlación a la que pertenece el comportamiento

  * **Devuelve**: un objeto del tipo `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`

    [Ver la clase de protección de hotlink](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class)
