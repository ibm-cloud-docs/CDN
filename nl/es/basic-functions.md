---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-21"

keywords: running status, additional steps, stop cdn, learn, configure cname, delete cdn, start cdn

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:warning: .warning}
{:download: .download}

# Cómo llevar la CDN a estado de ejecución
{: #getting-your-cdn-to-running-status}

Obtenga más información sobre cómo llevar la CDN en un estado de RUNNING siguiendo estas directrices. Este documento también le indica cómo iniciar y detener la CDN.

## Ponerla en ejecución
{: #get-to-running}

Después de crear una CDN, se mostrará en el panel de control de CDN. En el panel, verá el nombre de la CDN, el origen, el proveedor y el estado.  

 ![Captura de pantalla de la lista de correlación](images/mapping-list.png)


Si ha solicitado la CDN con HTTP o HTTPS con el certificado comodín, puede continuar con el Paso 1.

Si ha creado una CDN con un certificado SAN DV HTTPS, es posible que se necesiten pasos adicionales para verificar el dominio; se pueden encontrar en la página [Completar la validación de control de dominio para HTTPS](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#completing-domain-control-validation-for-https).

**Paso 1:**

Después de solicitar una CDN, deberá configurar el **CNAME** con el proveedor de DNS. La mayoría de los proveedores de DNS pueden suministrar instrucciones sobre cómo configurar o cambiar el CNAME.

   * Durante ese tiempo, el estado de la CDN se mostrará como **Configuración de CNAME**. Consulte a su proveedor de DNS cuándo se activarán los cambios.

   ![Configuración de CNAME](images/cname-config.png)  

**Paso 2:**

En cualquier momento tras haber configurado el CNAME con su proveedor de DNS, puede comprobar el estado seleccionando **Obtener el estado** en el menú de desbordamiento, situado a la derecha del estado de la CDN.

  ![getStatus de CNAME](images/cname-getstatus.png)  

**Paso 3:**

Cuando finalice el encadenamiento de CNAME, al seleccionar **Obtener estado** se cambiará el estado a *RUNNING* y la CDN está lista para su uso.

¡Enhorabuena! La CDN se está ejecutando. Desde aquí, la página [Gestionar la CDN](/docs/infrastructure/CDN?topic=CDN-manage-your-cdn#manage-your-cdn) contiene información adicional sobre cómo configurar opciones como por ejemplo [Tiempo de vida](docs/infrastructure/CDN?topic=CDN-manage-your-cdn#setting-content-caching-time-using-time-to-live-), [Depuración de contenido en memoria caché](/docs/infrastructure/CDN?topic=CDN-manage-your-cdn#purging-cached-content) y [Adición de detalles de la vía de acceso de origen](/docs/infrastructure/CDN?topic=CDN-manage-your-cdn#adding-origin-path-details).

## Inicio de la CDN
{: #starting-cdn}

Cuando se inicia la CDN se informa al DNS para que direccione el tráfico desde su origen al servidor perimetral Akamai. Una vez iniciada la correlación, la caché del DNS todavía podría redirigir tráfico al origen de forma que la funcionalidad podría no ser vista por el dominio inmediatamente después del inicio. El tiempo necesario para la actualización depende de la frecuencia con la que se renueva la caché de DNS, y varía según cada proveedor de DNS.

Una CDN solo se puede iniciar cuando su estado es `Stopped`.
{: note}

**Paso 1:**

Pulse **Iniciar CDN** en el menú de desbordamiento, que se muestra como tres puntos a la derecha de la fila de la CDN.

  ![Menú de desbordamiento](images/start_cdn.png)

**Paso 2:**

Se mostrará una ventana de diálogo más grande para que confirme que desea iniciar el servicio. Seleccione **Confirmar** para continuar.

**Paso 3:**

Si la acción se ha realizado correctamente, aparecerá un recuadro de diálogo en la esquina superior derecha de la pantalla, que le indica que se ha efectuado de forma satisfactoria y el tiempo dedicado.

**Paso 4:**

Este paso cambia el estado a `CNAME Configuration`

**Paso 5:**

Pulse **Obtener estado** en el menú de desbordamiento. Este paso cambia el estado a `Running`. Su CDN entrará en funcionamiento.

## Detener una CDN
{: #stopping-a-cdn}

La funcionalidad DETENER CDN está pensada para ventanas de mantenimiento que no superen los 7 días. Después de 7 días, se debe iniciar la CDN o se inhabilitará y el tráfico al CNAME de CDN no se redirigirá al servidor de origen.
{: important}

Una vez detenida la correlación, la búsqueda de DNS se conmuta al origen. El tráfico omite los servidores perimetrales de la CDN y el contenido se recupera directamente del origen. Una vez detenida la correlación, puede haber un periodo de tiempo breve en el que el contenido podría no ser accesible. Esto se debe a que la caché de DNS todavía podría redirigir el tráfico a los servidores perimetrales de Akamai. Sin embargo, durante este tiempo, el servidor perimetral Akamai denegará el tráfico para el dominio. Este periodo de tiempo depende de la frecuencia con la que se renueva la caché de DNS, y varía según cada proveedor de DNS.

Una CDN se puede detener solo cuando su estado es `Running`.
{: note}

**NO** se recomienda detener una CDN si está configurada con un certificado SAN de HTTPS puesto que el tráfico HTTPS podría no funcionar al volver a pasar la CDN de nuevo al estado `Running`.
{: important}

Detener una CDN para un dominio comodín **NO** se permite en este momento.
{: important}

**Paso 1:**

Pulse "Detener CDN" en el menú de desbordamiento (3 puntos verticales a la derecha del estado de la CDN).
 ![Menú de desbordamiento](images/stop_cdn.png)

**Paso 2:**

Se mostrará una ventana de diálogo más grande para que confirme que desea detener el servicio. Seleccione **Confirmar** para continuar.

**Paso 3:**

Transcurridos entre 5 y 15 segundos, el estado debería cambiar a "Stopped".

## Suprimir una CDN
{: #deleting-a-cdn}

Para suprimir una CDN, siga estos pasos:

Al seleccionar `Suprimir` en el menú de desbordamiento, solo se suprime la CDN; no se suprime la cuenta.
{: note}

**Paso 1:**

Pulse "Suprimir" en el menú de desbordamiento.

 ![Menú de desbordamiento Suprimir CDN](images/delete_cdn.png)

**Paso 2:**

Se mostrará una ventana de diálogo más grande para que confirme que desea efectuar la supresión. Pulse **Suprimir** para continuar.

Si la CDN está configurada utilizando HTTPS con Certificado SAN DV, se podría tardar hasta 5 horas en completar el proceso de supresión.
{: note}

  ![Supresión con advertencia](images/delete-with-warning.png)

**Paso 3:**

Después de completar los pasos 1 y 2, el estado de la CDN será `Suprimiendo`. Cuando finalice el proceso de supresión, al volver a pulsar en "Obtener estado" en el menú de desbordamiento, se eliminará la fila de la lista de CDN. Si el proceso de supresión no se ha completado, esta acción no tendrá efecto.
