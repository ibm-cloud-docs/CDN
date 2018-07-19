---

copyright:
  years: 2018
lastupdated: "2018-06-17"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Completar la validación de control de dominio para HTTPS

## Pasos iniciales para la validación de control de dominio

**Paso 1:**

Una vez que haya solicitado su CDN con un certificado SAN DV, comienza el proceso de solicitud de certificado. Durante este proceso IBM Cloud CDN solicita un certificado de Akamai. Una vez que hay un certificado disponible, Akamai emite una solicitud a la entidad emisora de certificados (CA).

  * Durante este tiempo, el estado de CDN se mostrará como **Solicitando certificado**.

    ![Solicitando certificado](images/requesting-cert.png)

**Paso 2:**

Una vez que la CA reciba la solicitud, emitirá un Desafío de validación de dominio.

  * Cuando esto sucede, el estado de la CDN cambia a **Validación de dominio necesaria**.

    ![Validación de dominio necesaria](images/domain-validation-needed.png)

**Paso 3:**

Pulse en el nombre de la CDN que se debe validar. Se abre la página Visión general, donde puede ver el estado general de la CDN. En la parte superior de la página se encuentra una alerta que le recuerda que es necesaria la validación de dominios. Seleccione el botón **Visualizar validación de dominio** para abrir una ventana que le muestra la información de reto necesaria para completar el proceso de validación.

   ![Validación de dominio necesaria](images/view-domain-validation.png)

**Paso 4:** Una vez que haya completado uno de los pasos de validación de la sección sobre cómo hacer frente a un Desafío de validación de dominio, la CDN se mueve al estado **Desplegando el certificado**. Durante este tiempo, Akamai distribuirá el certificado validado a sus servidores perimetrales. El despliegue de un certificado puede tardar de 2 a 4 horas.

  * Cuando este proceso se haya completado, todos los dominios, independientemente del método de validación utilizado, se trasladan a un estado **Configuración de CNAME**.

