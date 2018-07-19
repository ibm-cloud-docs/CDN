---

copyright:
  years: 2018
lastupdated: "2018-06-15"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Cómo llevar la CDN a estado de ejecución

Obtenga más información sobre cómo llevar la CDN en un estado de RUNNING siguiendo estas directrices. Este documento también le indica cómo iniciar y detener la CDN.

## Ponerla en ejecución

Después de crear una CDN, se mostrará en el panel de control de CDN. En el panel, verá el nombre de la CDN, el origen, el proveedor y el estado.  

 ![Captura de pantalla de la lista de correlación](images/mapping_list_cname.png)


Si ha solicitado la CDN con HTTP o HTTPS con el certificado comodín, puede continuar con el Paso 1.

Si ha creado una CDN con un certificado SAN DV HTTPS, es posible que se necesiten pasos adicionales para verificar el dominio; se pueden encontrar en la página [Completar la validación de control de dominio para HTTPS](how-to-https.html#completing-domain-control-validation-for-https).

**Paso 1:**

Después de solicitar una CDN, deberá configurar el **CNAME** con el proveedor de DNS. La mayoría de los proveedores de DNS pueden suministrar instrucciones sobre cómo configurar o cambiar el CNAME.

   * Durante ese tiempo, el estado de la CDN se mostrará como **Configuración de CNAME**. Consulte a su proveedor de DNS cuándo se activarán los cambios.

   ![Configuración de CNAME](images/cname-config.png)  

**Paso 2:**

En cualquier momento tras haber configurado el CNAME con su proveedor de DNS, puede comprobar el estado seleccionando **Obtener el estado** en el menú de desbordamiento, situado a la derecha del estado de la CDN.

  ![getStatus de CNAME](images/cname-getstatus.png)  

**Paso 3:**

Cuando finalice el encadenamiento de CNAME, al seleccionar **Obtener estado** se cambiará el estado a *RUNNING* y la CDN está lista para su uso.

¡Enhorabuena! La CDN se está ejecutando. Desde aquí, la página [Gestionar la CDN](how-to.html#manage-your-CDN) contiene información adicional sobre cómo configurar opciones como por ejemplo [Tiempo de vida](how-to.html#setting-content-caching-time-using-time-to-live-), [Depuración de contenido en memoria caché](how-to.html#purging-cached-content) y [Adición de detalles de la vía de acceso de origen](how-to.html#adding-origin-path-details).

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

## Detención de la CDN

Una CDN se puede detener solo cuando su estado es "Running".

**Paso 1:**

Pulse "Detener CDN" en el menú de desbordamiento (3 puntos verticales a la derecha del estado de la CDN).
 ![Menú de desbordamiento](images/stop_cdn.png)

**Paso 2:**

Se mostrará una ventana de diálogo más grande para que confirme que desea detener el servicio. Seleccione **Confirmar** para continuar.

**Paso 3:**

Transcurridos entre 5 y 15 segundos, el estado debería cambiar a "Stopped".

## Supresión de la CDN

Para suprimir una CDN, siga estos pasos:

**NOTA**: al seleccionar `Suprimir` en el menú de desbordamiento, solo se suprime la CDN; no se suprime la cuenta.

**Paso 1:**

Pulse "Suprimir" en el menú de desbordamiento.

 ![Menú de desbordamiento Suprimir CDN](images/delete_cdn.png)

**Paso 2:**

Se mostrará una ventana de diálogo más grande para que confirme que desea efectuar la supresión. Pulse **Suprimir** para continuar.

**NOTA**: Si la CDN está configurada utilizando HTTPS con Certificado SAN DV, podría tardar hasta 5 horas en completar el proceso de supresión.

  ![Supresión con advertencia](images/delete-with-warning.png)

**Paso 3:**

Después de completar los pasos 1 y 2, el estado de la CDN será `Suprimiendo`. Cuando finalice el proceso de supresión, al volver a hacer clic en "Obtener estado" en el menú de desbordamiento, se eliminará la fila de la lista de CDN. Si el proceso de supresión no se ha completado, esta acción no tendrá efecto.
