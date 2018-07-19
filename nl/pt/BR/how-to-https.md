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

# Concluindo Validação de Controle de Domínio para HTTPS

## Etapas Iniciais para Validação de Controle de

**Etapa 1:**

Depois de ter pedido seu CDN com um Certificado SAN DV, o processo de solicitação de certificado será iniciado. Durante esse processo, o IBM Cloud CDN solicita um certificado do Akamai. Quando um certificado se torna disponível, o Akamai emite uma solicitação para a Autoridade de Certificação (CA).

  * Durante esse tempo, o status do CDN será mostrado como **Solicitando certificado**.

    ![Requesting Certificate Status](images/requesting-cert.png)

**Etapa 2:**

Quando a CA recebe a solicitação, ela emite um Desafio de Validação de Domínio.

  * Quando isso acontece, o status de seu CDN muda para **Validação de domínio necessária**.

    ![Domain Validation Needed](images/domain-validation-needed.png)

**Etapa 3:**

Clique no nome do CDN que precisa ser validado. A página Visão geral é aberta, na qual é possível ver o status geral de seu CDN. Na parte superior da página há um alerta lembrando-o de que a validação de domínio é necessária. Selecione o botão **Visualizar validação de domínio** para abrir uma janela que mostra as informações de desafio necessárias para concluir o processo de validação.

   ![Domain Validation Needed](images/view-domain-validation.png)

**Etapa 4:** depois de ter concluído uma das etapas de validação da seção sobre como resolver um Desafio de Validação de Domínio, seu CDN mudará para o status **Implementando o certificado**. Durante esse tempo, o Akamai distribuirá seu certificado validado para seus servidores de borda. A implementação de um certificado pode levar de 2 a 4 horas.

  * Quando esse processo é concluído, todos os domínios, independentemente do método de validação usado, mudam para um estado **Configuração do CNAME**.

