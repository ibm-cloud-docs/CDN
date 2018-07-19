---

copyright:
  years: 2017, 2018
lastupdated: "2018-06-19"

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
* A limpeza de conteúdo no nível de diretório ou de múltiplos arquivos não é suportada atualmente.
* Limite de 10 CDNs permitido atualmente para uma determinada conta do {{site.data.keyword.BluSoftlayer_notm}}.
* O limite no número total de entradas Origens e TTL é 75 por CDN.
* A funcionalidade Serve Stale Content Option sempre será **On**.
* O tipo de certificado HTTPS não pode ser mudado uma vez que um mapeamento é criado, por exemplo, de
Curinga para SAN DV.
* O HTTPS com o certificado SAN DV está disponível apenas para novos CDNs.
* Um CDN criado com um certificado SAN DV não pode ser excluído, a menos que ele esteja em um estado
RUNNING, CNAME_Configuration, CREATE_ERROR ou DELETE_ERROR.
