---

copyright:
  years: 2017
lastupdated: "2017-09-10"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Limitações conhecidas

As limitações a seguir aplicam-se ao novo serviço CDN do fornecedor Akamai:
* HTTPS é suportado atualmente apenas por meio do certificado curinga.
* A limpeza de conteúdo no nível de diretório ou de múltiplos arquivos não é suportada atualmente.
* Limite de 10 CDNs permitido atualmente para uma determinada conta do {{site.data.keyword.BluSoftlayer_notm}}.
* O limite no número total de entradas Origens e TTL é 75 por CDN.
* A funcionalidade Entregar Opção de Conteúdo Obsoleto sempre estará **Ligado**, mesmo que o CDN seja criado com ela como **Desligado** 
* Se o CDN foi criado com **Servidor** e **Porta HTTP**, a Origem poderá ser incluída apenas com a opção **Servidor**.