Puede encontrar información adicional sobre cómo completar la configuración de CNAME, así como sobre supervisar el CDN, en la página [Puesta en marcha](basic-functions.html#get-to-running).


## Desafío de validación de dominio

El Desafío de Validación de Dominio prueba que es el propietario del dominio. A continuación se muestran tres maneras de hacer frente al Desafío de validación de dominios.

**NOTA**: si no responde a la solicitud de validación de dominio en un plazo de 48 horas, la solicitud caducará y tendrá que volver a empezar el proceso de solicitud.

### CNAME

Si el registro CNAME se ha añadido a su proveedor de DNS antes de solicitar la CDN, no es necesario que realice ninguna otra acción. The Domain Validation is automatically handled by IBM Cloud, Akamai, and the CA. La validación puede tardar de 2 a 4 horas.

  * Si todavía no ha configurado su CNAME con el proveedor de DNS, tiene que hacerlo en este momento. La mayoría de los proveedores de DNS pueden suministrar instrucciones sobre cómo configurar o cambiar el CNAME.

   ![CNAME de validación de dominio](images/domain-validation-cname.png)

**NOTA**: Este método se recomienda **SÓLO** si la CDN **no** da servicio a tráfico en directo. Si el dominio está sirviendo tráfico en directo, se recomienda utilizar el método Estándar para validar el dominio.

---
### Estándar

Si selecciona el método Estándar para la validación de dominios, la ventana Validación de dominio muestra un **URL de Reto** y una **Respuesta de desafío**. Para completar el proceso Validación de dominio, debe añadir la **Respuesta del reto** proporcionada al servidor de origen. Esto permite que la CA recupere la **respuesta de desafío** del servidor de origen utilizando el URL especificado en el **URL de reto**. Una vez que el servidor de origen esté configurado correctamente, la validación de dominio puede tardar de 2 a 4 horas.

   ![Desafío de validación de dominio estándar](images/domain-validation-standard.png)

Para completar correctamente la validación de dominio a través del método Estándar, debe configurar el servidor de origen de una forma concreta. A continuación se describen los procedimientos de ejemplo para los servidores Apache y Nginx.

**Ejemplo de situación**
* Servidor de origen: `www.example.com`
* Dominio de CDN: `cdn.example.com`
* URL de desafío: `http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject`
* Respuesta al desafío: `examplechallenge`

#### Configuración de Apache

  * **Paso 1:** Inicie la sesión en el sistema que ejecuta el servidor Apache2.

  * **Paso 2:** Cree el archivo de respuestas de reto para la respuesta de reto bajo `.known/acme-challenge/` en el directorio para el contenido del sitio web.  La ubicación predeterminada para el contenido del sitio web de Apache2 es `/var/www/html/`. Para este ejemplo, la respuesta de reto se colocaría en el directorio `/var/www/html/.well-known/acme-challenge/`.

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

  * **Paso 1:** Inicie la sesión en el sistema que ejecuta el servidor Nginx.

  * **Paso 2:** Cree el archivo de respuestas de reto para la respuesta de reto bajo `.known/acme-challenge/` en el directorio para el contenido del sitio web.  La ubicación predeterminada para el contenido del sitio web de Nginx es `/usr/share/nginx/html/`.  Para este ejemplo, la respuesta de reto se colocaría en el directorio `/usr/share/nginx/html/.well-known/acme-challenge/`.
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

* Para verificar que este método funciona a través de `curl`, ejecute ese mandato para el URL de Desafío:
    ```
    curl -v http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```
* Para verificar que este método funciona a través de un navegador, intente acceder al URL de Desafío desde el navegador.

En cualquiera de los casos, debe poder recuperar la copia del objeto de archivo de Reto de validación de dominio almacenado en el servidor de origen.

#### Limpieza para el método Estándar

Después de que la CDN haya alcanzado el estado **Desplegando certificado**:
1. Elimine el archivo `examplechallenge-fileobject`. (Opcional)
1. Elimine el ServerAlias agregado (Apache2) o el server_name (Nginx) de la configuración del servidor, si es necesario. (Opcional)
1. Elimine el registro A entre el dominio CDN y la dirección IP del servidor de origen.

---
### Redirección

Al pulsar en el separador **Redirigir** se muestra toda la información necesaria para la Validación de dominio a través de redirección. Esta información permite que la CA recupere una copia de la **respuesta de desafío** de Akamai a través del servidor de origen. Una vez que el servidor esté configurado correctamente, la validación de dominio puede tardar de 2 a 4 horas.

   ![Desafío de validación de dominio por redirección](images/domain-validation-redirect.png)

Para completar satisfactoriamente la validación de dominio a través del método Redirect, es posible que tenga que configurar el servidor web de una forma concreta. A continuación se describen los procedimientos de ejemplo para los servidores Apache y Nginx.

**Ejemplo de situación**
* Servidor de origen: `www.example.com`
* Dominio de CDN: `cdn.example.com`
* URL de desafío: `http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject`
* URL de redirección: `http://dcv.akamai.com/.well-known/acme-challenge/examplechallenge-fileobject`

#### Configuración de redirección de Apache

  * **Paso 1:** Inicie la sesión en el sistema que ejecuta el servidor Apache2.

  * **Paso 2:** Abra el archivo de configuración del servidor Apache2. Las ubicaciones predeterminadas para los archivos de configuración son `/etc/apache2/apache2.conf` y `/etc/apache2/sites-enabled/`.

  * **Paso 3:** Añada una sentencia de redirección en la ubicación adecuada dentro del archivo de configuración. Si es necesario, añada el dominio del CDN como **ServerAlias** adicional al host virtual para el origen.
    ```
    Redirect http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject http://dcv.akamai.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```

  * **Paso 4:** Reinicie el servidor Apache2 con un tiempo de inactividad mínimo utilizando el siguiente mandato:

    ```
    apachectl -k graceful
    ```

  * **Paso 5:** Cree un registro A en el DNS entre el dominio CDN y la dirección IP del servidor de origen.

#### Configuración de redirección de Nginx

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

Si se completan estos pasos, sólo se redirigirá el tráfico para el URL específico de Reto a la dirección URL. Puede verificar que la redirección ha funcionado correctamente a través de `curl` o del navegador.

* Para verificar que la redirección funciona a través de `curl`, ejecute este mandato para el URL de Desafío:

    ```
    curl -vL http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```

* Para verificar que la redirección funciona a través de un navegador, intente acceder al URL de Desafío desde el navegador.

En cualquier caso, debe poder recuperar la copia del objeto de archivo de Reto de validación de dominio de Akamai en el dominio dcv.akamai.com, al que se ha redirigido la solicitud original.

#### Limpieza para el método de Redirección

Después de que la CDN haya alcanzado el estado **Desplegando certificado**:
1. Elimine las sentencias o bloques de redirección del archivo de configuración. (Opcional)
1. Elimine el ServerAlias agregado (Apache2) o el server_name (Nginx) de la configuración del servidor, si es necesario. (Opcional)
1. Elimine el registro A entre el dominio CDN y la dirección IP del servidor de origen.
