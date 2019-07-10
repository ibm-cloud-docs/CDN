---

copyright:
  years: 2018, 2019
lastupdated: "2019-04-04"

keywords: rules, naming, conventions, hostname, cname, RFC, 1033, 1035, bucket, path, origin, purge, alphanumeric, top-level domain, valid, string

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:download: .download}

# Regole e convenzioni di denominazione
{: #rules-and-naming-conventions}

## Quali sono le regole per l'Hostname (Nome host) CDN?
{: #what-are-the-rules-for-the-cdn-hostname}

La stringa di input `Hostname` **deve**:
  * consistere in caratteri alfanumerici
  * essere inferiore ai 254 caratteri
  * terminare con un nome dominio di livello superiore valido
  * **non** deve contenere più di 10 etichette
  * **non** deve terminare con `cdnedge.bluemix.net` (tale termine viene utilizzato per i CNAME ed è riservato)

Consulta RFC 1035, sezione 2.3.4 per ulteriori dettagli. 

Inoltre, consigliamo vivamente di utilizzare un FQDN (fully-qualified domain name) come tuo Hostname (Nome host) CDN. Scegli un nome in formato `www.example.com` invece di un nome dominio root (indicato anche come dominio apex di zona o naked) nel formato `example.com`. Dovrai creare un record CNAME per l'Hostname (Nome host) CDN che utilizzi e la DNS RFC 1033 richiede che il record di dominio root sia un record A, non un CNAME. Ulteriori chiarimenti sono forniti nella RFC 2181, sezione 10.1

## Quali sono le convenzioni di denominazione CNAME personalizzate?
{: #what-are-the-custom-cname-naming-conventions}

La stringa di input `CNAME` deve rispettare le seguenti regole:
  * **deve** essere univoca (non può essere utilizzata da altre IBM Cloud CDN)
  * inferiore ai 64 caratteri alfanumerici
  * i caratteri speciali `! @ # $ % ^ & *` **non** sono consentiti
  * i caratteri di sottolineatura (`_`) **non** sono consentiti
  * i punti (`.`) **non** sono consentiti
  * i trattini `-` sono consentiti ma CNAME non può iniziare o terminare con un trattino

## Quali sono le regole per i nomi bucket?
{: #what-are-the-rules-for-bucket-names}

I nomi bucket:
  * **devono** avere almeno 1 carattere
  * devono avere meno di 200 caratteri
  * possono contenere lettere (sono consentite sia lettere maiuscole che minuscole), numeri e trattini
  * **non** devono avere il formato di un indirizzo IP (ad esempio, 127.0.0.1)
  * **non** possono iniziare con un punto (.)
  * possono terminare con un punto (.)
  * è consentito solo un punto (.) tra le etichette (ad esempio, my..ibmcloud.bucket non è consentito).

Anche se lettere maiuscole e punti possono superare la convalida, consigliamo di utilizzare sempre nomi di bucket conformi a DNS.
{: note}

## Quali sono le regole per la stringa percorso per l'origine?
{: #what-are-the-rules-for-the-path-string-for-the-origin}

Il percorso è facoltativo quando crei la tua CDN. Tuttavia, se fornito, il percorso **deve**:
  * avere una lunghezza inferiore ai 1000 caratteri
  * iniziare con `/`

## Quali sono le regole per la stringa percorso per l'eliminazione?
{: #what-are-the-rules-for-the-path-string-for-purge}

Il percorso di eliminazione:
  * deve avere una lunghezza inferiore ai 1000 caratteri
  * deve iniziare con `/`
  * non può terminare con `/`
  * non può terminare con un punto (.)
  * non può contenere `*`

L'eliminazione è consentita solo per i singoli file. L'eliminazione a livello di directory non è supportata, al momento.
{; note}

## Per il comando **Aggiungi origine**, esistono delle regole da seguire per la stringa del percorso?
{: #for-the-add-origin-command-are-there-any-rules-to-follow-for-the-path-string}

Per **Aggiungi origine**, il percorso è **obbligatorio**. Anche se la tua CDN è stata creata con un percorso, il percorso per **Aggiungi origine** deve iniziare con il percorso della CDN come prefisso. Ad esempio, se il percorso della CDN è stato specificato come `/storage`, per richiamare **Aggiungi origine** con un percorso denominato `/examplePath`, il percorso fornito sarà `/storage/examplePath`.

## Quali sono le regole per fornire le estensioni file?
{: #what-are-the-rules-for-providing-file-extensions}

Quando si crea un'origine con Object Storage, le estensioni file devono essere separate da virgole. Ad esempio, `txt, jpg, xml` è un elenco valido. Qualsiasi estensione particolare non può essere più lunga di 10 caratteri.
