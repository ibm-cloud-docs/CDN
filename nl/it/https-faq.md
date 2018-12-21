---

copyright:
  years: 2018
lastupdated: "2018-11-13"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{: faq: data-hd-content-type=‘faq’}

# Domande frequenti per HTTPS 

## Per la mia CDN, qual è la differenza tra HTTPS con certificato jolly e HTTPS con certificato SAN?
{: faq}

Con il certificato jolly, tutti i clienti utilizzano lo stesso certificato distribuito sulle reti CDN del fornitore. Per accedere al servizio, è necessario utilizzare il CNAME, compreso il suffisso IBM `.cdnedge.bluemix.net`. Ad esempio, `https://www.example-cname.cdnedge.bluemix.net`

Nel caso del certificato SAN, più domini cliente condividono un singolo certificato SAN aggiungendo i loro nomi di dominio nelle voci SAN. È quindi possibile accedere al servizio utilizzando il nome host, ad esempio `https://www.example.com`

## Come viene realizzata la convalida del dominio con il reindirizzamento?
{: faq}

Dipende dal tuo server. La procedura per completare la convalida del dominio per i server Apache e Nginx può essere trovata nella pagina [Completamento della convalida del controllo del dominio per HTTPS](how-to-https.html#redirect).

## Quanto dura la convalida del dominio?
{: faq}

La convalida del dominio richiede normalmente da 2 a 4 ore, ma varia a seconda del metodo scelto per la convalida. La convalida del dominio con la convalida del CNAME è la più veloce, in genere impiega meno di un'ora. La convalida del dominio con i metodi Standard e Redirect in genere richiede ~4 ore, una volta risolta la verifica.

## Quanto ci vuole per creare e abilitare HTTPS per la mia CDN con un certificato DV SAN?
{: faq}

Una normale richiesta per abilitare HTTPS richiede in media 3 - 9 ore, dalla richiesta iniziale all'esecuzione.

## Quanto tempo ci vuole per eliminare una CDN con un certificato DV SAN?
{: faq}

L'eliminazione della tua CDN richiede che il tuo dominio venga rimosso dal certificato su tutti i server edge. Per completare questo processo, possono essere necessarie fino a 8 ore.

## C'è qualche costo aggiuntivo associato all'utilizzo di un certificato DV SAN per la mia CDN?
{: faq}

No. Le configurazioni dei certificati SAN DV vengono fornite senza costi aggiuntivi rispetto a HTTP o HTTPS con certificato jolly.

## È possibile aggiornare la mia CDN creata mediante HTTPS con jolly per utilizzare un certificato DV SAN?
{: faq}

No, al momento non è possibile modificare l'associazione di wildcard nel certificato SAN.

## Cos'è un'Autorità di certificazione?
{: faq}

Un'Autorità di certificazione (CA) è un'entità che emette i certificati digitali.

## Quale CA viene utilizzata dal servizio CDN IBM Cloud per l'emissione di un certificato SAN DV?
{: faq}

Il servizio CDN IBM Cloud utilizza l'Autorità di certificazione LetsEncrypt.

## Quali certificati SSL sono supportati per CDN IBM Cloud?
{: faq}

I certificati SSL supportati sono il certificato jolly e il certificato SAN (Subject Alternate Name) DV (Domain Validation). Il certificato SAN viene condiviso tra più clienti. La CDN IBM Cloud non supporta il caricamento di certificati personalizzati.

## Ho ricevuto un'e-mail che mi chiedeva di affrontare una verifica di convalida del dominio correlata alla mia CDN. Cosa faccio adesso?
{: faq}

La convalida del dominio può essere affrontata in uno dei tre seguenti modi: CNAME, Standard o Direct.

Per dettagli su come affrontare uno di questi modi, fai riferimento al documento [Completamento della convalida del controllo del dominio per HTTPS](how-to-https.html#how-to-https.html#initial-steps-to-domain-control-validation).

## Cosa succederà se non affronto la verifica per la convalida del dominio della mia CDN?
{: faq}

Se lo stato dell'associazione rimane DOMAIN_VALIDATION_PENDING per più di 48 ore, la creazione dell'associazione verrà annullata e lo stato dell'associazione sarà CREATE_ERROR. In questo stato, puoi scegliere di riprovare la creazione o eliminare l'associazione.

## Il certificato jolly deve convalidare un dominio per la mia CDN?
{: faq}

No, ma puoi utilizzare solo il CNAME per richiamare il contenuto dalla tua origine. `https://www.example-cname.cdnedge.bluemix.net`

## Se uso il tipo di certificato SAN per la mia CDN, posso ancora utilizzare il CNAME per accedere al mio servizio?
{: faq}

No. Per il certificato SAN, puoi solo utilizzare il dominio personalizzato per accedere al contenuto dall'origine. `https://www.example.com`

## Tutti i miei domini CDN IBM Cloud vengono aggiunti in un unico certificato?
{: faq}

Non necessariamente. La selezione del certificato è gestita da Akamai, al fine di garantire che i certificati siano nello stato più efficiente. I domini vengono aggiunti in diversi certificati in modo ben proporzionato, quindi non possiamo garantire che tutti i tuoi domini si troveranno sullo stesso certificato.

## Perché vedo "wildcard" quando eseguo un `dig` o quando provo ad accedere al contenuto con la CDN in stato `Requesting Certificate`, `Domain Validation pending` o `Deploying Certificate`?
{: faq}

Durante il processo di richiesta del certificato SAN DV, la catena di record DNS per la tua CDN è concatenata temporaneamente a un certificato jolly. Fino a quando il processo non viene completato, il contenuto viene temporaneamente fornito tramite questo certificato jolly. Una volta completato il processo di richiesta, la catena di record DNS viene aggiornata per il concatenamento al certificato SAN DV della CDN.
