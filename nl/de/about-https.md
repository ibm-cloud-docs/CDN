---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-21"

keywords: wildcard certificate, https, san certificate

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note .note}
{:download: .download}

# Informationen zu HTTPS
{: #about-https}

{{site.data.keyword.cloud}} bietet zwei Möglichkeiten zum Sichern von CDN mit HTTPS - ein Wildcard-Zertifikat oder ein Domain Validation-SAN-Zertifikat (DV-SAN-Zertifikat). Beide HTTPS-Optionen können durch Auswählen von **HTTPS-Port** bei der Konfiguration von CDN konfiguriert werden. Der HTTPS-Standardport ist 443, Sie können aber auch eine abweichende Portnummer zum Durchleiten des HTTPS-Datenverkehrs festlegen. Eine Liste der zulässigen Portnummern finden Sie in den [FAQ](docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-).

Die Entscheidung zwischen **Wildcard-Zertifikat** und **SAN-Zertifikat** für HTTPS richtet sich danach, ob HTTPS-Datenverkehr über den CDN-CNAME oder den CDN-Domänennamen bereitgestellt werden soll. Soll HTTPS-Datenverkehr über den CNAME bereitgestellt werden, wählen Sie **Wildcard-Zertifikat** aus. Wählen Sie **SAN-Zertifikat** aus, wenn HTTPS-Datenverkehr über den CDN-Domänennamen bereitgestellt werden soll.

## Unterstützung von Wildcard-Zertifikaten
{: #wildcard-certificate-support}

**Hinweis**:
Neue CDN-Zuordnungen mit Wildcard-Zertifikat werden zu diesem Zeitpunkt NICHT unterstützt.

Ein Wildcard-Zertifikat ist die einfachste Möglichkeit, Endbenutzern Webinhalte sicher bereitzustellen. Der vollständige CDN-CNAME, einschließlich Wildcard-Zertifikatssuffix, **muss** als Serviceeingangspunkt verwendet werden (zum Beispiel `https://example.cdnedge.bluemix.net`), damit das Wildcard-Zertifikat verwendet wird.

Von IBM Cloud CDN wird das Wildcard-Zertifikat `*.cdnedge.bluemix.net` verwendet. Der CNAME wird unabhängig davon, ob er für Sie erstellt oder von Ihnen angegeben wurde, mit der Endung `*.cdnedge.bluemix.net` zum Wildcard-Zertifikat hinzugefügt, das auf dem CDN-Edge-Server aufbewahrt wird. So wird der CNAME zur einzigen Möglichkeit für Endbenutzer, HTTPS für CDN zu verwenden.

![Diagramm für HTTP- und Wildcard-Zertifikat](images/state-diagram-wildcard.png)

## Unterstützung des Subject Alternate Name-Zertifikats (SAN-Zertifikats)
{: #san-certificate-suport}

Das SAN-Zertifikat (SAN - Subject Alternative Name) ist ein digitales SSL-Zertifikat, das das Sichern von mehreren Domänen oder Hostnamen durch ein einziges Zertifikat ermöglicht.

Bei der Verwendung des SAN-Zertifikats für HTTPS wird der primäre CDN-Hostname zu dem Zertifikat hinzugefügt, das von einer Zertifizierungsstelle ausgegeben wurde. Auf diese Art können die Benutzer anstatt über den CNAME über den Hostnamen sicher auf den Service zugreifen; Beispiel: `https://www.example.com`.

Falls eine CDN-Anforderung mithilfe eines HTTPS-SAN-Zertifikats durchgeführt wird, durchläuft es einen Prozess, in dem ein Zertifikat angefordert und eine Validierung der Domänensteuerung (Domain Control Validation, DCV) durchgeführt wird. Eine DCV ist der Prozess, mit dessen Hilfe eine Zertifizierungsstelle festlegt, dass Sie berechtigt sind, auf eine Domäne zuzugreifen und diese zu kontrollieren. Sie müssen hierbei selbst aktiv werden, damit dieser Schritt ausgeführt wird. Wenn Sie schließlich über die Kontrolle verfügen, wird das Zertifikat den CDN-Edge-Servern auf der ganzen Welt bereitgestellt. Sobald das Zertifikat erfolgreich bereitgestellt wurde, wird die Verlängerung des Zertifikat automatisch abgewickelt. Weitere Informationen zu dieser Funktion finden Sie in der [Funktionsbeschreibung](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#https-protocol-support). Die Methoden zur Validierung der Domänensteuerung (Domain Control Validation) werden auf der Seite [Validierung der Domänensteuerung für HTTPS ausführen](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#initial-steps-to-domain-control-validation) ausführlicher erklärt.

Sobald das CDN den Status AKTIV aufweist, müssen Sie den CNAME-Eintrag für den CDN-Hostnamen in Ihrem DNS (Domain Name System) beibehalten. Wird der CNAME-Eintrag entfernt, wird der CDN-Hostname möglicherweise innerhalb von 3 Tagen im SAN-Zertifikat entfernt. Ist dies der Fall, wird über den betreffenden CDN-Hostnamen kein HTTPS-Datenverkehr mehr bereitgestellt.
{:note}

![Diagramm für HTTPS mit SAN-Zertifikat](images/state-diagram-san.png)
