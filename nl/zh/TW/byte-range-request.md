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


# 使用位元組範圍要求
{: #working-with-byte-range-requests}

使用位元組範圍要求，您可以從原點伺服器擷取局部內容。此文件可協助您瞭解可能會看到的回應狀態碼。

使用 {{site.data.keyword.cloud}} CDN 與 Akamai 傳送**位元組範圍要求**時，使用者可能針對第一個要求會收到 `200 (OK)` 回應碼，而後續所有要求則都收到 `206` 回應碼，因為 Akamai 邊緣伺服器會以壓縮格式要求原點中的內容。因此，若邊緣伺服器在其快取中沒有某個物件，也沒有關於物件內容長度的任何資訊，它會將物件轉遞至原點，且要求整個物件。接著，原點會將沒有內容長度標頭的物件提供給 Akamai，而一般使用者會得到整個物件，即使是位元組範圍要求。因此是 `200` 狀態碼。在後續的要求中，邊緣伺服器的快取中已經有物件，將會提供 `206` 狀態碼。

**範圍 HTTP 要求**標頭指出伺服器應該傳回內容的哪個部分。一個範圍標頭可以一次要求數個部分，伺服器可以用多部分回應來傳回這些範圍。如果伺服器傳回範圍，它會回應 206（局部內容）狀態。

有一種確保 206 回應的方法（即使是第一個位元組範圍要求），就是在您的原點伺服器上停用 `Transfer-Encoding: chunked`。
