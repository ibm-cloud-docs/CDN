---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-05"

keywords: faq, https, wildcard, certificate, san certificate, domain validation, redirect, domains, challenge

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:faq: data-hd-content-type='faq'}

# Häufig gestellte Fragen zu HTTPS
{: #faq-for-https}

## Was ist bei meinem CDN der Unterschied zwischen HTTPS mit Wildcard-Zertifikat und HTTPS mit SAN-Zertifikat?
{: #for-my-cdn-what-is-the-difference-between-https-with-wildcard-certificate-and-https-with-san-certificate}
{:faq}

Bei Nutzung von Wildcard-Zertifikaten verwenden alle Kunden dasselbe Zertifikat, das in den CDN-Netzen des Anbieters bereitgestellt wird. Für den Zugriff auf den Service muss der CNAME mit dem IBM Suffix `.cdnedge.bluemix.net` verwendet werden. Beispiel: `https://www.example-cname.cdnedge.bluemix.net`.

Bei Verwendung eines SAN-Zertifikats wird ein einziges SAN-Zertifikat von mehreren Kundendomänen genutzt; hierzu werden ihre Domänennamen in den SAN-Einträgen hinzugefügt. Auf den Service kann dann mithilfe des Hostnamens zugegriffen werden, zum Beispiel in Form von `https://www.example.com`.

## Wie wird eine Validierung einer Domäne mit Weiterleitung durchgeführt?
{: #how-is-domain-validation-with-redirect-accomplished}
{:faq}

Dies hängt vom Server ab. Die Prozedur zur Ausführung der Domänenvalidierung für Apache- und Nginx-Server finden Sie auf der Seite [Validierung der Domänensteuerung für HTTPS ausführen](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#redirect).

## Wie lange dauert die Validierung einer Domäne?
{: #how-long-does-domain-validation-take}
{:faq}

Eine Domänenvalidierung dauert normalerweise zwischen zwei und vier Stunden, die Dauer hängt jedoch von der für die Validierung gewählten Methode ab. Eine Domänenvalidierung mit CNAME-Validierung ist die schnellste Variante und dauert in der Regel weniger als eine Stunde. Eine Domänenvalidierung mit Standard- und Weiterleitungsmethode dauert in der Regel nach dem Absenden der Abfrage ungefähr vier Stunden.

## Wie lange dauert es, bis HTTPS für mein CDN mit einem DV-SAN-Zertifikat erstellt und aktiviert wird?
{: #how-long-does-it-take-to-create-and-enable-https-for-my-cdn-with-a-dv-san-certificate}
{:faq}

Eine normale Anforderung zur Aktivierung von HTTPS dauert von der ersten Anforderung bis zur Ausführung im Durchschnitt drei bis neun Stunden.

## Wie lange dauert das Löschen eines CDN mit einem DV-SAN-Zertifikat?
{: #how-long-does-it-take-to-delete-a-cdn-with-a-dv-san-certificate}
{:faq}

Das Löschen Ihres CDN erfordert die Entfernung Ihrer Domäne aus dem Zertifikat auf allen Edge-Servern. Die Ausführung dieses Prozesses kann bis zu acht Stunden dauern.

## Verursacht die Nutzung eines DV-SAN-Zertifikats für mein CDN zusätzliche Kosten?
{: #is-there-any-additional-cost-associated-with-using-a-dv-san-certificate-for-my-cdn}
{:faq}

Nein. Die Konfigurationen der DV-SAN-Zertifikate werden im Vergleich zu HTTP oder HTTPS mit einem Wildcard-Zertifikat ohne Aufpreis bereitgestellt.

## Kann mein CDN, das mit HTTPS und einem Wildcard-Zertifikat erstellt wurde, für die Verwendung eines DV-SAN-Zertifikats aktualisiert werden?
{: #can-my-cdn-created-using-https-with-wildcard-be-updated-to-use-a-dv-san-certificate}
{:faq}

Nein, die Zuordnung eines Wildcard-Zertifikats kann nicht in die Zuordnung eines SAN-Zertifikats geändert werden.

## Was ist eine Zertifizierungsstelle?
{: #what-is-a-certificate-authority}
{:faq}

Eine Zertifizierungsstelle (Certificate Authority, CA) ist eine Entität, von der digitale Zertifikate ausgegeben werden.

## Welche Zertifizierungsstelle wird vom {{site.data.keyword.cloud}} CDN-Service zum Ausgeben eines DV-SAN-Zertifikats verwendet?
{: #which-ca-does-ibm-cloud-cdn-service-use-for-issuing-a-dv-san-certificate}
{:faq}

Vom IBM Cloud CDN-Service wird die Zertifizierungsstelle "Let's Encrypt" verwendet.

## Welche SSL-Zertifikate werden für IBM Cloud CDN unterstützt?
{: #what-ssl-certificates-are-supported-for-ibm-cloud-cdn}
{:faq}

Unterstützte SSL-Zertifikate sind Wildcard-Zertifikate und DV-SAN-Zertifikate (DV - Domain Validation, SAN - Subject Alternate Name). Ein SAN-Zertifikat wird von mehreren Kunden gemeinsam genutzt. Von IBM Cloud CDN wird nicht das Hochladen angepasster Zertifikate unterstützt.

## Ich habe eine E-Mail erhalten, in der ich aufgefordert werde, eine Abfrage zur Domänenvalidierung in Bezug auf mein CDN zu bearbeiten. Was kann ich tun?
{: #i-received-an-email-asking-me-to-address-a-domain-validation-challenge-related-to-my-cdn}
{:faq}

Eine Domänenvalidierung kann auf drei Arten durchgeführt werden: Mit CNAME, mit der Standardmethode oder direkt.

Details zur Ausführung dieser drei Varianten finden Sie unter [Validierung der Domänensteuerung für HTTPS ausführen](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#initial-steps-to-domain-control-validation).

## Was geschieht, wenn ich die Abfrage zur Domänenvalidierung für mein CDN nicht bearbeite?
{: #what-will-happen-if-i-dont-address-the-challenge-for-domain-validation-of-my-cdn}
{:faq}

Falls der Zuordnungsstatus länger als 48 Stunden DOMAIN_VALIDATION_PENDING lautet, wird die Erstellung der Zuordnung abgebrochen, und der Status der Zuordnung wechselt zu CREATE_ERROR. In diesem Status können Sie die Erstellung erneut versuchen oder die Zuordnung löschen.

## Muss bei Verwendung eines Wildcard-Zertifikats eine Domäne für mein CDN validiert werden?
{: #does-a-wildcard-certificate-need-to-validate-a-domain-for-my-cdn}
{:faq}

Nein, zum Abrufen des Inhalts vom Ursprungsserver brauchen Sie nur den CNAME. `https://www.example-cname.cdnedge.bluemix.net`

## Ich habe eine E-Mail erhalten, die darauf hinweist, dass meine Domänen nicht auf IBM CDN CNAME verweisen. Was kann ich tun?
{: #i-received-an-email-indicating-that-my-domain-is-not-pointed-to-IBM-CDN-CNAME}
{:faq}

Diese E-Mail bedeutet, dass Ihr CDN nicht verwendet wird. Um die CDN zu verwenden und die Domänen in den Zertifikaten zu aktivieren, müssen Sie die aufgelisteten CNAME-DNS-Datensätze in Ihrem DNS-Providersystem festlegen.
Wenn Sie diese Aktion innerhalb von 7 Tagen ausführen, werden sowohl der HTTP- als auch der HTTPS-Datenverkehr für Ihr CDN wiederhergestellt, und das CDN wird in den Status RUNNING (AKTIV) versetzt. Wenn das CDN nach 7 Tagen nicht verwendet wird, müssen wir HTTPS für Ihre CDN-Domäne dauerhaft inaktivieren, um zu verhindern, dass Ihre nicht verwendete Domäne neue CDN-Domänenanforderungen blockiert, die zum gemeinsam genutzten SAN-Zertifikat hinzugefügt werden sollen. Der HTTP-Datenzugriff über CDN kann später noch wiederhergestellt werden, indem ein CNAME-Datensatz für Ihre Domäne hinzugefügt wird. Details zur Ausführung dieser Situation finden Sie unter [Validierung der Domänensteuerung für HTTPS ausführen](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#cname).

## Kann ich bei Nutzung des SAN-Zertifikatstyps für mein CDN dennoch den CNAME für den Zugriff auf meinen Service verwenden?
{: #if-i-use-a-san-certificate-type-for-my-cdn-can-i-still-use-the-cname-for-access-to-my-service}
{:faq}

Nein. Bei Nutzung eines SAN-Zertifikats können Sie nur die angepasste Domäne für den Zugriff auf den Inhalt vom Ursprungsserver verwenden.

## Werden alle meine IBM Cloud CDN-Domänen in einem einzigen Zertifikat hinzugefügt?
{: #are-all-of-my-ibm-cloud-cdn-domains-added-into-one-certificate}
{:faq}

Nicht unbedingt. Die Zertifikatsauswahl wird von Akamai ausgeführt, um sicherzustellen, dass sich die Zertifikate im effizientesten Status befinden. Die Domänen werden in den unterschiedlichen Zertifikaten in einem ausgewogenen Verhältnis hinzugefügt; somit kann nicht garantiert werden, dass sich alle Domänen in demselben Zertifikat befinden.

## Warum wird "Wildcard" angezeigt, wenn ich `dig` verwende oder wenn ich versuche, auf Inhalt zuzugreifen und sich die CDN-Instanz im Status `Zertifikat wird angefordert`, `Domänenvalidierung anstehend` oder `Zertifikat wird bereitgestellt` befindet?
{: #why-do-i-see-wildcard}
{:faq}

Während des Anforderungsprozesses für das DV-SAN-Zertifikat wird die DNS-Datensatzkette für Ihr CDN vorübergehend mit einem Wildcard-Zertifikat verkettet. Bis der Prozess abgeschlossen ist, wird der Inhalt temporär über dieses Wildcard-Zertifikat bereitgestellt. Sobald der Anforderungsprozess abgeschlossen ist, wird die DNA-Datensatzkette aktualisiert und eine Verkettung mit dem DV-SAN-Zertifikat der CDN-Instanz durchgeführt.
