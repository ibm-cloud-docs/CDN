---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-25"

keywords: byte range request, byte-range request, origin server, range HTTP request, transfer-encoding

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}


# Trabalhando com Pedidos de Byte-range
{: #working-with-byte-range-requests}

Usando uma solicitação de intervalo de bytes, é possível recuperar conteúdo parcial de um servidor de
origem. Este documento pode ajudar a entender os códigos de status de resposta que são exibidos.

Quando uma **Solicitação de intervalo de bytes** é enviada usando o {{site.data.keyword.cloud}} CDN com a Akamai, o usuário pode receber um código de resposta `200 (OK)` para a primeira solicitação e um código de resposta `206` para todas as solicitações subsequentes, pois os servidores de borda da Akamai solicitam conteúdo da origem no formato compactado. Portanto, quando um
servidor de borda não tem um objeto em seu cache nem informações sobre o comprimento do conteúdo
do objeto, ele encaminha para a origem e solicita o objeto inteiro. Por sua vez, a origem entrega o objeto sem o
cabeçalho de comprimento de conteúdo para o Akamai e o usuário final recebe o objeto inteiro, mesmo que tenha
sido uma solicitação de intervalo de bytes. Portanto, o código de Status  ` 200 ` . Em
solicitações subsequentes, o servidor de borda tem o objeto em seu cache e entrega o código de status
`206`.

O cabeçalho **Solicitação de HTTP de intervalo** indica qual parte do conteúdo o
servidor deve retornar. Várias partes podem ser solicitadas com um cabeçalho de intervalo de uma vez e o servidor pode enviar de volta esses intervalos em uma resposta com múltiplas partes. Se o servidor enviar os intervalos de volta, ele responderá com um status de 206 (conteúdo parcial).

Uma maneira de assegurar uma resposta 206, mesmo para a primeira solicitação de intervalo de bytes, é desativar
`Transfer-Encoding: chunked` em seu servidor de origem.
