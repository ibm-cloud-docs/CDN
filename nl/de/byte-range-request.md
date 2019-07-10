---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-25"

keywords: byte range request, byte-range request, origin server, range HTTP request, transfer-encoding

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}


# Mit Bytebereichsanforderungen arbeiten
{: #working-with-byte-range-requests}

Mithilfe einer Bytebereichsanforderung können Sie Teilinhalt von einem Ursprungsserver abrufen. Der Inhalt in diesem Dokument kann dazu beitragen, dass Sie die unter Umständen angezeigten Antwortstatuscodes verstehen.

Wenn eine **Bytebereichsanforderung** mithilfe von {{site.data.keyword.cloud}} CDN mit Akamai gesendet wird, kann der Benutzer den Antwortcode `200 (OK)` für die erste Anforderung und den Antwortcode `206` für alle darauf folgenden Anforderungen empfangen, weil Akamai-Edge-Server den Inhalt vom Ursprung in einem komprimierten Format anfordern. Wenn sich im Cache eines Edge-Servers kein Objekt befindet und er auch nicht über Informationen zur Länge des Inhalts verfügt, führt er eine Weiterleitung zum Ursprung durch und fordert das gesamte Objekt an. Daraufhin stellt der Ursprung Akamai das Objekt ohne Header für die Länge des Inhalts bereit, und dem Endbenutzer wird das gesamte Objekt bereitgestellt, obwohl es sich um eine Bytebereichsanforderung handelte. Daraus ergibt sich der Statuscode `200`. Bei nachfolgenden Anforderungen befindet sich das Objekt im Cache des Edge-Servers und der Statuscode `206` wird angezeigt.

Der Header **HTTP-Anforderungsbereich** gibt an, welcher Teil des Inhalts vom Server zurückgegeben werden soll. Es können mehrere Teile zusammen durch einen Bereichsheader angefordert werden und der Server kann diese Bereiche in einer mehrteiligen Antwort zurücksenden. Wenn der Server Bereiche zurücksendet, antwortet er mit dem Status 206 (Teilinhalt).

Eine Möglichkeit, den Antwortcode 206 auch für die erste Bytebereichsanforderung sicherzustellen, besteht darin, die Option `Transfer-Encoding: chunked` auf Ihrem Ursprungsserver zu inaktivieren.
