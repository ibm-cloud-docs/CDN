---

copyright:
  years: 2018
lastupdated: "2018-06-28"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Acerca de HTTPS

IBM Cloud ofrece dos maneras de proteger la CDN con HTTPS: Certificado comodín y Certificado SAN de validación de dominio (DV). Ambas características se habilitan seleccionando **Puerto HTTPS** al configurar la CDN. El puerto HTTPS predeterminado es 443, o puede elegir un número de puerto diferente para direccionar el tráfico HTTPS. Se puede encontrar una lista de números de puerto permitidos en las [preguntas más frecuentes](faqs.html#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-).

## Soporte de certificado comodín
>El certificado comodín es la forma más simple de entregar contenido web a los usuarios de forma segura. **Debe** utilizarse El CNAME de la CDN completo, incluyendo el sufijo del certificado comodín, como punto de entrada del servicio (por ejemplo, `https://example.cdnedge.bluemix.net`) para utilizar el certificado comodín.
>
>La CDN de IBM Cloud utiliza el certificado comodín ` *.cdnedge.bluemix.net `. El CNAME, independientemente de si se ha creado para el usuario o lo ha proporcionado él, y que acaba en un sufijo `*.cdnedge.bluemix.net`, se añade al certificado comodín que se mantiene en el servidor de la CDN. Por lo tanto, el CNAME se convierte en la única manera de utilizar HTTPS para la CDN por parte de los usuarios finales.

![Diagrama para HTTP y comodín](images/state-diagram-wildcard.png)

## Soporte de certificado de nombre alternativo (SAN)

>El Certificado de nombre alternativo (SAN) es un certificado SSL digital que permite que varios dominios, o nombres de host, estén protegidos por un único certificado.
>
>Con el certificado SAN para HTTPS, el nombre de host del CDN primario se añade a un certificado emitido por una entidad emisora de certificados. Esto permite a los usuarios acceder a su servicio de forma segura a través del nombre de host en lugar del CNAME; por ejemplo, `https://www.example.com`.
>
>Cuando se realiza el pedido de CDN utilizando el certificado SAN de HTTPS, pasa por el proceso de solicitar un certificado y de crear una Validación de control de dominio (DCV). DCV es el proceso utilizado por una Autoridad Certificadora para establecer que se tiene acceso y control del dominio. Es necesaria su acción para completar este paso. Una vez que se ha establecido el control, el certificado se despliega en los servidores perimetrales de CDN de todo el mundo. Una vez que el certificado se ha desplegado correctamente, su renovación se maneja automáticamente. Puede encontrar más información sobre esta característica en la [ descripción de característica ](about.html#https-protocol-support). Los métodos de validación del control de dominio se explican con más detalle en la página [Completar la validación de control de dominio para HTTPS](how-to-https.html#initial-steps-to-domain-control-validation) page.

![Diagrama para HTTPS con Cert SAN](images/state-diagram-san.png)
