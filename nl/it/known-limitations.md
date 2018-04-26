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

# Limitazioni note

Le seguenti limitazioni si applicano al nuovo servizio CDN per il fornitore Akamai:
* HTTPS è attualmente supportato solo tramite certificato con carattere jolly.
* L'eliminazione di più file o di contenuto a livello di directory non è attualmente supportata.
* Limite di 10 CDN attualmente consentito per un determinato account {{site.data.keyword.BluSoftlayer_notm}}.
* Il limite sul numero totale di voci di origine e TTL è 75 per ogni CDN.
* L'opzione Utilizza contenuto obsoleto sarà sempre **Attiva**, anche se il CDN viene creato con essa **Non attiva** 
* Se il CDN è stato creato con il **Server** e la **Porta HTTP**, l'origine può essere aggiunta soltanto con l'opzione **Server**.
