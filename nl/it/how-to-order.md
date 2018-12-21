---

copyright:
  years: 2017,2018
lastupdated: "2018-11-19"

---

{:shortdesc: .shortdesc}
{:new_window: target="blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Ordina una CDN

Qui troverai le informazioni su come ordinare una CDN (Content Delivery Network). La tua CDN può essere ordinata dal [portale di IBM Cloud](https://www.ibm.com/cloud-computing/bluemix/).

## Andando alla pagina di CDN:

**Passo 1:**

Esegui l'accesso al tuo account dal [portale di IBM Cloud](https://www.ibm.com/cloud-computing/bluemix/)

**Passo 2:**

Fai clic su [Catalogo IBM Cloud](https://console.bluemix.net/catalog/). Dalla barra di navigazione a sinistra, seleziona **Rete**.

   ![Navigazione Bluemix CDN](images/bluemix_navigation.png)

**Passo 3:**

Fai clic sul **tile CDN**.

   ![Icona Bluemix CDN](images/bluemix_tile.png)


## Ordina una nuova CDN:

**Passo 1:**

Fai clic **Create** in basso a destra; tale selezione crea il tuo account CDN, se non ne hai già uno, e ti reindirizza alla schermata CDN Configure.

   ![Panoramica della CDN](images/content-delivery.png)

**Passo 2:**

Compila il campo **Configure Name** (Configura nome):  

  * Specifica il **Hostname** (Nome host) (**obbligatorio**), che funge da identificativo primario per la tua CDN (ad esempio `example.testingcdn.net`).  
  * Facoltativamente, puoi fornire un **CNAME** personalizzato (ad esempio `myfirstcdn.cdnedge.bluemix.net`). Se non fornisci un CNAME, ne verrà creato uno per te. Il suffisso `cdnedge.bluemix.net` viene accodato automaticamente al CNAME. L'utilizzo di un CNAME inappropriato potrebbe causare la terminazione dei servizi.

       ![Configura il nome](images/configure-hostname-cname.png)  

    **Nota**: dopo il provisioning della tua nuova CDN, **devi** configurare il CNAME con il tuo provider DNS.

**Passo 3:**

Compila il campo **Configura la tua origine**: per configurare questo campo, devi selezionare l'opzione **Server** o **Object Storage**.  

  * **Passo 3, opzione 1: L'opzione server **

     ![Configura l'origine](images/configure-origin-server.png)

      * Devi specificare l'indirizzo del server di origine (**Origin Server Address**) (nome host o indirizzo IPv4 del server di origine). Se viene selezionata anche la porta HTTPS (**HTTPS port**), l'indirizzo del server di origine (**Origin Server Address**) deve essere il nome host e non l'indirizzo IP.

      * Specifica l'intestazione host (**Host header**) (facoltativo). Se non ne viene fornita una, viene automaticamente impostata sul nome host (**Hostname**). Consulta la descrizione della funzione per [Supporto dell'intestazione host](feature-descriptions.html#host-header-support) per ulteriori informazioni sull'intestazione host.  

      * Fornisci un percorso (**Path**) da cui è possibile richiamare del contenuto sull'origine (facoltativo). Consulta la descrizione della funzione per le [Associazioni di CDN basate sui percorsi](feature-descriptions.html#path-based-cdn-mappings) per comprendere le implicazioni dell'aggiunta di un percorso in questo momento.

      * Puoi anche fornire una porta HTTP (**HTTP port**), una porta HTTPS (**HTTPS port**) o entrambe. Questi campi indicano quale protocollo e numero porta possono essere utilizzati per contattare il server di origine. Per i numeri porta non predefiniti, fai riferimento a [FAQ](faqs.html#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) per un elenco di numeri porta consentiti.

      * **Certificato SSL**: questa opzione viene visualizzata _solo_ quando viene selezionata la porta HTTPS. Se selezioni la porta HTTPS (**HTTPS Port**) per Server oppure Object Storage, puoi scegliere **Wildcard** (Jolly) oppure **DV SAN Certificate** (Certificato SAN DV) come tua opzione SSL Certificate (Certificato SSL). Entrambi offrono la sicurezza avanzata fornita da HTTPS.
        * Il **certificato jolly** consente il traffico HTTPS solo quando si utilizza il **CNAME** e non richiede alcuna ulteriore azione da parte tua
        * Il **certificato SAN DV** consente il traffico HTTPS sul tuo dominio, ma richiede ulteriori passi per la verifica. Per comprendere la procedura necessaria e i vincoli di tempi coinvolti con la scelta di questa opzione, consulta la pagina [Completamento della convalida del controllo del dominio per HTTPS](how-to-https.html#completing-domain-control-validation-for-https).

	     ![Configura il server di origine](images/ssl-cert-options.png)

  * **Passo 3, opzione 2: l'opzione Object Storage**

    ![Configura l'Object Storage](images/configure-origin-object-storage.png)

      * **Devi** specificare l'**Endpoint** da cui recuperare l'oggetto.

      * Specifica l'intestazione host (**Host header**) (facoltativo). Se non ne viene fornita una, viene automaticamente impostata sul nome host (**Hostname**). Consulta la descrizione della funzione per [Supporto dell'intestazione host](feature-descriptions.html#host-header-support) per ulteriori informazioni sull'intestazione host.  

      * Fornisci un percorso (**Path**) da cui è possibile richiamare del contenuto sull'origine (facoltativo). Consulta la descrizione della funzione per le [Associazioni di CDN basate sui percorsi](feature-descriptions.html#path-based-cdn-mappings) per comprendere le implicazioni dell'aggiunta di un percorso qui.

      * **Devi** fornire il nome del **Bucket** in cui è memorizzato il tuo contenuto.

      * Puoi anche fornire una porta HTTP (**HTTP port**), una porta HTTPS (**HTTPS port**) o entrambe. Questi campi indicano quale protocollo e numero porta possono essere utilizzati per contattare il server di origine. Per i numeri porta non predefiniti, fai riferimento a [FAQ](faqs.html#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) per un elenco di numeri porta consentiti.

      * **Certificato SSL**: questa opzione viene visualizzata _solo_ quando viene selezionata la porta HTTPS. Se selezioni la porta HTTPS (**HTTPS Port**) per Server oppure Object Storage, puoi scegliere **Wildcard** (Jolly) oppure **DV SAN Certificate** (Certificato SAN DV) come tua opzione SSL Certificate (Certificato SSL). Entrambi offrono la sicurezza avanzata fornita da HTTPS.
        * Il **Certificato jolly** consente il traffico HTTPS solo quando si utilizza il **CNAME** e non richiede alcuna ulteriore azione da parte tua
        * Il **Certificato SAN DV** consente il traffico HTTPS sul tuo dominio, ma richiede ulteriori passi per la verifica. Per comprendere la procedura necessaria e i vincoli di tempi coinvolti con la scelta di questa opzione, consulta la pagina [Completamento della convalida del controllo del dominio per HTTPS](how-to-https.html#completing-domain-control-validation-for-https).

        ![Configura HTTPS](images/ssl-cert-options.png)

      **NOTA** devi impostare l'ACL (**Access Control List**) per ciascun oggetto nel tuo bucket su "public-read" con il tuo provider COS (cloud object storage).
      
**Passo 4:**

* Devi selezionare **I have read the Master Service Agreement and agree to the terms therein** nella parte inferiore destra, sopra al pulsante **Create**.

* Seleziona quindi il pulsante **Create** nell'angolo in basso a destra per creare la tua CDN.
