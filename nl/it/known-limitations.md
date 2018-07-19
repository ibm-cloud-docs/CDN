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

# Limitazioni note

Le seguenti limitazioni si applicano al nuovo servizio CDN per il fornitore Akamai:
* L'eliminazione di più file o di contenuto a livello di directory non è attualmente supportata.
* Limite di 10 CDN attualmente consentito per un determinato account {{site.data.keyword.BluSoftlayer_notm}}.
* Il limite sul numero totale di voci di origine e TTL è 75 per ogni CDN.
* L'opzione Utilizza contenuto obsoleto sarà sempre **Attiva**.
* Il tipo di certificato HTTPS non può essere modificato una volta creata un'associazione, ad esempio da wildcard a SAN DV.
* HTTPS con certificato SAN DV è disponibile solo per le nuove CDN.
* Una CDN creata con un certificato SAN DV non può essere eliminata a meno che non si trovi in uno stato RUNNING, CNAME_Configuration, CREATE_ERROR o DELETE_ERROR.
