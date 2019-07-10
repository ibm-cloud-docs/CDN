---

copyright:
  years: 2017,2018, 2019
lastupdated: "2019-05-21"

keywords: order, create, configure, console, origin, preparation, bucket

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:download: .download}

# Cómo solicitar una CDN
{: #order-a-cdn}

A continuación, aprenderá a solicitar una red de entrega de contenido (CDN). La CDN se puede solicitar desde la [consola de {{site.data.keyword.cloud}}](https://cloud.ibm.com/login).

## Preparación para la solicitud
{: #preparation-for-ordering}

A continuación se detalla cómo navegar hasta la página de CDN para realizar el pedido.

**Paso 1**

* Inicie una sesión en su cuenta desde la [Consola de IBM Cloud](https://cloud.ibm.com/login)

**Paso 2**

Haga clic en [IBM Cloud Catalog](https://cloud.ibm.com/catalog/). En la barra de navegación de la izquierda, seleccione **Red**.

   ![Navegación de IBM Cloud CDN](images/bluemix_navigation.png)

**Paso 3**

Pulse el **mosaico de CDN**.

   ![Icono de IBM Cloud CDN](images/bluemix_tile.png)


## Solicite una nueva CDN
{: #order-a-new-cdn}

Una vez está en la página de pedidos, ésta es la forma de proceder a crear y configurar su CDN.

### Paso 1: Crear la cuenta de CDN
{: #create-your-cdn-account}

Pulse **Crear** en la parte inferior derecha, para crear su cuenta de CDN si aún no tiene una y redirigirle a la pantalla Configurar CDN.

   ![Visión general de CDN](images/content-delivery.png)

### Paso 2: Dar nombre al CDN
{: #step-2-name-your-cdn}

Rellene el campo **Configurar nombre**:  

  * Especifique el **Nombre de host** (**necesario**), que ejercerá de identificador primario de la CDN (por ejemplo, `example.testingcdn.net`).  
  * De manera opcional, puede proporcionar un **CNAME** personalizado (como `miprimercdn.cdnedge.bluemix.net`). Si no se suministra ningún CNAME, se creará uno. El sufijo `cdnedge.bluemix.net` se añade de forma automática al CNAME. El uso de un CNAME inadecuado puede llevar a la terminación de los servicios.

       ![Configuración del nombre](images/configure-hostname-cname.png)  

Después de suministrar su nueva CDN, **debe** configurar el CNAME con su proveedor de DNS.
{: note}
### Paso 3: Configurar el origen
{: #step-3-configure-your-origin}

Rellene el campo **Configure el origen**: para configurar este campo, debe seleccionar la opción **Servidor** u **Object Storage**.  

  * **Paso 3, Opción 1: Opción de servidor**

     ![Configurar origen](images/configure-origin-server.png)

      * Debe especificar la **Dirección del servidor de origen** (nombre de host o dirección IPv4 del servidor de origen). Si también se selecciona **puerto HTTPS**, la **Dirección del servidor de origen** debe ser el nombre de host y no la dirección IP.

      * Especifique la **Cabecera de host** (opcional). Si no se proporciona ninguna, se establece el valor predeterminado de **Nombre de host**. Consulte la descripción de la característica para [Soporte de la cabecera de host](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#host-header-support) para obtener más información sobre la cabecera de host.  

      * Proporcione una **Vía de acceso** en la que el contenido se pueda recuperar del origen (opcional). Consulte la descripción de la característica para [Correlaciones de CDN basadas en vías de acceso](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#path-based-cdn-mappings) para entender las implicaciones de añadir una vía de acceso en este instante.

      * También puede proporcionar un **Puerto HTTP**, un **Puerto HTTPS** o ambos. Estos campos indican el protocolo y el número de puerto que pueden utilizarse para contactar con el servidor de origen. Para los números de puerto no predeterminados, consulte [las Preguntas más frecuentes](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) para ver una lista de los números de puerto permitidos.

      * **Certificado SSL** Esta opción aparece _sólo_ cuando se seleccione el puerto HTTPS. Si selecciona **Puerto HTTPS** para Servidor o para Object Storage, puede elegir **Comodín** o **Certificado SAN DV** como la opción de certificado SSL. Ambos ofrecen la seguridad mejorada proporcionada por HTTPS.
        * El **Certificado comodín** permite el tráfico HTTPS sólo cuando se utiliza el **CNAME** y no requiere ninguna acción adicional por su parte
        * El **Certificado SAN DV** permite el tráfico HTTPS sobre el dominio, pero requiere pasos adicionales para su verificación. Consulte [Completar la validación de control de dominio para HTTPS](/docs/infrastructure/CDN/how-to-https.html#completing-domain-control-validation-for-https) para entender los pasos necesarios y las restricciones de tiempo que suponen esta opción.

	     ![Configuración del servidor de origen](images/ssl-cert-options.png)

  * **Paso 3, Opción 2: Opción Object Storage**

    ![Configurar almacenamiento de objetos](images/configure-origin-object-storage.png)

      * **Debe** especificar el **Punto final** desde el que recuperar los objetos.

      * Especifique la **Cabecera de host** (opcional). Si no se proporciona ninguna, se establece el valor predeterminado de **Nombre de host**. Consulte la descripción de la característica para [Soporte de la cabecera de host](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#host-header-support) para obtener más información sobre la cabecera de host.  

      * Proporcione una **Vía de acceso** en la que el contenido se pueda recuperar del origen (opcional). Consulte la descripción de la característica para [Correlaciones de CDN basadas en vías de acceso](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#path-based-cdn-mappings) para entender las implicaciones de añadir aquí una vía de acceso.

      * **Debe** proporcionar el nombre del **Grupo** en el que se almacena su contenido.

      * También puede proporcionar un **Puerto HTTP**, un **Puerto HTTPS** o ambos. Estos campos indican el protocolo y el número de puerto que pueden utilizarse para contactar con el servidor de origen. Para los números de puerto no predeterminados, consulte [las Preguntas más frecuentes](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) para ver una lista de los números de puerto permitidos.

      * **Certificado SSL** Esta opción aparece _sólo_ cuando se seleccione el puerto HTTPS. Si selecciona **Puerto HTTPS** para Servidor o para Object Storage, puede elegir **Comodín** o **Certificado SAN DV** como la opción de certificado SSL. Ambos ofrecen la seguridad mejorada proporcionada por HTTPS.
        * El **Certificado comodín** permite el tráfico HTTPS sólo cuando se utiliza el **CNAME** y no requiere ninguna acción adicional por su parte
        * El **Certificado SAN DV** permite el tráfico HTTPS sobre el dominio, pero requiere pasos adicionales para su verificación. Consulte [Completar la validación de control de dominio para HTTPS](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#completing-domain-control-validation-for-https) para entender los pasos necesarios y las restricciones de tiempo que suponen esta opción.

        ![Configurar HTTPS](images/ssl-cert-options.png)

Debe establecer la **Lista de control de acceso** (ACL) para cada objeto en su grupo en "public-read" con su proveedor de objetos de nube.
{: note}
      
### Paso 4
{: #step-4}

* Debe seleccionar **He leído el contrato maestro de servicio y acepto sus condiciones** en la parte inferior derecha, sobre el botón **Crear**.

* Luego seleccione el botón **Crear** en la esquina inferior derecha para crear su CDN.
