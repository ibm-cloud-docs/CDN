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

# 規則及命名慣例
{: #rules-and-naming-conventions}

## CDN 主機名稱的規則為何？
{: #what-are-the-rules-for-the-cdn-hostname}

CDN `Hostname` 輸入字串**必須**：
  * 由英數字元組成
  * 少於 254 個字元
  * 以有效的頂層網域名稱為結尾
  * **不**得包含超過 10 個標籤
  * **不**得以 `cdnedge.bluemix.net` 為結尾（該結尾用於 CNAMES 且已保留）

如需詳細資料，請參閱 RFC 1035 第 2.3.4 節。 

此外，我們高度建議使用完整的網域名稱作為您的 CDN 主機名稱。請選擇形式為 `www.example.com` 的名稱，而不要使用形式為 `example.com` 的根網域名稱（也稱為 Zone Apex 或裸網域）。您將需要為您使用的 CDN 主機名稱建立 CNAME 記錄，而 DNS RFC 1033 要求根網域記錄必須是 A 記錄，而不是 CNAME。進一步的說明提供於 RFC 2181 的 10.1 節。

## 自訂 CNAME 命名慣例為何？
{: #what-are-the-custom-cname-naming-conventions}

`CNAME` 輸入字串必須遵循下列規則：
  * **必須**是唯一的（它不能由任何其他 IBM Cloud CDN 使用中）
  * 少於 64 個英數字元
  * **不允許**特殊字元 `! @ # $ % ^ & *`
  * **不允許**底線 `_`
  * **不允許**句點 `.`
  * 允許橫線 `-`，但 CNAME 不得以橫線為開頭或結尾

## 儲存區名稱的規則為何？
{: #what-are-the-rules-for-bucket-names}

儲存區名稱：
  * **必須**至少有 1 個字元
  * 必須小於 200 個字元
  * 可能包含字母（允許大寫和小寫字母）、數字和連字號
  * **不得**格式化為 IP 位址（例如 127.0.0.1）
  * **不得**以句點 (.) 為開頭
  * 結尾可能是句點 (.)
  * 在標籤之間只允許一個句點 (.)（例如，不允許 my..ibmcloud.bucket）。

雖然大寫字母和句點可以通過驗證，我們仍建議您一律使用遵循 DNS 的儲存區名稱。
{: note}

## 原點路徑字串的規則為何？
{: #what-are-the-rules-for-the-path-string-for-the-origin}

建立 CDN 時路徑是選用性的。不過，如果提供，路徑**必須**：
  * 長度少於 1000 個字元
  * 以 `/` 為開頭

## 清除路徑字串的規則為何？
{: #what-are-the-rules-for-the-path-string-for-purge}

清除路徑：
  * 長度必須小於 1000 個字元
  * 必須以 `/` 為開頭
  * 結尾不能是 `/`
  * 結尾不能是句點 (.)
  * 不得包含 `*`

僅容許對單一檔案執行「清除」。目前不支援目錄層次清除。{; note}

## 對於**新增原點**指令，路徑字串有任何規則必須遵循嗎？
{: #for-the-add-origin-command-are-there-any-rules-to-follow-for-the-path-string}

對於**新增原點**，路徑是**必要項目**。此外，如果 CDN 建立時有路徑，則**新增原點**的路徑開頭必須以 CDN 路徑作為字首。例如，如果 CDN 路徑指定為 `/storage`，則若要呼叫**新增原點**並使用稱為 `/examplePath` 的路徑，所提供的路徑將為 `/storage/examplePath`。

## 提供副檔名的規則為何？
{: #what-are-the-rules-for-providing-file-extensions}

建立具有 Object Storage 的「原點」時，副檔名應該以逗點區隔。例如，`txt、jpg、xml` 是有效的清單。任何一個特定副檔名的長度都不得超過 10 個字元。
