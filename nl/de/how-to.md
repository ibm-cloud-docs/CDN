---

copyright:
  years: 2017
lastupdated: "2017-09-11"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# CDN verwalten

Hier erfahren Sie, wie Sie Ihre CDN-Konfiguration mithilfe der folgenden Richtlinien verwalten können.
 
## CDN in aktiven Status versetzen

Nachdem Sie ein CDN erstellt haben, wird es in Ihrem CDN-Dashboard aufgeführt. Dort werden CDN-Name, Ursprung, Anbieter und Status angezeigt.  

 ![Screenshot der Zuordnungsliste](images/mapping_list_cname.png)

1. Nachdem Sie ein CDN bestellt haben, müssen Sie in **CNAME** Ihren DNS-Anbieter konfigurieren. Die meisten DNS-Anbieter stellen Anweisungen zum Festlegen oder Ändern von CNAME zur Verfügung.
   
   * Während dieses Vorgangs wird der Status **CNAME Configuration** (CNAME wird konfiguriert) für Ihr CDN angezeigt. Wenden Sie sich an Ihren DNS-Anbieter, um zu erfahren, wann die Änderungen wirksam werden.

   ![CNAME config](images/cname-config.png)  
   
2. Nach dem Konfigurieren Ihres DNS-Anbieters in CNAME können Sie jederzeit den Status überprüfen, indem Sie im Überlaufmenü rechts neben dem CDN-Status die Option zum Abrufen des Status**** auswählen.
   
  ![CNAME getStatus](images/cname-getstatus.png)  
    
3. Sobald das Verketten von CNAME abgeschlossen ist, wird der Kontostatus in *RUNNING* geändert und das CDN kann verwendet werden.
   	  
Glückwunsch! Ihr CDN ist nun aktiv.  
	  
## CDN stoppen
Nur ein CDN, das den Status 'Aktiv' aufweist, kann gestoppt werden.. 

1. Klicken Sie im Überlaufmenü (3 vertikal angeordnete Punkte rechts neben dem CDN-Status) auf 'CDN stoppen'.
 ![Überlaufmenü](images/stop_cdn.png)

2. In einem größeren Dialogfenster werden Sie aufgefordert, das Stoppen des Service zu bestätigen. Wählen Sie **Bestätigen** aus, um fortzufahren.
  
3. Nach etwa 5 bis 15 Sekunden müsste der Status in 'Gestoppt' geändert werden.


## CDN starten
Nur ein CDN, das den Status 'Gestoppt' aufweist, kann gestartet werden.
1. Klicken Sie im Überlaufmenü (dargestellt durch 3 Punkte rechts neben der CDN-Zeile) auf 'CDN starten'.
 ![Überlaufmenü](images/start_cdn.png)

2. In einem größeren Dialogfenster werden Sie aufgefordert, das Starten des Service zu bestätigen. Wählen Sie **Bestätigen** aus, um fortzufahren.

3. Wenn die Aktion erfolgreich ausgeführt wurde, wird in der rechten oberen Ecke der Anzeige ein entsprechendes Dialogfeld mit einer Zeitangabe angezeigt.
  
4. Dadurch wird der Status in 'CNAME-Konfiguration' geändert.

5. Klicken Sie im Überlaufmenü auf 'Status abrufen'. Daraufhin wird der Status in 'Aktiv' geändert und CDN wird betriebsbereit.

## CDN löschen

Führen Sie die folgenden Schritte aus, um ein CDN zu löschen.
**Hinweis**: Durch das Auswählen der Option `Löschen` im Überlaufmenü wird nur das CDN gelöscht und nicht Ihr Konto.

1. Klicken Sie im Überlaufmenü auf 'Löschen'.

 ![Option 'CDN löschen' im Überlaufmenü](images/delete_cdn.png)

2. In einem größeren Dialogfenster werden Sie aufgefordert, das Löschen zu bestätigen. Klicken Sie auf **Löschen**, um fortzufahren.

3. Daraufhin wird der Status in 'Wird gelöscht' geändert. Klicken Sie im Überlaufmenü auf 'Status abrufen'. Daraufhin wird die betreffende Zeile aus der CDN-Liste entfernt.

## Zeitdauer für Inhalts-Caching mit 'Time To Live' festlegen

