---

copyright:
  years: 2017,2018
lastupdated: "2018-02-06"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Cómo solicitar una CDN

A continuación, aprenderá a solicitar una red de entrega de contenido (CDN). La CDN puede solicitarse desde el [Portal de cliente ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://control.softlayer.com/) o el [portal de Bluemix](https://www.ibm.com/cloud-computing/bluemix/).

## Desde el portal de control:

**Paso 1:**

Para empezar, inicie sesión en el [Portal de cliente ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://control.softlayer.com/) con sus credenciales exclusivas.

**Paso 2:**

En la barra de navegación de la parte superior de la pantalla, seleccione **Red-> CDN**.

   ![Opciones del menú Red](images/network-cdn.png)

**Paso 3:**

En la página **Redes de entrega de contenido**, seleccione el botón **Pedir CDN** en la esquina superior derecha.

   ![Seleccione Pedir CDN](images/order-cdn-button.png)

## En el portal de Bluemix:

**Paso 1:**

Inicie sesión en el [Portal de Bluemix](https://www.ibm.com/cloud-computing/bluemix/)

**Paso 2:**

Haga clic en [IBM Bluemix Catalog](https://console.bluemix.net/catalog/). En la barra de navegación de la izquierda, seleccione **Red**.

   ![Navegación en la CDN de Bluemix](images/bluemix_navigation.png)

**Paso 3:**

Pulse el **mosaico de CDN**, que le llevará a la pantalla Selección de proveedor.

   ![Icono CDN de Bluemix](images/bluemix_tile.png)


**Paso 4:**

En la pantalla **Seleccione un proveedor de CDN**, elija entre las opciones de proveedor de CDN. Pulse el botón **Seleccionar** para confirmar las opciones seleccionadas y, a continuación, pulse **Iniciar suministro** en la parte inferior derecha de la pantalla, para iniciar el proceso de suministro.  
       ![Seleccione un proveedor de CDN](images/Vendor_Select_And_Provision.png)

**Paso 5:**

Rellene el campo **Configurar nombre**:  

  * Especifique el **Nombre de host** (**necesario**), que ejercerá de identificador primario de la CDN (por ejemplo, _example.testingcdn.net_).  
  * De manera opcional, puede proporcionar un **CNAME** personalizado (como _miprimercdn.cdnedge.bluemix.net_). Si no se suministra ningún CNAME, se creará uno. <La información de validación se incluirá aquí>  

       ![Configuración del nombre](images/configure-hostname-cname.png)  

    **Nota**: El uso de un CNAME inadecuado puede llevar a la terminación de los servicios.

**Paso 6:**

Rellene el campo **Configure el origen**: para configurar este campo, debe seleccionar la opción **Servidor** u **Object Storage**.  

   * Especifique la **Cabecera de host** (opcional). Si no se proporciona ninguna, se establece el valor predeterminado del **Nombre de host**. Consulte la descripción de la característica para [Soporte de la cabecera de host](about.html#host-header-support-) para obtener más información sobre la cabecera de host.  
   
   * Proporcione una **Vía de acceso** (opcional). La vía de acceso debe ser relativa al **Nombre de host**. 
   
      ![Configurar origen](images/configure-origin.png)  

  * **Opción Servidor**: si selecciona la opción **Servidor**, indique el nombre de host o la dirección IP del servidor de origen desde el que se deberían almacenar los datos en memoria caché.
      * Debe especificar la **Dirección del servidor de origen** (nombre de host o dirección IPv4 del servidor de origen) si ha seleccionado esta opción. Si se selecciona **puerto HTTPS**, la **Dirección del servidor de origen** debe ser el nombre de host y no la dirección IP.
      * También puede proporcionar un **Puerto HTTP**, un **Puerto HTTPS** o ambos. Estos campos indican el protocolo y el número de puerto que pueden utilizarse para contactar con el servidor de origen. Para los números de puerto no predeterminados, consulte [las Preguntas más frecuentes](faq.html#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) para ver una lista de los números de puerto permitidos.

	     ![Configuración del servidor de origen](images/configure-origin-server.png)

  * **Opción Object Storage**: si selecciona la opción **Object Storage**, debe proporcionar la información siguiente:
      * el **Punto final** desde el que se captará el objeto;
      * el nombre del **Grupo** donde se almacenará el contenido; y
      * el **Puerto HTTPS**.
      * Además, puede indicar las extensiones de archivo, separadas por comas, que se pueden utilizar en el servicio de CDN. (Si no se especifica ninguna extensión de nombre de archivo, se permitirán todas las extensiones).
      * Debe establecer la **Lista de control de accesos** (ACL) para cada **objeto** del **grupo** como "public-read".

	     ![Configuración del almacenamiento de objetos](images/configure-origin-object-storage.png)

**Paso 7:**

Configure el campo **Otras opciones**: esta sección contiene las opciones de configuración del campo **Respetar cabeceras**.

   * Cuando la opción **Respetar cabeceras** está **activada**, los valores de TTL definidos en la cabecera para el origen alterarán los valores TTL predeterminados de la CDN. **Respetar cabeceras** está definido como **Activado** de forma predeterminada, pero debe configurar este campo.  

        ![Otras opciones](images/other-options.png)

**Paso 8:**

Seleccione el botón **Crear** situado en la esquina inferior derecha para crear la CDN.
