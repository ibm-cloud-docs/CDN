---

copyright:
  years: 2018
lastupdated: "2018-06-19"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Preguntas más frecuentes sobre HTTPS

## ¿Cuál es la diferencia entre HTTPS con Certificado comodín y HTTPS con Certificado SAN?

Con el Certificado comodín, todos los clientes utilizan el mismo certificado desplegado en las redes CDN del proveedor. Para acceder al servicio debe utilizarse el CNAME, incluido el sufijo de IBM `.cdnedge.bluemix.net`. Por ejemplo, `https://www.example-cname.cdnedge.bluemix.net`

En el caso del certificado SAN, varios dominios de cliente comparten un único certificado SAN añadiendo sus nombres de dominio a las entradas SAN. En ese caso puede accederse al servicio mediante el nombre de host, por ejemplo `https://www.example.com`

## ¿Cómo se realiza la Validación de dominios con redirección?

Depende del servidor. El procedimiento para completar la validación de dominios para los servidores Apache y Nginx se puede encontrar en la página [Completar la validación de control de dominio para HTTPS](how-to-https.html#redirect-).

## ¿Cuánto tiempo lleva la validación de dominio?

La validación de dominio normalmente tarda entre 2 y 4 horas, pero varía en función del método elegido para la validación. La validación de DV con CNAME es la más rápida, normalmente tarda menos de una hora. La validación utilizando los métodos Estándar y de Redirección tarda típicamente unas 4 horas, después de que se ha direccionado el desafío.

## ¿Cuánto tiempo tarda el proceso de creación con el certificado SAN DV?

Una solicitud normal para habilitar HTTPS tarda un promedio de 3 a 9 horas, desde la solicitud inicial a la ejecución.

## ¿Cuánto tiempo se tarda en suprimir una CDN con el certificado SAN DV?

La supresión de la CDN requiere que se elimine el dominio del certificado en todos los servidores perimetrales. Este proceso puede tardar hasta 8 horas en completarse.

## ¿Hay algún coste adicional asociado con el uso del certificado SAN DV?

No. Las configuraciones de certificado SAN DV se proporcionan sin coste adicional en comparación con HTTP o con HTTPS con Certificado comodín.

## ¿Se puede actualizar la CDN creada con HTTPS de certificado comodín para que utilice el certificado SAN DV?

No, actualmente no se puede cambiar una correlación con certificado comodín a certificado SAN.

## ¿Qué es una entidad emisora de certificados?

Una entidad emisora de certificados (CA) es una entidad que emite Certificados digitales.

## ¿Qué CA utiliza el servicio CDN de IBM Cloud para emitir un certificado SAN DV?

El servicio CDN de IBM Cloud utiliza la entidad emisora de certificados LetsEncrypt.

## ¿Qué certificados SSL están soportados?

Los certificados SSL soportados son certificados comodín y certificados de validación de dominio (DV) de nombre alternativo (SAN). El certificado SAN se comparte entre varios clientes. El CDN de IBM Cloud no admite la carga de certificados personalizados.

## He recibido un correo electrónico solicitándome que me dirija a un desafío de Validación de dominios. ¿Qué hago ahora?

La validación de dominio se puede resolver de una de estas tres maneras: CNAME, Estándar o Directa.

Para obtener detalles sobre cómo abordar cualquiera de ellos, consulte [Completar la validación de control de dominio para HTTPS](how-to-https.html#how-to-https.html#initial-steps-to-domain-control-validation).

## ¿Qué ocurrirá si no hago frente al reto para la validación de dominios?

Si el estado de la correlación está en estado DOMAIN_VALIDATION_PENDING durante más de 48 horas, la creación de la correlación se cancelará y el estado de la correlación será CREATE_ERROR. Y en este estado, puede optar por Reintentar la creación o suprimir la correlación.

## ¿El certificado de comodín necesita validar el dominio?

No, pero solo puede utilizar el CNAME para recuperar contenido de su origen. `https://www.example-cname.cdnedge.bluemix.net`

## Si utilizo el tipo de certificado SAN, ¿puedo seguir utilizando el CNAME para acceder a mi servicio?

No. Para el certificado SAN, solo puede utilizar el dominio personalizado para acceder al contenido desde el origen. `https://www.example.com`

## ¿Se han añadido todos mis dominios en un mismo certificado?

No necesariamente. Akamai maneja la selección de certificados para garantizar que los certificados se encuentran en el estado más eficiente. Se añaden dominios a distintos certificados de manera proporcionada, de modo que no se puede garantizar que todos los dominios estén en el mismo certificado.

## ¿Por qué veo "comodín" cuando realizo un `dig` o cuando intento acceder al contenido cuando mi CDN está en el estado `Solicitando certificado `, `Validación de dominio pendiente` o ` Despliegue de certificado`?

Durante el proceso de solicitud de certificado SAN DV, la cadena de registros de DNS para la CDN está encadenada a un certificado comodín, temporalmente. Hasta que se haya completado el proceso, el contenido se sirve temporalmente a través de este certificado de comodín. Una vez que el proceso de solicitud se ha completado, la cadena de registros de DNS se actualiza para encadenarse a su certificado SAN DV de CDN.
