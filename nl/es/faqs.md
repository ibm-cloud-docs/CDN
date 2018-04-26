---

copyright:
  years: 2017, 2018
lastupdated: "2018-03-29"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Preguntas más frecuentes

## ¿Qué es una red de entrega de contenido (CDN)?

Una red de entrega de contenido (CDN) es un conjunto de servidores perimetrales que se distribuyen por diversas regiones del país o del mundo. Su contenido web se proporciona desde un servidor perimetral, que se encuentra en el área geográfica más cercana al cliente que solicita el contenido. Con esta técnica, los usuarios reciben el contenido con menos retraso del que cabría esperar cuando se entrega el contenido desde una ubicación centralizada. Ofrece una mejor experiencia global a sus clientes.

## ¿Cómo funciona una red de entrega de contenido (CDN)?

Una CDN logra su objetivo almacenando contenido en la memoria caché de servidores perimetrales en todo el mundo. Cuando un usuario solicita contenido web, la solicitud se direcciona al servidor perimetral más cercano al usuario desde el punto de vista geográfico. Al reducir la distancia que debe desplazarse el contenido, la CDN ofrece rendimiento optimizado, latencia minimizada y aumento de rendimiento. 

## ¿Cómo se crea mi cuenta de servicio de IBM Cloud Content Delivery Network?

La cuenta se crea durante el proceso de pedido de CDN, cuando pulsa **Seleccionar** después de pasar por el menú "Selección de proveedor".

## ¿Qué hago cuando el estado de la CDN es CNAME_CONFIGURATION?

Para la CDN basada en HTTP, actualice el registro de DNS para que el sitio web apunte al `CNAME` asociado con la nueva correlación de CDN. Para la CDN basada en HTTPS, esta actualización de DNS **NO** es necesaria.

**Nota**: puede tardar entre 15 -30 minutos hasta que se active la actualización. Consulte a su proveedor de DNS el momento estimado.

## ¿Qué se facturará?

