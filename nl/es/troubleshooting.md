---

copyright:
  years: 2018, 2019
lastupdated: "2019-04-04"

keywords: troubleshooting, support, reference, number, error, 503, 301, redirects, https, moved, akamai-x-cache, cloud object storage

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Resolución de problemas
{: #troubleshooting}

En este documento, encontrará varias formas de resolver problemas en su {{site.data.keyword.cloud}} CDN. Si necesita ponerse en contacto con el soporte, asegúrese de proporcionar el número de referencia de la CDN.

## ¿Cómo se si mi CDN está funcionando?
{: #how-do-I-know-my-cdn-is-working}

Ejecute el siguiente mandato `curl` sustituyendo `http://your.cdn.domain/uri` por la vía de acceso de archivos correspondiente a su CDN:

`curl -I -H "Pragma: akamai-x-cache-on, akamai-x-cache-remote-on, akamai-x-check-cacheable, akamai-x-get-cache-key, akamai-x-get-extracted-values, akamai-x-get-nonces, akamai-x-get-ssl-client-session-id, akamai-x-get-true-cache-key, akamai-x-serial-no" http://your.cdn.domain/uri`

Si la salida del mandato `curl` es similar al formato del siguiente ejemplo, la CDN está funcionando correctamente:

```
    HTTP/1.1 200 OK

    Server: nginx/1.13.0

   ...

    X-Cache: TCP_HIT from a199-117-103-28.deploy.akamaitechnologies.com (AkamaiGHost/9.0.2.1-20488781) (-)

    X-Cache-Key: /L/1363/535014/1d/your.cdn.domain/uri

    X-True-Cache-Key: /L/your.cdn.domain/uri

    ...

    ...

    X-Akamai-Session-Info: name=WSD_BEST_PRACTICES_TD_TYPE; value=PATTERNS_BASED_PARENT_MAP

    X-Serial: 1363

    Connection: keep-alive

    X-Check-Cacheable: YES
```
{: screen}

## He recibido un error 503. ¿Por qué?
{: #i-received-a-503-error-why}

La causa más habitual por la que recibirá un error 503 será debido a un problema con un certificado en la cadena de certificados de SSL.

Este es el error que podría ver: `503 - Servicio no disponible`.  

Conjuntamente con el error 503, también puede ver un mensaje similar al siguiente: `An error occurred while processing your request. Reference #30.3598c0ba.1521745157.87201fff` (el número real de la referencia puede ser otro). En este caso, el número de referencia de la serie de error corresponde a un error de reconocimiento SSL.

Para corregir el problema, asegúrese de que el certificado o los certificados SSL del servidor de origen cumplen los siguientes criterios:
  * El certificado lo **debe** emitir una entidad emisora de certificados (CA) en la que Akamai confíe. Puede ver la lista de certificados de confianza de Akamai en [este enlace ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")]](https://community.akamai.com/docs/DOC-4447-ssltls-certificate-chains-for-akamai-managed-certificates)
  * **Debe** coincidir con la *Cabecera de host* configurada en la CDN
  * **No** debe ser autofirmado
  * **No** debe haber caducado

Si ha verificado que la cadena de certificados de origen utilizando el criterio anterior y sigue encontrando el mismo error, consulte la página [Obtención de soporte y ayuda](/docs/infrastructure/CDN?topic=CDN-gettinghelp). Anote la serie del error de Referencia e inclúyalo al comunicarse con nosotros.

## Mi nombre de host no se carga en el navegador cuando IBM Cloud Object Storage (COS) es el origen.
{: #my-hostname-doesnt-load-on-the-browser-when-ibm-cloud-object-storage-cos-is-the-origin}

Cuando su CDN de {{site.data.keyword.cloud_notm}} está configurada para utilizar COS como el almacenamiento de objetos, el acceso directo al sitio web no funciona. Debe especificar la vía de acceso completa de la solicitud en la barra de direcciones del navegador (por ejemplo, `www.example.com/index.html`). Este comportamiento se debe a una limitación en el documento de índice en IBM COS.

## No me puedo conectar con HTTPS a través del mandato `curl` o por el navegador utilizando el nombre de host.
{: #i-cant-conect-through-a-curl-command-or-browser-using-the-hostname-with-https}

Si su CDN se creó con HTTPS con un certificado comodín, la conexión se debe realizar utilizando el CNAME, por ejemplo, `https://www.exampleCname.cdnedge.bluemix.net`. Esto incluye **todas** las CDN creadas con HTTPS con anterioridad al 18 de junio del 2018. Se producirá un error si intenta conectarse utilizando el nombre de host.

## ¿Cuál es el comportamiento esperado al cargar el CNAME o el nombre de host en el navegador para los protocolos soportados?
{: #what-is-the-expected-behavior-when-loading-the-cname-or-hostname-on-your-browser-for-the-supported-protocols}

En esta tabla se muestra el comportamiento esperado para los protocolos soportados al cargar el **nombre de host** o el **CNAME** desde el navegador web.

<table>
<caption caption-side=“top”>Tabla de comportamientos esperados</caption>
<thead>
<tr>
<th rowspan=2 scope="col">URL de navegador</th>
<th rowspan=2 scope="col">CDN con solo el protocolo HTTP</th>
<th colspan=2 scope="col">CDN con solo el protocolo HTTPS</th>
<th colspan=2 scope="col">CDN con HTTP y HTTPS</th>
</tr>
<tr>
<th scope="col"> Comodín </th>
<th scope="col"> SAN compartido </th>
<th scope="col"> Comodín </th>
<th scope="col"> SAN compartido </th>
</tr>
</thead>
<tbody>
<tr>
<td> `http://hostname` </td>
<td> Carga satisfactoria </td>
<td> 301 - Movido permanentemente </td>
<td> Carga satisfactoria </td>
<td> 301 - Movido permanentemente </td>
<td> Carga satisfactoria </td>
</tr>
<tr>
<td> `https://hostname`</td>
<td> Acceso denegado </td>
<td> Redirecciona a la página web de IBM Cloud </td>
<td> Carga satisfactoria </td>
<td> Redirecciona a la página web de IBM Cloud </td>
<td> Carga satisfactoria </td>
</tr>
<tr>
		<td> `http://cname` </td>
		<td> 301 - Movido permanentemente </td>
		<td> Carga satisfactoria </td>
		<td> 301 - Movido permanentemente </td>
		<td> Carga satisfactoria </td>
		<td> 301 - Movido permanentemente </td>
</tr>
<tr>
		<td> `https://cname` </td>
		<td> Redirecciona a la página web de IBM Cloud </td>
		<td> Carga satisfactoria </td>
		<td> 301 - Movido permanentemente </td>
		<td> Carga satisfactoria </td>
		<td> Redirecciona a la página web de IBM Cloud </td>
</tr>
</tbody>
</table>

**Mensajes de error comunes:**

Un mensaje `301 - Movido permanentemente` indica que muy probablemente está intentando acceder a una CDN con un protocolo `HTTPS` o `HTTP_AND_HTTPS` utilizando el nombre de host. Debido a una limitación con el certificado comodín HTTPS, **debe** utilizar el CNAME para acceder a su CDN.

Con **solo** el protocolo HTTP, recibirá un mensaje `301 - Movido permanentemente` si intenta acceder a su CDN utilizando el CNAME. En este caso, _solo_ podrá acceder a su CDN utilizando el nombre de host.

El mensaje `Acceso denegado` aparece cuando se intenta acceder a una CDN utilizando un protocolo incorrecto. Asegúrese de que utiliza `http` para las CDN creadas con el protocolo HTTP o `https` para las CDN creadas con el protocolo HTTPS.

**Un posible error de redirección:**

El comportamiento de una redirección de URL a una página web de la CDN de {{site.data.keyword.cloud_notm}} se ve con mayor frecuencia cuando un URL es incorrecto para el protocolo. Si su CDN se ha creado con un protocolo HTTPS o HTTPS_AND_HTTPS, debe utilizar el CNAME para acceder a su CDN. Por ejemplo: `https://examplecname.cdnedge.bluemix.net` para correlaciones HTTPS o `http://examplecname.cdnedge.bluemix.net` o `https://examplecname.cdnedge.bluemix.net` para correlaciones HTTP_AND_HTTPS.

El URL se redirecciona a la página web de la CDN de {{site.data.keyword.cloud_notm}} en este caso porque tanto el protocolo como el dominio son incorrectos para el protocolo de la CDN. Para una CDN creada con HTTP como el _único_ protocolo, _sólo_ se puede acceder por medio del nombre de host. Por ejemplo, `http://example.com`.
