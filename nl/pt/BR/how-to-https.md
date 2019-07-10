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

# Concluindo a Domain Control Validation para HTTPS com DV SAN
{: #completing-domain-control-validation-for-https-with-dv-san}

O diagrama a seguir descreve os vários estados em que seu CDN entrará a partir do momento em que ele for criado, até que ele atinja o status de execução.

  ![Diagrama de estado do SAN](images/state-diagram-san.png)

## Etapas Iniciais para Validação de Controle de
{: #initial-steps-to-domain-control-validation}

**Etapa 1:**

Depois de ter pedido seu CDN com um Certificado SAN DV, o processo de solicitação de certificado será iniciado. Durante esse processo, o {{site.data.keyword.cloud}} CDN solicita um certificado da Akamai. Quando um certificado se torna disponível, o Akamai emite uma solicitação para a Autoridade de Certificação (CA).

  * Durante esse tempo, o status do CDN é mostrado como **Solicitando certificado**.

    ![Requesting Certificate Status](images/requesting-cert.png)

**Etapa 2:**

Depois que a CA recebe a solicitação, ela emite um Desafio de validação de domínio.

  * Quando isso acontece, o status de seu CDN muda para **Validação de domínio necessária**.

    ![Domain Validation Needed](images/domain-validation-needed.png)

**Etapa 3:**

Clique no nome do CDN que precisa ser validado. A página Visão geral é aberta, na qual é possível ver o status geral de seu CDN. Na parte superior da página aparece um alerta, lembrando-o de que a validação de domínio é necessária. Selecione o botão **Visualizar validação do domínio** para abrir uma janela mostrando as informações de desafio necessárias para concluir o processo de validação.

   ![Domain Validation Needed](images/view-domain-validation.png)

**Etapa 4:** depois que você tiver concluído uma das etapas de validação da seção sobre como tratar um Desafio de validação de domínio, seu CDN será movido para o status **Implementando certificado**. Durante esse tempo, o Akamai distribuirá seu certificado validado para seus servidores de borda. A implementação de um certificado pode levar de 2 a 4 horas.

  * Quando esse processo é concluído, todos os domínios, independentemente do método de validação usado, mudam para um estado **Configuração do CNAME**.

Informações adicionais sobre como concluir sua Configuração de CNAME e supervisionar seu CDN podem ser localizadas na página [Começando a execução](/docs/infrastructure/CDN?topic=CDN-getting-your-cdn-to-running-status#get-to-running).


## Domain Control Validation
{: #domain-control-validation}

Para que seu nome de domínio do CDN seja incluído no certificado SAN, deve-se provar que você tem controle administrativo sobre seu domínio. Esse processo de prova é conhecido como tratamento da Domain Control Validation (DCV). Deve-se tratar da DCV em 48 horas. Se houver falhar ao fazer isso, sua solicitação expirará e será necessário iniciar o processo de pedido novamente. As três maneiras diferentes de tratar da DCV são descritas nas seções a seguir.


### CNAME
{: #cname}

Esse método é recomendado **SOMENTE** se o seu CDN **não** está entregando tráfego em tempo real. Se o seu domínio estiver entregando tráfego em tempo real, recomendamos usar o método Padrão ou de Redirecionamento para validar seu domínio.

Para usar esse método, você incluirá um registro CNAME para seu domínio do CDN na configuração do DNS. O valor CNAME a ser usado é o CNAME que você usou quando criou o CDN. Ele deve terminar com o domínio `cdnedge.bluemix.net`. Nenhuma outra ação será necessária de sua parte. A DCV avançará automaticamente a partir deste ponto. A validação pode levar de 2 a 4 horas. Depois que o certificado é implementado, seu CDN é movido diretamente para o status EM EXECUÇÃO.

A maioria dos provedores DNS pode fornecer instruções sobre como configurar ou mudar o CNAME. Aqui está um exemplo de um registro CNAME típico:

| **Tipo de recurso** | **Host** | **Pontos para (CNAME)** | **TTL** |
|------------------|---------|-------------|----------------|
| CNAME | www.example.com | example.cdnedge.bluemix.net | 15 minutos |


---
### Padrão
{: #standard}

Se você escolher o método Padrão para a Validação de Domínio, a janela Validação de Domínio mostrará uma **URL de desafio** e uma **Resposta de segurança**. Para concluir o processo de Validação de domínio, inclua a **Resposta desafio** fornecida no servidor de origem. Após a inclusão, a CA pode recuperar a **Resposta desafio** do servidor de origem usando a URL especificada na **URL de desafio**. Depois que o servidor de origem estiver configurado corretamente, a Validação de Domínio poderá levar de 2 a 4 horas.

   ![Domain Validation Challenge Standard](images/domain-validation-standard.png)

Para concluir com sucesso a Validação de domínio por meio do método Padrão, deve-se configurar o servidor de origem de uma maneira específica. Os procedimentos de exemplo para os servidores _Apache(TM)_ e _Nginx(TM)_ são descritos abaixo.

**Situação de exemplo**
* Servidor de origem:  ` www.example.com `
* Domínio CDN:  ` cdn.example.com `
* URL do Challenge:  ` http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject `
* Resposta de Desafio:  ` examplechallenge `

#### Configuração do Apache
{: #apache-configuration}

  * **Etapa 1:** efetue login na máquina que está executando o servidor Apache2.

  * **Etapa 2:** crie o arquivo de resposta de segurança para a resposta de segurança sob `.well-known/acme-challenge/` no diretório para o conteúdo do seu website.  O local padrão para o conteúdo do website do Apache2 é `/var/www/html/`. Para este exemplo, a resposta de segurança é colocada no diretório `/var/www/html/.well-known/acme-challenge/`.

      ```
      mkdir -p /var/www/html/.well-known/acme-challenge
      printf "examplechallenge" > /var/www/html/.well-known/acme-challenge/examplechallenge-fileobject
      ```

  * **Etapa 3:** se necessário, abra o arquivo de configuração do servidor Apache2. Os diretórios `/etc/apache2/apache2.conf` e `/etc/apache2/sites-enabled/` são os locais padrão para os arquivos de configuração.

  * **Etapa 4:** se necessário, inclua seu domínio do CDN como um **ServerAlias** adicional no host virtual de sua origem.

  * **Etapa 5:** se você tiver que modificar a configuração do servidor Apache2, reinicie o servidor Apache2 com um tempo de inatividade mínimo usando o comando a seguir:

      ```
      apachectl -k graceful
      ```

  * **Etapa 6:** crie um registro A em seu DNS entre o domínio do CDN e o endereço IP do servidor de origem.

#### Configuração de Nginx
{: #nginx-configuration}

  * **Etapa 1:** efetue login na máquina que está executando o servidor Nginx.

  * **Etapa 2:** crie o arquivo de resposta de segurança para a resposta de segurança sob `.well-known/acme-challenge/` no diretório para o conteúdo do seu website.  O local padrão para o conteúdo do website do Nginx é `/usr/share/nginx/html/`.  Para este exemplo, a resposta de segurança é colocada no diretório `/usr/share/nginx/html/.well-known/acme-challenge/`.
      ```
      mkdir -p /usr/share/nginx/html/.well-known/acme-challenge
      printf "examplechallenge" > /usr/share/nginx/html/.well-known/acme-challenge/examplechallenge-fileobject
      ```

  * **Etapa 3:** se necessário, abra seu arquivo de configuração do servidor Nginx. Os diretórios `/etc/nginx/nginx.conf` e `/etc/nginx/conf.d/` são os locais padrão para os arquivos de configuração.

  * **Etapa 4:** se necessário, inclua seu domínio do CDN como um **server_name** adicional no bloco do servidor para sua origem.

  * **Etapa 5:** se você tiver que modificar sua configuração do servidor Nginx, reinicie o servidor Nginx com um tempo de inatividade mínimo usando o comando a seguir:

      ```
      nginx -s reload
      ```

  * **Etapa 6:** crie um registro A em seu DNS entre o domínio do CDN e o endereço IP do servidor de origem.

#### Verifique se este método Padrão para resolver a Validação de Domínio está pronto para a CA
{: #verify-that-this-standard-method-to-address-domain-validation-is-ready-for-the-ca}

* Para verificar se esse método funciona por meio de `curl`, execute esse comando para a URL de Desafio.
    ```
    curl -v http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```
* Para verificar se esse método funciona por meio de um navegador, tente acessar a URL do Desafio por meio de seu navegador.

Em qualquer um dos casos, é necessário ser capaz de recuperar a cópia do objeto de arquivo do Desafio de Validação de Domínio, que é armazenada em seu servidor de origem.

#### Limpeza para o método Standard
{: #clean-up-for-the-standard-method}

Após o seu CDN ter atingido o status **Implementação de certificado**:
1. Remova o arquivo  ` examplechallenge-fileobject ` . (opcional)
1. Remova o ServerAlias (Apache2) incluído ou o nome_do_servidor (Nginx) de sua configuração do servidor, se necessário. (opcional)
1. Remova o registro A entre o domínio do CDN e o IP do servidor de origem.

---
### Redirecionar
{: #redirect}

Clicar na guia **Redirecionar** exibe todas as informações necessárias para resolver a Validação de Domínio por meio de redirecionamento. Essas informações permitem que a CA recupere uma cópia da **Resposta de segurança** do Akamai por meio do servidor de origem. Depois que o servidor estiver configurado corretamente, a Validação de Domínio poderá levar de 2 a 4 horas.

   ![Redirecionamento de Desafio de Validação de Domínio](images/domain-validation-redirect.png)

Para concluir com sucesso a Validação de Domínio por meio do método Redirecionamento, poderá ser necessário configurar o servidor da web de uma maneira específica. Os procedimentos de exemplo para os servidores Apache e Nginx são descritos nas seções a seguir.

**Situação de exemplo**
* Servidor de origem:  ` www.example.com `
* Domínio CDN:  ` cdn.example.com `
* URL do Challenge:  ` http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject `
* Redirecionamento de URL:  ` http://dcv.akamai.com/.well-known/acme-challenge/examplechallenge-fileobject `

#### Configuração de Redirecionamento do Apache
{: #apache-redirect-configuration}

  * **Etapa 1:** efetue login na máquina que está executando o servidor Apache2.

  * **Etapa 2:** abra seu arquivo de configuração do servidor Apache2. `/etc/apache2/apache2.conf` e `/etc/apache2/sites-enabled/` são os locais padrão para o arquivo de configuração.

  * **Etapa 3:** inclua uma instrução de redirecionamento no local apropriado dentro do arquivo de configuração. Se necessário, inclua seu domínio CDN como um **ServerAlias** adicional no host virtual de sua origem.

    ```
    Redirect /.well-known/acme-challenge/examplechallenge-fileobject http://dcv.akamai.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```

  * **Etapa 4:** reinicie o servidor Apache2 com um tempo de inatividade mínimo usando o comando a seguir:

    ```
    apachectl -k graceful
    ```

  * **Etapa 5:** crie um registro A em seu DNS entre o domínio do CDN e o endereço
IP do servidor de origem.

#### Configuração de redirecionamento do Nginx
{: #nginx-redirect-confguration}

  * **Etapa 1:** efetue login na máquina que está executando o servidor Nginx.

  * **Etapa 2:** abra seu arquivo de configuração do servidor Nginx. Os diretórios `/etc/nginx/nginx.conf` e `/etc/nginx/conf.d/` são os locais padrão para os arquivos de configuração.

  * **Etapa 3:** há dois métodos igualmente válidos para esta etapa.

    * Opção 1: (recomendado) inclua um bloco `location` com uma diretiva `return` para executar o redirecionamento dentro do bloco `server` apropriado. Se necessário, inclua o seu domínio do CDN como um **nome_do_servidor** adicional para o bloco do servidor para a sua origem.

    ```
    server {
      listen 80;
      server_name www.example.com cdn.example.com;

      # Some server configuration directives #...

      location = /.well-known/acme-challenge/examplechallenge-fileobject  {
          return 302 http://dcv.akamai.com$request_uri;
      }

      # Some more server configuration directives #...
    }
    ```

   * Opção 2: inclua uma diretiva `rewrite` dentro do bloco `server`. Se necessário, inclua o seu domínio do CDN como um **nome_do_servidor** adicional para o bloco do servidor para a sua origem.

    ```
    server {
      listen 80;
      server_name www.exmaple.com cdn.example.com;

      # Some server configuration directives #...

      rewrite ^/(\.well-known/acme-challenge/examplechallenge-fileobject)$ http://dcv.akamai.com/$1 redirect;

      # Some more server configuration directives #...
    }
    ```

  * **Etapa 4:** reinicie o servidor Nginx com um tempo de inatividade mínimo usando o comando a seguir:

    ```
    nginx -s reload
    ```

  * **Etapa 5:** crie um registro A em seu DNS entre o domínio do CDN e o endereço
IP do servidor de origem.

#### Verifique se o redirecionamento está ocorrendo
{: #verify-that-the-redirect-is-occurring}

A conclusão dessas etapas redireciona _somente_ o tráfego da URL de desafio específica para o Redirecionamento de URL. É possível verificar se o redirecionamento funcionou adequadamente por meio de `curl` ou por meio do navegador.

* Para verificar se o redirecionamento funciona por meio do comando `curl`, execute esse comando para a URL de Desafio.

    ```
    curl -vL http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```

* Para verificar se o redirecionamento funciona por meio do navegador, tente acessar a URL de desafio de seu navegador.

Em qualquer um dos casos, será necessário recuperar a cópia do objeto de arquivo do Desafio de validação de domínio do Akamai no domínio `dcv.akamai.com`, para o qual a solicitação original foi redirecionada.

#### Limpeza para o método Redirect
{: #clean-up-for-the-redirect-method}

Após o seu CDN ter atingido o status **Implementação de certificado**:
1. Remova as instruções ou os blocos de redirecionamento do arquivo de configuração. (opcional)
1. Remova o ServerAlias (Apache2) incluído ou o nome_do_servidor (Nginx) de sua configuração do servidor, se necessário. (opcional)
1. Remova o registro A entre o domínio do CDN e o IP do servidor de origem.
