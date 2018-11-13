---

copyright:
  years: 2017,2018
lastupdated: "2018-07-17"

---

{:shortdesc: .shortdesc}
{:new_window: target="blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# CDN bestellen

Hier erfahren Sie, wie ein Content Delivery Network (CDN) bestellt wird. Sie können ein CDN über das [IBM Cloud-Portal](https://www.ibm.com/cloud-computing/bluemix/) bestellen.

## Zur CDN-Seite navigieren

**Schritt 1:**

Melden Sie sich im [IBM Cloud-Portal](https://www.ibm.com/cloud-computing/bluemix/) bei Ihrem Konto an.

**Schritt 2:**

Klicken Sie auf [IBM Cloud-Katalog](https://console.bluemix.net/catalog/). Wählen Sie in der linken Navigationsleiste die Option **Netz** aus.

   ![Bluemix - CDN-Navigation](images/bluemix_navigation.png)

**Schritt 3:**

Klicken Sie auf die **CDN-Kachel**.

   ![Bluemix - CDN-Symbol](images/bluemix_tile.png)


## Neues CDN bestellen

**Schritt 1:**

Klicken Sie unten rechts **Erstellen**. Dadurch wird Ihr CDN-Konto erstellt, sofern noch nicht vorhanden, und Sie werden an die Seite für die CDN-Konfiguration weitergeleitet.

   ![CDN-Übersicht](images/content-delivery.png)

**Schritt 2:**

Füllen Sie das Feld **Name konfigurieren** aus:  

  * Geben Sie einen Wert für **Hostname** (**erforderlich**) an, der als primäre Kennung für Ihr CDN fungiert (Beispiel: `example.testingcdn.net`).  
  * Geben Sie optional einen angepassten Wert für **CNAME** an (z. B. `myfirstcdn.cdnedge.bluemix.net`). Wenn Sie CNAME nicht angeben, wird dieser Wert automatisch erstellt. Das Suffix `cdnedge.bluemix.net` wird automatisch an den Wert für CNAME angehängt. Wenn Sie einen ungeeigneten Wert für CNAME verwenden, kann dies zur Beendigung von Services führen.

       ![Namen konfigurieren](images/configure-hostname-cname.png)  

    **Hinweis**: Nach der Bereitstellung Ihres neuen CDN **müssen** Sie den CNAME mit Ihrem DNS-Anbieter konfigurieren.

**Schritt 3:**

Füllen Sie das Feld **Ursprung konfigurieren** aus: Zum Konfigurieren dieses Felds müssen Sie entweder die Option **Server** oder die Option **Objektspeicher** auswählen.  

  * **Schritt 3, Option 1: Serveroption**

     ![Ursprungsserver konfigurieren](images/configure-origin-server.png)

      * Sie müssen die **Adresse des Ursprungsservers** (Hostname oder IPv4-Adresse des Ursprungsservers) angeben. Wenn **HTTPS-Port** ebenfalls ausgewählt ist, muss die **Adresse des Ursprungsservers** der Hostname und nicht die IP-Adresse sein.

      * Geben Sie einen Wert für **Host-Header** (optional) an. Wenn kein Wert angegeben wird, wird standardmäßig der **Hostname** verwendet. Weitere Informationen zum Host-Header finden Sie in der Funktionsbeschreibung zur [Host-Header-Unterstützung](feature-descriptions.html#host-header-support).  

      * Geben Sie einen **Pfad** an, über den Inhalte des Ursprungsservers abgerufen werden können (optional). Wählen Sie die Featurebeschreibung für [Pfadbasierte CDN-Zuordnungen](feature-descriptions.html#path-based-cdn-mappings) aus und informieren Sie sich über die Auswirkungen, die das Hinzufügen eines Pfads zu diesem Zeitpunkt hat.

      * Sie können außerdem einen **HTTP-Port** und/oder einen **HTTPS-Port** angeben. Diese Felder geben an, welches Protokoll und welche Portnummer zum Verbinden mit dem Ursprungsserver verwendet werden kann. Für nicht standardmäßig verwendete Portnummern finden Sie in [den häufig gestellten Fragen](faqs.html#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) eine Liste der zulässigen Portnummern.

      * Ein **SSL-Zertifikat** wird _nur_ angezeigt, wenn ein HTTPS-Port ausgewählt ist. Wenn Sie **HTTPS-Port** für den Server- oder Objektspeicher auswählen, können Sie **Wildcard-Zertifikat** oder **DV-SAN-Zertifikat** als SSL-Zertifikatsoption auswählen. Beide Optionen bieten die erweiterten Sicherheitsfunktionen von HTTPS.
        * Ein **Wildcard-Zertifikat** ermöglicht HTTPS-Datenverkehr nur bei Verwendung eines **CNAME** und erfordert keine weiteren Aktionen.
        * Ein **DV-SAN-Zertifikat** ermöglicht HTTPS-Datenverkehr über Ihre Domäne, es müssen jedoch noch weitere Schritte zur Überprüfung ausgeführt werden. Informationen zu den bei dieser Option erforderlichen Schritten und den implizierten Zeiteinschränkungen finden Sie unter [Validierung der Domänensteuerung für HTTPS durchführen](how-to-https.html#completing-domain-control-validation-for-https).

	     ![Ursprungsserver konfigurieren](images/ssl-cert-options.png)

  * **Schritt 3, Option 2: Objektspeicheroption**

    ![Objektspeicher konfigurieren](images/configure-origin-object-storage.png)

      * Sie **müssen** den **Endpunkt** angeben, bei dem das Objekt abgerufen werden soll.

      * Geben Sie einen Wert für **Host-Header** (optional) an. Wenn kein Wert angegeben wird, wird standardmäßig der **Hostname** verwendet. Weitere Informationen zum Host-Header finden Sie in der Funktionsbeschreibung zur [Host-Header-Unterstützung](feature-descriptions.html#host-header-support).  

      * Geben Sie einen **Pfad** an, über den Inhalte des Ursprungsservers abgerufen werden können (optional). Wählen Sie die Featurebeschreibung für [Pfadbasierte CDN-Zuordnungen](feature-descriptions.html#path-based-cdn-mappings) aus und informieren Sie sich über die Auswirkungen, die das Hinzufügen eines Pfads an diesem Punkt hat.

      * Sie **müssen** den Namen für den **Bucket** angeben, in dem Ihr Inhalte gespeichert werden. 

      * Sie können außerdem einen **HTTP-Port** und/oder einen **HTTPS-Port** angeben. Diese Felder geben an, welches Protokoll und welche Portnummer zum Verbinden mit dem Ursprungsserver verwendet werden kann. Für nicht standardmäßig verwendete Portnummern finden Sie in [den häufig gestellten Fragen](faqs.html#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) eine Liste der zulässigen Portnummern.

      * Ein **SSL-Zertifikat** wird _nur_ angezeigt, wenn der HTTPS-Port ausgewählt ist. Wenn Sie **HTTPS-Port** für den Server- oder Objektspeicher auswählen, können Sie **Wildcard-Zertifikat** oder **DV-SAN-Zertifikat** als SSL-Zertifikatsoption auswählen. Beide Optionen bieten die erweiterten Sicherheitsfunktionen von HTTPS.
        * Ein **Wildcard-Zertifikat** ermöglicht HTTPS-Datenverkehr nur bei Verwendung eines **CNAME** und erfordert keine weiteren Aktionen.
        * Ein **DV-SAN-Zertifikat** ermöglicht HTTPS-Datenverkehr über Ihre Domäne, es müssen jedoch noch weitere Schritte zur Überprüfung ausgeführt werden. Informationen zu den bei dieser Option erforderlichen Schritten und den implizierten Zeiteinschränkungen finden Sie unter [Validierung der Domänensteuerung für HTTPS durchführen](how-to-https.html#completing-domain-control-validation-for-https).

        ![HTTPS konfigurieren](images/ssl-cert-options.png)

      **HINWEIS**: Sie müssen in der **Zugriffssteuerungsliste** (ACL) für jedes Objekt in Ihrem Bucket die Berechtigung "public-read" mit Ihrem Provider für Cloudobjektspeicher konfigurieren. 

Wählen Sie die Schaltfläche **Erstellen** in der rechten unteren Ecke aus, um Ihr CDN zu erstellen.
