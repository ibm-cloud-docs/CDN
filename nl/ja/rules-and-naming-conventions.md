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

# ルールおよび命名規則
{: #rules-and-naming-conventions}

## CDN ホスト名に関するルールは何ですか
{: #what-are-the-rules-for-the-cdn-hostname}

CDN `ホスト名`の入力ストリングは以下のとおり**でなければなりません**。
  * 英数字で構成される
  * 254 文字未満
  * 有効な最上位ドメイン名で終わる
  * 10 個を超えるラベルを含ま**ない**
  * `cdnedge.bluemix.net` で終わら**ない** (この終わりは CNAME に使用され、予約済みです)

詳しくは、RFC 1035 のセクション 2.3.4 を参照してください。 

さらに、CDN ホスト名には完全修飾ドメイン・ネームを使用することを強くお勧めします。 `example.com` という形式のルート・ドメイン・ネーム (ゾーン Apex またはネイキッド・ドメインとも呼ばれます) ではなく、`www.example.com` という形式の名前を選択してください。使用する CDN ホスト名の CNAME レコードを作成する必要があります。また、DNS RFC 1033 では、ルート・ドメイン・レコードを CNAME でなく、A レコードにするように要求しています。 RFC 2181 のセクション 10.1 に、さらに詳しい説明があります。

## カスタム CNAME の命名規則は何ですか
{: #what-are-the-custom-cname-naming-conventions}

`CNAME` 入力ストリングは、以下のルールに従う必要があります。
  * 固有**でなければならない** (他の IBM Cloud CDN で使用されていてはいけません)
  * 64 文字未満の英数字
  * 特殊文字 `! @ # $ % ^ & *` は使用**できない**
  * 下線 `_` は使用**できない**
  * ピリオド `.` は使用**できない**
  * ダッシュ `-` は使用できるが、CNAME の先頭と末尾をダッシュにすることはできない

## バケット名に関するルールは何ですか
{: #what-are-the-rules-for-bucket-names}

バケット名:
  * 少なくとも 1 文字**でなければならない**
  * 200 文字未満でなければならない
  * 文字 (大文字と小文字の両方を使用できる)、数値、ハイフンを含めることができる
  * IP アドレス (例えば、127.0.0.1) の形式にすることは**できない**
  * ピリオド (.) で始めることは**できない**
  * ピリオド (.) で終わることができる
  * ラベルの間には 1 つのピリオド (.) のみを使用できる (例えば、my..ibmcloud.bucket は使用できません)

大文字とピリオドは検証に合格しますが、常に DNS 準拠のバケット名を使用することをお勧めします。
{: note}

## オリジンのパス・ストリングに関するルールは何ですか
{: #what-are-the-rules-for-the-path-string-for-the-origin}

CDN の作成時にパスはオプションです。 ただし、指定する場合、パスは以下のとおり**でなければなりません**。
  * 長さが 1000 文字未満
  * `/` で始まる

## パージのパス・ストリングに関するルールは何ですか
{: #what-are-the-rules-for-the-path-string-for-purge}

パージ・パス:
  * 長さが 1000 文字未満でなければならない
  * `/` で始まらなければならない
  * `/` で終わることはできない
  * ピリオド (.) で終わることはできない
  * `*` を含むことはできない

パージは、単一ファイルに対してのみ許可されます。現時点では、ディレクトリー・レベルのパージはサポートされていません。{; note}

## **Add Origin** コマンドの場合、パス・ストリングに関して従うべきルールはありますか。
{: #for-the-add-origin-command-are-there-any-rules-to-follow-for-the-path-string}

**「オリジンの追加 (Add Origin)」**にはパスは**必須**です。 また、CDN がパスを指定して作成された場合、**「オリジンの追加 (Add Origin)」**でのパスは、CDN パスを接頭部として先頭に付ける必要があります。 例えば、CDN パスが `/storage` と指定された場合、`/examplePath` というパスを使用して**「オリジンの追加 (Add Origin)」**を呼び出すには、指定するパスは `/storage/examplePath` となります。

## ファイル拡張子の指定に関するルールは何ですか
{: #what-are-the-rules-for-providing-file-extensions}

オブジェクト・ストレージを指定してオリジンを作成するときには、ファイル拡張子をコンマで区切る必要があります。 例えば、`txt, jpg, xml` は、有効なリストです。 個々の拡張子は、長さが 10 文字を超えてはなりません。
