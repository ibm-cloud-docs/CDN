---

copyright:
  years: 2017,2018
lastupdated: "2018-02-06"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# CDN bestellen

Hier erfahren Sie, wie ein Content Delivery Network (CDN) bestellt wird. Sie können Ihr CDN entweder über das [Kundenportal ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/) oder über das [Bluemix-Portal](https://www.ibm.com/cloud-computing/bluemix/) bestellen.

## Gehen Sie im Steuerportal wie folgt vor:

**Schritt 1:**

Melden Sie sich zunächst mit Ihren eindeutigen Berechtigungsnachweisen beim [Kundenportal ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/) an.

**Schritt 2:**

Wählen Sie in der Navigationsleiste oben in der Anzeige **Netz -> CDN** aus.

   ![Optionen im Netzmenü](images/network-cdn.png)

**Schritt 3:**

Wählen Sie auf der Seite **Content Delivery Networks** die Schaltfläche **CDN bestellen** in der rechten oberen Ecke aus.

   ![CDN-Bestellprozess auswählen](images/order-cdn-button.png)

## Gehen Sie im Bluemix-Portal wie folgt vor:

**Schritt 1:**

Melden Sie sich beim [Bluemix-Portal](https://www.ibm.com/cloud-computing/bluemix/) an.

**Schritt 2:**

Klicken Sie auf [IBM Bluemix-Katalog](https://console.bluemix.net/catalog/). Wähle Sie in der linken Navigationsleiste die Option **Netz** aus.

   ![Bluemix - CDN-Navigation](images/bluemix_navigation.png)

**Schritt 3:**

Klicken Sie auf die Kachel **CDN**, um die Anzeige für Anbieterauswahl aufzurufen.

   ![Bluemix - CDN-Symbol](images/bluemix_tile.png)


**Schritt 4:**

Wählen Sie in der Anzeige **CDN-Anbieter auswählen** unter den verfügbaren Optionen für CDN-Anbieter aus. Klicken Sie auf die Schaltfläche **Auswählen**, um die ausgewählten Optionen zu bestätigen, und klicken Sie dann auf **Bereitstellung starten** in der rechten unteren Ecke der Anzeige, um den Bereitstellungsprozess zu starten.  
       ![CDN-Anbieter auswählen](images/Vendor_Select_And_Provision.png)

**Schritt 5:**

Füllen Sie das Feld **Name konfigurieren** aus:  

  * Geben Sie einen Wert für **Hostname** (**Erforderlich**) an, der als primäre Kennung für Ihr CDN fungiert (Beispiel: _example.testingcdn.net_).  
  * Geben Sie optional einen angepassten Wert für **CNAME** an (z. B. _myfirstcdn.cdnedge.bluemix.net_). Wenn Sie CNAME nicht angeben, wird dieser Wert automatisch erstellt. <Validierungsinformationen hier einfügen>  

       ![Namen konfigurieren](images/configure-hostname-cname.png)  

    **Hinweis**: Wenn Sie einen ungeeigneten Wert für CNAME verwenden, kann dies zur Beendigung von Services führen.

**Schritt 6:**

Füllen Sie das Feld **Ursprung konfigurieren** aus: Zum Konfigurieren dieses Felds müssen Sie entweder die Option **Server** oder die Option **Objektspeicher** auswählen.  

   * Geben Sie einen Wert für **Host-Header** (optional) an. Wenn kein Host-Header angegeben wird, wird standardmäßig der Wert von **Hostname** verwendet. Weitere Informationen zum Host-Header finden Sie in der Funktionsbeschreibung zur [Host-Header-Unterstützung](about.html#host-header-support-).  
   
   * Geben Sie einen **Pfad** (optional) an. Der Pfad muss relativ zum **Hostnamen** angegeben werden. 
   
      ![Ursprungsserver konfigurieren](images/configure-origin.png)  

  * **Option 'Server'**: Wenn Sie die Option **Server** auswählen, geben Sie den Hostnamen oder die IP-Adresse des Ursprungsservers an, aus dem Daten im Cache gespeichert werden sollen.
      * Wenn Sie diese Option auswählen, müssen Sie die **Adresse des Ursprungsservers** (Hostname oder IPv4-Adresse des Ursprungsservers) angeben. Wenn **HTTPS-Port** ausgewählt wird, muss die **Adresse des Ursprungsservers** der Hostname und nicht die IP-Adresse sein.
      * Sie können außerdem einen **HTTP-Port** und/oder einen **HTTPS-Port** angeben. Diese Felder geben an, welches Protokoll und welche Portnummer zum Verbinden mit dem Ursprungsserver verwendet werden kann. Für nicht standardmäßig verwendete Portnummern finden Sie in [den häufig gestellten Fragen](faq.html#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) eine Liste der zulässigen Portnummern.

	     ![Ursprungsserver konfigurieren](images/configure-origin-server.png)

  * **Option 'Objektspeicher'**: Wenn Sie die Option **Objektspeicher** auswählen, müssen Sie die folgenden Informationen angeben.
      * Den **Endpunkt**, aus dem das Objekt abgerufen werden soll
      * Den Namen des **Buckets**, in dem Ihr Inhalt gespeichert ist
      * Den **HTTPS-Port**
      * Außerdem können Sie die Dateierweiterungen angeben (getrennt durch Kommas), die im CDN-Service verwendet werden können. (Wenn keine Dateinamenerweiterungen angegeben werden, sind alle Erweiterungen zulässig.)
      * Sie müssen in der **Zugriffssteuerungsliste** (Access Control List, ACL) für jedes **Objekt** in Ihrem **Bucket** die Berechtigung 'public-read' festlegen.

	     ![Objektspeicher konfigurieren](images/configure-origin-object-storage.png)

**Schritt 7:**

Konfigurieren Sie das Feld **Weitere Optionen**: Dieser Abschnitt enthält Konfigurationsoptionen für das Feld **Header beibehalten**.

   * Wenn für das Feld **Header beibehalten** der Wert **On** (Ein) angegeben ist, überschreiben die vom Ursprungsserver im Header definierten TTL-Einstellungen die Standardeinstellungen für die CDN-TTL. Für **Header beibehalten** wird standardmäßig der Wert **On** (Ein) angegeben, aber Sie müssen dieses Feld konfigurieren.  

        ![Weitere Optionen](images/other-options.png)

**Schritt 8:**

Wählen Sie die Schaltfläche **Erstellen** in der rechten unteren Ecke aus, um Ihr CDN zu erstellen.
