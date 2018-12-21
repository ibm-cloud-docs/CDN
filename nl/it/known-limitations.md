---

copyright:
  years: 2017, 2018
lastupdated: "2018-11-02"

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
* Limite di 75 per il numero totale di origini per ogni CDN.
* HTTPS con certificato SAN DV è disponibile solo per le nuove CDN.
* Hotlink Protection non supporta i termini di corrispondenza di URL `refererValues` dove la serie di caratteri davanti al primo carattere `.` contiene `://`, ad esempio `http://www.example.com` o `www.example.com http://www.example.com`
