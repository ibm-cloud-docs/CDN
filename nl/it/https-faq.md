---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-05"

keywords: faq, https, wildcard, certificate, san certificate, domain validation, redirect, domains, challenge

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:faq: data-hd-content-type='faq'}

# Domande frequenti per HTTPS
{: #faq-for-https}

## Per la mia CDN, qual è la differenza tra HTTPS con certificato jolly e HTTPS con certificato SAN?
{: #for-my-cdn-what-is-the-difference-between-https-with-wildcard-certificate-and-https-with-san-certificate}
{:faq}

Con i certificati jolly, tutti i clienti utilizzano lo stesso certificato distribuito sulle reti CDN del fornitore. Per l'accesso al servizio, è necessario utilizzare il CNAME, compreso il suffisso IBM `.cdnedge.bluemix.net`. Ad esempio, `https://www.example-cname.cdnedge.bluemix.net`

Nel caso del certificato SAN, più domini cliente condividono un singolo certificato SAN aggiungendo i loro nomi di dominio nelle voci SAN. È quindi possibile accedere al servizio utilizzando il nome host, ad esempio `https://www.example.com`

## Come viene realizzata la convalida del dominio con il reindirizzamento?
{: #how-is-domain-validation-with-redirect-accomplished}
{:faq}

Dipende dal tuo server. La procedura per completare la convalida del dominio per i server Apache e Nginx può essere trovata nella pagina [Completamento della convalida del controllo del dominio per HTTPS](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#redirect).

## Quanto dura la convalida del dominio?
{: #how-long-does-domain-validation-take}
{:faq}

La convalida del dominio richiede normalmente da 2 a 4 ore, ma varia a seconda del metodo scelto per la convalida. La convalida del dominio con la convalida del CNAME è la più veloce, in genere impiega meno di un'ora. La convalida del dominio con i metodi Standard e Redirect in genere richiede ~4 ore, una volta risolta la verifica.

## Quanto ci vuole per creare e abilitare HTTPS per la mia CDN con un certificato SAN DV?
{: #how-long-does-it-take-to-create-and-enable-https-for-my-cdn-with-a-dv-san-certificate}
{:faq}

Una normale richiesta per abilitare HTTPS richiede in media 3 - 9 ore, dalla richiesta iniziale all'esecuzione.

## Quanto tempo ci vuole per eliminare una CDN con un certificato SAN DV?
{: #how-long-does-it-take-to-delete-a-cdn-with-a-dv-san-certificate}
{:faq}

L'eliminazione della tua CDN richiede che il tuo dominio venga rimosso dal certificato su tutti i server edge. Per completare questo processo, possono essere necessarie fino a 8 ore.

## C'è qualche costo aggiuntivo associato all'utilizzo di un certificato SAN DV per la mia CDN?
{: #is-there-any-additional-cost-associated-with-using-a-dv-san-certificate-for-my-cdn}
{:faq}

No. Le configurazioni dei certificati SAN DV vengono fornite senza costi aggiuntivi rispetto a HTTP o HTTPS con certificato jolly.

## È possibile aggiornare la mia CDN creata mediante HTTPS con jolly per utilizzare un certificato SAN DV?
{: #can-my-cdn-created-using-https-with-wildcard-be-updated-to-use-a-dv-san-certificate}
{:faq}

No, non è possibile modificare l'associazione di wildcard nel certificato SAN.

## Cos'è un'Autorità di certificazione?
{: #what-is-a-certificate-authority}
{:faq}

Un'Autorità di certificazione (CA) è un'entità che emette i certificati digitali.

## Quale CA viene utilizzata dal servizio {{site.data.keyword.cloud}} CDN per l'emissione di un certificato SAN DV?
{: #which-ca-does-ibm-cloud-cdn-service-use-for-issuing-a-dv-san-certificate}
{:faq}

Il servizio IBM Cloud CDN utilizza l'Autorità di certificazione LetsEncrypt.

## Quali certificati SSL sono supportati per IBM Cloud CDN?
{: #what-ssl-certificates-are-supported-for-ibm-cloud-cdn}
{:faq}

I certificati SSL supportati sono il certificato jolly e il certificato SAN (Subject Alternate Name) DV (Domain Validation). Il certificato SAN viene condiviso tra più clienti. IBM Cloud CDN non supporta il caricamento di certificati personalizzati.

## Ho ricevuto un'e-mail che mi chiedeva di affrontare una verifica di convalida del dominio correlata alla mia CDN. Cosa faccio adesso?
{: #i-received-an-email-asking-me-to-address-a-domain-validation-challenge-related-to-my-cdn}
{:faq}

La convalida del dominio può essere affrontata in uno dei tre seguenti modi: CNAME, Standard o Direct.

Per dettagli su come affrontare uno di questi modi, fai riferimento al documento [Completamento della convalida del controllo del dominio per HTTPS](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#initial-steps-to-domain-control-validation).

## Cosa succederà se non affronto la verifica per la convalida del dominio della mia CDN?
{: #what-will-happen-if-i-dont-address-the-challenge-for-domain-validation-of-my-cdn}
{:faq}

Se lo stato dell'associazione rimane DOMAIN_VALIDATION_PENDING per più di 48 ore, la creazione dell'associazione verrà annullata e lo stato dell'associazione sarà CREATE_ERROR. In questo stato, puoi scegliere di riprovare la creazione o eliminare l'associazione.

## Il certificato jolly deve convalidare un dominio per la mia CDN?
{: #does-a-wildcard-certificate-need-to-validate-a-domain-for-my-cdn}
{:faq}

No, ma puoi utilizzare solo il CNAME per richiamare il contenuto dalla tua origine. `https://www.example-cname.cdnedge.bluemix.net`

## Ho ricevuto un'email che indica che i miei domini non puntano al CNAME CDN IBM. Cosa faccio adesso?
{: #i-received-an-email-indicating-that-my-domain-is-not-pointed-to-IBM-CDN-CNAME}
{:faq}

Questa email significa che la tua CDN non viene utilizzata. Per utilizzare la CDN per rendere attivi i domini nei certificati, devi configurare i record DNS CNAME elencati nel tuo sistema provider DNS.
Se completi questa azione entro 7 giorni, sia il traffico HTTP che HTTPS sarà ripristinato per la tua CDN ed essa tornerà nello stato RUNNING. Se la CDN non viene ancora utilizzata dopo 7 giorni, dobbiamo disabilitare HTTPS in modo permanente per il tuo dominio CDN, per evitare che il tuo dominio non utilizzato blocchi l'aggiunta delle nuove richieste di dominio CDN al certificato SAN condiviso. Il traffico HTTP che accede tramite la CDN può ancora essere ripristinato in un secondo momento aggiungendo un record CNAME al tuo dominio.
Per dettagli su come affrontare questa situazione, fai riferimento al documento [Completamento della convalida del controllo del dominio per HTTPS](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#cname).

## Se uso il tipo di certificato SAN per la mia CDN, posso ancora utilizzare il CNAME per accedere al mio servizio?
{: #if-i-use-a-san-certificate-type-for-my-cdn-can-i-still-use-the-cname-for-access-to-my-service}
{:faq}

No. Per il certificato SAN, puoi solo utilizzare il dominio personalizzato per accedere al contenuto dall'origine.

## Tutti i miei domini IBM Cloud CDN vengono aggiunti in un unico certificato?
{: #are-all-of-my-ibm-cloud-cdn-domains-added-into-one-certificate}
{:faq}

Non necessariamente. La selezione del certificato è gestita da Akamai, al fine di garantire che i certificati siano nello stato più efficiente. I domini vengono aggiunti in diversi certificati in modo ben proporzionato, quindi non possiamo garantire che tutti i tuoi domini si troveranno sullo stesso certificato.

## Perché vedo "wildcard" quando eseguo un `dig` o quando provo ad accedere al contenuto con la CDN in stato `Requesting Certificate`, `Domain Validation pending` o `Deploying Certificate`?
{: #why-do-i-see-wildcard}
{:faq}

Durante il processo di richiesta del certificato SAN DV, la catena di record DNS per la tua CDN è concatenata temporaneamente a un certificato jolly. Fino a quando il processo non viene completato, il contenuto viene temporaneamente fornito tramite questo certificato jolly. Una volta completato il processo di richiesta, la catena di record DNS viene aggiornata per il concatenamento al certificato SAN DV della CDN.
