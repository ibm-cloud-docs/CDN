---

copyright:
  years: 2017,2018
lastupdated: "2018-11-19"

---

{:shortdesc: .shortdesc}
{:new_window: target="blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Cómo solicitar una CDN

A continuación, aprenderá a solicitar una red de entrega de contenido (CDN). La CDN se puede solicitar desde el [Portal de IBM Cloud](https://www.ibm.com/cloud-computing/bluemix/).

## Navegación a la página de CDN:

**Paso 1:**

Inicie una sesión en su cuenta desde el [Portal de IBM Cloud](https://www.ibm.com/cloud-computing/bluemix/)

**Paso 2:**

Haga clic en [IBM Cloud Catalog](https://console.bluemix.net/catalog/). En la barra de navegación de la izquierda, seleccione **Red**.

   ![Navegación en la CDN de Bluemix](images/bluemix_navigation.png)

**Paso 3:**

Pulse el **mosaico de CDN**.

   ![Icono CDN de Bluemix](images/bluemix_tile.png)


## Solicite una nueva CDN:

**Paso 1:**

Pulse **Crear** en la parte inferior derecha, para crear su cuenta de CDN si aún no tiene una y redirigirle a la pantalla Configurar CDN.

   ![Visión general de CDN](images/content-delivery.png)

**Paso 2:**

Rellene el campo **Configurar nombre**:  

  * Especifique el **Nombre de host** (**necesario**), que ejercerá de identificador primario de la CDN (por ejemplo, `example.testingcdn.net`).  
  * De manera opcional, puede proporcionar un **CNAME** personalizado (como `miprimercdn.cdnedge.bluemix.net`). Si no se suministra ningún CNAME, se creará uno. El sufijo `cdnedge.bluemix.net` se añade de forma automática al CNAME. El uso de un CNAME inadecuado puede llevar a la terminación de los servicios.

       ![Configuración del nombre](images/configure-hostname-cname.png)  

    **Nota**: Después de suministrar su nueva CDN, **debe** configurar el CNAME con su proveedor de DNS.

**Paso 3:**

Rellene el campo **Configure el origen**: para configurar este campo, debe seleccionar la opción **Servidor** u **Object Storage**.  

  * **Paso 3, Opción 1: Opción de servidor **

     ![Configurar origen](images/configure-origin-server.png)

      * Debe especificar la **Dirección del servidor de origen** (nombre de host o dirección IPv4 del servidor de origen). Si también se selecciona **puerto HTTPS**, la **Dirección del servidor de origen** debe ser el nombre de host y no la dirección IP.

      * Especifique la **Cabecera de host** (opcional). Si no se proporciona ninguna, se establece el valor predeterminado de **Nombre de host**. Consulte la descripción de la característica para [Soporte de la cabecera de host](feature-descriptions.html#host-header-support) para obtener más información sobre la cabecera de host.  

      * Proporcione una **Vía de acceso** en la que el contenido se pueda recuperar del origen (opcional). Consulte la descripción de la característica para [Correlaciones de CDN basadas en vías de acceso](feature-descriptions.html#path-based-cdn-mappings) para entender las implicaciones de añadir una vía de acceso en este instante.

      * También puede proporcionar un **Puerto HTTP**, un **Puerto HTTPS** o ambos. Estos campos indican el protocolo y el número de puerto que pueden utilizarse para contactar con el servidor de origen. Para los números de puerto no predeterminados, consulte [las Preguntas más frecuentes](faqs.html#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) para ver una lista de los números de puerto permitidos.

      * **Certificado SSL** Esta opción aparece _sólo_ cuando se seleccione el puerto HTTPS. Si selecciona **Puerto HTTPS** para Servidor o para Object Storage, puede elegir **Comodín** o **Certificado SAN DV** como la opción de certificado SSL. Ambos ofrecen la seguridad mejorada proporcionada por HTTPS.
        * El **Certificado comodín** permite el tráfico HTTPS sólo cuando se utiliza el **CNAME** y no requiere ninguna acción adicional por su parte
        * El **Certificado SAN DV** permite el tráfico HTTPS sobre el dominio, pero requiere pasos adicionales para su verificación. Consulte [Completar la validación de control de dominio para HTTPS](how-to-https.html#completing-domain-control-validation-for-https) para entender los pasos necesarios y las restricciones de tiempo que suponen esta opción.

	     ![Configuración del servidor de origen](images/ssl-cert-options.png)

  * **Paso 3, Opción 2: Opción Object Storage**

    ![Configurar almacenamiento de objetos](images/configure-origin-object-storage.png)

      * **Debe** especificar el **Punto final** desde el que recuperar los objetos.

      * Especifique la **Cabecera de host** (opcional). Si no se proporciona ninguna, se establece el valor predeterminado de **Nombre de host**. Consulte la descripción de la característica para [Soporte de la cabecera de host](feature-descriptions.html#host-header-support) para obtener más información sobre la cabecera de host.  

      * Proporcione una **Vía de acceso** en la que el contenido se pueda recuperar del origen (opcional). Consulte la descripción de la característica para [Correlaciones de CDN basadas en vías de acceso](feature-descriptions.html#path-based-cdn-mappings) para entender las implicaciones de añadir aquí una vía de acceso.

      * **Debe** proporcionar el nombre del **Grupo** en el que se almacena su contenido.

      * También puede proporcionar un **Puerto HTTP**, un **Puerto HTTPS** o ambos. Estos campos indican el protocolo y el número de puerto que pueden utilizarse para contactar con el servidor de origen. Para los números de puerto no predeterminados, consulte [las Preguntas más frecuentes](faqs.html#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) para ver una lista de los números de puerto permitidos.

      * **Certificado SSL** Esta opción aparece _sólo_ cuando se seleccione el puerto HTTPS. Si selecciona **Puerto HTTPS** para Servidor o para Object Storage, puede elegir **Comodín** o **Certificado SAN DV** como la opción de certificado SSL. Ambos ofrecen la seguridad mejorada proporcionada por HTTPS.
        * El **Certificado comodín** permite el tráfico HTTPS sólo cuando se utiliza el **CNAME** y no requiere ninguna acción adicional por su parte
        * El **Certificado SAN DV** permite el tráfico HTTPS sobre el dominio, pero requiere pasos adicionales para su verificación. Consulte [Completar la validación de control de dominio para HTTPS](how-to-https.html#completing-domain-control-validation-for-https) para entender los pasos necesarios y las restricciones de tiempo que suponen esta opción.

        ![Configurar HTTPS](images/ssl-cert-options.png)

      **NOTA** Debe establecer la **Lista de control de acceso** (ACL) para cada objeto en su grupo en "public-read" con su proveedor de objetos de nube.
      
**Paso 4:**

* Debe seleccionar **He leído el contrato maestro de servicio y acepto sus condiciones** en la parte inferior derecha, sobre el botón **Crear**.

* Luego seleccione el botón **Crear** en la esquina inferior derecha para crear su CDN.
