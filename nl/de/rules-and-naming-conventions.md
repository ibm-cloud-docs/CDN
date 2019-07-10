---

copyright:
  years: 2018, 2019
lastupdated: "2019-04-04"

keywords: rules, naming, conventions, hostname, cname, RFC, 1033, 1035, bucket, path, origin, purge, alphanumeric, top-level domain, valid, string

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

# Regeln und Namenskonventionen
{: #rules-and-naming-conventions}

## Welche Regeln gelten für den CDN-Hostnamen?
{: #what-are-the-rules-for-the-cdn-hostname}

Die Eingabezeichenfolge für den CDN-Hostnamen`` **muss** die folgenden Voraussetzungen erfüllen:
  * Sie muss aus alphanumerischen Zeichen bestehen.
  * Sie muss kürzer als 254 Zeichen sein.
  * Sie muss auf einen gültigen Domänennamen der höchsten Ebene enden.
  * Sie darf **nicht** mehr als 10 Bezeichnungen enthalten.
  * Sie darf **nicht** mit `cdnedge.bluemix.net` enden (diese Endung wird für CNAMES verwendet und ist für diese reserviert).

Weitere Details finden Sie in RFC 1035, Abschnitt 2.3.4. 

Darüber hinaus empfehlen wir die Verwendung eines vollständig qualifizierten Domänennamens als CDN-Hostnamen. Wählen Sie einen Namen der Form `www.example.com` anstelle eines Rootdomänennamens (auch als 'Zonen-Apex' oder 'Naked domain' bezeichnet) der Form `example.com`. Sie müssen einen CNAME-Eintrag für den verwendeten CDN-Hostnamen erstellen und bei dem Rootdomäneneintrag muss es sich gemäß RFC 1033 für DNS um einen A-Eintrag, nicht um einen CNAME-Eintrag handeln. Weitere Erläuterungen hierzu finden Sie in RFC 2181, Abschnitt 10.1.

## Welche angepassten Konventionen gelten für die CNAME-Benennung?
{: #what-are-the-custom-cname-naming-conventions}

Die Eingabezeichenfolge für `CNAME` muss die folgenden Regeln einhalten:
  * Sie **muss** eindeutig sein (d. h. sie kann nicht von einem anderen IBM Cloud CDN verwendet werden).
  * Sie muss kürzer als 64 alphanumerische Zeichen sein.
  * Sie Sonderzeichen `! @ # $ % ^ & *` sind **nicht** zulässig.
  * Unterstreichungszeichen (`_`) sind **nicht** zulässig.
  * Punkte (`.`) sind **nicht** zulässig.
  * Gedankenstriche (`-`) sind zulässig, jedoch kann der CNAME nicht mit einem Gedankenstrich beginnen oder enden.

## Welche Regeln gelten für Bucketnamen?
{: #what-are-the-rules-for-bucket-names}

Die folgenden Regeln gelten für Bucketnamen:
  * Sie **müssen** aus mindestens einem Zeichen bestehen.
  * Sie dürfen aus maximal 200 Zeichen bestehen.
  * Sie können Buchstaben (sowohl Groß- als auch Kleinbuchstabe sind zulässig), Ziffern und Bindestriche enthalten.
  * Sie dürfen **nicht** als IP-Adresse formatiert werden (z. B. 127.0.0.1).
  * Sie dürfen **nicht** mit einem Punkt (.) beginnen.
  * Sie dürfen mit einem Punkte (.) enden.
  * Sie dürfen zwischen Bezeichnungen nur einen Punkt (.) aufweisen (Beispiel: my..ibmcloud.bucket ist nicht zulässig).

Obwohl Großbuchstaben und Punkte die Gültigkeitsprüfung bestehen können, wird empfohlen, DNS-konforme Bucketnamen zu verwenden.
{: note}

## Welche Regeln gelten für die Pfadzeichenfolge für den Ursprung?
{: #what-are-the-rules-for-the-path-string-for-the-origin}

Der Pfad ist beim Erstellen des CDN optional. Wenn er angegeben wird, **muss** der Pfad folgenden Regeln genügen:
  * Er muss kürzer als 1000 Zeichen lang sein.
  * Er muss mit `/` enden.

## Welche Regeln gelten für die Pfadzeichenfolge für die Bereinigung?
{: #what-are-the-rules-for-the-path-string-for-purge}

Für den Bereinigungspfad gilt Folgendes:
  * Er muss kürzer als 1000 Zeichen sein.
  * Er muss mit `/` beginnen.
  * Er darf nicht mit `/` enden.
  * Er darf nicht mit einem Punkte (.) enden.
  * Er darf nicht einen Stern `*` enthalten.

Eine Bereinigung ist nur für einzelne Dateien zulässig. Derzeit wird eine Bereinigung auf Verzeichnisebene nicht unterstützt.
{; note}

## Gibt des für den Befehl '**Ursprung hinzufügen**' Regeln, die für die Pfadzeichenfolge beachtet werden müssen?
{: #for-the-add-origin-command-are-there-any-rules-to-follow-for-the-path-string}

Der Pfad für **Ursprung hinzufügen** ist **obligatorisch**. Darüber hinaus gilt, dass der Pfad für den Befehl **Ursprung hinzufügen**, wenn Ihr CDN mit einem Pfad erstellt wurde, mit dem CDN-Pfad als Präfix beginnen muss. Beispiel: Wenn für den CDN-Pfad `/storage` angegeben wurde, um **Ursprung hinzufügen** mit dem Pfad `/examplePath` aufzurufen, würde der angegebene Pfad `/storage/examplePath` lauten.

## Welche Regeln gelten für das Angeben von Dateierweiterungen?
{: #what-are-the-rules-for-providing-file-extensions}

Wenn ein Ursprung mit Object Storage erstellt wird, müssen die Dateierweiterungen durch Kommas getrennt werden. Beispiel: `txt, jpg, xml` ist eine gültige Liste. Jede einzelne Erweiterung darf nicht länger als 10 Zeichen sein.
