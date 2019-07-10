---

copyright:
  years: 2017,2018, 2019
lastupdated: "2019-05-21"

keywords: order, create, configure, console, origin, preparation, bucket

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:download: .download}

# CDN bestellen
{: #order-a-cdn}

Hier erfahren Sie, wie ein Content Delivery Network (CDN) bestellt wird. Das CDN kann über die [{{site.data.keyword.cloud}}-Konsole](https://cloud.ibm.com/login) bestellt werden.

## Vorbereitungen für die Bestellung
{: #preparation-for-ordering}

Im Folgenden wird beschrieben, wie Sie zur CDN-Seite navigieren können, um Ihre Bestellung vorzunehmen.

**Schritt 1**

* Melden Sie sich über die [IBM Cloud-Konsole](https://cloud.ibm.com/login) bei Ihrem Konto an.

**Schritt 2**

Klicken Sie auf [IBM Cloud-Katalog](https://cloud.ibm.com/catalog/). Wählen Sie in der linken Navigationsleiste die Option **Netz** aus.

   ![IBM Cloud CDN-Navigation](images/bluemix_navigation.png)

**Schritt 3**

Klicken Sie auf die **CDN-Kachel**.

   ![IBM Cloud CDN-Symbol](images/bluemix_tile.png)


## Neues CDN bestellen
{: #order-a-new-cdn}

Wenn Sie sich auf der Bestellseite befinden, können Sie anhand der nachfolgend beschriebenen Schritte das CDN erstellen und konfigurieren.

### Schritt 1: CDN-Konto erstellen
{: #create-your-cdn-account}

Klicken Sie unten rechts **Erstellen**. Dadurch wird Ihr CDN-Konto erstellt, sofern noch nicht vorhanden, und Sie werden an die Seite für die CDN-Konfiguration weitergeleitet.

   ![CDN-Übersicht](images/content-delivery.png)

### Schritt 2: Namen für das CDN festlegen
{: #step-2-name-your-cdn}

Füllen Sie das Feld **Name konfigurieren** aus:  

  * Geben Sie einen Wert für **Hostname** (**erforderlich**) an, der als primäre Kennung für Ihr CDN fungiert (Beispiel: `example.testingcdn.net`).  
  * Geben Sie optional einen angepassten Wert für **CNAME** an (z. B. `myfirstcdn.cdnedge.bluemix.net`). Wenn Sie CNAME nicht angeben, wird dieser Wert automatisch erstellt. Das Suffix `cdnedge.bluemix.net` wird automatisch an den Wert für CNAME angehängt. Wenn Sie einen ungeeigneten Wert für CNAME verwenden, kann dies zur Beendigung von Services führen.

       ![Namen konfigurieren](images/configure-hostname-cname.png)  

Nach der Bereitstellung Ihres neuen CDN **müssen** Sie den CNAME mit Ihrem DNS-Anbieter konfigurieren.
{: note}
### Schritt 3: Ursprung konfigurieren
{: #step-3-configure-your-origin}

Füllen Sie das Feld **Ursprung konfigurieren** aus: Zum Konfigurieren dieses Felds müssen Sie entweder die Option **Server** oder die Option **Objektspeicher** auswählen.  

  * **Schritt 3, Option 1: Serveroption**

     ![Ursprungsserver konfigurieren](images/configure-origin-server.png)

      * Sie müssen die **Adresse des Ursprungsservers** (Hostname oder IPv4-Adresse des Ursprungsservers) angeben. Wenn **HTTPS-Port** ebenfalls ausgewählt ist, muss die **Adresse des Ursprungsservers** der Hostname und nicht die IP-Adresse sein.

      * Geben Sie einen Wert für **Host-Header** (optional) an. Wenn kein Wert angegeben wird, wird standardmäßig der **Hostname** verwendet. Weitere Informationen zum Host-Header finden Sie in der Funktionsbeschreibung zur [Host-Header-Unterstützung](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#host-header-support).  

      * Geben Sie einen **Pfad** an, über den Inhalte des Ursprungsservers abgerufen werden können (optional). Wählen Sie die Featurebeschreibung für [Pfadbasierte CDN-Zuordnungen](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#path-based-cdn-mappings) aus und informieren Sie sich über die Auswirkungen, die das Hinzufügen eines Pfads zu diesem Zeitpunkt hat.

      * Sie können außerdem einen **HTTP-Port** und/oder einen **HTTPS-Port** angeben. Diese Felder geben an, welches Protokoll und welche Portnummer zum Verbinden mit dem Ursprungsserver verwendet werden kann. Für nicht standardmäßig verwendete Portnummern finden Sie in [den häufig gestellten Fragen](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) eine Liste der zulässigen Portnummern.

      * Ein **SSL-Zertifikat** wird _nur_ angezeigt, wenn der HTTPS-Port ausgewählt ist. Wenn Sie **HTTPS-Port** für den Server- oder Objektspeicher auswählen, können Sie **Wildcard-Zertifikat** oder **DV-SAN-Zertifikat** als SSL-Zertifikatsoption auswählen. Beide Optionen bieten die erweiterten Sicherheitsfunktionen von HTTPS.
        * Ein **Wildcard-Zertifikat** ermöglicht HTTPS-Datenverkehr nur bei Verwendung eines **CNAME** und erfordert keine weiteren Aktionen.
        * Ein **DV-SAN-Zertifikat** ermöglicht HTTPS-Datenverkehr über Ihre Domäne, es müssen jedoch noch weitere Schritte zur Überprüfung ausgeführt werden. Informationen zu den bei dieser Option erforderlichen Schritten und den implizierten Zeiteinschränkungen finden Sie unter [Validierung der Domänensteuerung für HTTPS durchführen](/docs/infrastructure/CDN/how-to-https.html#completing-domain-control-validation-for-https).

	     ![Ursprungsserver konfigurieren](images/ssl-cert-options.png)

  * **Schritt 3, Option 2: Objektspeicheroption**

    ![Objektspeicher konfigurieren](images/configure-origin-object-storage.png)

      * Sie **müssen** den **Endpunkt** angeben, bei dem das Objekt abgerufen werden soll.

      * Geben Sie einen Wert für **Host-Header** (optional) an. Wenn kein Wert angegeben wird, wird standardmäßig der **Hostname** verwendet. Weitere Informationen zum Host-Header finden Sie in der Funktionsbeschreibung zur [Host-Header-Unterstützung](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#host-header-support).  

      * Geben Sie einen **Pfad** an, über den Inhalte des Ursprungsservers abgerufen werden können (optional). Wählen Sie die Featurebeschreibung für [Pfadbasierte CDN-Zuordnungen](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#path-based-cdn-mappings) aus und informieren Sie sich über die Auswirkungen, die das Hinzufügen eines Pfads an diesem Punkt hat.

      * Sie **müssen** den Namen für den **Bucket** angeben, in dem Ihr Inhalte gespeichert werden.

      * Sie können außerdem einen **HTTP-Port** und/oder einen **HTTPS-Port** angeben. Diese Felder geben an, welches Protokoll und welche Portnummer zum Verbinden mit dem Ursprungsserver verwendet werden kann. Für nicht standardmäßig verwendete Portnummern finden Sie in [den häufig gestellten Fragen](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) eine Liste der zulässigen Portnummern.

      * Ein **SSL-Zertifikat** wird _nur_ angezeigt, wenn der HTTPS-Port ausgewählt ist. Wenn Sie **HTTPS-Port** für den Server- oder Objektspeicher auswählen, können Sie **Wildcard-Zertifikat** oder **DV-SAN-Zertifikat** als SSL-Zertifikatsoption auswählen. Beide Optionen bieten die erweiterten Sicherheitsfunktionen von HTTPS.
        * Ein **Wildcard-Zertifikat** ermöglicht HTTPS-Datenverkehr nur bei Verwendung eines **CNAME** und erfordert keine weiteren Aktionen.
        * Ein **DV-SAN-Zertifikat** ermöglicht HTTPS-Datenverkehr über Ihre Domäne, es müssen jedoch noch weitere Schritte zur Überprüfung ausgeführt werden. Informationen zu den bei dieser Option erforderlichen Schritten und den implizierten Zeiteinschränkungen finden Sie unter [Validierung der Domänensteuerung für HTTPS durchführen](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#completing-domain-control-validation-for-https).

        ![HTTPS konfigurieren](images/ssl-cert-options.png)

Sie müssen in der **Zugriffssteuerungsliste** (ACL) für jedes Objekt in Ihrem Bucket die Berechtigung "public-read" mit Ihrem Provider für Cloudobjektspeicher konfigurieren.
{: note}
      
### Schritt 4
{: #step-4}

* Sie müssen unten rechts oberhalb der Schaltfläche**Erstellen** das Feld **Ich habe die Rahmenvereinbarung gelesen und stimme den Bedingungen zu** auswählen.

* Wählen Sie anschließend die Schaltfläche **Erstellen** in der rechten unteren Ecke aus, um Ihr CDN zu erstellen.
