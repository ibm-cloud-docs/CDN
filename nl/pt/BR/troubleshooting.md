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

# Resolução de Problemas
{: #troubleshooting}

Neste documento, você localizará várias maneiras de solucionar problemas de seu {{site.data.keyword.cloud}} CDN. Se precisar entrar em contato com o suporte, certifique-se de fornecer o número de referência de seu CDN.

## Como sei que o meu CDN está funcionando?
{: #how-do-I-know-my-cdn-is-working}

Execute o seguinte comando `curl` substituindo `http://your.cdn.domain/uri` pelo respectivo caminho de arquivo em seu CDN:

`curl -I -H "Pragma: akamai-x-cache-on, akamai-x-cache-remote-on, akamai-x-check-cacheable, akamai-x-get-cache-key, akamai-x-get-extracted-values, akamai-x-get-nonces, akamai-x-get-ssl-client-session-id, akamai-x-get-true-cache-key, akamai-x-serial-no" http://your.cdn.domain/uri`

Se a saída do comando `curl` for semelhante ao formato de exemplo a seguir, o CDN estará funcionando conforme o esperado:

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

## Eu recebi um erro 503. Por quê?
{: #i-received-a-503-error-why}

O motivo mais comum que temos visto para o erro 503 é devido a um problema com um certificado na cadeia de certificados SSL.

Este é o erro que você pode ver: `503 Service Unavailable`.  

Em conjunto com o erro 503, também é possível ver uma mensagem semelhante à seguinte: `An error occurred while processing your request. Reference #30.3598c0ba.1521745157.87201fff` (o número de referência real pode variar). Nesse caso, o número de referência na sequência de erro é convertido em uma falha de handshake SSL.

Para corrigir o problema, assegure-se de que os certificados SSL do servidor de origem atendam aos critérios a seguir:
  * O certificado **deve** ser emitido por uma Autoridade de certificação confiável do Akamai. É possível visualizar a lista de certificados de confiança da Akamai [neste link ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")]](https://community.akamai.com/docs/DOC-4447-ssltls-certificate-chains-for-akamai-managed-certificates)
  * Ele **deve** corresponder ao *Cabeçalho do host* configurado no CDN
  * Ele **não** deve ser autoassinado
  * Ele **não** deve estar expirado

Se você tiver verificado a cadeia de certificados de sua Origem usando os critérios anteriores e ainda estiver encontrando o mesmo erro, consulte nossa página [Obtendo ajuda e suporte](/docs/infrastructure/CDN?topic=CDN-gettinghelp). Anote a sequência de erro de referência e inclua-a em qualquer comunicação conosco.

## Meu nome de host não é carregado no navegador quando o IBM Cloud Object Storage (COS) é a origem.
{: #my-hostname-doesnt-load-on-the-browser-when-ibm-cloud-object-storage-cos-is-the-origin}

Quando seu I{{site.data.keyword.cloud_notm}} CDN for configurado para usar o COS como o armazenamento de objeto, o acesso direto ao website não funcionará. Deve-se especificar o caminho completo da solicitação na barra de endereço do navegador (por exemplo, `www.example.com/index.html`). Esse comportamento é causado pela limitação do documento de índice no IBM COS.

## Não consigo me conectar por meio de um comando `curl` ou pelo navegador usando o Nome do host com HTTPS.
{: #i-cant-conect-through-a-curl-command-or-browser-using-the-hostname-with-https}

Se o seu CDN foi criado usando HTTPS com um certificado curinga, a conexão deverá ser feita usando o CNAME; por exemplo, `https://www.exampleCname.cdnedge.bluemix.net`. Isso inclui **todos** os CDNs criados com HTTPS antes de 18 de junho de 2018. A tentativa de conexão usando o Nome do host resultará em erro.

## Qual é o comportamento esperado ao carregar o CNAME ou o nome do host em seu navegador para os protocolos suportados?
{: #what-is-the-expected-behavior-when-loading-the-cname-or-hostname-on-your-browser-for-the-supported-protocols}

Esta tabela mostra o comportamento esperado para os protocolos suportados ao carregar o **nome do host** ou **CNAME** por meio do navegador da web.

<table>
<caption caption-side=“top”>Tabela de comportamentos esperados</caption>
<thead>
<tr>
<th rowspan=2 scope="col">URL do navegador</th>
<th rowspan=2 scope="col">CDN com protocolo HTTP apenas</th>
<th colspan=2 scope="col">CDN com protocolo HTTPS apenas</th>
<th colspan=2 scope="col">CDN com ambos os protocolos, HTTP e HTTPS</th>
</tr>
<tr>
<th scope="col"> Curinga </th>
<th scope="col"> SAN compartilhada </th>
<th scope="col"> Curinga </th>
<th scope="col"> SAN compartilhada </th>
</tr>
</thead>
<tbody>
<tr>
<td> `http://hostname` </td>
<td> Carregamento bem-sucedido </td>
<td> 301 Movido permanentemente </td>
<td> Carregamento bem-sucedido </td>
<td> 301 Movido permanentemente </td>
<td> Carregamento bem-sucedido </td>
</tr>
<tr>
<td> `https://hostname `</td>
<td> Acesso negado </td>
<td> Redireciona a página da web do IBM Cloud </td>
<td> Carregamento bem-sucedido </td>
<td> Redireciona a página da web do IBM Cloud </td>
<td> Carregamento bem-sucedido </td>
</tr>
<tr>
		<td> `http://cname` </td>
		<td> 301 Movido permanentemente </td>
		<td> Carregamento bem-sucedido </td>
		<td> 301 Movido permanentemente </td>
		<td> Carregamento bem-sucedido </td>
		<td> 301 Movido permanentemente </td>
</tr>
<tr>
		<td> `https://cname` </td>
		<td> Redireciona a página da web do IBM Cloud </td>
		<td> Carregamento bem-sucedido </td>
		<td> 301 Movido permanentemente </td>
		<td> Carregamento bem-sucedido </td>
		<td> Redireciona a página da web do IBM Cloud </td>
</tr>
</tbody>
</table>

**Mensagens de erro comuns:**

Uma mensagem `301 Moved permanently` indica mais provavelmente que você está tentando acessar um CDN com um protocolo `HTTPS` ou `HTTP_AND_HTTPS` usando o nome do host. Devido a uma limitação com o certificado curinga HTTPS, **deve-se** usar o CNAME para acessar seu CDN.

Com um protocolo **somente** HTTP, você receberá a mensagem `301 Moved permanently` se tentar acessar seu CDN usando o CNAME. Nesse caso, _somente_ será possível obter acesso a seu CDN usando o nome do host.

A mensagem `Access denied` é vista quando você está tentando acessar um CDN usando um protocolo incorreto. Assegure-se de que esteja usando `http` para CDNs criados com o protocolo HTTP ou `https` para CDNs criados com o protocolo HTTPS.

**Um possível erro de redirecionamento:**

O comportamento de uma URL redirecionando para a página da web do {{site.data.keyword.cloud_notm}} CDN é visto com mais frequência quando a URL está incorreta para o protocolo. Caso o seu CDN seja criado com um protocolo de HTTPS ou HTTPS_AND_HTTPS, deve-se usar o CNAME para acesso ao seu CDN. Por exemplo: `https://examplecname.cdnedge.bluemix.net` para mapeamentos de HTTPS ou `http://examplecname.cdnedge.bluemix.net` ou `https://examplecname.cdnedge.bluemix.net` para mapeamentos de HTTP_AND_HTTPS.

A URL redireciona para a página da web do {{site.data.keyword.cloud_notm}} CDN nesse caso porque o protocolo e o domínio estão incorretos para o protocolo do CDN. Para um CDN criado com HTTP como o _único_ protocolo, ele pode ser acessado _somente_ por meio do nome do host. Por exemplo, `http://example.com`.
