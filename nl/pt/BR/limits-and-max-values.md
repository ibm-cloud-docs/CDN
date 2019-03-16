---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-19"

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

O valor máximo para Tempo de Vida é 2.147.483.647 segundos, o que equivale a cerca de 68 anos! O valor mínimo é 0 segundos.

## Há um limite no número de entradas de Origem e de TTL?

Sim, o limite combinado é 75 entradas por CDN.

## Qual é o maior tamanho do arquivo que pode ser entregue por meio do CDN do Akamai?

As tentativas de recuperar ou entregar arquivos maiores que 1,8 GB receberão uma resposta `403 Access Forbidden` para a configuração de desempenho padrão. Se a otimização de arquivos grandes estiver ativada, downloads de arquivos até 320 GB serão possíveis.
