---

copyright:
  years: 2017, 2018
lastupdated: "2018-03-06"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Gestión de la CDN

Aprenda a gestionar la configuración de la CDN siguiendo estas directrices.

## Cómo llevar la CDN a estado de ejecución

Después de crear una CDN, se mostrará en el panel de control de CDN. En el panel, verá el nombre de la CDN, el origen, el proveedor y el estado.  

 ![Captura de pantalla de la lista de correlación](images/mapping_list_cname.png)

**Paso 1:**

Después de solicitar una CDN, deberá configurar el **CNAME** con el proveedor de DNS. La mayoría de los proveedores de DNS pueden suministrar instrucciones sobre cómo configurar o cambiar el CNAME.

   * Durante ese tiempo, el estado de la CDN se mostrará como **Configuración de CNAME**. Consulte a su proveedor de DNS cuándo se activarán los cambios.

   ![Configuración de CNAME](images/cname-config.png)  

**Paso 2:**  

En cualquier momento tras haber configurado el CNAME con su proveedor de DNS, puede comprobar el estado seleccionando **Obtener el estado** en el menú de desbordamiento, situado a la derecha del estado de la CDN.

  ![getStatus de CNAME](images/cname-getstatus.png)  

**Paso 3:**  

Cuando se completa el encadenamiento de CNAME, el estado de la cuenta cambia a *RUNNING* y la CDN estará lista para usarse.

¡Enhorabuena! La CDN se está ejecutando.  

## Detención de la CDN
Una CDN se puede detener solo cuando su estado es "Running".

**Paso 1:**  

Pulse "Detener CDN" en el menú de desbordamiento (3 puntos verticales a la derecha del estado de la CDN).
 ![Menú de desbordamiento](images/stop_cdn.png)

**Paso 2:**  

Se mostrará una ventana de diálogo más grande para que confirme que desea detener el servicio. Seleccione **Confirmar** para continuar.

**Paso 3:**  

Transcurridos entre 5 y 15 segundos, el estado debería cambiar a "Stopped".


## Inicio de la CDN
Una CDN solo se puede iniciar cuando su estado es "Stopped".  

**Paso 1:**  

Pulse "Iniciar CDN" en el menú de desbordamiento, que se muestra como tres puntos a la derecha de la fila de la CDN.
 ![Menú de desbordamiento](images/start_cdn.png)

**Paso 2:**  

Se mostrará una ventana de diálogo más grande para que confirme que desea iniciar el servicio. Seleccione **Confirmar** para continuar.

**Paso 3:**  

Si la acción se ha realizado correctamente, aparecerá un recuadro de diálogo en la esquina superior derecha de la pantalla, que le indica que se ha efectuado de forma satisfactoria y el tiempo dedicado.

**Paso 4:**  

Este paso cambia el estado a 'CNAME Configuration'

**Paso 5:**  

Pulse "Obtener estado" en el menú de desbordamiento. Este paso cambia el estado a 'Running'. Su CDN entrará en funcionamiento.

## Supresión de la CDN

Para suprimir una CDN, siga estos pasos:

**NOTA**: al seleccionar `Suprimir` en el menú de desbordamiento, solo se suprime la CDN; no se suprime la cuenta.

**Paso 1:**  

Pulse "Suprimir" en el menú de desbordamiento.

 ![Menú de desbordamiento Suprimir CDN](images/delete_cdn.png)

**Paso 2:**  

Se mostrará una ventana de diálogo más grande para que confirme que desea efectuar la supresión. Pulse **Suprimir** para continuar.

**Paso 3:**  

Este paso cambia el estado a 'Deleting'. Pulse 'Obtener estado' en el menú de desbordamiento, y elimine la fila de la lista de CDN.

## Definición del tiempo de almacenamiento de contenido en memoria caché usando "Tiempo de duración"

Cuando la CDN se está ejecutando, puede definir el tiempo de almacenamiento de contenido en memoria caché usando Tiempo de duración (TTL). El Tiempo de duración de una vía de acceso de directorio o archivo determinado indica cuánto tiempo debería almacenarse el contenido en memoria caché. Cuando ha creado la correlación de CDN, se ha generado un TTL global predeterminado de 3600 segundos.

**Paso 1:**  

En la página CDN, seleccione su CDN para ir a la página **Visión general**.

**Paso 2:**  

