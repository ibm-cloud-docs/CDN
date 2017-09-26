---

copyright:
  years: 2017
lastupdated: "2017-09-10"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Cómo solicitar una CDN

A continuación, aprenderá a solicitar una red de entrega de contenido (CDN). La CDN puede solicitarse desde el [Portal de cliente ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://control.softlayer.com/) o el [portal del catálogo de Bluemix](https://www.ibm.com/cloud-computing/bluemix/).

## Desde el portal de control:

1. Para empezar, inicie sesión en el [Portal de cliente ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://control.softlayer.com/) con sus credenciales exclusivas.

2. En la barra de navegación de la parte superior de la pantalla, seleccione **Red-> CDN**.

   ![Opciones del menú Red](images/network-cdn.png)

3. En la página **Redes de entrega de contenido**, seleccione el botón **Pedir CDN** en la esquina superior derecha. 

	![Seleccione Pedir CDN](images/order-cdn-button.png)

## En el portal de Bluemix:

1. Inicie sesión en el [Portal de Bluemix](https://www.ibm.com/cloud-computing/bluemix/)

2. Haga clic en **IBM Bluemix Catalog**. En la barra de navegación de la izquierda, seleccione **Red**.

   ![Navegación en la CDN de Bluemix](images/bluemix_navigation.png)

3. Pulse el **mosaico de CDN**, que le llevará a la pantalla Selección de proveedor. 

   ![Icono CDN de Bluemix](images/bluemix_tile.png)

4. En la pantalla **Seleccione un proveedor de CDN**, elija entre las opciones de proveedor de CDN. Pulse el botón **Seleccionado** situado en la parte inferior de la pantalla para confirmar las opciones seleccionadas y, a continuación, pulse **Iniciar suministro** para que empiece el proceso de suministro. 

	![Seleccione un proveedor de CDN](images/newReducedSizeVendorSelectAndProvision.png)
	
5. Rellene el campo **Configurar nombre**: 
      * Especifique el _nombre de host_ (**necesario**), que ejercerá de identificador primario de la CDN (por ejemplo, _ejemplo.testingcdn.net_).
      * De manera opcional, puede proporcionar un _CNAME_ personalizado (como _miprimercdn.cdnedge.bluemix.net_). Si no se suministra ningún CNAME, se creará uno. <La información de validación se incluirá aquí>
      
      ![Configuración del nombre](images/configure-hostname-cname.png)
		
6. Rellene el campo **Configure el origen**: para configurar este campo, debe seleccionar la opción **Servidor** u **Object Storage**. (La especificación de la **Vía de acceso** es opcional. <Información de validación>)
		
  * **Opción Servidor**: si selecciona la opción **Servidor**, indique el nombre de host o la dirección IP del servidor de origen desde el que se deberían almacenar los datos en memoria caché.  
      * Debe especificar la **Dirección IP** del servidor de origen si elige esta opción. 
      * También puede proporcionar un **Puerto HTTP**, un **Puerto HTTPS** o ambos. (Estos campos indican el protocolo y el número de puerto que pueden utilizarse para contactar con el servidor de origen).

	   ![Configuración del servidor de origen](images/configure-origin-server.png)
		
  * **Opción Object Storage**: si selecciona la opción **Object Storage**, debe proporcionar la información siguiente:
      * el **Punto final** desde el que se captará el objeto;
      * el nombre del **Grupo** donde se almacenará el contenido; y
      * el **Puerto HTTPS**.
      * Además, puede indicar las extensiones de archivo, separadas por comas, que se pueden utilizar en el servicio de CDN. (Si no se especifica ninguna extensión de nombre de archivo, se permitirán todas las extensiones).
      * Debe establecer la **Lista de control de accesos** (ACL) para cada **objeto** del **grupo** como "public-read".
		
	   ![Configuración del almacenamiento de objetos](images/configure-origin-object-storage.png)

7. Configure el campo **Otras opciones**: esta sección contiene las opciones de configuración del campo **Proporcionar contenido obsoleto** y el campo **Respetar cabeceras**.
    
     * El campo **Proporcionar contenido obsoleto**: **Proporcionar contenido obsoleto** es un valor booleano que indica si se suministra contenido almacenado en memoria caché que haya caducado, en caso de que no se pueda alcanzar el servidor de origen. Debido a una limitación actual, la funcionalidad **Proporcionar contenido obsoleto** está habilitada de forma predeterminada, independientemente de que un usuario la haya definido como **Activado** o **Desactivado**.
     * El campo **Respetar cabeceras**: cuando la opción **Respetar cabeceras** está **activada**, los valores de TTL definidos en la cabecera para el origen alterarán los valores TTL predeterminados de la CDN. **Respetar cabeceras** está definido como **Activado** de forma predeterminada, pero debe configurar este campo. 

		![Otras opciones](images/other-options.png)
		
8. Seleccione el botón **Crear** situado en la esquina inferior derecha para crear la CDN. 
