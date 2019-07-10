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

# Limitazioni note
{: #known-limitations}

Le seguenti limitazioni si applicano al servizio CDN per il fornitore Akamai:
* L'eliminazione di più file o di contenuto a livello di directory non è attualmente supportata.
* C'è un limite di 75 come numero totale di origini per ogni CDN.
* HTTPS con certificato SAN DV è disponibile solo per le nuove CDN.
* Hotlink Protection non supporta i termini di corrispondenza di URL `refererValues` in cui la serie di caratteri davanti al primo carattere `.` contiene il raggruppamento di caratteri `://`, ad esempio `http://www.example.com` o `www.example.com http://www.example.com`
* HTTPS che utilizza i certificati jolly non è supportato in questo momento.
* La funzionalità STOP CDN non è supportata per i domini del certificato jolly.

La funzionalità STOP CDN è destinata alle finestre di manutenzione che non superano i 7 giorni. Dopo 7 giorni, la CDN deve essere avviata o sarà disabilitata e il traffico al CNAME CDN non sarà reindirizzato al server di origine.
{: important}
