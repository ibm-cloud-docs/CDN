---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-05-21"

keywords: manage, time to live, origin path, cache key, server, object storage, bucket, configuration, details, updating

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# CDN verwalten
{: #manage-your-cdn}

In diesem Dokument werden allgemeine Tasks für die Verwaltung Ihres CDN beschrieben.

## Zeitdauer für Inhalts-Caching mit 'Time To Live' festlegen
{: #setting-content-caching-time-using-time-to-live}

Wenn Ihr CDN aktiv ist, können Sie die Zeitdauer für das Inhalts-Caching über die Lebensdauer (Time To Live, TTL) festlegen. Die Lebensdauer für eine bestimmte Datei bzw. einen bestimmten Verzeichnispfad gibt an, wie lange der zugehörige Inhalt im Cache gespeichert werden soll. Beim Erstellen der CDN-Zuordnung wurde für die Lebensdauer (TTL) ein globaler Standardwert von 3600 Sekunden (1 Stunde) erstellt.

**Schritt 1:**  

Wählen Sie auf der Seite 'CDN' Ihr CDN aus, um die Seite **Übersicht** zu öffnen.

**Schritt 2:**  

Sie können den Zeitraum mithilfe der Pfeile anpassen, oder indem Sie einen neuen Wert eingeben. Der Zeitwert wird in Sekunden angegeben. Beispiel: 3600 Sekunden entsprechen einer Stunde. Der niedrigste verfügbare Wert für `timeToLive` beträgt 0 Sekunden, der höchste Wert 2147483647 Sekunden (etwa 24855 Tage). Wählen Sie die Schaltfläche **Speichern** aus, um den Zeitraum für das Inhalts-Caching festzulegen.

  ![TTL hinzufügen](images/adding-path.png)

**Schritt 3:**

Nach dem Speichern können Sie die Einstellung für TTL mit den Optionen **Bearbeiten** bzw. **Löschen** des Überlaufmenüs bearbeiten oder löschen. (**Hinweis**: Der Pfad für TTL kann nicht geändert werden. Wenn der Zuordnungspfad geändert wird, wird der Pfad für TTL automatisch aktualisiert.)

  ![TTL bearbeiten oder löschen](images/edit-delete-ttl-setting.png)  

  * Wenn der Inhalt mehreren Regeln entspricht, erhält die zuletzt hinzugefügte Konfiguration Vorrang.

  * TTL-Werte können nur für einen bestimmten Dateinamen oder ein bestimmtes Verzeichnis festgelegt werden. Reguläre Ausdrücke werden nicht unterstützt, da sie zu unvorhersehbarem Verhalten führen können.

## Details für Ursprungspfad hinzufügen
{: #adding-origin-path-details}

Wenn Ihr CDN den Status *CNAME_Configuration* (CNAME wird konfiguriert) oder *Running* (Aktiv) aufweist, können Sie Details für den Ursprungspfad hinzufügen. Sie können angeben, dass Inhalte von mehreren Ursprungsservern bereitgestellt werden sollen. Zum Beispiel können Fotos von einem anderen Server bereitgestellt werden als Videos. Als Ursprung kann ein Host-Server oder ein Objektspeicher dienen.

Das CDN führt eine URL-Umsetzung für den Ursprungsserver durch. Beispiel: Wenn der Ursprungsserver `xyz.example.com` mit dem Pfad `/example/*` hinzugefügt wurde und ein Benutzer die URL `www.example.com/example/*` öffnet ruft der CDN-Edge-Server den Inhalt von `xyz.example.com/*` ab.
{: note}

**Schritt 1:**

Wählen Sie auf der Seite 'CDN' Ihr CDN aus, um die Seite **Übersicht** zu öffnen.  

**Schritt 2:**

Wählen Sie die Registerkarte **Ursprünge** und anschließend die Schaltfläche **Ursprung hinzufügen** aus. Dieser Schritt öffnet ein neues Dialogfenster, in dem Sie Ihren Ursprung konfigurieren können.  

   ![Ursprünge - Ursprung hinzufügen](images/add-origins.png)

**Schritt 3:**

Sie *müssen* einen Pfad angeben. Sie können optional einen Host-Header angeben.  

   ![Ursprünge - Ursprung hinzufügen](images/add-origin-path.png)

**Schritt 4:**

Wählen Sie entweder **Server** oder **Objektspeicher** aus.

  * Wenn Sie **Server** ausgewählt haben, geben Sie die Adresse des Ursprungsservers als IPv4-Adresse oder den _Hostnamen_ ein. Es wird empfohlen, den Hostnamen und einen vollständig qualifizierten Domänennamen (Fully Qualified Domain Name - FQDN) anzugeben. Geben Sie abhängig von dem Protokoll, das Sie beim Erstellen des CDN ausgewählt haben, auch einen HTTP-Port, einen HTTPS-Port oder beides an. Wenn Sie einen HTTPS-Port verwenden, **muss** die Adresse des Ursprungsservers ein _Hostname_ und keine IP-Adresse sein.

       ![Ursprungsserver hinzufügen](images/add-origin-server-default.png)

  * Wenn Sie **Objektspeicher** ausgewählt haben, geben Sie Endpunkt, Bucketname und HTTPS-Port an. (Optional) Geben Sie an, welche Dateierweiterungen in dem CDN-Service verwendet werden können. Wenn keine Angabe gemacht wird, sind alle Dateierweiterungen zulässig.

       ![Ursprung hinzufügen (Objektspeicher)](images/add-origin-object-storage.png)

  * Die Optionen **Optimierung** und **Cacheschlüssel** sind für die Serverkonfiguration und die Objektspeicherkonfiguration (Object Storage) gleich.

    * Wählen Sie Optionen für **Optimierung** im Dropdown-Menü aus. **Allgemeine Webbereitstellung** ist die Standardoption. Sie können auch die Optimierungen **Große Datei** oder **Video-on-Demand** auswählen. Die Option **Allgemeine Webbereitstellung** gibt dem CDN die Möglichkeit, Inhalt bis zu einer Größe von 1,8 GB zu bedienen, während die Optimierung **Große Datei** Downloads von Dateien mit Größen zwischen 1,8 GB und 320 GB zulässt. **Video-on-Demand** optimiert Ihr CDN für die Bereitstellung von segmentierten Streamingformaten. Die Funktionsbeschreibungen für [Optimierung für große Dateien](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#large-file-optimization) und [Video-on-Demand](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#video-on-demand) enthalten weitere Informationen.

        ![Optionen für die Leistungskonfiguration](images/performance-config-options.png)

    * Wählen Sie Optionen für **Cacheschlüssel** im Dropdown-Menü aus. Die Standardoption ist **Alle einschließen** (Include-all). Wenn Sie die Option **Angegebene einschließen** oder **Angegebene ignorieren** auswählen, **müssen** Sie durch Leerzeichen getrennte Abfragezeichenfolgen für einzuschließende oder zu ignorierende Inhalte eingeben. Beispiel: Geben Sie als einzelne Abfragezeichenfolge `uuid=123456` oder zwei Abfragezeichenfolgen wie `uuid=123456 issue=important` ein.  Weitere Informationen zu [Abfrageargumenten im Cacheschlüssel](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#cache-key-query-args) finden Sie in der Funktionsbeschreibung.

        ![Optionen für Cacheschlüssel](images/cache-key-options.png)

Die Optionen für Protokoll und Port, die in der Benutzerschnittstelle angezeigt werden, entsprechen der Auswahl, die Sie bei der Bestellung des CDN angegeben haben. Beispiel: Wenn **HTTP-Port** bei der Bestellung eines CDN ausgewählt wurde, wird nur die Option **HTTP-Port** für die Funktion 'Ursprung hinzufügen' angezeigt.
{: note}

**Schritt 5:**

Wählen Sie die Schaltfläche **Hinzufügen** aus, um Ihren Ursprungspfad hinzuzufügen.

Wenn Sie Dateierweiterungen für einen Objektspeicherursprungspfad angeben, wird der Geltungsbereich der TTL-Einstellung mit derselben URL wie der Ursprungspfad angepasst, um alle Dateien einzuschließen, die diese angegebenen Dateierweiterungen haben. Beispiel: Wenn Sie den Ursprungspfad `/example` erstellen und die Dateierweiterungen "jpg png gif" angeben, hat der TTL-Wert des TTL-Pfads `/example` einen Geltungsbereich, der alle Dateien mit den Erweiterungen JPG/PNG/GIF unter dem Verzeichnis `/example` und den zugehörigen Unterverzeichnissen einschließt.
{: note}

**Schritt 6:**

Nach dem Hinzufügen können Sie die den Ursprung mit den Optionen **Bearbeiten** oder **Löschen** des Überlaufmenüs bearbeiten oder löschen.

  ![Ursprung bearbeiten oder löschen](images/edit-delete-origin.png)

## Cacheinhalt bereinigen
{: #purging-cached-content}

Wenn Ihr CDN aktiv ist, können Sie Cacheinhalte auf dem Server des Anbieters bereinigen.

**Schritt 1:**

Wählen Sie auf der Seite 'CDN' Ihr CDN aus, um die Seite **Übersicht** zu öffnen.

**Schritt 2:**

Wählen Sie die Registerkarte **Bereinigen** aus.

   ![Seite 'Bereinigen'](images/purge_tab.png)

**Schritt 3:**

Geben Sie unter Verwendung der UNIX-Standardpfadsyntax an, welche Datei bereinigt werden soll, und wählen Sie anschließend die Schaltfläche **Bereinigen** aus. Das Bereinigungen ist gegenwärtig nur für eine einzelne Datei zulässig. Weitere Details zur zulässigen Syntax für den Bereinigungspfad finden Sie auf der Seite [Regeln und Namenskonventionen](/docs/infrastructure/CDN?topic=CDN-rules-and-naming-conventions#what-are-the-rules-for-the-path-string-for-purge-).

**Schritt 4:**

Nach dem Bereinigen wird die Aktivität unter **Bereinigungsaktivität** aufgelistet. Im Überlaufmenü sind die Optionen **Bereinigung wiederholen** und **Favorit** verfügbar.

   ![Bereinigungsaktivität](images/purge-activity.png)

Wenn mehr als 15 Bereinigungsaktionen vorhanden sind wird die Liste 'Bereinigungsaktivität' alle 15 Tage automatisch abgeschnitten.
{: note}

## CDN-Konfigurationsdetails aktualisieren
{: #updating-cdn-configuration-details}

Wenn Ihr CDN aktiv ist, können Sie die CDN-Konfigurationsdetails ändern.

**Schritt 1:**

Wählen Sie auf der Seite 'CDN' Ihr CDN aus, um die Seite **Übersicht** zu öffnen.

**Schritt 2:**

Wählen Sie die Registerkarte **Einstellungen** aus. Die Konfigurationsdetails für Ihr CDN werden angezeigt.

   ![Registerkarte 'Einstellungen'](images/settings-tab.png)  

SSL-Zertifikate werden nur angezeigt, wenn CDN mit HTTPS konfiguriert wurde.
{: note}

Für **Server** können die folgenden Felder geändert werden:
  * Host-Header
  * Adresse des Ursprungsservers
  * HTTP-/HTTPS-Port
  * Nicht aktuelle Inhalte bereitstellen
  * Header beibehalten
  * Optimierungsoptionen
  * Cacheabfrage    

Für **Objektspeicher** (Object Storage) können die folgenden Felder geändert werden:
  * Host-Header
  * Endpunkt
  * Bucketname
  * HTTPS-Port
  * Zulässige Dateierweiterungen
  * Nicht aktuelle Inhalte bereitstellen
  * Header beibehalten
  * Optimierungsoptionen
  * Cacheabfrage

**Schritt 3:**

Aktualisieren Sie bei Bedarf die Details für **Ursprung** oder **Weitere Optionen** und klicken Sie anschließend auf die Schaltfläche **Speichern** in der rechten unteren Ecke, um Ihre CDN-Konfigurationsdetails zu aktualisieren.

   ![Schaltfläche 'Speichern'](images/save-button.png)