Wenn Ihr CDN aktiv ist, können Sie die Zeitdauer für das Inhalts-Caching über die Lebensdauer (Time To Live, TTL) festlegen. Die Lebensdauer für eine bestimmte Datei bzw. einen bestimmten Verzeichnispfad gibt an, wie lange der zugehörige Inhalt im Cache gespeichert werden soll. Beim Erstellen der CDN-Zuordnung wurde für die Lebensdauer (TTL) ein globaler Standardwert von 3600 Sekunden erstellt. 

1. Wählen Sie auf der Seite 'CDN' Ihr CDN aus, um die Seite **Übersicht** zu öffnen

2. Sie können den Zeitraum mithilfe der Pfeile anpassen, oder indem Sie einen neuen Wert eingeben. Der Zeitwert wird in Sekunden angegeben. Beispiel: 3600 Sekunden entsprechen einer Stunde. Der niedrigste zulässige Wert für `timeToLive` ist 30 Sekunden, der höchste Wert ist 21474836471 Sekunden. Wählen Sie die Schaltfläche **Speichern** aus, um den Zeitraum für das Inhalts-Caching festzulegen. 

  ![TTL hinzufügen](images/adding-path.png)

3. Nach dem Speichern können Sie die Einstellung für TTL mit den Optionen **Bearbeiten** bzw. **Löschen** des Überlaufmenüs bearbeiten oder löschen. (**Hinweis**: Der Pfad für TTL kann nicht geändert werden. Wenn die Zuordnung geändert wird, wird der TTL-Pfad automatisch aktualisiert.)

  ![TTL bearbeiten oder löschen](images/edit-delete-ttl-setting.png)  
	
  * Wenn der Inhalt mit mehreren Regeln übereinstimmt, hat die zuletzt hinzugefügte Konfiguration Vorrang.
  
  * TTL-Werte können nur für einen bestimmten Dateinamen oder ein bestimmtes Verzeichnis festgelegt werden. Reguläre Ausdrücke werden nicht unterstützt, da sie zu unvorhersehbarem Verhalten führen können.
	
## Details für Ursprungspfad hinzufügen

Wenn Ihr CDN den Status *CNAME_Configuration* (CNAME wird konfiguriert) oder *Running* (Aktiv) aufweist, können Sie Details für den Ursprungspfad hinzufügen. Sie können angeben, dass Inhalte von mehreren Ursprungsservern bereitgestellt werden sollen. Zum Beispiel können Fotos von einem anderen Server bereitgestellt werden als Videos. Als Ursprung kann ein Host-Server oder ein Objektspeicher dienen.

1. Wählen Sie auf der Seite 'CDN' Ihr CDN aus, um die Seite **Übersicht** zu öffnen.
	
2. Wählen Sie die Registerkarte **Ursprünge** und anschließend die Schaltfläche **Ursprung hinzufügen** aus.
	
3. Sie *müssen* einen Pfad angeben. Wählen Sie entweder **Server** oder **Objektspeicher** aus.
	
   ![Ursprünge - Ursprung hinzufügen](images/add-origin.png)
		
  * Wenn Sie **Server** ausgewählt haben, geben Sie die IP-Adresse des Servers oder den _Hostnamen_ ein. Geben Sie entsprechend dem Protokoll, das Sie bei der CDN-Erstellung angegeben haben, einen HTTP-Port und/oder einen HTTPS-Port an. (Ihre Angabe sollte der Zuordnung entsprechen.)
	
  ![Ursprungsserver hinzufügen](images/add-origin-server.png)
	
  * Wenn Sie **Objektspeicher** ausgewählt haben, geben Sie Endpunkt, Bucketname und HTTPS-Port an. (Optional) Geben Sie an, welche Dateierweiterungen in dem CDN-Service verwendet werden können. Wenn keine Angabe gemacht wird, sind alle Dateierweiterungen zulässig.
	
  ![Ursprung hinzufügen (Objektspeicher)](images/add-origin-object-storage.png)
		
4. Wählen Sie die Schaltfläche **Hinzufügen** aus, um Ihren Ursprungspfad hinzuzufügen.

  **Hinweis**: Wenn Sie Dateierweiterungen für einen Ursprungspfad im Objektspeicher angeben, wird der TTL-Einstellung mit derselben URL wie der Ursprungspfad ein Bereich zugewiesen, der alle Dateien mit den angegebenen Dateierweiterungen beinhaltet. Beispiel: Wenn Sie den Ursprungspfad "/beispiel" erstellen und als Dateierweiterungen "jpg png gif" angeben, wird dem TTL-Wert des TTL-Pfads "/beispiel" ein Bereich zugewiesen, der alle Dateien mit den Erweiterungen JPG, PNG oder GIF im Verzeichnis "/beispiel" und in den zugehörigen untergeordneten Verzeichnissen einschließt.

