---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-31"

keywords: limits, maximum, values, time to live, entries, large file, size, optimization, downloads, years

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Grenzwerte und Maximalwerte
{: #limits-and-maximum-values}

## Gibt es einen Maximalwert für die Lebensdauer? Gibt es einen Mindestwert?
{: #is-there-a-maximum-value-for-time-to-live}

Der Maximalwert für die Lebensdauer (Time To Live, TTL) sind 2.147.483.647 Sekunden. Dies entspricht ungefähr 68 Jahren. Der Mindestwert beträgt 0 Sekunden.

## Gibt es einen Grenzwert für die Anzahl der Einträge für Ursprung und TTL?
{: #is-there-a-limit-on-the-number-of-origin-and-ttl-entries}

Ja, der kombinierte Grenzwert sind 75 Einträge pro CDN.

## Was ist die größte Dateigröße, die über Akamai CDN zugestellt werden kann?
{: #what-is-the-largest-file-size-that-can-be-delivered-through-akamai-cdn}

Versuche, Dateien mit Größen über 1,8 GB abzurufen oder zuzustellen, empfangen unter der Standardleistungskonfiguration eine Antwort `403 Zugriff nicht zulässig`. Wenn die Optimierung für große Dateien aktiviert ist, sind Dateidownloads bis zu 320 GB möglich.

## Wie groß ist die maximale Größe für die Ursprungsantwortheader?
{: #what-is-the-maximum-size-for-the-orignin-response-headers}

Die maximale Antwortheadergröße beträgt 16 KB. Falls bestimmte Ursprünge versuchen, Cookies zu setzen, die für den Client zu groß sind, um in nachfolgenden Anforderungen zurückgegeben zu werden, legt Akamai diesen Grenzwert auf den Edge-Servern fest. Wenn die Antwortheader größer als 16 KB sind, erhält die Anforderung eine Antwort vom Typ `502 Bad Gateway`.

## Wie groß ist die maximale Größe für die Anforderungsheader des Clients?
{: #what-is-the-maximum-size-for-the-client-request-headers}

Die maximale Größe der Anforderungsheader beträgt 32 KB. Wenn die Anforderungsheader größer als 32 KB sind, gibt der Akamai-Edge-Server die Antwort von `400 Bad Request` zurück.
