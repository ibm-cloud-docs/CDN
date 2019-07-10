---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-21"

keywords: running status, additional steps, stop cdn, learn, configure cname, delete cdn, start cdn

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:warning: .warning}
{:download: .download}

# Wechsel von CDN in den Ausführungsstatus
{: #getting-your-cdn-to-running-status}

In diesem Abschnitt erfahren Sie anhand der folgenden Anleitungen, was Sie tun müssen, damit CDN in den Ausführungsstatus (AKTIV) wechselt. Außerdem wird in diesem Dokument beschrieben, wie CDN gestartet und gestoppt wird.

## In den Ausführungsstatus wechseln
{: #get-to-running}

Nachdem Sie ein CDN erstellt haben, wird es in Ihrem CDN-Dashboard aufgeführt. Dort werden CDN-Name, Ursprung, Anbieter und Status angezeigt.  

 ![Screenshot der Zuordnungsliste](images/mapping-list.png)


Wenn Sie CDN mit HTTP bzw. HTTPS mit einem Wildcard-Zertifikat bestellt haben, können Sie mit Schritt 1 fortfahren.

Wenn Sie CDN mit einem HTTPS-DV-SAN-Zertifikat erstellt haben, können zusätzliche Schritte zum Überprüfen Ihrer Domäne erforderlich sein; Informationen zu diesen Schritten finden Sie auf der Seite [Validierung der Domänensteuerung für HTTPS ausführen](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#completing-domain-control-validation-for-https).

**Schritt 1:**

Nachdem Sie ein CDN bestellt haben, müssen Sie in **CNAME** Ihren DNS-Anbieter konfigurieren. Die meisten DNS-Anbieter stellen Anweisungen zum Festlegen oder Ändern von CNAME zur Verfügung.

   * Während dieses Vorgangs wird der Status **CNAME Configuration** (CNAME wird konfiguriert) für Ihr CDN angezeigt. Wenden Sie sich an Ihren DNS-Anbieter, um zu erfahren, wann die Änderungen wirksam werden.

   ![CNAME config](images/cname-config.png)  

**Schritt 2:**

Nach dem Konfigurieren des CNAME-Werts mit Ihrem DNS-Anbieter können Sie jederzeit den Status überprüfen, indem Sie im Überlaufmenü rechts neben dem CDN-Status die Option zum Abrufen des Status**** auswählen.

  ![CNAME getStatus](images/cname-getstatus.png)  

**Schritt 3:**

Wenn die CNAME-Verkettung abgeschlossen ist, wechselt der Status bei Auswahl von **Status abrufen** zu *AKTIV* und CDN kann verwendet werden.

Glückwunsch! Ihr CDN ist nun aktiv. Ab jetzt werden auf der Seite [CDN verwalten](/docs/infrastructure/CDN?topic=CDN-manage-your-cdn#manage-your-cdn) zusätzliche Informationen zu Konfigurationsoptionen angezeigt, zum Beispiel [Lebensdauer](docs/infrastructure/CDN?topic=CDN-manage-your-cdn#setting-content-caching-time-using-time-to-live-), [Cacheinhalt bereinigen](/docs/infrastructure/CDN?topic=CDN-manage-your-cdn#purging-cached-content) und [Details für Ursprungspfad hinzufügen](/docs/infrastructure/CDN?topic=CDN-manage-your-cdn#adding-origin-path-details).

## CDN starten
{: #starting-cdn}

Beim CDN-Start wird das DNS benachrichtigt, dass Datenverkehr vom Ursprungsserver an den Akamai-Edge-Server weitergeleitet werden soll. Sobald die Zuordnung gestartet wurde, wird vom DNS-Cache möglicherweise weiterhin Datenverkehr an den Ursprungsserver geleitet. Die ordnungsgemäße Funktionsweise ist deshalb unmittelbar nach dem Start der Zuordnung möglicherweise nicht sofort für die Domäne erkennbar. Die für die Aktualisierung erforderliche Zeit richtet sich danach, wie oft der DNS-Cache aktualisiert wird, und variiert je nach DNS-Anbieter.

Ein CDN kann nur gestartet werden, wenn es den Status `Gestoppt` aufweist.
{: note}

**Schritt 1:**

Klicken Sie auf **CDN starten** im Überlaufmenü, das rechts neben der CDN-Zeile in Form von drei Punkten angezeigt wird.

  ![Überlaufmenü](images/start_cdn.png)

**Schritt 2:**

In einem größeren Dialogfenster werden Sie aufgefordert, das Starten des Service zu bestätigen. Wählen Sie **Bestätigen** aus, um fortzufahren.

**Schritt 3:**

Wenn die Aktion erfolgreich ausgeführt wurde, wird in der rechten oberen Ecke der Anzeige ein entsprechendes Dialogfeld mit einer Zeitangabe angezeigt.

**Schritt 4:**

Dieser Schritt ändert den Status in `CNAME-Konfiguration`.

**Schritt 5:**

Klicken Sie im Überlaufmenü auf **Status abrufen**. Dieser Schritt ändert den Status in `Aktiv`. Ihr CDN wird einsatzfähig.

## Ein CDN stoppen
{: #stopping-a-cdn}

Die STOP CDN-Funktionalität ist für Wartungsfenster von maximal 7 Tagen vorgesehen. Nach 7 Tagen muss das CDN gestartet werden. Anderenfalls wird es inaktiviert, und der Datenverkehr zum CDN-CNAME wird nicht an den Ursprungsserver weitergeleitet.
{: important}

Wird eine Zuordnung gestoppt, wechselt die DNS-Suche wieder zum Ursprungsserver. Beim Datenverkehr wird der CDN-Edge-Server ausgelassen und Inhalte werden direkt beim Ursprungsserver abgerufen. Nach dem Stoppen einer Zuordnung gibt es möglicherweise einen kurzen Zeitraum, in dem nicht auf Ihre Inhalte zugegriffen werden kann, da der DNS-Cache u. U. Datenverkehr weiterhin an die Akamai-Edge-Server leitet. Zu diesem Zeitpunkt wird der Datenverkehr für die Domäne jedoch vom Akamai-Edge-Server blockiert. Die Länge diese Zeitraums richtet sich danach, wie oft der DNS-Cache aktualisiert wird, und variiert je nach DNS-Anbieter.

Ein CDN kann nur gestoppt werden, wenn es den Status `Aktiv` aufweist.
{: note}

Es empfiehlt sich **NICHT**, ein CDN zu stoppen, das mit einem HTTPS-SAN-Zertifikat konfiguriert ist, da HTTPS-Datenverkehr möglicherweise nach dem erneuten Aktivieren des CDN (Status `Aktiv`) nicht mehr aufgenommen wird.
{: important}

Das Stoppen eines CDN für eine Wildcard-Domäne ist zu diesem Zeitpunkt **NICHT** zulässig.
{: important}

**Schritt 1:**

Klicken Sie im Überlaufmenü (3 vertikal angeordnete Punkte rechts neben dem CDN-Status) auf 'CDN stoppen'.
 ![Überlaufmenü](images/stop_cdn.png)

**Schritt 2:**

In einem größeren Dialogfenster werden Sie aufgefordert, das Stoppen des Service zu bestätigen. Wählen Sie **Bestätigen** aus, um fortzufahren.

**Schritt 3:**

Nach etwa 5 bis 15 Sekunden müsste der Status in 'Gestoppt' geändert werden.

## Ein CDN löschen
{: #deleting-a-cdn}

Führen Sie die folgenden Schritte aus, um ein CDN zu löschen:

Durch das Auswählen der Option `Löschen` im Überlaufmenü wird nur das CDN gelöscht und nicht Ihr Konto.
{: note}

**Schritt 1:**

Klicken Sie im Überlaufmenü auf 'Löschen'.

 ![Option 'CDN löschen' im Überlaufmenü](images/delete_cdn.png)

**Schritt 2:**

In einem größeren Dialogfenster werden Sie aufgefordert, das Löschen zu bestätigen. Klicken Sie auf **Löschen**, um fortzufahren.

Falls CDN unter Verwendung von HTTPS mit einem DV-SAN-Zertifikat konfiguriert ist, kann es bis zu fünf Stunden dauern, bis der Löschprozess abgeschlossen ist.
{: note}

  ![Löschen mit Warnung](images/delete-with-warning.png)

**Schritt 3:**

Nach Ausführung der Schritte 1 und 2 ändert sich der CDN-Status in `Wird gelöscht`. Klicken Sie nach Abschluss des Löschprozesses im Überlaufmenü auf 'Status abrufen', um die Zeile in der CDN-Liste zu entfernen. Wenn der Löschprozess nicht abgeschlossen ist, ist diese Aktion wirkungslos.
