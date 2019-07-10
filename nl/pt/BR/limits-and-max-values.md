---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-31"

keywords: limits, maximum, values, time to live, entries, large file, size, optimization, downloads, years

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Limites e Valores Máximos
{: #limits-and-maximum-values}

## Há um valor máximo para o Tempo de vida? Um mínimo?
{: #is-there-a-maximum-value-for-time-to-live}

O valor máximo para Tempo de Vida é 2.147.483.647 segundos, o que equivale a cerca de 68 anos! O valor mínimo é 0 segundos.

## Há um limite no número de entradas de Origem e de TTL?
{: #is-there-a-limit-on-the-number-of-origin-and-ttl-entries}

Sim, o limite combinado é 75 entradas por CDN.

## Qual é o maior tamanho de arquivo que pode ser entregue por meio do CDN do Akamai?
{: #what-is-the-largest-file-size-that-can-be-delivered-through-akamai-cdn}

As tentativas de recuperar ou entregar arquivos maiores que 1,8 GB receberão uma resposta `403 Access Forbidden` para a configuração de desempenho padrão. Se a otimização de arquivos grandes estiver ativada, downloads de arquivos até 320 GB serão possíveis.

## Qual é o tamanho máximo para os cabeçalhos de resposta de origem?
{: #what-is-the-maximum-size-for-the-orignin-response-headers}

O tamanho máximo do cabeçalho de resposta é 16 KB. No caso de determinadas origens tentarem configurar cookies que são muito grandes para o cliente retornar em solicitações subsequentes, o Akamai configura esse limite nos servidores de borda. Se os cabeçalhos de resposta forem maiores que 16 KB, a solicitação obterá uma resposta `502 Bad Gateway`.

## Qual é o tamanho máximo para os cabeçalhos de solicitação do cliente?
{: #what-is-the-maximum-size-for-the-client-request-headers}

O tamanho máximo de cabeçalhos da solicitação é 32 KB. Se os cabeçalhos da solicitação forem maiores que 32 KB, o servidor de borda do Akamai retornará uma resposta `400 Bad Request`.
