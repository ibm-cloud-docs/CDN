---

copyright:
  years: 2017
lastupdated: "2017-09-11"

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

1. Después de solicitar una CDN, deberá configurar el **CNAME** con el proveedor de DNS. La mayoría de los proveedores de DNS pueden suministrar instrucciones sobre cómo configurar o cambiar el CNAME.
   
   * Durante ese tiempo, el estado de la CDN se mostrará como **Configuración de CNAME**. Consulte a su proveedor de DNS cuándo se activarán los cambios. 

   ![Configuración de CNAME](images/cname-config.png)  
   
2. En cualquier momento tras haber configurado el CNAME con su proveedor de DNS, puede comprobar el estado seleccionando **Obtener el estado** en el menú de desbordamiento, situado a la derecha del estado de la CDN. 
   
  ![getStatus de CNAME](images/cname-getstatus.png)  
    
3. Cuando se completa el encadenamiento de CNAME, el estado de la cuenta cambia a *RUNNING* y la CDN estará lista para usarse. 
   	  
¡Enhorabuena! La CDN se está ejecutando.   
	  
## Detención de la CDN
Una CDN se puede detener solo cuando su estado es "Running".  

1. Pulse "Detener CDN" en el menú de desbordamiento (3 puntos verticales a la derecha del estado de la CDN).
 ![Menú de desbordamiento](images/stop_cdn.png)

2. Se mostrará una ventana de diálogo más grande para que confirme que desea iniciar el servicio. Seleccione **Confirmar** para continuar.
  
3. Transcurridos entre 5 y 15 segundos, el estado debería cambiar a "Stopped". 


## Inicio de la CDN
Una CDN solo se puede iniciar cuando su estado es "Stopped".
1. Pulse "Iniciar CDN" en el menú de desbordamiento, que se muestra como tres puntos a la derecha de la fila de la CDN.
 ![Menú de desbordamiento](images/start_cdn.png)

2. Se mostrará una ventana de diálogo más grande para que confirme que desea iniciar el servicio. Seleccione **Confirmar** para continuar.

3. Si la acción se ha realizado correctamente, aparecerá un recuadro de diálogo en la esquina superior derecha de la pantalla, que le indica que se ha efectuado de forma satisfactoria y el tiempo dedicado. 
  
4. Esto cambiará el estado a "CNAME_Configuration".

5. Pulse "Obtener estado" en el menú de desbordamiento. Esto cambiará el estado a "Running". Y la CDN entrará en funcionamiento. 

## Supresión de la CDN

Para suprimir una CDN, realice los pasos siguientes:
**NOTA**: al seleccionar `Suprimir` en el menú de desbordamiento, solo se suprime la CDN; no se suprimirá la cuenta. 

1. Pulse "Suprimir" en el menú de desbordamiento. 

 ![Menú de desbordamiento Suprimir CDN](images/delete_cdn.png)

2. Se mostrará una ventana de diálogo más grande para que confirme que desea efectuar la supresión. Pulse **Suprimir** para continuar.

3. Esto cambiará el estado a "Deleting". Pulse "Obtener estado" en el menú de desbordamiento. Ello eliminará la fila de la lista de CDN.

## Definición del tiempo de almacenamiento de contenido en memoria caché usando "Tiempo de duración"

Cuando la CDN se está ejecutando, puede definir el tiempo de almacenamiento de contenido en memoria caché usando Tiempo de duración (TTL). El Tiempo de duración de una vía de acceso de directorio o archivo determinado indica cuánto tiempo debería almacenarse el contenido en memoria caché. Cuando ha creado la correlación de CDN, se ha generado un TTL global predeterminado de 3600 segundos. 

1. En la página CDN, seleccione su CDN, que le llevará a la página **Visión general**.

2. Puede ajustar el tiempo utilizando las flechas o introduciendo un nuevo tiempo. El valor de tiempo se especifica en segundos. Por ejemplo, 3600 segundos es igual a 1 hora. El valor más bajo para `timeToLive` que se puede escoger es 30 segundos y el más alto es 21474836471 segundos. Seleccione el botón **Guardar** para establecer el tiempo de almacenamiento de contenido en memoria caché.  

  ![Añadir ttl](images/adding-path.png)

3. Después de guardar, puede **Editar** o **Suprimir** el parámetro de TTL desde las opciones del menú de desbordamiento. (**NOTA**: la vía de acceso para TTL no se puede modificar. Si se cambia la vía de acceso de la correlación, la vía de acceso de TTL se actualizara automáticamente).

  ![Editar o suprimir ttl](images/edit-delete-ttl-setting.png)  
	
  * Cuando el contenido coincide con varias reglas, prevalecerá la configuración añadida más recientemente.
  
  * Los valores de TTL pueden definirse solo para un directorio o nombre de archivo específico. No se admiten las expresiones regulares, porque pueden crear un comportamiento imprevisible. 
	
