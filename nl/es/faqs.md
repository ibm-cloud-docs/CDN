---

copyright:
  years: 2017
lastupdated: "2017-09-06"

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

La red de entrega de contenido (CDN) es un conjunto de servidores perimetrales que se distribuyen por diversas regiones del país o del mundo. Su contenido web se proporciona desde un servidor perimetral, que se encuentra en el área geográfica más cercana al cliente que solicita el contenido. Con esta técnica, los usuarios reciben el contenido con menos retraso del que cabría esperar cuando se entrega el contenido desde una ubicación centralizada. Ofrece una mejor experiencia global a sus clientes.

## ¿Cómo funciona la CDN?

La CDN logra su objetivo almacenando contenido en la memoria caché de servidores perimetrales en todo el mundo. Cuando un usuario solicita contenido web, la solicitud se direcciona al servidor perimetral más cercano al usuario desde el punto de vista geográfico. Al reducir la distancia que debe desplazarse el contenido, la CDN ofrece rendimiento optimizado, latencia minimizada y aumento de rendimiento. 

## ¿Cómo se crea una cuenta de CDN?

La cuenta se crea durante el proceso de pedido de CDN, cuando pulsa **Seleccionar** después de pasar por el menú "Selección de proveedor".

## ¿Qué hago cuando el estado de la CDN es CNAME_CONFIGURATION?

Actualice el registro de DNS para que el sitio web apunte al `CNAME` asociado con la nueva correlación de CDN. **Nota**: puede tardar entre 15 -30 minutos hasta que se active la actualización. Consulte a su proveedor de DNS el momento estimado.

## ¿Qué se facturará?

La factura se realizará por el ancho de banda usado por cada CDN. Si la CDN no utiliza ancho de banda, no incurrirá en gastos. Los precios del ancho de banda varían en función de la ubicación regional del servidor perimetral. 

## ¿Cuándo se facturará?

La facturación de CDN se produce según el periodo de facturación establecido en su cuenta de {{site.data.keyword.BluSoftlayer_notm}}.

## ¿Cómo visualizo las métricas y el uso?

Puede ver las métricas y el uso en la página **Visión general**, a la que puede navegar seleccionando la CDN en la página **Redes de entrega de contenido**. **Nota**: después de crear la CDN, puede tardar 48 horas hasta que aparezcan las métricas.

## ¿Existe un número mínimo de días para los que puedo ver las métricas? ¿Hay un máximo?

Sí. Se pueden recopilar métricas para un periodo mínimo de 7 días. Se pueden visualizar métricas para un periodo máximo de 90 días. A los que utilizan la API, se les recomienda usar 89 días como máximo para que se tengan en cuenta las diferencias de huso horario. 

## ¿Cómo funciona el certificado comodín HTTPS?

El certificado comodín es la forma más asequible de entregar contenido web a los usuarios de forma segura. Para utilizar el certificado comodín, el cliente debe usar el nombre de host CNAME como punto de entrada al servicio (por ejemplo, _https://ejemplo.cdnedge.bluemix.net_). Después de habilitar la correlación de CDN para HTTPS mediante el certificado comodín, el servidor perimetral contactará con el servidor de origen a través de HTTPS. Si el servidor de origen se especifica como un nombre de host, el servidor perimetral utiliza el dominio de origen como cabecera de Server Name Indication (SNI) en la negociación de TLS (seguridad de la capa de transporte) con el servidor de origen de forma predeterminada. Sin embargo, si el dominio de origen se especifica como dirección IP, debe proporcionarse la cabecera de host de origen. Otra sugerencia es que una entidad emisora de certificados (CA) reconocida firme el certificado de origen.

## Si selecciono la opción `Suprimir` en el menú de desbordamiento de CDN, ¿se suprime mi cuenta?

No, solo se suprimirá esa CDN. Su cuenta seguirá existiendo y podrá crear CDN adicionales. 

## ¿El almacenamiento en memoria caché utiliza la transferencia o la extracción?

El almacenamiento de contenido en memoria caché se realiza utilizando el modelo de _extracción de origen_. La extracción de origen es un método mediante el cual el servidor perimetral "extrae" datos del servidor de origen, en contraposición a la carga manual de contenido al servidor perimetral.  

## ¿Hay un valor máximo de Tiempo de duración? ¿Y un valor mínimo?

El valor máximo de Tiempo de duración es 21.474.836.471 segundos, que equivale a aproximadamente 681 años. El valor
mínimo es de 30 segundos.

## ¿Qué es la opción Proporcionar contenido obsoleto?

Si el tiempo de almacenamiento en memoria caché caduca para un contenido, el servidor perimetral intentará captar el contenido del servidor de origen. Si, por algún motivo, el servidor de origen está inactivo o no se puede contactar con él, el servidor perimetral puede suministrar el contenido obsoleto si se ha definido dicha opción, que es la preferencia de la mayoría de los clientes. En este momento, nuestras CDN solo se pueden configurar con esta opción definida como **Sí** de forma predeterminada. Cambiar la opción a **No** no tendrá ninguna repercusión: la opción **Proporcionar contenido obsoleto** seguirá establecida como **Sí**.

## ¿Existe un límite en el número de entradas de TTL y origen?

Sí, el límite combinado es 75 entradas por CDN.

## ¿Existe un límite en el número de CDN que puedo tener?

Sí, el límite es 10 CDN por cuenta.

## ¿Se recomienda algún navegador para utilizar la configuración del servicio de la CDN?

Sí, se recomiendan las últimas versiones de Firefox y Chrome. 

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

## ¿Dónde encuentro mi CName si no he proporcionado ninguno? 
Pulse en la CDN para ir a la página **Visión general**. En la esquina derecha, puede ver una sección **Detalles** con la información de `CName`.

## ¿Existe un límite en el número de solicitudes de depuración que se pueden activar a la vez?
Sí. Solo se pueden activar 5 solicitudes de depuración a la vez. 

## Mi solicitud de depuración para una vía de acceso de archivo determinada está en curso. ¿Puedo enviar una solicitud nueva para la misma vía de acceso de archivo? 
No. Solo puede haber una solicitud de depuración activa a la vez para una vía de acceso de archivo.
