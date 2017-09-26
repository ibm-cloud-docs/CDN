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

# CDN bestellen

Hier erfahren Sie, wie ein Content Delivery Network (CDN) bestellt wird. Sie können Ihr CDN entweder über das [Kundenportal ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/) oder über das [Portal des Bluemix-Katalogs](https://www.ibm.com/cloud-computing/bluemix/) bestellen.

## Gehen Sie im Steuerportal wie folgt vor:

1. Melden Sie sich zunächst mit Ihren eindeutigen Berechtigungsnachweisen beim [Kundenportal ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/) an.

2. Wählen Sie in der Navigationsleiste oben in der Anzeige **Netz -> CDN** aus.

   ![Optionen im Netzmenü](images/network-cdn.png)

3. Wählen Sie auf der Seite **Content Delivery Networks** die Schaltfläche **CDN bestellen** in der rechten oberen Ecke aus.

	![CDN-Bestellprozess auswählen](images/order-cdn-button.png)

## Gehen Sie im Bluemix-Portal wie folgt vor:

1. Melden Sie sich beim [Bluemix-Portal](https://www.ibm.com/cloud-computing/bluemix/) an.

2. Klicken Sie auf **IBM Bluemix-Katalog**. Wähle Sie in der linken Navigationsleiste die Option **Netz** aus.

   ![Bluemix - CDN-Navigation](images/bluemix_navigation.png)

3. Klicken Sie auf die Kachel **CDN**, um die Anzeige für Anbieterauswahl aufzurufen.

   ![Bluemix - CDN-Symbol](images/bluemix_tile.png)

4. Wählen Sie in der Anzeige **CDN-Anbieter auswählen** unter den verfügbaren Optionen für CDN-Anbieter aus. Klicken Sie auf die Schaltfläche **Ausgewählt** unten in der Anzeige, um die ausgewählten Optionen zu bestätigen, und klicken Sie anschließend auf **Bereitstellung starten**, um den Bereitstellungsvorgang zu starten.

	![CDN-Anbieter auswählen](images/newReducedSizeVendorSelectAndProvision.png)
	
5. Füllen Sie das Feld **Name konfigurieren** aus: 
      * Geben Sie _hostname_ an (**erforderlich**). Dieser Name dient als primäre Kennung für Ihr CDN (z. B. _example.testingcdn.net_).
      * Geben Sie optional einen angepassten Wert für _CNAME_ an (z. B. _myfirstcdn.cdnedge.bluemix.net_). Wenn Sie CNAME nicht angeben, wird dieser Wert automatisch erstellt. <Validierungsinformationen hier einfügen>
      
      ![Namen konfigurieren](images/configure-hostname-cname.png)
		
6. Füllen Sie das Feld **Ursprung konfigurieren** aus: Zum Konfigurieren dieses Felds müssen Sie entweder die Option **Server** oder die Option **Objektspeicher** auswählen. (Der **Pfad** kann optional angegeben werden. <Validierungsinformationen>)
		
  * **Option 'Server'**: Wenn Sie die Option **Server** auswählen, geben Sie den Hostnamen oder die IP-Adresse des Ursprungsservers an, aus dem Daten im Cache gespeichert werden sollen. 
      * Sie müssen die **IP-Adresse** des Ursprungsservers angeben, wenn Sie diese Option ausgewählt haben.
      * Sie können außerdem einen **HTTP-Port** und/oder einen **HTTPS-Port** angeben. (Diese Felder geben an, welches Protokoll und welche Portnummer zum Verbinden mit dem Ursprungsserver verwendet werden kann.)

	   ![Ursprungsserver konfigurieren](images/configure-origin-server.png)
		
  * **Option 'Objektspeicher'**: Wenn Sie die Option **Objektspeicher** auswählen, müssen Sie die folgenden Informationen angeben.
      * Den **Endpunkt**, aus dem das Objekt abgerufen werden soll
      * Den Namen des **Buckets**, in dem Ihr Inhalt gespeichert ist
      * Den **HTTPS-Port**
      * Außerdem können Sie die Dateierweiterungen angeben (getrennt durch Kommas), die im CDN-Service verwendet werden können. (Wenn keine Dateinamenerweiterungen angegeben werden, sind alle Erweiterungen zulässig.)
      * Sie müssen in der **Zugriffssteuerungsliste** (Access Control List, ACL) für jedes **Objekt** in Ihrem **Bucket** die Berechtigung 'public-read' festlegen.
		
	   ![Objektspeicher konfigurieren](images/configure-origin-object-storage.png)

7. Konfigurieren Sie das Feld **Weitere Optionen**: Dieser Abschnitt enthält Konfigurationsoptionen für das Feld **Nicht aktuelle Inhalte bereitstellen** und für das Feld **Header beibehalten**.
    
     * Feld **Nicht aktuelle Inhalte bereitstellen**: In **Nicht aktuelle Inhalte bereitstellen** wird durch einen booleschen Wert angegeben, ob abgelaufene Cacheinhalte bereitgestellt werden sollen, wenn der Ursprungsserver nicht erreichbar ist. Aufgrund einer aktuellen Einschränkung ist die Funktion **Nicht aktuelle Inhalte bereitstellen** standardmäßig aktiviert (unabhängig davon, ob sie vom Benutzer auf den Wert **On** (Ein) oder **Off** (Aus) gesetzt wird).
     * Feld **Header beibehalten**: Wenn für die Option **Header beibehalten** der Wert **On** (Ein) angegeben ist, überschreiben die vom Ursprungsserver im Header definierten TTL-Einstellungen die Standardeinstellungen für die CDN-TTL.Für **Header beibehalten** wird standardmäßig der Wert **On** (Ein) angegeben, aber Sie müssen dieses Feld konfigurieren.

		![Weitere Optionen](images/other-options.png)
		
8. Wählen Sie die Schaltfläche **Erstellen** in der rechten unteren Ecke aus, um Ihr CDN zu erstellen.
