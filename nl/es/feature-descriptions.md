---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-21"

keywords: features, descriptions, metrics, multiple, origins, mapping, time to live, purge, cached, content, key, stale, https, geographical, access, protocol, compression, video, hotlink

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

# Descripciones de características
{: #feature-descriptions}

En esta página se destacan muchas de las características incluidas con {{site.data.keyword.cloud}} CDN basadas en Akamai.

## Soporte al origen de servidor de host
{: #host-server-origin-support}

IBM Cloud Content Delivery Network (CDN) se puede configurar para que sirva contenido desde un origen de servidor de host proporcionando el nombre de host del origen, protocolo, número de puerto y, opcionalmente, la vía de acceso desde la cual servir el contenido. La vía de acceso predeterminada es `/*`. El protocolo puede ser HTTP, HTTPS o ambos. Akamai solo admite determinados números de puerto. Consulte las [preguntas más frecuentes](docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) para conocer los números de puerto/rangos soportados.

## Soporte al origen de Object Storage
{: #object-storage-file-support}

IBM Cloud CDN se puede configurar para servir contenido desde un punto final de Object Storage proporcionando el punto final, el nombre de grupo, protocolo, y puerto. Opcionalmente, se puede especificar una lista de extensiones de archivo para que permitan únicamente el almacenamiento en caché de archivos con dichas extensiones. Todos los objetos del grupo tienen que configurarse con acceso de lectura público o anónimo.

Esta guía de aprendizaje sobre [Cómo configurar IBM Cloud Object Storage con CDN](/docs/tutorials?topic=solution-tutorials-static-files-cdn#static-files-cdn) proporciona información más detallada.

## Soporte para múltiples orígenes con distintas vías de acceso
{: #support-for-multiple-origins-with-distinct-paths}

En algunos casos, es posible que desee entregar contenido determinado desde un servidor de origen diferente. Por ejemplo, puede que desee servir algunas fotos o vídeos desde distintos servidores de origen. IBM Cloud CDN ofrece la opción de configurar múltiples servidores de origen con varias vías de acceso. Esto proporciona flexibilidad sobre cómo y dónde se almacenan los datos. La vía de acceso proporcionada para el servidor de origen debe ser exclusivo con respecto a la CDN. El servidor de origen no tiene que ser exclusivo.

## Correlaciones de CDN basadas en vías de acceso
{: #path-based-cdn-mappings}

El servicio de IBM Cloud CDN puede restringirse a una vía de acceso del directorio determinada en el servidor de origen proporcionando la vía de acceso al crear la CDN. Un usuario final solo podrá acceder al contenido que resida en dicha vía de acceso del directorio. Por ejemplo, si se crea una CDN `www.example.com` con la vía de acceso `/videos`, **únicamente** es accesible a través de `www.example.com/videos/*`.

## Depuración de contenido almacenado en memoria caché
{: #purge-cached-content}

IBM Cloud CDN proporciona la capacidad de eliminar de forma cómoda y rápida, o "depurar", el contenido en la memora caché desde los servidores perimetrales. En este momento, solo se permite la depuración para una vía de acceso de archivo. Actualmente, la implementación de la depuración con Akamai depura el archivo en unos 5 segundos. La interfaz de usuario también proporciona la capacidad de visualizar el historial de depuración y de etiquetar vías de acceso de depuración específicas como favoritas.

## Tiempo de duración (TTL)
{: #time-to-live-ttl}

El Tiempo de duración indica la cantidad de tiempo (en segundos) que el servidor perimetral retendrá el contenido almacenado en memoria caché para una vía de acceso de directorio o archivo determinada. Cuando se crea una CDN por primera vez, se crea un Tiempo de duración (TTL) global para la vía de acceso `/\*` con un tiempo predeterminado de 3600 segundos. El valor mínimo para TTL es 0 segundos, y el valor máximo es 2147483647 segundos. Para cada entrada, la vía de acceso de TTL debe ser exclusiva para la CDN. Si varias vías de acceso coinciden con un contenido determinado, la coincidencia de vía de acceso configurada más recientemente será la que se aplique a dicho contenido. Por ejemplo, considere dos TTL, `/example/file` creada primero con un valor de tiempo de duración de 3000 segundos y `/example/*` creada posteriormente, con un valor de 4000 segundos. Aunque `/example/file` es más específica, `/example/*` se ha creado más recientemente, por lo que el TTL para `/example/file` será de 4000 segundos. Una vez creadas, las entradas de TTL se pueden editar para la vía de acceso y/o el tiempo. También se pueden suprimir.

## Métricas con vistas gráficas
{: #metrics-with-graphical-views}

Las métricas para una CDN individual se proporcionan en el separador Visión general del Portal de clientes para dicha correlación de CDN. Se calculan dos tipos de métricas desde el uso de CDN: las que muestran las métricas durante un periodo de tiempo como un gráfico, y las que se muestran como valores agregados.

Para métricas que muestran el cambio durante un periodo de tiempo como un gráfico, puede ver tres gráficos de líneas y un gráfico circular. Los tres gráficos de líneas son: **Ancho de bando**, **Coincidencias por correlación** y **Coincidencias por tipo**. Muestran la actividad diaria durante un marco de tiempo especificado. Los gráficos para **Ancho de banda** y **Coincidencias por correlación** son gráficos de una sola línea, mientras que el desglose de **Coincidencias por tipo** muestra una línea para cada uno de los tipos de coincidencia proporcionados. El gráfico circular muestra un desglose regional de un ancho de banda para una correlación de CDN en forma de porcentaje.

Las métricas mostradas para los valores agregados incluyen **Uso de ancho de banda** en GB, **Total de coincidencias** en el servidor perimetral de CDN y la **Proporción de coincidencias**. La proporción de coincidencias indica el porcentaje de veces que el contenido es entregado por el servidor perimetral, _no_ a través de su origen. La proporción de coincidencias actualmente se muestra como una función de todas sus correlaciones de CDN, no solo la que se visualiza.

De forma predeterminada, los números agregados y los gráficos muestran las métricas de los últimos 30 días, pero puede cambiar este valor a través del [Portal de clientes ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://control.softlayer.com/). Ambas categorías son capaces de mostrar las métricas para periodos de 7, 15, 30, 60 o 90 días.

## Soporte a la cabecera de host
{: #host-header-support}

El servidor perimetral utiliza la **cabecera de host** cuando se comunica con el host de origen. Esta característica proporciona flexibilidad para configurar el servicio web en el host de origen. En concreto, habilita un caso de uso en el que un cliente tiene varios servidores web configurados en el mismo host de origen. Si no se proporciona la entrada de cabecera de host, el servicio utiliza el nombre de host del servidor de origen como cabecera de host HTTP predeterminada si el servidor de origen se especifica como nombre de host (en lugar de una dirección IP). Si no se proporciona la cabecera de host como entrada y se proporciona el servidor de origen como una dirección IP, se utiliza el nombre de host de CDN (también denominado Nombre de dominio de CDN) como la cabecera de host HTTP predeterminada.

## Soporte de protocolo HTTPS
{: #https-protocol-support}

La CDN se puede configurar para que utilice el protocolo HTTPS para servir el contenido de forma segura a los usuarios finales. Esta configuración requiere que se configure un certificado SSL como parte de la configuración de la CDN. Hay dos tipos de opciones de certificado SSL disponibles para HTTPS: [Certificado de comodín](/docs/infrastructure/CDN?topic=CDN-about-https#wildcard-certificate-support) y [Certificado de nombre alternativo (SAN) validado](/docs/infrastructure/CDN?topic=CDN-about-https#san-certificate-suport). Este tipo también se denominará _certificado SAN_ en esta documentación.

El tipo de certificado SSL que se debe utilizar es una consideración importante para la CDN de HTTPS. La configuración por certificado comodín es rápida, pero tiene la desventaja de que sólo se puede acceder a la CDN por medio de un CNAME. El proceso de certificado SAN tarda de 4 a 8 horas en completarse, pero proporciona la capacidad de utilizar la CDN con su dominio de CDN (es decir, el nombre de host). El certificado SAN también requiere un paso adicional de [**Validación de control de dominio**](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san) durante la configuración. No hay ningún coste asociado a utilizar ninguno de estos certificados. Consulte el [Documento de resolución de problemas](/docs/infrastructure/CDN?topic=CDN-troubleshooting#what-is-the-expected-behavior-when-loading-the-cname-or-hostname-on-your-browser-for-the-supported-protocols-) para entender las implicaciones de la selección de un determinado tipo de certificado.

El host de origen también debe tener su propio certificado SSL para el nombre de host de la CDN, y debe estar firmado por una autoridad certificadora (CA) reconocida.

Como mejor práctica del sector, Akamai solo confía en los certificados raíz y no en los certificados intermedios, porque el conjunto de certificados intermediarios de confianza cambia con frecuencia. Encontrará los certificados de confianza de Akamai [en esta ubicación web](https://community.akamai.com/docs/DOC-4447-ssltls-certificate-chains-for-akamai-managed-certificates).

## Respetar cabeceras
{: #respect-headers}

La opción **Respetar cabeceras** permite que la configuración de la cabecera HTTP en el origen sustituya a la configuración de CDN. Esta característica está activada de forma predeterminada, pero puede desactivarse.

## Proporcionar contenido obsoleto
{: #serve-stale-content}

Cuando el servidor perimetral de CDN recibe una solicitud de usuario, y el contenido de la solicitud no está almacenado en caché, el servidor perimetral contacta con el host de origen para recoger el contenido. En ese momento, dicho contenido se almacena en caché para el valor de Tiempo de duración (TTL) especificado para el contenido. Si se recibe una solicitud de usuario después de que caduque el TTL, el servidor perimetral contacta con el host de origen para recoger el contenido. Si no se puede contactar con el servidor de origen por algún motivo (por ejemplo, el host de origen está inactivo o existe un problema de red), el servidor perimetral sirve el contenido caducado (obsoleto) a la solicitud. Esta característica está admitida por Akamai y **no se puede** desactivar.

## Optimización de clave de caché
{: #cache-key-optimization}

Los servidores perimetrales de Akamai almacenan en caché el contenido en lo que denominan **Almacén de caché**. Para utilizar el contenido del **almacén de caché**, los servidores perimetrales utilizan una **clave de caché**. Normalmente, la **clave de caché** se genera a partir de una parte del URL de un usuario final. En algunos casos, el URL contiene argumentos de función de consulta que son distintos para usuarios individuales, pero el contenido proporcionado es el mismo. De forma predeterminada, Akamai utiliza los argumentos de la función de consulta para generar la clave de caché y, por tanto, para generar una clave de caché exclusiva para cada usuario. Este método no es óptimo, porque fuerza al servidor perimetral a contactar con el servidor de origen para contenido que ya está almacenado en caché, pero utilizando una clave de caché distinta. La característica de **Optimización de clave de caché** le permite especificar qué argumentos de consulta se van a incluir o a ignorar cuando se genere una clave de caché. Esta característica se aplica a cualquier acción de `crear` o `actualizar` de una configuración de correlación de CDN, así como para cualquier acción de `crear` o `actualizar` de una vía de acceso de origen.

El valor de **Optimización de clave de caché** se puede configurar desde el separador **Valores** después de la creación de una correlación de CDN. Para la vía de acceso de origen, se pueden configurar durante las operaciones `create` o `update` de una vía de acceso de origen.
{: note}

## Compresión de contenido
{: #content-compression}

La **compresión de contenidos** está habilitada de forma predeterminada en las CDN de Akamai para los siguientes tipos de contenido:
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

## Optimización de archivos de gran tamaño
{: #large-file-optimization}

La optimización de archivos de gran tamaño permite a la red CDN optimizar la entrega de contenido de tamaño superior a 10 MB. Esta habilitación incrementa el rendimiento para archivos de gran tamaño y reduce la latencia y los tiempos de descarga. Sin esta característica, la CDN no puede servir archivos de más de 1,8 GB. Esta característica permite descargar archivos de más de 1,8 GB, hasta un máximo de 320 GB.

Para que funcione la optimización de archivos de gran tamaño, las solicitudes de rango de bytes **deben** estar habilitadas en el servidor de origen. La CDN de Akamai utiliza una técnica denominada _Partial Object Caching_ para esta optimización. Cuando se solicita un archivo de gran tamaño, el servidor perimetral comprueba si el archivo cumple los requisitos de tamaño. Esto significa que los archivos de más de 10 MB se solicitarán desde el servidor de origen en bloques. Cuando el bloque llega al servidor perimetral, se almacena en caché y se sirve inmediatamente al usuario. El servidor perimetral recoge el siguiente bloque en paralelo, reduciendo así la latencia. Este proceso continúa hasta que se recupera todo el archivo, o finaliza la conexión.

Cuando esta característica está habilitada, existe un pequeño coste de rendimiento asociado al servicio de contenido inferior a 10 MB. Por tanto, esta característica se recomienda únicamente para el servicio de archivos de gran tamaño. Un caso de uso típico sería crear una nueva vía de acceso de origen en la configuración de la CDN y habilitar la optimización de archivos de gran tamaño para dicha vía de acceso.

## Vídeo on Demand
{: #video-on-demand}

La optimización del rendimiento de **Vídeo on Demand** ofrece streaming de alta calidad en distintos tipos de red. Mediante el aprovechamiento de los valores de control preconfigurados de caché y la capacidad de la red distribuida para distribuir la carga dinámicamente, la CDN de IBM Cloud con Akamai le proporciona la capacidad de escalar rápidamente para públicos de gran tamaño, tanto si lo había planificado como si no.

La opción **Vídeo on Demand** está optimizada para la distribución de formatos de streaming segmentados, como HLS, DASH, HDS y HSS. El streaming de vídeo en directo **no** está admitido por ahora. Puede habilitar la característica **Vídeo on Demand** seleccionando la opción desde el menú desplegable bajo **Optimizar para** en el separador Valores, o al crear una nueva vía de acceso de origen. Debe habilitar esta característica solo cuando optimice la entrega de archivos de vídeo.

## Control de acceso geográfico
{: #geographical-access-control}

El control de acceso geográfico es un comportamiento basado en reglas que permite establecer el parámetro `access-type` para un grupo de usuarios, en función de su ubicación geográfica. Hay dos tipos de comportamientos disponibles: **Allow** y **Deny**.

El tipo de acceso `Allow` permite de forma específica el tráfico a regiones seleccionadas, en función del tipo de la región. Permitir el tráfico a regiones específicas bloquea implícitamente el tráfico de todas las otras. Por ejemplo, puede elegir permitir (`Allow`) el tráfico a determinados continentes, como Europa y Oceanía, lo que bloqueará el acceso a todos los demás continentes.

Por otro lado, el comportamiento de denegar (`Deny`) bloquea el acceso al servicio al grupo especificado, pero permite el acceso a todas las demás regiones no especificadas. Por ejemplo, si establece el tipo de acceso de control de acceso geográfico en denegar (`Deny`) a los continentes de Europa y Oceanía, los usuarios de dichos continentes **no** podrán utilizar el servicio, mientras que los usuarios de todos los demás continentes tendrán acceso al mismo.

Esta característica es accesible desde la página de **Valores** de su configuración de CDN.

## Hotlink Protection
{: #hotlink-protection}

Hotlink Protection es un comportamiento basado en reglas que le permite controlar si determinados sitios web tienen permiso para acceder a su contenido desde su CDN. El navegador suele incluir una cabecera `Referer` cuando se realiza una solicitud HTTP desde un enlace de una página web, y cuando dicho enlace apunta a un activo remoto. El enlace que un sitio web utiliza para acceder a un activo de otro sitio web se denomina un _hotlink_.  Hay dos tipos de comportamientos disponibles: **ALLOW** y **DENY**.

Si `protectionType` tiene el valor `ALLOW`:
* Si el valor de la cabecera `Referer` de una solicitud enviada a su CDN coincide con uno de los `refererValues` especificados, la CDN **servirá** el contenido solicitado.
* De lo contrario, su CDN **no** servirá el contenido.

Si `protectionType` tiene el valor `DENY`:
* Si el valor de la cabecera `Referer` de una solicitud enviada a su CDN coincide con uno de los `refererValues` especificados, la CDN **no servirá** el contenido solicitado.
* De lo contrario, su CDN servirá el contenido.
