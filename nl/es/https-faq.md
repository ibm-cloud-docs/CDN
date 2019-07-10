---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-05"

keywords: faq, https, wildcard, certificate, san certificate, domain validation, redirect, domains, challenge

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:faq: data-hd-content-type='faq'}

# Preguntas más frecuentes sobre HTTPS
{: #faq-for-https}

## Para mi CDN, ¿cuál es la diferencia entre HTTPS con certificado comodín y HTTPS con certificado SAN?
{: #for-my-cdn-what-is-the-difference-between-https-with-wildcard-certificate-and-https-with-san-certificate}
{:faq}

Con los certificados comodín, todos los clientes utilizan el mismo certificado desplegado en las redes CDN del proveedor. Para acceder al servicio debe utilizarse el CNAME, incluido el sufijo de IBM `.cdnedge.bluemix.net`. Por ejemplo, `https://www.example-cname.cdnedge.bluemix.net`

En el caso del certificado SAN, varios dominios de cliente comparten un único certificado SAN añadiendo sus nombres de dominio a las entradas SAN. En ese caso puede accederse al servicio mediante el nombre de host, por ejemplo `https://www.example.com`

## ¿Cómo se realiza la Validación de dominios con redirección?
{: #how-is-domain-validation-with-redirect-accomplished}
{:faq}

Depende del servidor. El procedimiento para completar la validación de dominios para los servidores Apache y Nginx se puede encontrar en la página [Completar la validación de control de dominio para HTTPS](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#redirect).

## ¿Cuánto tiempo lleva la validación de dominio?
{: #how-long-does-domain-validation-take}
{:faq}

La validación de dominio normalmente tarda entre 2 y 4 horas, pero varía en función del método elegido para la validación. La validación de DV con CNAME es la más rápida, normalmente tarda menos de una hora. La validación utilizando los métodos Estándar y de Redirección tarda típicamente unas 4 horas, después de que se ha direccionado el desafío.

## ¿Cuánto tiempo se tarda en crear y habilitar HTTPS para una CDN con un certificado SAN DV?
{: #how-long-does-it-take-to-create-and-enable-https-for-my-cdn-with-a-dv-san-certificate}
{:faq}

Una solicitud normal para habilitar HTTPS tarda un promedio de 3 a 9 horas, desde la solicitud inicial a la ejecución.

## ¿Cuánto tiempo se tarda en suprimir una CDN con un certificado SAN DV?
{: #how-long-does-it-take-to-delete-a-cdn-with-a-dv-san-certificate}
{:faq}

La supresión de la CDN requiere que se elimine el dominio del certificado en todos los servidores perimetrales. Este proceso puede tardar hasta 8 horas en completarse.

## ¿Hay algún coste adicional asociado con el uso del certificado SAN DV para mi CDN?
{: #is-there-any-additional-cost-associated-with-using-a-dv-san-certificate-for-my-cdn}
{:faq}

No. Las configuraciones de certificado SAN DV se proporcionan sin coste adicional en comparación con HTTP o con HTTPS con Certificado comodín.

## ¿Se puede actualizar la CDN creada con HTTPS de certificado comodín para que utilice un certificado SAN DV?
{: #can-my-cdn-created-using-https-with-wildcard-be-updated-to-use-a-dv-san-certificate}
{:faq}

No, no se puede cambiar una correlación con certificado comodín a certificado SAN.

## ¿Qué es una entidad emisora de certificados?
{: #what-is-a-certificate-authority}
{:faq}

Una entidad emisora de certificados (CA) es una entidad que emite Certificados digitales.

## ¿Qué CA utiliza el servicio CDN de {{site.data.keyword.cloud}} para emitir un certificado SAN DV?
{: #which-ca-does-ibm-cloud-cdn-service-use-for-issuing-a-dv-san-certificate}
{:faq}

El servicio CDN de IBM Cloud utiliza la entidad emisora de certificados LetsEncrypt.

## ¿Qué certificados SSL reciben soporte para la CDN de IBM Cloud?
{: #what-ssl-certificates-are-supported-for-ibm-cloud-cdn}
{:faq}

Los certificados SSL soportados son certificados comodín y certificados de validación de dominio (DV) de nombre alternativo (SAN). El certificado SAN se comparte entre varios clientes. La CDN de IBM Cloud no admite la carga de certificados personalizados.

## He recibido un correo electrónico solicitándome que haga frente a un reto de validación de dominios relacionado con mi CDN. ¿Qué hago ahora?
{: #i-received-an-email-asking-me-to-address-a-domain-validation-challenge-related-to-my-cdn}
{:faq}

La validación de dominio se puede resolver de una de estas tres maneras: CNAME, Estándar o Directa.

Para obtener detalles sobre cómo abordar cualquiera de ellos, consulte [Completar la validación de control de dominio para HTTPS](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#initial-steps-to-domain-control-validation).

## ¿Qué ocurrirá si no hago frente al reto para la validación de dominios de mi CDN?
{: #what-will-happen-if-i-dont-address-the-challenge-for-domain-validation-of-my-cdn}
{:faq}

Si el estado de la correlación está en estado DOMAIN_VALIDATION_PENDING durante más de 48 horas, la creación de la correlación se cancelará y el estado de la correlación será CREATE_ERROR. Y en este estado, puede optar por Reintentar la creación o suprimir la correlación.

## ¿Es necesario que un certificado comodín valide un dominio para mi CDN?
{: #does-a-wildcard-certificate-need-to-validate-a-domain-for-my-cdn}
{:faq}

No, pero solo puede utilizar el CNAME para recuperar contenido de su origen. `https://www.example-cname.cdnedge.bluemix.net`

## He recibido un correo electrónico que indica que mis dominios no apuntan al CNAME de CDN de IBM. ¿Qué hago ahora?
{: #i-received-an-email-indicating-that-my-domain-is-not-pointed-to-IBM-CDN-CNAME}
{:faq}

Este correo electrónico significa que la CDN no se está utilizando. Para utilizar la CDN y activar el dominio o dominios en el certificado o certificados, debe establecer los registros de DNS CNAME en su sistema proveedor de DNS.
Si completa esta acción en 7 días, se restaurará el tráfico HTTP y HTTPS para su CDN y la CDN pasará a estado RUNNING. Si al cabo de 7 días la CDN aún está sin utilizar, deberemos inhabilitar permanentemente HTTPS para el dominio de la CDN, para impedir que el dominio no utilizado bloquee que se añadan nuevas solicitudes de dominio de CDN al certificado SAN compartido. El acceso al tráfico HTTP a través de la CDN aún se podría restaurar posteriormente añadiendo un registro CNAME de su dominio.
Para obtener detalles sobre cómo abordar esta situación, consulte [Completar la validación de control de dominio para HTTPS](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#cname).

## Si utilizo el tipo de certificado SAN para mi CDN, ¿puedo seguir utilizando el CNAME para acceder a mi servicio?
{: #if-i-use-a-san-certificate-type-for-my-cdn-can-i-still-use-the-cname-for-access-to-my-service}
{:faq}

No. Para el certificado SAN, solo puede utilizar el dominio personalizado para acceder al contenido desde el origen.

## ¿Se han añadido todos mis dominios de CDN de IBM Cloud en un mismo certificado?
{: #are-all-of-my-ibm-cloud-cdn-domains-added-into-one-certificate}
{:faq}

No necesariamente. Akamai maneja la selección de certificados para garantizar que los certificados se encuentran en el estado más eficiente. Se añaden dominios a distintos certificados de manera proporcionada, de modo que no se puede garantizar que todos los dominios estén en el mismo certificado.

## ¿Por qué veo "comodín" cuando realizo un `dig` o cuando intento acceder al contenido cuando mi CDN está en el estado `Solicitando certificado`, `Validación de dominio pendiente` o `Despliegue de certificado`?
{: #why-do-i-see-wildcard}
{:faq}

Durante el proceso de solicitud de certificado SAN DV, la cadena de registros de DNS para la CDN está encadenada a un certificado comodín, temporalmente. Hasta que se haya completado el proceso, el contenido se sirve temporalmente a través de este certificado de comodín. Una vez que el proceso de solicitud se ha completado, la cadena de registros de DNS se actualiza para encadenarse a su certificado SAN DV de CDN.
