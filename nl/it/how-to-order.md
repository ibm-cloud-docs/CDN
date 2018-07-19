---

copyright:
  years: 2017,2018
lastupdated: "2018-06-05"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Ordina una CDN

Qui troverai le informazioni su come ordinare una CDN (Content Delivery Network). La tua CDN può essere ordinata dal [Portale del cliente ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com/) o dal [Portale Bluemix](https://www.ibm.com/cloud-computing/bluemix/).

## Dal Portale del cliente:

**Passo 1:**

Per iniziare, accedi al [Portale del cliente ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com/) utilizzando le tue credenziali univoche.

**Passo 2:**

Dalla barra di navigazione nella parte superiore dello schermo, seleziona **Rete -> CDN**.

   ![Opzioni di menu Rete](images/network-cdn.png)

**Passo 3:**

Nella pagina **Content Delivery Networks**, seleziona il pulsante **Order CDN** (Ordina CDN) in alto a destra.

   ![Seleziona l'ordinazione della CDN](images/order-cdn-button.png)

## Dal portale IBM Cloud:

**Passo 1:**

Accedi al [Portale IBM Cloud](https://www.ibm.com/cloud-computing/bluemix/)

**Passo 2:**

Fai clic su [Catalogo IBM Cloud](https://console.bluemix.net/catalog/). Dalla barra di navigazione a sinistra, seleziona **Rete**.

   ![Navigazione Bluemix CDN](images/bluemix_navigation.png)

**Passo 3:**

Fai clic sul **tile CDN**, che ti porta alla schermata Selezione fornitore.

   ![Icona Bluemix CDN](images/bluemix_tile.png)


**Passo 4:**

Dalla schermata **Select a CDN Provider** (Seleziona un provider CDN), scegli tra le opzioni del provider CDN. Fai clic sul pulsante **Select** (Seleziona) per confermare le tue opzioni selezionate e fai quindi clic su **Next** (Avanti) nella parte inferiore destra dello schermo per avviare il processo di provisioning.  
       ![Seleziona provider CDN](images/Vendor_Select_And_Provision.png)

**Passo 5:**

Compila il campo **Configure Name** (Configura nome):  

  * Specifica il **Hostname** (Nome host) (**obbligatorio**), che funge da identificativo primario per la tua CDN (ad esempio _example.testingcdn.net_).  
  * Facoltativamente, puoi fornire un **CNAME** personalizzato (ad esempio _myfirstcdn.cdnedge.bluemix.net_). Se non fornisci un CNAME, ne verrà creato uno per te. <informazioni di convalida da includere qui>  

       ![Configura il nome](images/configure-hostname-cname.png)  

    **Nota**: l'utilizzo di un CNAME inappropriato potrebbe causare la chiusura dei servizi.

**Passo 6:**

Compila il campo **Configura la tua origine**: per configurare questo campo, devi selezionare l'opzione **Server** o **Object Storage**.  

   * Specifica la **Host header** (Intestazione host) (facoltativo). Se non ne viene fornita una, verrà automaticamente impostata sull'**Hostname** (Nome host). Consulta la descrizione della funzione per [Supporto dell'intestazione host](about.html#host-header-support-) per ulteriori informazioni sull'intestazione host.  

   * Fornisci un **Path** (Percorso) (facoltativo). Il percorso deve essere relativo all'**Hostname** (Nome host).

      ![Configura l'origine](images/configure-origin.png)  

  * **Opzione Server**: se selezioni l'opzione **Server**, immetti il nome host o l'indirizzo IP del server di origine da cui i dati devono essere memorizzati nella cache.
      * Devi specificare l'**Indirizzo del server di origine** (nome host o indirizzo IPv4 del server di origine) se selezioni questa opzione. Se viene selezionata la **Porta HTTPS**, l'**Indirizzo del server di origine** deve essere il nome host e non l'indirizzo IP.
      * Puoi anche fornire una **Porta HTTP**, una **Porta HTTPS** o entrambe. Questi campi indicano quale protocollo e numero porta possono essere utilizzati per contattare il server di origine. Per i numeri porta non predefiniti, fai riferimento a [FAQ](faqs.html#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai) per un elenco di numeri porta consentiti.
      * **Certificato SSL **: questa opzione viene visualizzata _solo_ quando si seleziona la porta HTTPS. \*Ulteriori informazioni per le configurazioni dei certificati HTTPS e SSL seguono la descrizione dell'opzione Object Storage.

	     ![Configura il server di origine](images/configure-origin-server.png)

  * **Opzione Object Storage**: se selezioni l'opzione **Object Storage**, devi fornire le seguenti informazioni:
      * l'**Endpoint** da cui recuperare l'oggetto.
      * il nome del **Bucket** in cui è memorizzato il tuo contenuto e
      * la **porta HTTPS**.
      * **Certificato SSL **: questa opzione viene visualizzata _solo_ quando si seleziona la porta HTTPS. \*Ulteriori informazioni per le configurazioni dei certificati HTTPS e SSL seguono la descrizione dell'opzione Object Storage.
      * Puoi anche specificare le estensioni file, separate da virgole, che possono essere utilizzate nel servizio CDN. (Se non si specificano estensioni file, sono consentite tutte le estensioni.)
      * Devi impostare l'ACL (**Access Control List**) per ogni **Oggetto** presente nel tuo **Bucket** per l'accesso "public-read".

      	  ![Configura l'Object Storage](images/configure-origin-cos.png)

  * **Certificato SSL**: se selezioni la **Porta HTTPS** per Server o Object Storage, puoi scegliere **Wildcard** o **Certificato SAN DV** come opzione per il tuo **Certificato SSL**. Entrambi offrono la sicurezza avanzata fornita da HTTPS.
    * Il **Certificato wildcard** consente il traffico HTTPS solo quando si utilizza il **CNAME** e non richiede alcuna ulteriore azione da parte tua
    * Il **Certificato SAN DV** consente il traffico HTTPS sul tuo dominio, ma richiede ulteriori passi per la verifica.

        ![Configura HTTPS](images/configure-https.png)


**Passo 7:**

Configura il campo **Altre opzioni**: questa sezione contiene le opzioni di configurazione per il campo **Rispetta intestazioni**.

   * Quando l'opzione **Respect Headers** (Rispetta intestazioni) è impostata su **On** (Attivo), le impostazioni TTL definite nell'intestazione dall'origine sovrascriveranno il TTL CDN predefinito. **Respect Headers** (Rispetta intestazioni) è impostata su **On** (Attivo) per impostazione predefinita, tuttavia devi configurare questo campo.  

        ![Altre opzioni](images/other-options.png)

**Passo 8:**

Seleziona il pulsante **Create** (Crea) nell'angolo in basso a destra per creare la tua CDN.
