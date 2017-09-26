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

# Come ordinare un CDN

Qui troverai le informazioni su come ordinare un CDN (Content Delivery Network). Puoi ordinare il tuo CDN dal [Portale del cliente ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com/) o dal [Portale del catalogo Bluemix](https://www.ibm.com/cloud-computing/bluemix/).

## Dal Portale del cliente:

1. Per iniziare, accedi al [Portale del cliente ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com/) utilizzando le tue credenziali univoche.

2. Dalla barra di navigazione nella parte superiore dello schermo, seleziona **Rete -> CDN**.

   ![Opzioni di menu Rete](images/network-cdn.png)

3. Nella pagina **Content Delivery Networks**, seleziona il pulsante **Ordina CDN** in alto a destra.

	![Seleziona ordine CDN](images/order-cdn-button.png)

## Dal portale Bluemix:

1. Accedi al [Portale Bluemix](https://www.ibm.com/cloud-computing/bluemix/)

2. Fai clic su **Catalogo IBM Bluemix**. Dalla barra di navigazione a sinistra, seleziona **Rete**.

   ![Navigazione Bluemix CDN](images/bluemix_navigation.png)

3. Fai clic sul **tile CDN**, che ti porta alla schermata Selezione fornitore.

   ![Icona Bluemix CDN](images/bluemix_tile.png)

4. Dalla schermata **Seleziona un provider CDN**, scegli tra le opzioni del provider CDN. Fai clic sul pulsante **Selezionato** nella parte inferiore dello schermo per confermare le tue opzioni selezionate, quindi fai clic su **Avvia provisioning** per avviare il processo di provisioning.

	![Seleziona provider CDN](images/newReducedSizeVendorSelectAndProvision.png)
	
5. Compila il campo **Configura nome**: 
      * Specifica il _nome host_ (**obbligatorio**), che funge da identificativo primario per il tuo CDN (ad esempio, _example.testingcdn.net_).
      * Facoltativamente, puoi fornire un _CNAME_ personalizzato (ad esempio _myfirstcdn.cdnedge.bluemix.net_). Se non fornisci un CNAME, ne verrà creato uno per te. <informazioni di convalida da includere qui>
      
      ![Configura nome](images/configure-hostname-cname.png)
		
6. Compila il campo **Configura la tua origine**: per configurare questo campo, devi selezionare l'opzione **Server** o **Archiviazione oggetti**. (La specifica di **Percorso** è facoltativa. <informazioni di convalida>)
		
  * **Opzione Server**: se selezioni l'opzione **Server**, immetti il nome host o l'indirizzo IP del server di origine da cui i dati devono essere memorizzati nella cache. 
      * Se selezioni questa opzione, devi specificare l'**Indirizzo IP** del server di origine.
      * Puoi anche fornire una **Porta HTTP**, una **Porta HTTPS** o entrambe. (Questi campi indicano quale protocollo e numero di porta possono essere utilizzati per contattare il server di origine.)

	   ![Configura server di origine](images/configure-origin-server.png)
		
  * **Opzione Archiviazione oggetti**: se selezioni l'opzione **Archiviazione oggetti**, devi fornire le seguenti informazioni:
      * l'**Endpoint** da cui recuperare l'oggetto.
      * il nome del **Bucket** in cui è memorizzato il tuo contenuto e
      * la **porta HTTPS**.
      * Puoi anche specificare le estensioni file, separate da virgole, che possono essere utilizzate nel servizio CDN. (Se non si specificano estensioni file, sono consentite tutte le estensioni.)
      * Devi impostare l'**Access Control List** (ACL) per ogni **Oggetto** presente nel tuo **Bucket** per l'accesso "public-read".
		
	   ![Configura Archiviazione oggetti](images/configure-origin-object-storage.png)

7. Configura il campo **Altre opzioni**: questa sezione contiene le opzioni di configurazione per il campo **Utilizza contenuto obsoleto** e il campo **Rispetta intestazioni**.
    
     * Campo **Utilizza contenuto obsoleto**: **Utilizza contenuto obsoleto** è un valore booleano che indica se utilizzare il contenuto memorizzato nella cache che è scaduto, nel caso in cui non sia possibile raggiungere il server di origine. A causa di una limitazione corrente, la funzionalità **Utilizza contenuto obsoleto**  è abilitata per impostazione predefinita, indipendentemente dal fatto che l'utente la imposti su **Attivo** o **Disattivo**.
     * Campo **Rispetta intestazioni**: quando l'opzione **Rispetta intestazioni** è impostata su **Attivo**, le impostazioni TTL definite nell'intestazione dall'origine sovrascriveranno il TTL CDN predefinito. **Rispetta intestazioni** è impostata su **Attivo** per impostazione predefinita, tuttavia devi configurare questo campo.

		![Altre opzioni](images/other-options.png)
		
8. Seleziona il pulsante **Crea** nell'angolo in basso a destra per creare il tuo CDN.
