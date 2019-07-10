---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-21"

keywords: wildcard certificate, https, san certificate

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note .note}
{:download: .download}

# Informazioni su HTTPS
{: #about-https}

{{site.data.keyword.cloud}} offre due modi per proteggere la tua CDN con HTTPS, ossia il certificato jolly e il certificato SAN DV (Domain Validation). Entrambe le opzioni HTTPS possono essere configurate selezionando **HTTPS Port** quando configuri la tua CDN. La porta HTTPS predefinita è la 443, ma puoi scegliere un numero di porta diverso per instradare il traffico HTTPS. Puoi trovare un elenco dei numeri di porta consentiti nelle [FAQ](docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-).

Per decidere tra l'utilizzo del **certificato jolly** e il **certificato SAN** per HTTPS, rispondi a questa domanda: vuoi gestire traffico HTTPS dal CNAME CDN o dal nome dominio CDN? Se vuoi gestire traffico HTTPS dal CNAME, seleziona il **certificato jolly**. Se vuoi gestire traffico HTTP dal nome dominio CDN, seleziona il **certificato SAN**.

## Supporto del certificato jolly
{: #wildcard-certificate-support}

**Nota**:
Le nuove associazioni CDN con il certificato jolly NON sono supportate in questo momento.

Il certificato jolly è il modo più semplice per consegnare in modo sicuro contenuti web agli utenti finali. Il CNAME CDN completo, incluso il suffisso del certificato jolly, **deve** essere utilizzato come punto di ingresso del servizio (ad esempio, `https://example.cdnedge.bluemix.net`) per poter utilizzare il certificato jolly.

IBM Cloud CDN utilizza il certificato jolly `*.cdnedge.bluemix.net`. Il CNAME, indipendentemente dal fatto che sia stato creato automaticamente o fornito da te, e che termina con il suffisso `*.cdnedge.bluemix.net` viene aggiunto al certificato jolly conservato nel server edge CDN. Pertanto il CNAME diventa l'unico modo con cui gli utenti finali possono utilizzare HTTPS per la tua CDN.

![Diagramma per HTTP e certificato jolly](images/state-diagram-wildcard.png)

## Supporto del certificato SAN (Subject Alternate Name)
{: #san-certificate-suport}

Il certificato SAN (Subject Alternative Name) è un certificato SSL digitale che consente di proteggere più domini o nomi host mediante un singolo certificato.

Con il certificato SAN per HTTPS, il nome host CDN principale viene aggiunto a un certificato emesso da un'autorità di certificazione (CA). Ciò consente agli utenti di accedere al tuo servizio in modo sicuro tramite il nome host anziché il CNAME; ad esempio, `https://www.example.com`.

Quando si effettua l'ordine della CDN utilizzando il certificato SAN HTTPS, questo passa attraverso il processo di richiesta di un certificato e creazione di una convalida del controllo del dominio (in inglese, Domain Control Validation o DCV). DCV è il processo che un'Autorità di certificazione utilizza per stabilire che sei autorizzato ad accedere e controllare il dominio. È necessario il tuo intervento per completare questo passo. Dopo aver stabilito il controllo, il certificato viene distribuito ai server edge CDN in tutto il mondo. Una volta che il certificato è stato distribuito, il rinnovo del certificato viene gestito automaticamente. Ulteriori informazioni su questa funzione sono disponibili nella [descrizione della funzione](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#https-protocol-support). I metodi di convalida del controllo del dominio vengono spiegati in modo più dettagliato nella pagina [Completamento della convalida del controllo del dominio per HTTPS](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#initial-steps-to-domain-control-validation).

Dopo che la CDN ha raggiunto lo stato di RUNNING (IN ESECUZIONE), devi mantenere il record CNAME dell'Hostname (Nome host) CDN nel tuo DNS. Se il record CNAME viene rimosso, l'Hostname (Nome host) CDN può essere rimosso dal certificato SAN entro 3 giorni. Se ciò si verifica, il traffico HTTPS non viene più gestito con tale Hostname (Nome host) CDN.
{:note}

![Diagramma per HTTPS con certificato SAN](images/state-diagram-san.png)
