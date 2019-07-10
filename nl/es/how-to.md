---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-05-21"

keywords: manage, time to live, origin path, cache key, server, object storage, bucket, configuration, details, updating

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
{:DomainName: data-hd-keyref="DomainName"}

# Gestión de la CDN
{: #manage-your-cdn}

En este documento se describen las tareas comunes para gestionar la CDN.

## Definición del tiempo de almacenamiento de contenido en memoria caché usando "Tiempo de duración"
{: #setting-content-caching-time-using-time-to-live}

Cuando la CDN se está ejecutando, puede definir el tiempo de almacenamiento de contenido en memoria caché usando Tiempo de duración (TTL). El Tiempo de duración de una vía de acceso de directorio o archivo determinado indica cuánto tiempo debería almacenarse el contenido en memoria caché. Cuando ha creado la correlación de CDN, se ha generado un TTL global predeterminado de 3600 segundos (1 hora).

**Paso 1:**  

En la página CDN, seleccione su CDN para ir a la página **Visión general**.

**Paso 2:**  

Puede ajustar el tiempo utilizando las flechas o introduciendo un nuevo tiempo. El valor de tiempo se especifica en segundos. Por ejemplo, 3600 segundos es igual a 1 hora. El valor más bajo para `timeToLive` que se puede escoger es 0 segundos y el más alto es 2147483647 segundos (aproximadamente 24855 días). Seleccione el botón **Guardar** para establecer el tiempo de almacenamiento de contenido en memoria caché.

  ![Añadir ttl](images/adding-path.png)

**Paso 3:**

Después de guardar, puede **Editar** o **Suprimir** el parámetro de TTL desde las opciones del menú de desbordamiento. (**NOTA**: la vía de acceso para TTL no se puede modificar. Si se cambia la vía de acceso de la correlación, la vía de acceso de TTL se actualiza automáticamente).

  ![Editar o suprimir ttl](images/edit-delete-ttl-setting.png)  

  * Cuando el contenido coincide con varias reglas, prevalece la configuración añadida más recientemente.

  * Los valores de TTL pueden definirse solo para un directorio o nombre de archivo específico. No se admiten las expresiones regulares, porque pueden crear un comportamiento imprevisible.

## Adición de detalles de la vía de acceso de origen
{: #adding-origin-path-details}

Cuando el estado de la CDN es *CNAME_Configuration* o *Running*, puede agregar detalles de la vía de acceso de origen. Puede optar por proporcionar contenido de varios servidores de origen. Por ejemplo, se pueden suministrar fotografías desde un servidor distinto del servidor de vídeos. El origen puede basarse en un servidor de host o en Object Storage.

La CDN realiza una transformación del URL para el servidor de origen. Por ejemplo, si se añade el origen `xyz.example.com` con la vía de acceso `/example/*` cuando un usuario abre el URL `www.example.com/example/*`, el servidor perimetral de la CDN recupera el contenido de `xyz.example.com/*`.
{: note}

**Paso 1:**

En la página CDN, seleccione su CDN para ir a la página **Visión general**.  

**Paso 2:**

Seleccione el separador **Orígenes** y luego pulse el botón **Añadir origen**. Este paso abre una nueva ventana de diálogo, donde puede configurar su origen.  

   ![Orígenes: Añadir origen](images/add-origins.png)

**Paso 3:**

*Debe* proporcionar una vía de acceso. También puede proporcionar una cabecera de host.  

   ![Orígenes: Añadir origen](images/add-origin-path.png)

**Paso 4:**

Seleccione **Servidor** u **Object Storage**.

  * Si ha seleccionado **Servidor**, especifique la dirección del servidor de origen como dirección IPv4 o el _nombre de host_. Se recomienda proporcionar el nombre de host y proporcionar un Nombre de dominio completo (FQDN). En función del protocolo que haya seleccionado durante la creación de la CDN, también debe proporcionar un puerto HTTP, un puerto HTTPS o ambos. Si utiliza un puerto HTTPS, la dirección del servidor de origen **debe** ser un _nombre de host_ y no una dirección IP.

       ![Añadir servidor de origen](images/add-origin-server-default.png)

  * Si ha seleccionado **Object Storage**, proporcione el punto final, el nombre de grupo y el puerto HTTPS. Opcionalmente, especifique las extensiones de archivo que se pueden utilizar en el servicio de la CDN. Si no se especifica nada, se permitirán todas las extensiones.

       ![Añadir almacenamiento de objetos de origen](images/add-origin-object-storage.png)

  * Las opciones **Optimización** y **Clave de caché** son las mismas para las configuraciones de Servidor y de Object Storage.

    * Elija las opciones de **Optimización** desde el menú desplegable. **Distribución web general** es la opción predeterminada, o puede elegir las optimizaciones **Archivo de gran tamaño** o **Vídeo on demand**. **Distribución web general** permite a la CDN servir contenido de hasta 1,8 GB, mientras que la optimización **Archivo de gran tamaño** permite descargas de archivos de 1,8 GB a 320 GB. **Vídeo on demand** optimiza su CDN para la distribución de formatos de streaming segmentados. Las descripciones de característica para [Optimización de archivos de gran tamaño](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#large-file-optimization) y [Vídeo on Demand](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#video-on-demand) proporcionan más información.

        ![Opciones de configuración del rendimiento](images/performance-config-options.png)

    * Seleccione las opciones de **Clave de caché** desde el menú desplegable. La opción predeterminada es **Incluir todo**. Si selecciona **Incluir especificado** o **Ignorar especificado**, **debe** introducir las series de consulta que se incluirán o ignorarán, separadas por un espacio. Por ejemplo, especifique `uuid=123456` para una sola serie de consulta, o `uuid=123456 issue=important` para dos series de consulta.  Puede obtener más información sobre los [argumentos de consulta de clave de caché](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#cache-key-query-args) en la descripción de la característica.

        ![Opciones de clave de caché](images/cache-key-options.png)

Las opciones de protocolo y puerto que muestra la interfaz de usuario coinciden con lo que se ha seleccionado al realizar el pedido de la CDN. Por ejemplo, si ha seleccionado **Puerto HTTP** como parte de la solicitud de la CDN, la opción **Puerto HTTP** se mostrará solo como parte de Añadir origen.
{: note}

**Paso 5:**

Seleccione el botón **Añadir** para añadir la vía de acceso de origen.

Cuando proporciona extensiones de archivo para una vía de acceso de origen de Object Storage, se incluirá en el ámbito la opción de TTL con el mismo URL que la vía de acceso de origen, de forma que se incorporen todos los archivos con esas extensiones especificadas. Por ejemplo, si crea una vía de acceso de origen de `/example` y especifica las extensiones de archivo "jpg png gif", el valor de TTL de la vía de acceso de TTL `/example` tendrá un ámbito que incluye todos los archivos JPG/PNG/GIF del directorio `/example` y sus subdirectorios.
{: note}

**Paso 6:**

Después de la adición, puede **Editar** o **Suprimir** el origen desde las opciones del menú de desbordamiento.

  ![Editar o suprimir origen](images/edit-delete-origin.png)

## Depuración de contenido almacenado en memoria caché
{: #purging-cached-content}

Cuando la CDN se está ejecutando, puede depurar el contenido almacenado en memoria caché desde el servidor del proveedor.

**Paso 1:**

En la página CDN, seleccione su CDN para ir a la página **Visión general**.

**Paso 2:**

Seleccione el separador **Depurar**.

   ![Página Depurar](images/purge_tab.png)

**Paso 3:**

Especifique la sintaxis de vía de acceso Unix estándar para indicar qué archivo desea depurar y, a continuación, seleccione el botón **Depurar**. La depuración se permite para un solo archivo a la vez. Consulte la página [Convenios de reglas y denominación](/docs/infrastructure/CDN?topic=CDN-rules-and-naming-conventions#what-are-the-rules-for-the-path-string-for-purge-) para obtener más detalles sobre qué sintaxis se permite para la vía de acceso de depuración.

**Paso 4:**

Tras la depuración, la actividad se lista en **Depurar actividad**. Puede **rehacer la depuración** o **marcar como favorita** la vía de acceso mediante las opciones del menú de desbordamiento.

   ![Actividad de depuración](images/purge-activity.png)

Si hay más de 15 depuraciones, la actividad de depuración se recorta cada 15 días de forma automática.
{: note}

## Actualización de los detalles de configuración de la CDN
{: #updating-cdn-configuration-details}

Cuando la CDN se está ejecutando, puede actualizar los detalles de configuración de la CDN.

**Paso 1:**

En la página CDN, seleccione su CDN para ir a la página **Visión general**.

**Paso 2:**

Seleccione el separador **Valores**. Se mostrarán los detalles de configuración de la CDN.

   ![Separador Valores](images/settings-tab.png)  

Sólo verá el certificado SSL si la CDN se ha configurado con HTTPS.
{: note}

Para **Servidor**, se pueden cambiar los campos siguientes:
  * Cabecera de host
  * Dirección de servidor de origen
  * Puerto HTTP/HTTPS
  * Proporcionar contenido obsoleto
  * Respetar cabeceras
  * Opciones de optimización
  * Consulta de caché    

Para **Object Storage**, se pueden modificar los campos siguientes:
  * Cabecera de host
  * Punto final
  * Nombre de grupo
  * Puerto HTTPS
  * Extensiones de archivo permitidas
  * Proporcionar contenido obsoleto
  * Respetar cabeceras
  * Opciones de optimización
  * Consulta de caché

**Paso 3:**

Actualice los detalles de **Origen** u **Otras opciones** si es necesario y, a continuación, haga clic en el botón **Guardar** de la esquina inferior derecha para que se actualicen los detalles de configuración de la CDN.

   ![Botón Guardar](images/save-button.png)
