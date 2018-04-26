---

copyright:
  years: 2017, 2018
lastupdated: "2018-03-23"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Acerca de las redes de entrega de contenidos (CDN)

Una red de entrega de contenido (CDN) es un conjunto de servidores perimetrales que se distribuyen por diversas regiones del país o del mundo. El contenido web se proporciona desde un servidor perimetral, que se encuentra en el área geográfica más cercana al cliente que solicita el contenido. Esta técnica permite que los usuarios finales reciban el contenido con menos retraso, además de ofrecer una mejor experiencia a sus clientes.

## ¿Cómo funciona una CDN?

Una CDN logra su objetivo almacenando contenido en la memoria caché de servidores perimetrales en todo el mundo. Cuando un cliente solicita contenido web, la solicitud se direcciona al servidor perimetral más cercano al cliente desde el punto de vista geográfico. Al reducir la distancia que debe desplazarse el contenido, la CDN ofrece rendimiento optimizado, latencia minimizada y aumento de rendimiento. Al utilizar IBM Cloud Content Delivery Network con Akamai, los proveedores de contenidos garantizan la entrega eficiente del contenido solicitado en todo el mundo, con una configuración mínima.

Las características principales del servicio IBM Cloud Content Delivery Network incluyen:
  * Soporte al origen de servidor de host
  * Soporte al origen de Object Storage
  * Soporte para múltiples orígenes con distintas vías de acceso
  * Correlaciones de CDN basadas en vías de acceso
  * Depuración de contenido almacenado en memoria caché
  * Tiempo de duración (TTL)
  * Métricas con vistas gráficas
  * Soporte para HTTPS con certificado comodín
  * Soporte a la cabecera de host
  * Proporcionar contenido obsoleto
  * Característica "Ignorar argumentos de consulta en la clave de caché"
  * Compresión de contenido
  * Optimización de archivos de gran tamaño
  * Vídeo on Demand

## Descripciones de características

### Soporte al origen de servidor de host

