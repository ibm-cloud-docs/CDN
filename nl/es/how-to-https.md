---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-21"

keywords: domain, control, validation, https, san certificate, challenge, apache, nginx, redirect

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Cómo completar la validación de control de dominio (DCV) para HTTPS con DV SAN
{: #completing-domain-control-validation-for-https-with-dv-san}

En el siguiente diagrama se describen los diversos estados por los que pasará su CDN desde que se crea hasta que alcanza el estado de ejecución.

  ![Diagrama de estado de SAN](images/state-diagram-san.png)

## Pasos iniciales para la validación de control de dominio
{: #initial-steps-to-domain-control-validation}

**Paso 1:**

Una vez que haya solicitado su CDN con un certificado SAN DV, comienza el proceso de solicitud de certificado. Durante este proceso {{site.data.keyword.cloud}} CDN solicita un certificado de Akamai. Una vez que hay un certificado disponible, Akamai emite una solicitud a la entidad emisora de certificados (CA).

  * Durante este tiempo, el estado de CDN se muestra como **Solicitando certificado**.

    ![Solicitando estado del certificado](images/requesting-cert.png)

**Paso 2:**

Una vez que la CA recibe la solicitud, emite un Desafío de validación de dominio.

  * Cuando esto sucede, el estado de la CDN cambia a **Validación de dominio necesaria**.

    ![Validación de dominio necesaria](images/domain-validation-needed.png)

**Paso 3:**

Pulse en el nombre de la CDN que se debe validar. Se abre la página Visión general, donde puede ver el estado general de la CDN. En la parte superior de la página aparece una alerta que le recuerda que es necesaria la validación de dominio. Seleccione el botón **Visualizar validación de dominio** para abrir una ventana que muestra la información de desafío necesaria para completar el proceso de validación.

   ![Validación de dominio necesaria](images/view-domain-validation.png)

**Paso 4:** Una vez que haya completado uno de los pasos de validación de la sección sobre cómo hacer frente a un Desafío de validación de dominio, la CDN pasa al estado **Desplegando el certificado**. Durante este tiempo, Akamai distribuye el certificado validado a sus servidores perimetrales. El despliegue de un certificado puede tardar de 2 a 4 horas.

  * Cuando este proceso se haya completado, todos los dominios, independientemente del método de validación utilizado, se trasladan a un estado **Configuración de CNAME**.

Puede encontrar información adicional sobre cómo completar la configuración de CNAME y cómo supervisar la CDN, en la página [Puesta en marcha](/docs/infrastructure/CDN?topic=CDN-getting-your-cdn-to-running-status#get-to-running).


## Validación de control de dominio
{: #domain-control-validation}

Para añadir el nombre del dominio de la CDN al certificado SAN, debe probar que tiene el control administrativo sobre el dominio. A este proceso de prueba se le hace referencia como la validación de control de dominio (DCV). Debe realizar la validación de control de dominio (DCV) en 48 horas. Si no lo hace, la solicitud caduca, debe empezar de nuevo el proceso de pedido. Las tres maneras de abordar la DCV se describen en las secciones que siguen.


### CNAME
{: #cname}

Este método se recomienda **SÓLO** si la CDN **no** da servicio a tráfico en directo. Si el dominio está sirviendo tráfico en directo, se recomienda utilizar el método Estándar o el método de Redireccionar para validar el dominio.

Para utilizar este método, añadirá un registro CNAME para el dominio de CDN en su configuración de DNS. El valor de CNAME que se debe utilizar es el CNAME que ha utilizado al crear la CDN. Debería finalizar con el dominio `cdnedge.bluemix.net`. No se requiere ninguna otra acción. La verificación del control de dominio (DCV) continuará de forma automática a partir de este punto. La validación puede tardar de 2 a 4 horas. Una vez que se haya desplegado el certificado, la CDN pasa directamente al estado de ejecución (RUNNING).

La mayoría de los proveedores de DNS pueden suministrar instrucciones sobre cómo configurar o cambiar el CNAME. A continuación, se muestra un ejemplo de un registro CNAME típico:

| **Tipo de recurso** | **Host** | **Apunta a (CNAME)** | **TTL** |
|------------------|---------|-------------|----------------|
| CNAME | www.example.com | example.cdnedge.bluemix.net | 15 minutos |


---
### Estándar
{: #standard}

Si selecciona el método Estándar para la validación de dominios, la ventana Validación de dominio muestra un **URL de desafío** y una **Respuesta de desafío**. Para completar el proceso de Validación de dominio, añada la **Respuesta de desafío** proporcionada al servidor de origen. Después de que se añada, la CA puede recuperar la **respuesta de desafío** del servidor de origen utilizando el URL especificado en el **URL de desafío**. Una vez que el servidor de origen esté configurado correctamente, la validación de dominio puede tardar de 2 a 4 horas.

   ![Desafío de validación de dominio estándar](images/domain-validation-standard.png)

Para completar correctamente la validación de dominio a través del método Estándar, debe configurar el servidor de origen de una forma concreta. A continuación se describen los procedimientos de ejemplo para los servidores de _Apache_ y _Nginx_.

**Ejemplo de situación**
* Servidor de origen: `www.example.com`
* Dominio de CDN: `cdn.example.com`
* URL de desafío: `http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject`
* Respuesta al desafío: `examplechallenge`

#### Configuración de Apache
{: #apache-configuration}

  * **Paso 1:** Inicie la sesión en el sistema que ejecuta el servidor Apache2.

  * **Paso 2:** Cree el archivo de respuestas de desafío para la respuesta de desafío bajo `.well-known/acme-challenge/` en el directorio para el contenido del sitio web.  La ubicación predeterminada para el contenido del sitio web de Apache2 es `/var/www/html/`. Para este ejemplo, la respuesta de desafío se colocaría en el directorio `/var/www/html/.well-known/acme-challenge/`.

      ```
      mkdir -p /var/www/html/.well-known/acme-challenge
      printf "examplechallenge" > /var/www/html/.well-known/acme-challenge/examplechallenge-fileobject
      ```

  * **Paso 3:** Si es necesario, abra el archivo de configuración del servidor Apache2. Las ubicaciones predeterminadas para los archivos de configuración son `/etc/apache2/apache2.conf` y `/etc/apache2/sites-enabled/`.

  * **Paso 4:** Si es necesario, añada el dominio del CDN como **ServerAlias** adicional al host virtual para el origen.

  * **Paso 5:** Si ha tenido que modificar la configuración del servidor Apache2, reinícielo con un tiempo de inactividad mínimo utilizando el siguiente mandato:

      ```
      apachectl -k graceful
      ```

  * **Paso 6:** Cree un registro A en el DNS entre el dominio CDN y la dirección IP del servidor de origen.

#### Nginx Configuration
{: #nginx-configuration}

  * **Paso 1:** Inicie la sesión en el sistema que ejecuta el servidor Nginx.

  * **Paso 2:** Cree el archivo de respuestas de desafío para la respuesta de desafío bajo `.well-known/acme-challenge/` en el directorio para el contenido del sitio web.  La ubicación predeterminada para el contenido del sitio web de Nginx es `/usr/share/nginx/html/`.  Para este ejemplo, la respuesta de desafío se colocaría en el directorio `/usr/share/nginx/html/.well-known/acme-challenge/`.
      ```
      mkdir -p /usr/share/nginx/html/.well-known/acme-challenge
      printf "examplechallenge" > /usr/share/nginx/html/.well-known/acme-challenge/examplechallenge-fileobject
      ```

  * **Paso 3:** Si es necesario, abra el archivo de configuración del servidor Nginx. Las ubicaciones predeterminadas para los archivos de configuración son `/etc/nginx/nginx.conf` y `/etc/nginx/conf.d/`.

  * **Paso 4:** Si es necesario, añada el dominio del CDN como **server_name** adicional al bloque del servidor para el origen.

  * **Paso 5:** Si ha tenido que modificar la configuración del servidor Nginx, reinícielo con un tiempo de inactividad mínimo utilizando el siguiente mandato:

      ```
      nginx -s reload
      ```

  * **Paso 6:** Cree un registro A en el DNS entre el dominio CDN y la dirección IP del servidor de origen.

#### Verifique que este método Estándar para abordar la Validación de dominio está preparado para la CA
{: #verify-that-this-standard-method-to-address-domain-validation-is-ready-for-the-ca}

* Para verificar que este método funciona a través de `curl`, ejecute ese mandato para el URL de Desafío:
    ```
    curl -v http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```
* Para verificar que este método funciona a través de un navegador, intente acceder al URL de Desafío desde el navegador.

En cualquiera de los casos, debe poder recuperar la copia del objeto de archivo de Desafío de validación de dominio almacenado en el servidor de origen.

#### Limpieza para el método Estándar
{: #clean-up-for-the-standard-method}

Después de que la CDN haya alcanzado el estado **Desplegando certificado**:
1. Elimine el archivo `examplechallenge-fileobject`. (Opcional)
1. Elimine el ServerAlias agregado (Apache2) o el server_name (Nginx) de la configuración del servidor, si es necesario. (Opcional)
1. Elimine el registro A entre el dominio CDN y la dirección IP del servidor de origen.

---
### Redirección
{: #redirect}

Al pulsar en el separador **Redirigir** se muestra toda la información necesaria para la Validación de dominio a través de redirección. Esta información permite que la CA recupere una copia de la **respuesta de desafío** de Akamai a través del servidor de origen. Una vez que el servidor esté configurado correctamente, la validación de dominio puede tardar de 2 a 4 horas.

   ![Desafío de validación de dominio por redirección](images/domain-validation-redirect.png)

Para completar satisfactoriamente la validación de dominio a través del método Redirect, es posible que tenga que configurar el servidor web de una forma concreta. A continuación se describen en las siguientes secciones los procedimientos de ejemplo para los servidores Apache y Nginx.

**Ejemplo de situación**
* Servidor de origen: `www.example.com`
* Dominio de CDN: `cdn.example.com`
* URL de desafío: `http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject`
* Redirección de URL: `http://dcv.akamai.com/.well-known/acme-challenge/examplechallenge-fileobject`

#### Configuración de redirección de Apache
{: #apache-redirect-configuration}

  * **Paso 1:** Inicie la sesión en el sistema que ejecuta el servidor Apache2.

  * **Paso 2:** Abra el archivo de configuración del servidor Apache2. Las ubicaciones predeterminadas para los archivos de configuración son `/etc/apache2/apache2.conf` y `/etc/apache2/sites-enabled/`.

  * **Paso 3:** Añada una sentencia de redirección en la ubicación adecuada dentro del archivo de configuración. Si es necesario, añada el dominio del CDN como **ServerAlias** adicional al host virtual para el origen.

    ```
    Redirect /.well-known/acme-challenge/examplechallenge-fileobject http://dcv.akamai.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```

  * **Paso 4:** Reinicie el servidor Apache2 con un tiempo de inactividad mínimo utilizando el siguiente mandato:

    ```
    apachectl -k graceful
    ```

  * **Paso 5:** Cree un registro A en el DNS entre el dominio CDN y la dirección IP del servidor de origen.

#### Configuración de redirección de Nginx
{: #nginx-redirect-confguration}

  * **Paso 1:** Inicie la sesión en el sistema que ejecuta el servidor Nginx.

  * **Paso 2:** Abra el archivo de configuración del servidor Nginx. Las ubicaciones predeterminadas para los archivos de configuración son `/etc/nginx/nginx.conf` y `/etc/nginx/conf.d/`.

  * **Paso 3:** Hay dos métodos igualmente válidos para este paso.

    * Opción 1: (recomendada) Añada un bloque `location` con una directiva `return` para realizar la redirección dentro del bloque `server` adecuado. Si es necesario, añada el dominio del CDN como **server_name** adicional al bloque del servidor para el origen.

    ```
    server {
      listen 80;
      server_name www.example.com cdn.example.com;

      # Algunas directivas de configuración del servidor
      # ...

      location = /.well-known/acme-challenge/examplechallenge-fileobject  {
          return 302 http://dcv.akamai.com$request_uri;
      }

      # Algunas directivas de configuración de servidor más
      # ...
    }
    ```

   * Opción 2: Añada una directiva `rewrite` dentro del bloque `server`. Si es necesario, añada el dominio del CDN como **server_name** adicional al bloque del servidor para el origen.

    ```
    server {
      listen 80;
      server_name www.exmaple.com cdn.example.com;

      # Algunas directivas de configuración del servidor
      # ...

      rewrite ^/(\.well-known/acme-challenge/examplechallenge-fileobject)$ http://dcv.akamai.com/$1 redirect;

      # Algunas directivas de configuración de servidor más
      # ...
    }
    ```

  * **Paso 4:** Reinicie el servidor Nginx con un tiempo de inactividad mínimo utilizando el siguiente mandato:

    ```
    nginx -s reload
    ```

  * **Paso 5:** Cree un registro A en el DNS entre el dominio CDN y la dirección IP del servidor de origen.

#### Verifique que se está produciendo la redirección
{: #verify-that-the-redirect-is-occurring}

Si se completan estos pasos, se redirigirá _únicamente_ el tráfico para el URL específico de Desafío a la Redirección URL. Puede verificar que la redirección ha funcionado correctamente a través de `curl` o a través del navegador.

* Para verificar que la redirección funciona a través de `curl`, ejecute este mandato para el URL de Desafío:

    ```
    curl -vL http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```

* Para verificar que la redirección funciona a través de un navegador, intente acceder al URL de Desafío desde el navegador.

En cualquier caso, debe poder recuperar la copia del objeto de archivo de Desafío de validación de dominio de Akamai en el dominio `dcv.akamai.com`, al que se ha redirigido la solicitud original.

#### Limpieza para el método de Redirección
{: #clean-up-for-the-redirect-method}

Después de que la CDN haya alcanzado el estado **Desplegando certificado**:
1. Elimine las sentencias o bloques de redirección del archivo de configuración. (Opcional)
1. Elimine el ServerAlias agregado (Apache2) o el server_name (Nginx) de la configuración del servidor, si es necesario. (Opcional)
1. Elimine el registro A entre el dominio CDN y la dirección IP del servidor de origen.
