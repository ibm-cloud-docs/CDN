---

copyright:
  years: 2018
lastupdated: "2018-06-28"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Informazioni su HTTPS

IBM Cloud offre due modi per proteggere la tua CDN con HTTPS, ossia il certificato wildcard e il certificato SAN DV (Domain Validation). Entrambe le opzioni HTTPS possono essere configurate selezionando **HTTPS Port** quando configuri la tua CDN. La porta HTTPS predefinita è la 443, ma puoi scegliere un numero di porta diverso per instradare il traffico HTTPS. Puoi trovare un elenco dei numeri di porta consentiti nelle [FAQ](faqs.html#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-).

## Supporto del certificato wildcard
>Il certificato wildcard è il modo più semplice per consegnare in modo sicuro contenuti web agli utenti finali. Il CNAME CDN completo, incluso il suffisso del certificato wildcard, **deve** essere utilizzato come punto di ingresso del servizio (ad esempio, `https://example.cdnedge.bluemix.net`) per poter utilizzare il certificato wildcard.
>
>La CDN di IBM Cloud utilizza il certificato wildcard `*.cdnedge.bluemix.net`. Il CNAME, indipendentemente dal fatto che sia stato creato automaticamente o fornito da te, e che termina con il suffisso `*.cdnedge.bluemix.net` viene aggiunto al certificato wildcard conservato nel server edge CDN. Pertanto il CNAME diventa l'unico modo con cui gli utenti finali possono utilizzare HTTPS per la tua CDN.

![Diagramma per HTTP e wildcard](images/state-diagram-wildcard.png)

## Supporto del certificato SAN (Subject Alternate Name)

>Il certificato SAN (Subject Alternative Name) è un certificato SSL digitale che consente di proteggere più domini o nomi host mediante un singolo certificato.
>
>Con il certificato SAN per HTTPS, il nome host CDN principale viene aggiunto a un certificato emesso da un'autorità di certificazione (CA). Ciò consente agli utenti di accedere al tuo servizio in modo sicuro tramite il nome host anziché il CNAME; ad esempio, `https://www.example.com`.
>
>Quando si effettua l'ordine della CDN utilizzando il certificato SAN HTTPS, questo passa attraverso il processo di richiesta di un certificato e creazione di una convalida del controllo del dominio (in inglese, Domain Control Validation o DCV). DCV è il processo che un'Autorità di certificazione utilizza per stabilire che sei autorizzato ad accedere e controllare il dominio. È necessario il tuo intervento per completare questo passo. Dopo aver stabilito il controllo, il certificato viene distribuito ai server edge CDN in tutto il mondo. Una volta che il certificato è stato distribuito, il rinnovo del certificato viene gestito automaticamente. Ulteriori informazioni su questa funzione sono disponibili nella [descrizione della funzione](about.html#https-protocol-support). I metodi di convalida del controllo del dominio vengono spiegati in modo più dettagliato nella pagina [Completamento della convalida del controllo del dominio per HTTPS](how-to-https.html#initial-steps-to-domain-control-validation).

![Diagramma per HTTPS con certificato SAN](images/state-diagram-san.png)
