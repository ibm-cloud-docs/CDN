---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-30"

keywords: limitations, Akamai, multiple files, purge, hotlink, limit

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}

# Limitações conhecidas
{: #known-limitations}

As limitações a seguir se aplicam ao serviço CDN para o provedor da Akamai:
* A limpeza de conteúdo no nível de diretório ou de múltiplos arquivos atualmente não é suportada.
* Há um limite de 75 como o número total de Origens por CDN.
* O HTTPS com o certificado SAN DV está disponível apenas para novos CDNs.
* A Proteção do hotlink não suporta os termos de correspondência da URL `refererValues` nos quais o conjunto de caracteres que está na frente do primeiro caractere `.` contém o agrupamento de caracteres `://`, por exemplo, `http://www.example.com` ou `www.example.com http://www.example.com`
* O HTTPS que usa certificados curingas não é suportado neste momento.
* A funcionalidade STOP do CDN não é suportada para domínios de certificado curinga.

A funcionalidade STOP do CDN é destinada para janelas de manutenção que não excedem sete dias. Após sete dias, o CDN deverá ser iniciado ou será desativado e o tráfego para o CNAME do CDN não será redirecionado para o servidor de origem.
{: important}