5. Nach dem Hinzufügen können Sie die den Ursprung mit den Optionen **Bearbeiten** oder **Löschen** des Überlaufmenüs bearbeiten oder löschen.

  ![Ursprung bearbeiten oder löschen](images/edit-delete-origin.png)
	
## Cacheinhalt bereinigen

Wenn Ihr CDN aktiv ist, können Sie Cacheinhalte auf dem Server des Anbieters bereinigen.  

1. Wählen Sie auf der Seite 'CDN' Ihr CDN aus, um die Seite **Übersicht** zu öffnen.
	
2. Wählen Sie die Registerkarte **Bereinigen** aus.

	![Seite 'Bereinigen'](images/purge_tab.png)
	
3. Geben Sie unter Verwendung der UNIX-Standardpfadsyntax an, welche Datei bereinigt werden soll, und wählen Sie anschließend die Schaltfläche **Bereinigen** aus. Die Bereinigungsfunktion kann gegenwärtig nur auf eine einzelne Datei angewendet werden.

4. Nach dem Bereinigen wird die Aktivität unter **Bereinigungsaktivität** aufgelistet. Im Überlaufmenü sind die Optionen **Bereinigung wiederholen** und **Favorit** verfügbar.

	![Bereinigungsaktivität](images/purge-activity.png)
	
   **Hinweis:** Wenn mehr als 15 Bereinigungsaktionen vorhanden sind wird die Liste 'Bereinigungsaktivität' alle 15 Tage automatisch abgeschnitten.
	
## CDN-Konfigurationsdetails aktualisieren

Wenn Ihr CDN aktiv ist, können Sie die CDN-Konfigurationsdetails ändern.

1. Wählen Sie auf der Seite 'CDN' Ihr CDN aus, um die Seite **Übersicht** zu öffnen.

2. Wählen Sie die Registerkarte **Einstellungen** aus. Die Konfigurationsdetails für Ihr CDN werden angezeigt.

  Für **Objektspeicher** können die folgenden Felder geändert werden: 
   * Endpunkt
   * Bucketname
   * HTTPS-Port
   * Zulässige Dateierweiterungen
   * Nicht aktuelle Inhalte bereitstellen
   * Header beibehalten

  Für **Server** können die folgenden Felder geändert werden: 
   * Adresse des Ursprungsservers
   * HTTP-/HTTPS-Port
   * Nicht aktuelle Inhalte bereitstellen
   * Header beibehalten

3. Aktualisieren Sie bei Bedarf die Details für **Ursprung** oder **Weitere Optionen** und klicken Sie anschließend auf die Schaltfläche **Speichern** in der rechten unteren Ecke, um Ihre CDN-Konfigurationsdetails zu aktualisieren.

   ![Schaltfläche 'Speichern'](images/save-button.png)


## IBM Cloud Object Storage für CDN konfigurieren

Um in IBM Cloud Object Storage gespeicherte Objekte zu verwenden, müssen Sie in der Eigenschaft 'acl' (Access Control List, Zugriffssteuerungsliste) für jedes Objekt in Ihrem Bucket den Zugriff 'public-read' festlegen. 

Informationen zum Installieren der erforderlichen Clients oder Tools finden Sie im Abschnitt über Tools in IBM Cloud Object Storage Developer Center (https://developer.ibm.com/cloudobjectstorage/). Diese Anleitung geht davon aus, dass Sie die offizielle Befehlszeilenschnittstelle AWS installiert haben, die mit der IBM Cloud Object Storage S3-API kompatibel ist.

Der folgende Beispielcode zeigt, wie der Zugriff 'public-read' für alle Objekt in Ihrem Bucket über die Befehlszeilenschnittstelle festgelegt wird.

```
$ export ENDPOINT="YOUR_ENDPOINT"
$ export BUCKET="IHR_BUCKET"
$ KEYS=( "$(aws --endpoint-url "$ENDPOINT" s3api list-objects --no-paginate --query 'Contents[].{key: Key}' --output text --bucket "$BUCKET")" )
$ for KEY in "${KEYS[@]}"
  > do
  >   aws --endpoint-url "$ENDPOINT" s3api put-object-acl --bucket "$BUCKET" --key "$KEY" --acl "public-read"
  > done
```