Solo se le facturará por el ancho de banda utilizado por instancia de IBM Cloud Content Delivery Network. Si la CDN no utiliza ancho de banda, no incurrirá en gastos. Los precios del ancho de banda varían en función de la ubicación regional del servidor perimetral. Puede consultar los precios del ancho de banda por región geográfica en la sección [Cómo empezar](https://github.com/IBM-Bluemix-Docs/CDN/blob/master/getting-started.md#pricing-shown-in-usd) para este servicio.

## ¿Cuándo se facturará?

La facturación de IBM Cloud Content Delivery Network se produce según el periodo de facturación establecido en su cuenta de {{site.data.keyword.BluSoftlayer_notm}}.

## ¿Cómo visualizo las métricas y el uso?

Puede ver las métricas y el uso en la página **Visión general**, a la que puede navegar seleccionando la CDN en la página **Redes de entrega de contenido**. **NOTA**: después de crear la CDN, puede tardar 48 horas hasta que aparezcan las métricas.

## He creado una CDN y había tráfico de datos a través de la CDN. ¿Por qué no aparecen mis métricas?

Después de crear una CDN, las métricas tardan 48 horas en aparecer.

## ¿Existe un número mínimo de días para los que puedo ver las métricas? ¿Hay un máximo?

Existe un número mínimo y máximo de días durante el cual puede visualizar las métricas utilizando IBM Cloud Content Delivery Network con Akamai. Se pueden recopilar métricas para un periodo mínimo de 7 días. Se pueden visualizar métricas para un periodo máximo de 90 días. A los que utilizan la API, se les recomienda usar 89 días como máximo para que se tengan en cuenta las diferencias de huso horario.

## ¿Por qué la proporción de coincidencias es distinta a cero cuando el total de coincidencias es cero?
La proporción de coincidencias representa el porcentaje de veces que el contenido se ha suministrado desde la memoria caché del servidor Edge, en lugar de desde el servidor de origen. Se calcula de la siguiente forma:

```
((Edge hits - Ingress hits)/Edge hits) * 100

donde:
Coincidencias Edge se definen como "All hits to the edge servers from the end-users"
Coincidencias Ingress se definen como "Origin or Ingress hits are for traffic from your origin to Akamai edge servers"
```
 
Como la proporción de coincidencias se calcula a nivel de cuenta y no por CDN, la proporción de coincidencias será la misma para todas las CDN de su cuenta. Esto también explica por qué la proporción de coincidencias pueda ser distinta a cero cuando el número de coincidencias Edge para una CDN determinada sea cero.

## Si selecciono la opción `Suprimir` en el menú de desbordamiento de CDN, ¿se suprime mi cuenta?

No, solo se suprimirá esa CDN. Su cuenta seguirá existiendo y podrá crear CDN adicionales.

## ¿El almacenamiento en memoria caché utiliza la transferencia o la extracción?

El almacenamiento de contenido en memoria caché se realiza utilizando el modelo de _extracción de origen_. La extracción de origen es un método mediante el cual el servidor perimetral "extrae" datos del servidor de origen, en contraposición a la carga manual de contenido al servidor perimetral. 

## ¿Hay un valor máximo de Tiempo de duración? ¿Y un valor mínimo?

El valor máximo de Tiempo de duración es 2.147.483.647 segundos, que equivale a aproximadamente 68 años. El valor
mínimo es de 30 segundos.

## ¿Existe un límite en el número de entradas de TTL y origen?

Sí, el límite combinado es 75 entradas por CDN.

## ¿Existe un límite en el número de CDN que puedo tener?

Sí, el límite es 10 CDN por cuenta.

## ¿Se recomienda algún navegador para utilizar la configuración del servicio de la CDN?

Sí, los navegadores recomendados son Firefox y Chrome. Se recomienda utilizar las últimas versiones con IBM Cloud Content Delivery Network.

## ¿Cuál es el objetivo de proporcionar una vía de acceso al crear la CDN?

Proporcionar una vía de acceso durante la creación de una CDN permite aislar los archivos que se pueden suministrar a través de la CDN desde un servidor de origen determinado.

## ¿Cómo sé que la CDN está funcionando?
Ejecute el siguiente mandato "curl" sustituyendo "origin.cdntesting.net/assets/ibm_3d.gif" por la vía de acceso de archivo correspondiente a su origen: 
```
curl -I -H "Pragma: akamai-x-cache-on, akamai-x-cache-remote-on, akamai-x-check-cacheable, akamai-x-get-cache-key, akamai-x-get-extracted-values, akamai-x-get-nonces, akamai-x-get-ssl-client-session-id, akamai-x-get-true-cache-key, akamai-x-serial-no" origin.cdntesting.net/assets/ibm_3d.gif
```
Si la salida del mandato coincide con el formato siguiente significa que la CDN funciona como está previsto: 
```
HTTP/1.1 200 OK

Server: nginx/1.13.0

...

X-Cache: TCP_HIT from a199-117-103-28.deploy.akamaitechnologies.com (AkamaiGHost/9.0.2.1-20488781) (-)

X-Cache-Key: /L/1363/535014/1d/origin.cdntesting.net.ibm.com/assets/ibm_3d.gif

X-True-Cache-Key: /L/origin.cdntesting.net.ibm.com/assets/ibm_3d.gif

...

...

X-Akamai-Session-Info: name=WSD_BEST_PRACTICES_TD_TYPE; value=PATTERNS_BASED_PARENT_MAP

X-Serial: 1363

Connection: keep-alive

X-Check-Cacheable: YES
```

## El estado de la CDN es Error. ¿Qué hago ahora?
Consulte la página [Obtención de ayuda y soporte](https://console.stage1.bluemix.net/docs/infrastructure/CDN/getting-help.html#gettinghelp) o abra una incidencia en el [Portal de cliente ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://control.softlayer.com/).

## ¿Dónde encuentro mi CNAME si no he proporcionado ninguno? 
Pulse sobre su CDN para acceder a la página **Visión general** en el Portal. En la esquina derecha, puede ver una sección **Detalles** con la información de `CName`.

## ¿Existe un límite en el número de solicitudes de depuración que se pueden activar a la vez?
Sí. Solo se pueden activar 5 solicitudes de depuración a la vez.

## Mi solicitud de depuración para una vía de acceso de archivo determinada está en curso. ¿Puedo enviar una solicitud nueva para la misma vía de acceso de archivo?
No. Solo puede haber una solicitud de depuración activa a la vez para una vía de acceso de archivo.

## ¿Se admite Internet Protocol versión 6 (IPv6) con el servicio IBM Cloud Content Delivery Network? ¿Cómo funciona?
IPv6 (o soporte de pila dual) está soportada por los servidores Edge de Akamai. Está pensado para ayudar a los clientes con solo origen IPv4 a aceptar las conexiones de los clientes IPv6, convertir de IPv6 a IPv4 en Edge e ir al origen con IPv4.

**NOTA:** No se da soporte a la creación de una CDN de IBM Cloud utilizando una dirección IPv6 como dirección del servidor de origen.

## ¿Cuáles son las reglas para el nombre de host?
La serie de entrada `Hostname` **debe**:
  * constar de caracteres alfanuméricos
  * ser inferior a 254 caracteres
  * finalizar por un nombre de dominio de nivel superior válido
  * **no** debe finalizar en 'cdnedge.bluemix.net` (esta terminación se utiliza para CNAMES y está reservada)

Consulte RFC 1035, sección 2.3.4, para obtener más detalles.

## ¿Cuáles son las convenciones de nomenclatura de CNAME personalizado?
La serie de entrada `CNAME` debe cumplir las siguientes reglas:
  * **debe** ser exclusivo (no puede estar en uso por ninguna otra CDN de IBM Cloud)
  * tener menos de 64 caracteres alfanuméricos
  * los caracteres especiales `! @ # $ % ^ & *` **no** están permitidos
  * los guiones bajos `_` **no** están permitidos
  * los puntos `.` **no** están permitidos
  * los guiones `-` están permitidos, pero CNAME no puede empezar ni terminar con un guión

## ¿Cuáles son las reglas para los nombres de grupo?
Nombres de grupo:
  * deben tener como mínimo 1 carácter
  * no pueden tener más de 199 caracteres
  * pueden contener letras (se permiten mayúsculas y minúsculas), números y guiones
  * **no** deben tener formato de dirección IP (por ejemplo, 127.0.0.1)
  * **no pueden** empezar con un punto (.)
  * pueden finalizar con un punto (.)
  * Solo se permite un punto (.) entre etiquetas (por ejemplo, my..ibmcloud.bucket no está permitido). 

**NOTA**: aunque las letras mayúsculas y los puntos pueden pasar la validación, recomendamos utilizar siempre nombres de grupo compatibles con DNS.

## ¿Cuáles son las reglas para las series de la vía de acceso para el origen?
La vía de acceso es opcional al crear la CDN. Sin embargo, si se proporciona, la vía de acceso **debe**:
  * ser inferior a 1000 caracteres
  * empezar por '/'

## En el mandato **Añadir origen**, ¿hay algunas reglas a seguir para la serie de la vía de acceso?
En **Añadir origen**, la vía de acceso es obligatoria. También si la CDN se ha creado con una vía de acceso, la vía de acceso para **Añadir origen** debe empezar por la vía de acceso de la CDN como prefijo. Por ejemplo, si la vía de acceso de la CDN se ha especificado como `/storage`, para invocar **Añadir origen** con una vía de acceso llamada `/examplePath`, la vía de acceso proporcionada será `/storage/examplePath`'.

## ¿En qué formato debo proporcionar las extensiones de archivos para crear un tipo de origen de Object Storage con este servicio de CDN?

Las extensiones de archivos deben estar separadas por comas. Por ejemplo, la lista "txt, jpg, xml" es válida. Cualquier extensión determinada no puede tener una longitud de más de 10 caracteres.

## ¿Hay restricciones sobre qué números de puerto HTTP y HTTPS están permitidos en Akamai?
Sí. Para el proveedor Akamai, solo se admiten los siguientes números de puertos:
72, 80-89, 443, 488, 591, 777, 1080, 1088, 1111, 1443, 2080, 7001, 7070, 7612, 7777, 8000-9001, 9090, 9901-9908, 11080-11110, 12900-12949, 20410 y 45002.

## ¿Qué URL debe utilizarse para acceder a datos en la CDN o la vía de acceso de origen? 
La vía de acceso para una correlación de CDN o para el origen se trata como directorio. Por lo tanto, los usuarios que intentan acceder a la vía de acceso de origen deben acceder a un directorio (con una barra inclinada). Por ejemplo, si la CDN `www.example.com` se crea mediante la vía de acceso que incluye el directorio `/images`, el URL para llegar a ella debe ser `www.example.com/images/`

Si omite la barra inclinada, por ejemplo, si utiliza `www.example.com/images` se producirá un error **No se ha encontrado la página**.

## Para HTTPS, ¿por qué no puedo conectarme a través de un mandato curl o un navegador utilizando el nombre de host?

Actualmente se admite HTTPS solo mediante certificado comodín. Como resultado de esta limitación, la conexión debe realizarse mediante CNAME; si intenta conectarse mediante el nombre de host se producirá un error.

## ¿Cuál debería ser el comportamiento esperado al cargar el nombre de host o CNAME en el navegador para los protocolos de CDN soportados?

|URL del navegador| CDN solo con protocolo HTTP | CDN solo con protocolo HTTPS | CDN con protocolos HTTP y HTTPS |
|-------|-----|-----|-----|
|http://hostname| Carga correcta | 301 Movido permanentemente | 301 Movido permanentemente |
|https://hostname | Acceso denegado | Redirige a la página web de IBM Cloud | Redirige a la página web de IBM Cloud | 
|http://cname| 301 Movido permanentemente | Acceso denegado | Carga correcta | 
|https://cname| Redirige a la página web de IBM Cloud | Carga correcta | Carga correcta |

## ¿Cómo configuro mi red de entrega de contenido para IBM Cloud Object Storage (COS)?
[Aquí se incluye una guía de aprendizaje](https://console.bluemix.net/docs/tutorials/static-files-cdn.html#accelerate-delivery-of-static-files-using-a-cdn) sobre cómo crear una red de entrega de contenido para IBM Cloud Object Storage.

## ¿Por qué mi nombre de host no se carga en el navegador cuando selecciono IBM Cloud Object Storage (COS) como origen?

Cuando se configura una CDN de IBM Cloud para utilizar IBM COS como el almacenamiento de objetos, el acceso directo al sitio web no funciona. Debe especificar la vía de acceso de solicitud completa en la barra de direcciones del navegador (por ejemplo, `www.example.com/index.html`). Este comportamiento se debe a la limitación del documento de índice en IBM COS.

## ¿Cuál es el tamaño de archivo máximo que se puede entregar a través de la CDN de Akamai?

Los intentos de recuperar o entregar archivos de más de 1,8 GB recibirán una respuesta `403 Acceso prohibido` para la configuración de rendimiento predeterminada. Si se habilita la optimización de archivos de gran tamaño, se pueden descargar archivos de hasta 320 GB.

## He recibido una notificación de que mi certificado de origen caduca. ¿Qué hago ahora?

Siga los pasos indicados en [este artículo](https://community.akamai.com/docs/DOC-7708) de Akamai.

## ¿Qué es una solicitud de rango de bytes, y cómo funciona con la CDN de Akamai?

Una solicitud de rango de bytes se utiliza para recuperar contenido parcial de un servidor de origen. La cabecera de solicitud HTTP de rango indica qué parte del contenido debe devolver el servidor. Se pueden solicitar varias partes con una sola cabecera de rango a la vez, y el servidor puede devolver estos rangos en una respuesta en varias partes. Si el servidor devuelve los rangos, responde con un estado 206 (contenido parcial).

Cuando se envía una solicitud de rango de bytes mediante una CDN de IBM Cloud con Akamai, el usuario puede recibir un código de respuesta 200 (De acuerdo) para la primera solicitud, y un código de respuesta 206 para todas las solicitudes posteriores. Esto se debe a que los servidores perimetrales de Akamai solicitan el contenido del origen en formato comprimido. Por lo tanto, cuando un servidor perimetral no tiene un objeto en su memoria caché, ni tiene ninguna información sobre la longitud del contenido del objeto, irá directamente al origen y solicitará todo el objeto. En este caso, el origen proporciona el objeto sin la cabecera de longitud de contenido a Akamai, y el usuario final recibirá el objeto completo aunque sea una solicitud de rango de bytes. El código de estado es 200. En solicitudes posteriores, el servidor perimetral tiene el objeto en su memoria caché y proporcionará el código de estado 206.

Un modo de garantizar una respuesta 206, incluso para la primera solicitud de rango de bytes, es inhabilitar `Transfer-Encoding: chunked` en el servidor de origen.

## ¿Qué seguridad se incluye en la solución IBM CDN con Akamai?

Con la plataforma Akamai distribuida, obtendrá una escalabilidad y una capacidad de recuperación sin precedentes, con más de 240.000 servidores en más de 130 países. La plataforma Akamai se sitúa entre su infraestructura y sus usuarios finales, y actúa como primer nivel de defensa en caso de aumento repentino del tráfico. Akamai Intelligent Platform es también un proxy inverso que escucha y responde a las solicitudes en puertos 80 y 443, lo que significa que el tráfico en otros puertos se desvía al extremo sin reenviarse a su infraestructura.
