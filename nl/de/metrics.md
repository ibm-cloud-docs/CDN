---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-25"

keywords: metrics, bandwidth, overview, hit ratio, edge server, cache, ingress, hits

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Metriken
{: #metrics}

Wenn Sie Ihr CDN zum ersten Mal in der Liste auswählen, wird die Übersichtsseite angezeigt. Auf dieser Seite werden für den ausgewählten Zeitraum (Standardzeitraum: 30 Tage) Werte für die Gesamtbandbreite, die Gesamtzahl der Cache-Treffer und die Trefferquote angezeigt.

  ![Metrikübersicht](images/metrics-overview.png)

Direkt unterhalb der Übersicht wird eine grafische Darstellung zu der Gesamtbandbreite nach Region, der Gesamtzahl der Cache-Treffer und den Treffern nach Typ angezeigt.

  ![Metrikdiagramme](images/metrics-graphs.png)

**Hinweis:** Nach dem Erstellen eines CDN kann es bis zu 48 Stunden dauern, bevor die Metriken angezeigt werden.

## Gibt es eine Mindestanzahl von Tagen, für die ich Metriken anzeigen kann? Gibt es einen Maximalwert?

Es gibt eine minimale und eine maximale Anzahl von Tagen, für die Sie Metriken mit {{site.data.keyword.cloud}} Content Delivery Network und Akamai anzeigen können. Metriken können für einen Mindestzeitraum von 7 Tagen erfasst werden. Der maximale Zeitraum, für den Metriken angezeigt werden können, beträgt 90 Tage. Falls Sie die API verwenden, werden 89 Tage als Maximalwert empfohlen, um Zeitzonenabweichungen zu berücksichtigen.

## Warum ist die Trefferquote ungleich null, wenn die Gesamtanzahl der Treffer Null ist?
Die Trefferquote stellt den Prozentsatz der Male dar, die Inhalte anstatt aus dem Ursprungsserver aus dem Edge-Server-Cache bereitgestellt wurden. Sie wird wie folgt berechnet:

`((Edge-Treffer - Eingangstreffer)/Edge-Treffer) * 100`

Dabei gilt:

Die Definition für _Edge-Treffer_ lautet "Alle Treffer für Edge-Server von Endbenutzern".  
Die Definition für _Eingangstreffer_ lautet "Ursprungs- oder Eingangstreffer im Datenverkehr vom Ursprungsserver zu Akamai-Edge-Servern".

Da die Trefferquote nicht pro CDN, sondern auf Kontoebene berechnet wird, ist die Trefferquote für alle CDNs in Ihrem Konto gleich. Diese Tatsache erklärt auch, warum die Trefferrate ungleich null sein kann, wenn die Anzahl der Edge-Treffer für einen bestimmten CDN Null ist.

## Werden die Metriken in Echtzeit aktualisiert?

Nein. Die Metriken werden alle 24 Stunden aktualisiert.