Puede ajustar el tiempo utilizando las flechas o introduciendo un nuevo tiempo. El valor de tiempo se especifica en segundos. Por ejemplo, 3600 segundos es igual a 1 hora. El valor más bajo para `timeToLive` que se puede escoger es 30 segundos y el más alto es 2147483647 segundos. Seleccione el botón **Guardar** para establecer el tiempo de almacenamiento de contenido en memoria caché.

  ![Añadir ttl](images/adding-path.png)

**Paso 3:**

Después de guardar, puede **Editar** o **Suprimir** el parámetro de TTL desde las opciones del menú de desbordamiento. (**NOTA**: la vía de acceso para TTL no se puede modificar. Si se cambia la vía de acceso de la correlación, la vía de acceso de TTL se actualiza automáticamente).

  ![Editar o suprimir ttl](images/edit-delete-ttl-setting.png)  

  * Cuando el contenido coincide con varias reglas, prevalece la configuración añadida más recientemente.

  * Los valores de TTL pueden definirse solo para un directorio o nombre de archivo específico. No se admiten las expresiones regulares, porque pueden crear un comportamiento imprevisible.

## Adición de detalles de la vía de acceso de origen

Cuando el estado de la CDN es *CNAME_Configuration* o *Running*, puede agregar detalles de la vía de acceso de origen. Puede optar por proporcionar contenido de varios servidores de origen. Por ejemplo, se pueden suministrar fotografías desde un servidor distinto del servidor de vídeos. El origen puede basarse en un servidor de host o en Object Storage.

**Nota:** la CDN realiza una transformación del URL para el servidor de origen. Por ejemplo, si se añade el origen `xyz.example.com` con la vía de acceso `/example/*` cuando un usuario abre el URL `www.example.com/example/*`, el servidor perimetral de la CDN recupera el contenido de `xyz.example.com/*`.

**Paso 1:**  

En la página CDN, seleccione su CDN para ir a la página **Visión general**.  

**Paso 2:**  

Seleccione el separador **Orígenes** y luego pulse el botón **Añadir origen**. Este paso abre una nueva ventana de diálogo, donde puede configurar su origen.  

   ![Orígenes: Añadir origen](images/add-origins.png)

**Paso 3:**  

*Debe* proporcionar una vía de acceso. La vía de acceso *debe* empezar con la vía de acceso de CDN como prefijo, si CDN se ha creado con una vía de acceso.  
  Por ejemplo, si la CDN se ha creado con una vía de acceso de `/examplePath`, la vía de acceso para el origen debe empezar con el prefijo `/examplePath/`. También puede proporcionar una cabecera de host.  
  
   ![Orígenes: Añadir origen](images/add-origin-path.png)

**Paso 4:**  