Informações adicionais referentes à conclusão da configuração do CNAME, bem como à supervisão de seu CDN, podem ser localizadas na página [Chegando à execução](basic-functions.html#get-to-running).


## Desafio de Validação de Domínio

O Desafio de Validação de Domínio prova que você é o proprietário do domínio. Abaixo há três maneiras de resolver o Desafio de Validação de Domínio.

**NOTA**: se você não responder à solicitação de Validação de Domínio dentro de 48 horas, sua solicitação expirará e será necessário iniciar o processo de pedido novamente.

### CNAME

Se o seu registro do CNAME foi incluído em seu provedor DNS antes de solicitar seu CDN, nenhuma outra ação será necessária. A Validação de Domínio é manipulada automaticamente pelo IBM Cloud, pelo Akamai e pela CA. A validação pode levar de 2 a 4 horas.

  * Se você ainda não configurou seu CNAME com seu provedor DNS, será necessário fazer isso neste momento. A maioria dos provedores DNS pode fornecer instruções sobre como configurar ou mudar o CNAME.

   ![Domain Validation CNAME](images/domain-validation-cname.png)

**NOTA**: esse método será recomendado **APENAS** se o CDN **não** estiver entregando o tráfego em tempo real. Se seu domínio está entregando o tráfego em tempo real, recomenda-se usar o método Padrão para validar seu domínio.

---
### Padrão

Se você escolher o método Padrão para a Validação de Domínio, a janela Validação de Domínio mostrará uma **URL de desafio** e uma **Resposta de segurança**. Para concluir o processo de Validação de Domínio, deve-se incluir a **Resposta de segurança** fornecida em seu servidor de origem. Isso permite que a CA recupere a **Resposta de segurança** de seu servidor de origem usando a URL especificada na **URL de desafio**. Depois que o servidor de origem estiver configurado corretamente, a Validação de Domínio poderá levar de 2 a 4 horas.

   ![Domain Validation Challenge Standard](images/domain-validation-standard.png)

Para concluir com sucesso a Validação de Domínio por meio do método Padrão, é necessário configurar seu servidor de origem de uma maneira específica. Os procedimentos de exemplo para os servidores Apache e Nginx estão resumidos abaixo.

** Situação de exemplo **
* Servidor de origem:  ` www.example.com `
* Domínio CDN:  ` cdn.example.com `
* URL do Challenge:  ` http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject `
* Resposta de Desafio:  ` examplechallenge `

#### Configuração do Apache

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

* Para verificar se esse método funciona por meio de `curl`, execute esse comando para a URL de Desafio.
    ```
    curl -v http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```
* Para verificar se esse método funciona por meio de um navegador, tente acessar a URL do Desafio por meio de seu navegador.

Em qualquer um dos casos, é necessário ser capaz de recuperar a cópia do objeto de arquivo do Desafio de Validação de Domínio, que é armazenada em seu servidor de origem.

#### Limpeza para o método Standard

Após o seu CDN ter atingido o status **Implementação de certificado**:
1. Remova o arquivo  ` examplechallenge-fileobject ` . (opcional)
1. Remova o ServerAlias (Apache2) incluído ou o nome_do_servidor (Nginx) de sua configuração do servidor, se necessário. (opcional)
1. Remova o registro A entre o domínio do CDN e o IP do servidor de origem.

---
### Redirecionar

Clicar na guia **Redirecionar** exibe todas as informações necessárias para resolver a Validação de Domínio por meio de redirecionamento. Essas informações permitem que a CA recupere uma cópia da **Resposta de segurança** do Akamai por meio do servidor de origem. Depois que o servidor estiver configurado corretamente, a Validação de Domínio poderá levar de 2 a 4 horas.

   ![Redirecionamento de Desafio de Validação de Domínio](images/domain-validation-redirect.png)

Para concluir com sucesso a Validação de Domínio por meio do método Redirecionamento, poderá ser necessário configurar o servidor da web de uma maneira específica. Os procedimentos de exemplo para os servidores Apache e Nginx estão resumidos abaixo.

**Situação de exemplo**
* Servidor de origem:  ` www.example.com `
* Domínio CDN:  ` cdn.example.com `
* URL do Challenge:  ` http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject `
* Redirecionamento de URL:  ` http://dcv.akamai.com/.well-known/acme-challenge/examplechallenge-fileobject `

#### Configuração de Redirecionamento do Apache

  * **Etapa 1:** efetue login na máquina que está executando o servidor Apache2.

  * **Etapa 2:** abra seu arquivo de configuração do servidor Apache2. Os diretórios `/etc/apache2/apache2.conf` e `/etc/apache2/sites-enabled/` são os locais padrão para os arquivos de configuração.

  * **Etapa 3:** inclua uma instrução de redirecionamento no local apropriado dentro do arquivo de configuração. Se necessário, inclua seu domínio CDN como um **ServerAlias** adicional no host virtual de sua origem.
    ```
    Redirecionar http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject http://dcv.akamai.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```

  * **Etapa 4:** reinicie o servidor Apache2 com um tempo de inatividade mínimo usando o comando a seguir:

    ```
    apachectl -k graceful
    ```

  * **Etapa 5:** crie um registro A em seu DNS entre o domínio do CDN e o endereço
IP do servidor de origem.

#### Configuração de redirecionamento do Nginx

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

  * **Etapa 5:** crie um registro A em seu DNS entre o domínio do CDN e o IP do servidor de origem.

#### Verifique se o redirecionamento está ocorrendo

A conclusão dessas etapas redirecionará apenas o tráfego para a URL de Desafio específica para o Redirecionamento de URL. É possível verificar se o redirecionamento funcionou apropriadamente por meio do `curl` ou do navegador.

* Para verificar se o redirecionamento funciona por meio do comando `curl`, execute esse comando para a URL de Desafio.

    ```
    curl -vL http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```

* Para verificar se esse redirecionamento funciona pelo navegador, tente acessar a URL de Desafio por meio de seu navegador.

Em qualquer um dos casos, é necessário ser capaz de recuperar a cópia do objeto de arquivo de Desafio de Validação de Domínio do Akamai no domínio dcv.akamai.com, para o qual a solicitação original foi redirecionada.

#### Limpeza para o método Redirect

Após o seu CDN ter atingido o status **Implementação de certificado**:
1. Remova as instruções ou os blocos de redirecionamento do arquivo de configuração. (opcional)
1. Remova o ServerAlias (Apache2) incluído ou o nome_do_servidor (Nginx) de sua configuração do servidor, se necessário. (opcional)
1. Remova o registro A entre o domínio do CDN e o IP do servidor de origem.