IBM Cloud Content Delivery Network (CDN) se puede configurar para que sirva contenido desde un origen de servidor de host proporcionando el nombre de host del origen, protocolo, número de puerto y, opcionalmente, la vía de acceso desde la cual servir el contenido. La vía de acceso predeterminada es '/\*'. El protocolo puede ser HTTP, HTTPS o ambos. Akamai solo admite determinados números de puerto. Consulte las [preguntas más frecuentes](faqs.html#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) para conocer los números de puerto/rangos soportados. Actualmente, HTTPS se admite utilizando el certificado comodín, lo que significa que el servicio será accesible solo a través de la terminación de CNAME con `.cdnedge.bluemix.net`. Consulte la descripción de la característica [HTTPS con certificado comodín](about.html#https-with-wildcard-certificate-) para obtener más información.

### Soporte al origen de Object Storage

IBM Cloud CDN se puede configurar para servir contenido desde un punto final de Object Storage proporcionando el _punto final_, el _nombre de grupo_, _protocolo_, y _puerto_. Opcionalmente, se puede especificar una lista de extensiones de archivo para que permitan únicamente el almacenamiento en caché de archivos con dichas extensiones. Todos los objetos del grupo tienen que configurarse con acceso de lectura público o anónimo. Esta guía de aprendizaje sobre [Cómo configurar IBM Bluemix Object Storage con CDN](https://console.bluemix.net/docs/tutorials/static-files-cdn.html#accelerate-delivery-of-static-files-using-a-cdn) proporciona información más detallada.

### Soporte para múltiples orígenes con distintas vías de acceso

En algunos casos, es posible que desee entregar contenido determinado desde un servidor de origen diferente. Por ejemplo, puede que desee servir algunas fotos o vídeos desde distintos servidores de origen. IBM Cloud CDN ofrece la opción de configurar múltiples servidores de origen con varias vías de acceso. Esto proporciona flexibilidad sobre cómo y dónde se almacenan los datos. La vía de acceso proporcionada para el servidor de origen debe ser exclusivo con respecto a la CDN. El servidor de origen no tiene que ser exclusivo.

### Correlaciones de CDN basadas en vías de acceso

El servicio de IBM Cloud CDN puede restringirse a una vía de acceso del directorio determinada en el servidor de origen proporcionando la vía de acceso al crear la CDN. Un usuario final solo podrá acceder al contenido que resida en dicha vía de acceso del directorio. Por ejemplo, si se crea una CDN www.example.com con la vía de acceso '/vídeos', solo será accesible a través de `www.example.com/videos/`.

### Depuración de contenido almacenado en memoria caché

IBM Cloud CDN proporciona la capacidad de eliminar de forma cómoda y rápida, o "depurar", el contenido en la memora caché desde los servidores perimetrales. En este momento, solo se permite la depuración para una vía de acceso de archivo. Actualmente, la implementación de la depuración con Akamai depura el archivo en unos 5 segundos. La interfaz de usuario también proporciona la capacidad de visualizar el historial de depuración y de etiquetar vías de acceso de depuración específicas como favoritas.

### Tiempo de duración (TTL)

El Tiempo de duración indica la cantidad de tiempo (en segundos) que el servidor perimetral almacenará en memoria caché el contenido para una vía de acceso de directorio o archivo determinada. Cuando se crea una CDN por primera vez, se crea un Tiempo de duración (TTL) global para la vía de acceso ('/\*') con un tiempo predeterminado de 3600 segundos. El valor mínimo para TTL es 30 segundos, y el valor máximo es 2147483647 segundos. Para cada entrada, la vía de acceso de TTL debe ser exclusiva para la CDN. Si varias vías de acceso coinciden con un contenido determinado, la coincidencia de vía de acceso configurada más recientemente será la que se aplique a dicho contenido. Por ejemplo, considere dos TTL, `/example/file` creada primero con un valor de tiempo de duración de 3000 segundos y `/example/*` creada posteriormente, con un valor de 4000 segundos. Aunque `/example/file` es más específica, `/example/*` se ha creado más recientemente, por lo que el TTL para `/example/file` será de 4000 segundos. Una vez creadas, las entradas de TTL se pueden editar para la vía de acceso y/o el tiempo. También se pueden suprimir.

### Métricas con vistas gráficas

Las métricas para una CDN individual se proporcionan en el separador Visión general del Portal del cliente para dicha correlación de CDN. Se calculan dos tipos de métricas desde el uso de CDN: las que muestran las métricas durante un periodo de tiempo como un gráfico, y las que se muestran como valores agregados.

Para métricas que muestran el cambio durante un periodo de tiempo como un gráfico, puede ver tres gráficos de líneas y un gráfico circular. Los tres gráficos de líneas son: **Ancho de bando**, **Coincidencias por correlación** y **Coincidencias por tipo**. Muestran la actividad diaria durante un marco de tiempo especificado. Los gráficos para **Ancho de banda** y **Coincidencias por correlación** son gráficos de una sola línea, mientras que el desglose de **Coincidencias por tipo** muestra una línea para cada uno de los tipos de coincidencia proporcionados. El gráfico circular muestra un desglose regional de un ancho de banda para una correlación de CDN en forma de porcentaje.

Las métricas mostradas para los valores agregados incluyen **Uso de ancho de banda** en GB, **Total de coincidencias** en el servidor perimetral de CDN y la **Proporción de coincidencias**. La proporción de coincidencias indica el porcentaje de veces que el contenido es entregado por el servidor perimetral, no a través de su origen. La proporción de coincidencias actualmente se muestra como una función de todas sus correlaciones de CDN, no solo la que se visualiza.

De forma predeterminada, los números agregados y los gráficos muestran las métricas de los últimos 30 días, pero puede cambiar este valor a través del [Portal del cliente ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://control.softlayer.com/). Ambas categorías son capaces de mostrar las métricas para periodos de 7, 15, 30, 60 o 90 días.

### Soporte para HTTPS con certificado comodín

El certificado comodín es la forma más asequible de entregar contenido web a los usuarios de forma segura. Esta característica se habilita seleccionando `Puerto HTTPS` al configurar la CDN o la vía de acceso de origen. Después de habilitar la correlación de CDN para HTTPS mediante el certificado comodín, el servidor perimetral contactará con el servidor de origen a través de HTTPS. Si el servidor de origen se especifica como un nombre de host, el servidor perimetral utiliza el nombre de host del origen como cabecera de Server Name Indication (SNI) para la negociación de TLS (seguridad de la capa de transporte) con el servidor de origen de forma predeterminada. Sin embargo, si el servidor de origen se especifica como dirección IP, el nombre de host de la CDN se utilizará como cabecera de SNI. Una entidad emisora de certificados (CA) reconocida debe firmar el certificado de origen.

Si se utiliza el certificado de comodín, un usuario final **solo** tendrá acceso al servicio mediante el CNAME. Por ejemplo, si el dominio es `example.com` y el CNAME es `example.cdnedge.bluemix.net`, el servicio de CDN **solo** será accesible a través de `https://example.cdnedge.bluemix.net`. Consulte las [preguntas más frecuentes](faqs.html#what-should-be-the-expected-behavior-when-loading-the-cname-or-hostname-on-your-browser-for-the-supported-cdn-protocols-) para comprender las implicaciones de utilizar HTTPS con certificado comodín.

Como mejor práctica del sector, Akamai solo confía en los certificados raíz y no en los certificados intermedios, porque el conjunto de certificados intermediarios de confianza cambia con frecuencia. Por motivos de seguridad, Akamai solo incluye entidades emisoras de certificados raíz en el almacén de confianza de Akamai. Por tanto, el origen debe enviar toda la cadena de certificado, incluyendo el certificado hoja y todos los certificados intermedios (sin incluir el certificado raíz) como parte del reconocimiento SSL con el servidor perimetral. Encontrará los certificados de confianza de Akamai [aquí](https://community.akamai.com/docs/DOC-4447-ssltls-certificate-chains-for-akamai-managed-certificates).

### Soporte a la cabecera de host

El servidor perimetral utiliza la cabecera de host cuando se comunica con el host de origen. Esta característica proporciona flexibilidad para configurar el servicio web en el host de origen. Si no se proporciona la entrada de cabecera de host, el servicio utiliza el nombre de host del servidor de origen como cabecera de host HTTP predeterminada si el servidor de origen se especifica como nombre de host (en lugar de una dirección IP). Si no se proporciona la cabecera de host como entrada y se proporciona el servidor de origen como una dirección IP, se utiliza el nombre de host de CDN (también denominado Nombre de dominio de CDN) como la cabecera de host HTTP predeterminada.

### Respetar cabeceras

La opción Respetar cabeceras permite que la configuración de la cabecera HTTP en el origen sustituya a la configuración de CDN. Esta característica está activada de forma predeterminada, pero puede desactivarse.

### Proporcionar contenido obsoleto

Cuando el servidor perimetral de CDN recibe una solicitud de usuario, y el contenido de la solicitud no está almacenado en caché, el servidor perimetral contacta con el host de origen para recoger el contenido. En ese momento, dicho contenido se almacena en caché para el valor de Tiempo de duración (TTL) especificado para el contenido. Si se recibe una solicitud de usuario después de que caduque el TTL, el servidor perimetral contacta con el host de origen para recoger el contenido. Si no se puede contactar con el servidor de origen por algún motivo (por ejemplo, el host de origen está inactivo o existe un problema de red), el servidor perimetral sirve el contenido caducado (obsoleto) a la solicitud. Esta característica está admitida por Akamai y **no se puede** desactivar.

### Característica "Ignorar argumentos de consulta en la clave de caché"

Los servidores perimetrales de Akamai almacenan en caché el contenido en lo que denominan "Cache Store". Para utilizar el contenido de "Cache Store", los servidores perimetrales utilizan una clave de caché. Normalmente, la clave de caché se genera a partir de una parte del URL de un usuario final. En algunos casos, el URL contiene argumentos de función de consulta que son distintos para usuarios individuales, pero el contenido proporcionado es el mismo. De forma predeterminada, Akamai utiliza los argumentos de la función de consulta para generar la clave de caché y, por tanto, para generar una clave de caché exclusiva para cada usuario. Este método no es óptimo, porque fuerza al servidor perimetral a contactar con el servidor de origen para contenido que ya está almacenado en caché, pero utilizando una clave de caché distinta. La característica **Ignorar argumentos de consulta en la clave de caché** le permite especificar si deben ignorarse los argumentos de consulta cuando se genera una clave de caché. Esta característica se aplica a cualquier acción de `crear` o `actualizar` de una configuración de correlación de CDN, así como para cualquier acción de `crear` o `actualizar` de una vía de acceso de origen.

**Nota:** los argumentos de consulta de clave de caché se pueden configurar desde el separador Valores después de crear una correlación de CDN. Para la vía de acceso de origen, se pueden configurar durante la acción de `crear` o `actualizar` de una vía de acceso de origen.

### Compresión de contenido

La compresión de contenidos está habilitada de forma predeterminada en las CDN de Akamai para los siguientes tipos de contenido:
* text/html*
* text/css*
* text/xml*
* text/json
* text/javascript*
* text/plain*
* application/x-javascript*
* application/json
* application/xml*

Si el servidor perimetral es el que gestiona la compresión, el contenido debe ser como mínimo de 10 KB.  En algunos casos, es el servidor de origen el que se encarga de la compresión, y entonces no hay límite en el tamaño de los archivos que se comprimen. Si el contenido ya está siendo comprimido por el servidor de origen, no se volverá a comprimir. Para habilitar la compresión de contenidos, la cabecera de solicitud debe definir `Accept-Encoding: gzip`.

### Optimización de archivos de gran tamaño

La optimización de archivos de gran tamaño permite a la red CDN optimizar la entrega de contenido de tamaño superior a 10 MB. Esta habilitación incrementa el rendimiento para archivos de gran tamaño y reduce la latencia y los tiempos de descarga. Sin esta característica, la CDN no puede servir archivos de más de 1,8 GB. Esta característica permite descargar archivos de más de 1,8 GB, hasta un máximo de 320 GB.

Para que funcione la optimización de archivos de gran tamaño, las solicitudes de rango de bytes **deben** estar habilitadas en el servidor de origen. La CDN de Akamai utiliza una técnica denominada Partial Object Caching para esta optimización. Cuando se solicita un archivo de gran tamaño, el servidor perimetral comprueba si el archivo cumple los requisitos de tamaño. Esto significa que los archivos de más de 10 MB se solicitarán desde el servidor de origen en bloques. Cuando el bloque llega al servidor perimetral, se almacena en caché y se sirve inmediatamente al usuario. El servidor perimetral recoge el siguiente bloque en paralelo, reduciendo así la latencia. Este proceso continúa hasta que se recupera todo el archivo, o finaliza la conexión.  

Cuando esta característica está habilitada, existe un pequeño coste de rendimiento asociado al servicio de contenido inferior a 10 MB. Por tanto, esta característica se recomienda únicamente para el servicio de archivos de gran tamaño. Un caso de uso típico sería crear una nueva vía de acceso de origen en la configuración de la CDN y habilitar la optimización de archivos de gran tamaño para dicha vía de acceso.

### Vídeo on Demand

La optimización del rendimiento de **Vídeo on Demand** ofrece streaming de alta calidad en distintos tipos de red. Con la capacidad de la red distribuida para distribuir la carga dinámicamente, la CDN de IBM Cloud con Akamai le proporciona la capacidad de escalar rápidamente para públicos de gran tamaño, tanto si lo había planificado como si no.

La opción **Vídeo on Demand** está optimizada para la distribución de formatos de streaming segmentados, como HLS, DASH, HDS y HSS. El streaming de vídeo en directo **no** está admitido por ahora. Puede habilitar la característica **Vídeo on Demand** seleccionando la opción desde el menú desplegable bajo **Optimizar para** en el separador Valores, o al crear una nueva vía de acceso de origen. Debe habilitar esta característica solo cuando optimice la entrega de archivos de vídeo.
