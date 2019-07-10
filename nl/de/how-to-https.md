---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-21"

keywords: domain, control, validation, https, san certificate, challenge, apache, nginx, redirect

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Validierung der Domänensteuerung für HTTPS mit DV-SAN-Zertifikat durchführen
{: #completing-domain-control-validation-for-https-with-dv-san}

Das folgende Diagramm veranschaulicht die verschiedenen Stufen, die eine CDN-Instanz vom Erstellen bis zum aktiven Status durchläuft.

  ![SAN-Zustandsdiagramm](images/state-diagram-san.png)

## Erste Schritte für die Validierung der Domänensteuerung
{: #initial-steps-to-domain-control-validation}

**Schritt 1:**

Nachdem Sie CDN mit einem DV-SAN-Zertifikat bestellt haben, beginnt die Anforderungsverarbeitung für das Zertifikat. Während dieses Prozesses wird von {{site.data.keyword.cloud}} CDN ein Zertifikat von Akamai angefordert. Sobald ein Zertifikat verfügbar wird, wird von Akamai eine Anforderung an die Zertifizierungsstelle (Certificate Authority, CA) übergeben.

  * Während dieser Zeit wird der CDN-Status **Zertifikat wird angefordert** angezeigt.

    ![Status 'Zertifikat wird angefordert'](images/requesting-cert.png)

**Schritt 2:**

Nach Eingang der Anforderung setzt die Zertifizierungsstelle eine Abfrage zur Validierung der Domäne ab.

  * Wenn dies geschieht, ändert sich der CDN-Status in **Domänenvalidierung erforderlich**.

    ![Domänenvalidierung erforderlich](images/domain-validation-needed.png)

**Schritt 3:**

Klicken Sie auf den Namen der CDN-Instanz, die validiert werden soll. Die Seite 'Übersicht' wird geöffnet; auf ihr wird der allgemeine Status der CDN-Instanz angezeigt. Ganz oben auf der Seite wird ein Alert angezeigt, der Sie darauf hinweist, dass eine Validierung der Domäne erforderlich ist. Wählen Sie die Schaltfläche **Domänenvalidierung anzeigen** aus, um ein Fenster zu öffnen, in dem die Abfrageinformationen angezeigt werden, die Sie für den Validierungsprozess benötigen.

   ![Domänenvalidierung erforderlich](images/view-domain-validation.png)

**Schritt 4:** Sobald Sie einen der Validierungsschritte aus dem Abschnitt zu Abfragen zur Domänenvalidierung ausgeführt haben, wechselt der CDN-Status zu **Zertifikat wird bereitgestellt**. Während dieser Zeit wird das validierte Zertifikat von Akamai an die zugehörigen Edge-Server verteilt. Das Bereitstellen eines Zertifikats kann zwischen zwei und vier Stunden dauern.

  * Wenn dieser Prozess abgeschlossen ist, wechselt der Status aller Domänen unabhängig von der jeweils verwendeten Validierungsmethode zu **CNAME-Konfiguration**.

Weitere Informationen zur CNAME-Konfiguration sowie zur Überwachung der CDN-Instanz finden Sie auf der Seite [In den Ausführungsstatus wechseln](/docs/infrastructure/CDN?topic=CDN-getting-your-cdn-to-running-status#get-to-running).


## Validierung der Domänensteuerung
{: #domain-control-validation}

Um den CDN-Domänennamen, der dem SAN-Zertifikat hinzugefügt wurde, abrufen zu können, müssen Sie nachweisen, dass Sie über eine Verwaltungsberechtigung für Ihre Domäne verfügen. Dieser Prozess zum Erbringen des Nachweises wird als Validierung der Domänensteuerung bezeichnet. Sie müssen die Validierung der Domänensteuerung innerhalb von 48 Stunden durchführen. Wenn Sie diese Frist nicht einhalten, verfällt Ihre Anforderung und Sie müssen den Bestellprozess erneut durchführen. In den folgenden Abschnitten werden die drei Methoden beschrieben, die bei der Validierung der Domänensteuerung zur Auswahl stehen.


### CNAME
{: #cname}

Die im Folgenden beschriebene Methode ist **NUR** empfehlenswert, wenn Ihr CDN **nicht** für Live-Datenverkehr vorgesehen ist. Stellt Ihre Domäne Live-Datenverkehr bereit, ist die Standardmethode oder die Weiterleitungsmethode besser zur Validierung der Domäne geeignet.

Bei dieser Methode müssen Sie einen CNAME-Eintrag für Ihre CDN-Domäne in Ihrer DNS-Konfiguration hinzufügen. Verwenden Sie als CNAME-Wert den Wert für CNAME, den Sie beim Erstellen des CDN verwendet haben. Der Name sollte auf den Domänennamen `cdnedge.bluemix.net` enden. Nun sind keine weiteren Maßnahmen erforderlich. Die Validierung der Domänensteuerung wird von diesem Zeitpunkt an automatisch durchgeführt. Die Validierung kann zwischen zwei und vier Stunden dauern. Sobald das Zertifikat bereitgestellt ist, erhält Ihr CDN sofort den Status 'Aktiv'.

Anweisungen zum Festlegen oder Ändern von CNAME erhalten Sie in der Regel beim DNS-Anbieter. Es folgt ein Beispiel für einen typischen CNAME-Eintrag:

| **Ressourcentyp** | **Host** | **Verweist auf (CNAME)** | **TTL** |
|------------------|---------|-------------|----------------|
| CNAME | www.example.com | example.cdnedge.bluemix.net | 15 Minuten |


---
### Standard
{: #standard}

Falls Sie die Standardmethode zur Domänenvalidierung verwenden, werden im Fenster für die Validierung der Domäne **Abfrage-URL** und **Abfrageantwort** angezeigt. Fügen Sie für den Prozess der Domänenvalidierung die angegebene **Abfrageantwort** zum Ursprungsserver hinzu. Nach dem Hinzufügen kann die Zertifizierungsstelle die **Abfrageantwort** vom Ursprungsserver mithilfe der URL abrufen, die in der **Abfrage-URL** angegeben ist. Wenn der Ursprungsserver ordnungsgemäß konfiguriert ist, kann die Validierung der Domäne zwischen zwei und vier Stunden dauern.

   ![Standardmethode für die Abfrage zur Domänenvalidierung](images/domain-validation-standard.png)

Damit die Ausführung der Domänenvalidierung über die Standardmethode erfolgreich ist, müssen Sie den Ursprungsserver auf eine bestimmte Weise konfigurieren. Im Folgenden werden die Beispielprozeduren für den _Apache(TM)_-Server und den _Nginx(TM)_-Server beschrieben:

**Beispielsituation**
* Ursprungsserver: `www.example.com`
* CDN-Domäne: `cdn.example.com`
* Abfrage-URL: `http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject`
* Abfrageantwort: `examplechallenge`

#### Apache-Konfiguration
{: #apache-configuration}

  * **Schritt 1:** Melden Sie sich an dem System an, auf dem der Apache2-Server ausgeführt wird.

  * **Schritt 2:** Erstellen Sie die Abfrageantwortdatei für die Antwort auf die Abfrage unter `.well-known/acme-challenge/` in dem Verzeichnis für Ihren Websiteinhalt.  Die Standardposition für Apache2-Websiteinhalt ist `/var/www/html/`. In diesem Beispiel wird die Abfrageantwort im Verzeichnis `/var/www/html/.well-known/acme-challenge/` gespeichert.

      ```
      mkdir -p /var/www/html/.well-known/acme-challenge
      printf "examplechallenge" > /var/www/html/.well-known/acme-challenge/examplechallenge-fileobject
      ```

  * **Schritt 3:** Öffnen Sie bei Bedarf die Apache2-Serverkonfigurationsdatei. Die Standardpositionen für Konfigurationsdateien sind `/etc/apache2/apache2.conf` und `/etc/apache2/sites-enabled/`.

  * **Schritt 4:** Fügen Sie bei Bedarf die CDN-Domäne als zusätzlichen Serveralias (**ServerAlias**) zum virtuellen Host für den Ursprungsserver hinzu.

  * **Schritt 5:** Falls Sie die Apache2-Serverkonfiguration ändern mussten, starten Sie den Apache2-Server mit minimaler Ausfallzeit mithilfe des folgenden Befehls erneut:

      ```
      apachectl -k graceful
      ```

  * **Schritt 6:** Erstellen Sie einen A-Datensatz im DNS zwischen der CDN-Domäne und der IP-Adresse des Ursprungsservers.

#### Nginx-Konfiguration
{: #nginx-configuration}

  * **Schritt 1:** Melden Sie sich an dem System an, auf dem der Nginx-Server ausgeführt wird.

  * **Schritt 2:** Erstellen Sie die Abfrageantwortdatei für die Antwort auf die Abfrage unter `.well-known/acme-challenge/` in dem Verzeichnis für Ihren Websiteinhalt.  Die Standardposition für Nginx-Websiteinhalt ist `/usr/share/nginx/html/`.  In diesem Beispiel wird die Abfrageantwort im Verzeichnis `/usr/share/nginx/html/.well-known/acme-challenge/` gespeichert.
      ```
      mkdir -p /usr/share/nginx/html/.well-known/acme-challenge
      printf "examplechallenge" > /usr/share/nginx/html/.well-known/acme-challenge/examplechallenge-fileobject
      ```

  * **Schritt 3:** Öffnen Sie bei Bedarf die Nginx-Serverkonfigurationsdatei. Die Standardpositionen für Konfigurationsdateien sind `/etc/nginx/nginx.conf` und `/etc/nginx/conf.d/`.

  * **Schritt 4:** Fügen Sie bei Bedarf die CDN-Domäne als zusätzlichen Servernamen (**server_name**) zum virtuellen Host für den Ursprungsserver hinzu.

  * **Schritt 5:** Falls Sie die Nginx-Serverkonfiguration ändern mussten, starten Sie den Nginx-Server mit minimaler Ausfallzeit mithilfe des folgenden Befehls erneut:

      ```
      nginx -s reload
      ```

  * **Schritt 6:** Erstellen Sie einen A-Datensatz im DNS zwischen der CDN-Domäne und der IP-Adresse des Ursprungsservers.

#### Stellen Sie sicher, dass diese Standardmethode zur Ausführung der Domänenvalidierung von der Zertifizierungsstelle verwendet werden kann.
{: #verify-that-this-standard-method-to-address-domain-validation-is-ready-for-the-ca}

* Stellen Sie mit einem `curl`-Befehl sicher, dass diese Methode funktioniert, indem Sie den betreffenden Befehl für die Abfrage-URL ausführen.
    ```
    curl -v http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```
* Wenn Sie sicherstellen möchten, dass diese Methode in einem Browser funktioniert, versuchen Sie, die Abfrage-URL in einem Browser aufzurufen.

In beiden Fällen müssen Sie in der Lage sein, die Kopie des Dateiobjekts für die Abfrage der Domänenvalidierung abrufen zu können, die auf dem Ursprungsserver gespeichert ist.

#### Bereinigung für die Standardmethode
{: #clean-up-for-the-standard-method}

Gehen Sie wie folgt vor, wenn für die CDN-Instanz der Status **Zertifikat wird bereitgestellt** angezeigt wird:
1. Entfernen Sie die Datei `examplechallenge-fileobject` (optional).
1. Entfernen Sie den hinzugefügten Serveralias (ServerAlias für Apache2) oder den Servernamen (server_name für Nginx) aus der Serverkonfiguration, sofern dies erforderlich ist (optional).
1. Entfernen Sie den A-Datensatz zwischen der CDN-Domäne und der IP-Adresse des Ursprungsservers.

---
### Weiterleitung
{: #redirect}

Wenn Sie auf die Registerkarte **Weiterleiten** klicken, werden alle Informationen angezeigt, die für die Verarbeitung der Domänenvalidierung über die Weiterleitung erforderlich sind. Anhand dieser Informationen kann die Zertifizierungsstelle eine Kopie der **Abfrageantwort** von Akamai über den Ursprungsserver abrufen. Wenn der Server ordnungsgemäß konfiguriert ist, kann die Validierung der Domäne zwischen zwei und vier Stunden dauern.

   ![Weiterleitungsmethode für die Abfrage zur Domänenvalidierung](images/domain-validation-redirect.png)

Damit die Ausführung der Domänenvalidierung unter Verwendung der Weiterleitungsmethode erfolgreich ist, müssen Sie auf dem Web-Server unter Umständen eine besondere Konfiguration durchführen. Die nachfolgenden Abschnitte enthalten die Beispielprozeduren für Apache- und Nginx-Server.

**Beispielsituation**
* Ursprungsserver: `www.example.com`
* CDN-Domäne: `cdn.example.com`
* Abfrage-URL: `http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject`
* URL-Weiterleitung: `http://dcv.akamai.com/.well-known/acme-challenge/examplechallenge-fileobject`

#### Konfiguration der Apache-Weiterleitung
{: #apache-redirect-configuration}

  * **Schritt 1:** Melden Sie sich an dem System an, auf dem der Apache2-Server ausgeführt wird.

  * **Schritt 2:** Öffnen Sie die Apache2-Serverkonfigurationsdatei. Die Standardpositionen für die Konfigurationsdatei ist `/etc/apache2/apache2.conf` und `/etc/apache2/sites-enabled/`.

  * **Schritt 3:** Fügen Sie die Weiterleitungsanweisung an der entsprechenden Position mit der Konfigurationsdatei hinzu. Fügen Sie bei Bedarf die CDN-Domäne als zusätzlichen Serveralias (**ServerAlias**) zum virtuellen Host für den Ursprungsserver hinzu.

    ```
    Redirect /.well-known/acme-challenge/examplechallenge-fileobject http://dcv.akamai.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```

  * **Schritt 4:** Starten Sie den Apache2-Server mit minimaler Ausfallzeit mithilfe des folgenden Befehls erneut:

    ```
    apachectl -k graceful
    ```

  * **Schritt 5:** Erstellen Sie einen A-Datensatz im DNS zwischen der CDN-Domäne und der IP-Adresse des Ursprungsservers.

#### Konfiguration der Nginx-Weiterleitung
{: #nginx-redirect-confguration}

  * **Schritt 1:** Melden Sie sich an dem System an, auf dem der Nginx-Server ausgeführt wird.

  * **Schritt 2:** Öffnen Sie die Nginx-Serverkonfigurationsdatei. Die Standardpositionen für Konfigurationsdateien sind `/etc/nginx/nginx.conf` und `/etc/nginx/conf.d/`.

  * **Schritt 3:** Für diesen Schritt stehen zwei gleichermaßen zulässige Methoden zur Verfügung.

    * Option 1: (Empfohlen) Fügen Sie den Block `location` mit der Anweisung `return` zur Ausführung der Weiterleitung mit dem entsprechenden Block `server` hinzu. Fügen Sie bei Bedarf die CDN-Domäne als zusätzlichen Servernamen (**server_name**) zum Serverblock für den Ursprungsserver hinzu.

    ```
    server {
      listen 80;
      server_name www.example.com cdn.example.com;

      # Einige Serverkonfigurationsanweisungen
      # ...

      location = /.well-known/acme-challenge/examplechallenge-fileobject  {
          return 302 http://dcv.akamai.com$request_uri;
      }

      # Noch mehr Serverkonfigurationsanweisungen
      # ...
    }
    ```

   * Option 2: Fügen Sie die Anweisung `rewrite` im Block `server` hinzu. Fügen Sie bei Bedarf die CDN-Domäne als zusätzlichen Servernamen (**server_name**) zum Serverblock für den Ursprungsserver hinzu.

    ```
    server {
      listen 80;
      server_name www.exmaple.com cdn.example.com;

      # Einige Serverkonfigurationsanweisungen
      # ...

      rewrite ^/(\.well-known/acme-challenge/examplechallenge-fileobject)$ http://dcv.akamai.com/$1 redirect;

      # Noch mehr Serverkonfigurationsanweisungen
      # ...
    }
    ```

  * **Schritt 4:** Starten Sie den Nginx-Server mit minimaler Ausfallzeit mithilfe des folgenden Befehls erneut:

    ```
    nginx -s reload
    ```

  * **Schritt 5:** Erstellen Sie einen A-Datensatz im DNS zwischen der CDN-Domäne und der IP-Adresse des Ursprungsservers.

#### Funktion der Weiterleitung sicherstellen
{: #verify-that-the-redirect-is-occurring}

Mit diesen Schritten wird _ausschließlich_ der Datenverkehr für eine bestimmte Abfrage-URL zur URL-Weiterleitung umgeleitet. Sie können mithilfe von `curl` oder über den Browser überprüfen, ob die Weiterleitung ordnungsgemäß funktioniert.

* Stellen Sie mit einem `curl`-Befehl sicher, dass die Weiterleitung funktioniert, indem Sie den folgenden Befehl für die Abfrage-URL ausführen.

    ```
    curl -vL http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```

* Wenn Sie über den Browser sicherstellen möchten, dass die Weiterleitung funktioniert, versuchen Sie, die Abfrage-URL über den Browser zu erreichen.

In beiden Fällen sollten Sie in der Lage sein, die Kopie des Dateiobjekts für die Abfrage der Domänenvalidierung von Akamai unter der Domäne `dcv.akamai.com` abzurufen, zu der die ursprüngliche Anforderung weitergeleitet wurde.

#### Bereinigung für Weiterleitungsmethode
{: #clean-up-for-the-redirect-method}

Gehen Sie wie folgt vor, wenn für die CDN-Instanz der Status **Zertifikat wird bereitgestellt** angezeigt wird:
1. Entfernen Sie die Weiterleitungsanweisungen aus der Konfigurationsdatei (optional).
1. Entfernen Sie den hinzugefügten Serveralias (ServerAlias für Apache2) oder den Servernamen (server_name für Nginx) aus der Serverkonfiguration, sofern dies erforderlich ist (optional).
1. Entfernen Sie den A-Datensatz zwischen der CDN-Domäne und der IP-Adresse des Ursprungsservers.
