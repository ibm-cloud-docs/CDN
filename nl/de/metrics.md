---

copyright:
  years: 2018
lastupdated: "2018-05-03"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Metriken

## Wie kann ich die Metriken und die Nutzung anzeigen?

Die Metriken und die Nutzung können auf der Seite **Übersicht** angezeigt werden, die durch das Auswählen Ihres CDN auf der Seite **Content Delivery Networks** aufgerufen werden kann. **Hinweis:** Nach dem Erstellen eines CDN kann es bis zu 48 Stunden dauern, bevor die Metriken angezeigt werden.

## Ich habe ein CDN erstellt und es hat Datenverkehr über das CDN stattgefunden. Warum werden meine Metriken nicht angezeigt?

Nach der Erstellung eines CDN dauert es 48 Stunden, bis die Metriken angezeigt werden.


## Gibt es eine Mindestanzahl von Tagen, für die ich Metriken anzeigen kann? Gibt es einen Maximalwert?

Es gibt eine minimale und eine maximale Anzahl von Tagen, für die Sie Metriken mit IBM Cloud Content Delivery Network und Akamai anzeigen können. Metriken können für einen Mindestzeitraum von 7 Tagen erfasst werden. Der maximale Zeitraum, für den Metriken angezeigt werden können, beträgt 90 Tage. Falls Sie die API verwenden, werden 89 Tage als Maximalwert empfohlen, um Zeitzonenabweichungen zu berücksichtigen.

## Warum ist die Trefferquote ungleich null, wenn die Gesamtanzahl der Treffer Null ist?
Die Trefferquote stellt den Prozentsatz der Male dar, die Inhalte anstatt aus dem Ursprungsserver aus dem Edge-Server-Cache bereitgestellt wurden. Sie wird wie folgt berechnet:

> ((Edge-Treffer - Eingangstreffer)/Edge-Treffer) * 100

Dabei gilt:
Die Definition für Edge-Treffer lautet "Alle Treffer für Edge-Server von Endbenutzern".  
Die Definition für Eingangstreffer ist "Ursprungs- oder Eingangstreffer im Datenverkehr vom Ursprungsserver zu Akamai-Edge-Servern".

Da die Trefferquote nicht pro CDN, sondern auf Kontoebene berechnet wird, ist die Trefferquote für alle CDNs in Ihrem Konto gleich. Diese Tatsache erklärt auch, warum die Trefferrate ungleich null sein kann, wenn die Anzahl der Edge-Treffer für einen bestimmten CDN Null ist.

