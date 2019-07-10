---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-04"

keywords: cache control, cache-control, cache duration, max-age,  edge server, edge-level, respect header, HTTP client

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Usando o Controle de cache para controlar a duração de cache de um cliente HTTP
{: #using-cache-control-to-control-an-http-client-s-cache-duration}

Ao usar um CDN, dois níveis de armazenamento em cache estão disponíveis:

  * O **armazenamento em cache na borda** ocorre quando um servidor de borda CDN armazena em cache uma parte do conteúdo da origem.
  * O **armazenamento em cache de recebimento de dados** da rede de borda de servidores ocorre quando um cliente de usuário final ou um cliente HTTP, como um navegador solicitante, armazena em cache uma parte do conteúdo de um servidor de borda.

O método escolhido para controlar por quanto tempo o conteúdo é armazenado em cache no solicitante, tal como um navegador, depende dos fatores a seguir:

  * Se a [configuração Respeitar cabeçalho](/docs/infrastructure/CDN?topic=CDN-manage-your-cdn#updating-cdn-configuration-details) está ON ou OFF. Por padrão, ela está configurada como ON.
  * Se o servidor de origem fornece um valor `max-age` no cabeçalho Controle de cache para uma parte específica do conteúdo. 

Independentemente de como esses fatores mudam, sua origem deve fornecer um cabeçalho Controle de cache para o conteúdo desejado para a borda, se você deseja que os servidores de borda enviem respostas HTTP com o cabeçalho Controle de cache desse conteúdo.

Essencialmente, os cabeçalhos Controle de cache enviados de um recebimento de dados do servidor de borda pedem ao solicitante para armazenar em cache o conteúdo associado de acordo com as diretivas de armazenamento em cache ou com os valores especificados pelo servidor de borda.

## Respeitar cabeçalho: desativado
{: #respect-header-off}

Se a sua origem fornecer um cabeçalho Controle de cache com uma diretiva `max-age` e o valor para uma parte específica de conteúdo, a duração do cache da parte específica do conteúdo armazenado em cache na borda ainda será derivada das configurações de TTL do CDN. Além disso, o servidor de borda responde ao recebimento de dados do solicitante com um valor `max-age` do Controle de cache igual ao menor de:
  * O valor `max-age` do Controle de cache da origem.
  * O tempo restante até o conteúdo ficar antigo na borda.

No entanto, se a sua origem não fornecer um cabeçalho Controle de cache para o servidor de borda, os servidores de borda não fornecerão um cabeçalho Controle de cache para o solicitante. A duração do edge cache para o seu conteúdo ainda é derivada das Configurações de TTL do CDN.

## Respeitar cabeçalho: ativado
{: #respect-header-on}

Se a sua origem fornecer um cabeçalho Controle de cache com `max-age` para uma parte específica de conteúdo, o valor `max-age` do Controle de cache da origem se tornará a duração do cache para essa parte específica de conteúdo armazenado em cache na borda, substituindo qualquer configuração de TTL aplicável dessa parte de conteúdo. Além disso, a borda responde ao solicitante com um valor `max-age` de Controle de cache igual ao tempo restante até que o conteúdo fique antigo no servidor de borda.

No entanto, se a sua origem não fornecer um cabeçalho Controle de cache para o servidor de borda, o servidor de borda não fornecerá um cabeçalho Controle de cache para o solicitante. A duração do edge cache para o seu conteúdo ainda é derivada das Configurações de TTL do CDN.

## Resumo
{: #summary}

|Respeitar cabeçalho|A origem fornece Controle de cache|Duração do cache do conteúdo específico no servidor de borda|O Servidor de borda fornece Controle de cache|
|---|---|---|---|
|Ativado|Sim, a origem especifica um `max-age`|Duração do edge cache substituída pelo valor `max-age` da Origem|Sim, a borda também fornece um `max-age` com um valor que é o tempo (substituído) antes que a borda precise atualizar o conteúdo da origem|
|Ativado|Sim, mas a origem não especifica um `max-age`|Duração do edge cache com base na configuração de TTL do CDN|Sim, a borda também fornece um `max-age` com um valor que é o tempo antes que a borda precise atualizar o conteúdo da origem|
|Ativado|Não|Duração do edge cache com base na configuração de TTL do CDN|Não|
|Desativado|Sim, a origem especifica um `max-age`|Duração do edge cache com base na configuração de TTL do CDN|Sim, a borda também fornece um valor `max-age` que é o menor valor `max-age` da origem e o tempo antes que a borda precise atualizar o conteúdo da origem|
|Desativado|Sim, mas a origem não especifica um `max-age`|Duração do edge cache com base na configuração de TTL do CDN|Sim, a borda também fornece um `max-age` com um valor que é o tempo antes que a borda precise atualizar o conteúdo da origem|
|Desativado|Não|Duração do edge cache com base na configuração de TTL do CDN|Não|

## Mais informações sobre o controle de cache
{: #more-information-on-cache-control}

* Como [gerenciar seu CDN](/docs/infrastructure/CDN?topic=CDN-manage-your-cdn)
* Controle de cache conforme definido na seção 14.9 da [RFC 2616 ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ietf.org/rfc/rfc2616.txt)