## Adición de detalles de la vía de acceso de origen

Cuando el estado de la CDN es *CNAME_Configuration* o *Running*, puede agregar detalles de la vía de acceso de origen. Puede optar por proporcionar contenido de varios servidores de origen. Por ejemplo, se pueden suministrar fotografías desde un servidor distinto del servidor de vídeos. El origen puede basarse en un servidor de host o en Object Storage. 

1. En la página CDN, seleccione su CDN para ir a la página **Visión general**.
	
2. Seleccione el separador **Orígenes** y luego pulse el botón **Añadir origen**.
	
3. *Debe* proporcionar una vía de acceso. Seleccione **Servidor** u **Object Storage**.
	
   ![Orígenes: Añadir origen](images/add-origin.png)
		
  * Si ha seleccionado **Servidor**, especifique la dirección IP del servidor o el _nombre de host_. Proporcione un puerto HTTP, un puerto HTTPS, o ambos, en función del protocolo seleccionado durante la creación de la CDN; debería coincidir con la correlación.
	
  ![Añadir servidor de origen](images/add-origin-server.png)
	
  * Si ha seleccionado **Object Storage**, proporcione el punto final, el nombre de grupo y el puerto HTTPS. Opcionalmente, especifique las extensiones de archivo que se pueden utilizar en el servicio de la CDN. Si no se especifica nada, se permitirán todas las extensiones.
	
  ![Añadir almacenamiento de objetos de origen](images/add-origin-object-storage.png)
		
4. Seleccione el botón **Añadir** para añadir la vía de acceso de origen.

  **Nota**: cuando proporciona extensiones de archivo para una vía de acceso de origen de Object Storage, se incluirá en el ámbito la opción de TTL que tenga el mismo URL que la vía de acceso de origen, de forma que se incorporen todos los archivos con esas extensiones especificadas. Por ejemplo, si crea una vía de acceso de origen de "/ejemplo" y especifica las extensiones de archivo "jpg png gif", el valor de TTL de la vía de acceso de TTL "/ejemplo" tendrá un ámbito que incluye todos los archivos JPG/PNG/GIF del directorio "/ejemplo" y sus subdirectorios.

5. Después de la adición, puede **Editar** o **Suprimir** el origen desde las opciones del menú de desbordamiento. 

  ![Editar o suprimir origen](images/edit-delete-origin.png)
	
## Depuración de contenido almacenado en memoria caché

Cuando la CDN se está ejecutando, puede depurar el contenido almacenado en memoria caché desde el servidor del proveedor.   

1. En la página CDN, seleccione su CDN para ir a la página **Visión general**.
	
2. Seleccione el separador **Depurar**. 

	![Página Depurar](images/purge_tab.png)
	
3. Especifique la sintaxis de vía de acceso unix estándar para indicar qué archivo desea depurar y, a continuación, seleccione el botón **Depurar**. La depuración solo se permite para un solo archivo en este momento.

4. Tras la depuración, la actividad se lista en **Depurar actividad**. Puede **rehacer la depuración** o **marcar como favorita** la vía de acceso mediante las opciones del menú de desbordamiento.

	![Actividad de depuración](images/purge-activity.png)
	
   **NOTA:** si hay más de 15 depuraciones, la actividad de depuración se recorta cada 15 días de forma automática. 
	
## Actualización de los detalles de configuración de la CDN

Cuando la CDN se está ejecutando, puede actualizar los detalles de configuración de la CDN. 

1. En la página CDN, seleccione su CDN para ir a la página **Visión general**.

2. Seleccione el separador **Valores**. Se mostrarán los detalles de configuración de la CDN.

  Para **Object Storage**, se pueden modificar los campos siguientes: 
   * Punto final
   * Nombre de grupo
   * Puerto HTTPS
   * Extensiones de archivo permitidas
   * Proporcionar contenido obsoleto
   * Respetar cabeceras

  Para **Servidor**, se pueden cambiar los campos siguientes: 
   * Dirección de servidor de origen
   * Puerto HTTP/HTTPS
   * Proporcionar contenido obsoleto
   * Respetar cabeceras

3. Actualice los detalles de **Origen** u **Otras opciones** si es necesario y, a continuación, haga clic en el botón **Guardar** de la esquina inferior derecha para que se actualicen los detalles de configuración de la CDN. 

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