Seleccione **Servidor** u **Object Storage**.

  * Si ha seleccionado **Servidor**, especifique la dirección del servidor de origen como dirección IPv4 o el _nombre de host_. Se recomienda proporcionar el nombre de host y proporcionar un Nombre de dominio completo (FQDN). En función del protocolo que haya seleccionado durante la creación de la CDN, también debe proporcionar un puerto HTTP, un puerto HTTPS o ambos. Si utiliza un puerto HTTPS, la dirección del servidor de origen **debe** ser un _nombre de host_ y no una dirección IP.
  
       ![Añadir servidor de origen](images/add-origin-server-default.png)

  * Si ha seleccionado **Object Storage**, proporcione el punto final, el nombre de grupo y el puerto HTTPS. Opcionalmente, especifique las extensiones de archivo que se pueden utilizar en el servicio de la CDN. Si no se especifica nada, se permitirán todas las extensiones.
  
       ![Añadir almacenamiento de objetos de origen](images/add-origin-object-storage.png)

  * Las opciones **Optimización** y **Clave de caché** son las mismas para las configuraciones de servidor y de almacenamiento de objetos.
  
    * Elija las opciones de **Optimización** desde el menú desplegable. **Distribución web general** es la opción predeterminada, o puede elegir las optimizaciones **Archivo de gran tamaño** o **Vídeo on demand**. **Distribución web general** permite a la CDN servir contenido de hasta 1,8 GB, mientras que la optimización **Archivo de gran tamaño** permite descargas de archivos de 1,8 GB a 320 GB. **Vídeo on demand** optimiza su CDN para la distribución de formatos de streaming segmentados. Las descripciones de característica para [Optimización de archivos de gran tamaño](about.html#large-file-optimization-) y [Vídeo on Demand](about.html#video-on-demand-) proporcionan más información.
    
        ![Opciones de configuración del rendimiento](images/performance-config-options.png)
    
    * Seleccione las opciones de **Clave de caché** desde el menú desplegable. La opción predeterminada es **Incluir todo**. Si selecciona **Incluir especificado** o **Ignorar especificado**, **debe** introducir las series de consulta que se incluirán o ignorarán, separadas por un espacio. Por ejemplo, especifique `uuid=123456` para una sola serie de consulta, o `uuid=123456 issue=important` para dos series de consulta.  Puede obtener más información sobre los [argumentos de consulta de clave de caché](about.html#ignore-query-args-in-cache-key-feature-) en la descripción de la característica.
    
        ![Opciones de clave de caché](images/cache-key-options.png)
    
**NOTA**: las opciones de protocolo y puerto que muestra la interfaz de usuario coinciden con lo que se ha seleccionado al realizar el pedido de la CDN. Por ejemplo, si ha seleccionado **Puerto HTTP** como parte de la solicitud de la CDN, la opción **Puerto HTTP** se mostrará solo como parte de Añadir origen.

**Paso 5:**  

Seleccione el botón **Añadir** para añadir la vía de acceso de origen.

  **Nota**: cuando proporciona extensiones de archivo para una vía de acceso de origen de Object Storage, se incluirá en el ámbito la opción de TTL con el mismo URL que la vía de acceso de origen, de forma que se incorporen todos los archivos con esas extensiones especificadas. Por ejemplo, si crea una vía de acceso de origen de `/example` y especifica las extensiones de archivo "jpg png gif", el valor de TTL de la vía de acceso de TTL `/example` tendrá un ámbito que incluye todos los archivos JPG/PNG/GIF del directorio `/example` y sus subdirectorios.

**Paso 6:**  

Después de la adición, puede **Editar** o **Suprimir** el origen desde las opciones del menú de desbordamiento.

  ![Editar o suprimir origen](images/edit-delete-origin.png)

## Depuración de contenido almacenado en memoria caché

Cuando la CDN se está ejecutando, puede depurar el contenido almacenado en memoria caché desde el servidor del proveedor.  

**Paso 1:**  

En la página CDN, seleccione su CDN para ir a la página **Visión general**.

**Paso 2:**  

Seleccione el separador **Depurar**.

   ![Página Depurar](images/purge_tab.png)

**Paso 3:**  

Especifique la sintaxis de vía de acceso unix estándar para indicar qué archivo desea depurar y, a continuación, seleccione el botón **Depurar**. La depuración se permite para un solo archivo en este momento.

**Paso 4:**  

Tras la depuración, la actividad se lista en **Depurar actividad**. Puede **rehacer la depuración** o **marcar como favorita** la vía de acceso mediante las opciones del menú de desbordamiento.

   ![Actividad de depuración](images/purge-activity.png)

   **NOTA:** si hay más de 15 depuraciones, la actividad de depuración se recorta cada 15 días de forma automática.

## Actualización de los detalles de configuración de la CDN

Cuando la CDN se está ejecutando, puede actualizar los detalles de configuración de la CDN.

**Paso 1:**

En la página CDN, seleccione su CDN para ir a la página **Visión general**.

**Paso 2:**  

Seleccione el separador **Valores**. Se mostrarán los detalles de configuración de la CDN.

   ![Separador Valores](images/settings-tab.png)

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


## Configuración de IBM Cloud Object Storage for CDN

Para utilizar objetos almacenados en IBM Cloud Object Storage, debe establecer el valor de la propiedad "acl" (es decir, la lista de control de accesos) para que cada objeto del grupo disponga de acceso "public-read".

Consulte la sección Tools de IBM Cloud Object Storage Developer Center (https://developer.ibm.com/cloudobjectstorage/) para instalar las herramientas o clientes necesarios. Esta guía presupone que ha instalado la interfaz de línea de mandatos AWS oficial, que es compatible con la API de IBM Cloud Object Storage S3.

El código de ejemplo siguiente es una muestra sobre cómo definir el acceso "public-read" para todos los objetos del grupo mediante la interfaz de línea de mandatos.

```
$ export ENDPOINT="YOUR_ENDPOINT"
$ export BUCKET="YOUR_BUCKET"
$ KEYS=( "$(aws --endpoint-url "$ENDPOINT" s3api list-objects --no-paginate --query 'Contents[].{key: Key}' --output text --bucket "$BUCKET")" )
$ for KEY in "${KEYS[@]}"
  > do
  >   aws --endpoint-url "$ENDPOINT" s3api put-object-acl --bucket "$BUCKET" --key "$KEY" --acl "public-read"
  > done
```
