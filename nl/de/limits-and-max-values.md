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

# Grenzwerte und Maximalwerte

## Gibt es einen Maximalwert für die Lebensdauer? Gibt es einen Mindestwert?

Der Maximalwert für die Lebensdauer (Time To Live, TTL) sind 2.147.483.647 Sekunden. Dies entspricht ungefähr 68 Jahren. Der Mindestwert sind 30 Sekunden.

## Gibt es einen Grenzwert für die Anzahl der Einträge für Ursprung und TTL?

Ja, der kombinierte Grenzwert sind 75 Einträge pro CDN.

## Gibt es einen Grenzwert für die Anzahl der CDN, die ich verwenden kann?

Ja, Sie können maximal 10 CDN pro Konto verwenden.

## Gibt es einen Grenzwert für die Anzahl der gleichzeitig aktiven Bereinigungsanforderungen?
Ja. Es können höchstens 5 Bereinigungsanforderungen gleichzeitig aktiv sein.

## Was ist die größte Dateigröße, die über Akamai CDN zugestellt werden kann?

Versuche, Dateien mit Größen über 1,8 GB abzurufen oder zuzustellen, empfangen unter der Standardleistungskonfiguration eine Antwort `403 Zugriff nicht zulässig`. Wenn die Optimierung für große Dateien aktiviert ist, sind Dateidownloads bis zu 320 GB möglich.
