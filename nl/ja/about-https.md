---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-21"

keywords: wildcard certificate, https, san certificate

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note .note}
{:download: .download}

# HTTPS について
{: #about-https}

{{site.data.keyword.cloud}} では、HTTPS を使用して CDN を保護する方法として、ワイルドカード証明書とドメイン検証 (DV) SAN 証明書という 2 つの方法が提供されています。 どちらの HTTPS オプションも、CDN を構成するときに**「HTTPS ポート」**を選択することで構成できます。 デフォルトの HTTPS ポートは 443 ですが、別のポート番号を選択して HTTPS トラフィックがそのポートを通過するようにもできます。 許可されるポート番号のリストは、『[よくある質問](docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)』にあります。

HTTPS に**ワイルドカード証明書**と **SAN 証明書**のどちらを使用すべきかは、「CDN CNAME と CDN ドメイン・ネームのいずれから HTTPS トラフィックを提供する必要があるか?」という質問の答えによって決まります。 CNAME から HTTPS トラフィックを提供する場合は、**ワイルドカード証明書**を選択します。 CDN ドメイン・ネームから HTTPS トラフィックを提供する場合は、**SAN 証明書**を選択します。

## ワイルドカード証明書のサポート
{: #wildcard-certificate-support}

**注**: 現時点では、ワイルドカード証明書を使用した新しい CDN マッピングはサポートされていません。

ワイルドカード証明書は、Web コンテンツをエンド・ユーザーに安全に配信するための最もシンプルな方法です。 ワイルドカード証明書を使用するためには、ワイルドカード証明書サフィックスを含む完全な CDN CNAME をサービス・エントリー・ポイントとして使用**しなければなりません** (例えば `https://example.cdnedge.bluemix.net`)。

IBM Cloud CDN は、ワイルドカード証明書 `*.cdnedge.bluemix.net` を使用します。 お客様に提供されたものか、お客様が自身で作成したものかどうかにかかわらず、サフィックス `*.cdnedge.bluemix.net` で終わる CNAME が、CDN エッジ・サーバーで保持されているワイルドカード証明書に追加されます。 したがって、エンド・ユーザーがお客様の CDN に HTTPS を使用する唯一の方法は CNAME になります。

![HTTP およびワイルドカードの図](images/state-diagram-wildcard.png)

## サブジェクト代替名 (SAN) 証明書のサポート
{: #san-certificate-suport}

サブジェクト代替名 (Subject Alternative Name (SAN)) 証明書は、複数のドメイン (つまりホスト名) を単一の証明書で保護するのを可能にするデジタル SSL 証明書です。

HTTPS 用に SAN 証明書を使用すると、認証局によって発行済みの証明書に、お客様の 1 次 CDN ホスト名が追加されます。 これにより、お客様のユーザーは、CNAME ではなくホスト名を介して、お客様のサービスに安全にアクセスできます (例えば、`https://www.example.com`)。

HTTPS SAN 証明書を使用して CDN が注文されると、証明書の要求とドメイン制御検証 (DCV) の作成を行うプロセスが実行されます。 DCV は、ドメインへのアクセスおよびドメインの制御を行う権限がお客様に付与されていることを確立するために認証局が使用するプロセスです。 このステップを完了するには、お客様のアクションが必要です。 制御が確立された後、証明書は世界中の CDN エッジ・サーバーにデプロイされます。 証明書がいったん正常にデプロイされると、証明書の更新は自動的に処理されます。 この機能について詳しくは、『[機能の説明](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#https-protocol-support)』を参照してください。 ドメイン制御検証の方式については、『[HTTPS のためのドメイン制御検証の実行](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#initial-steps-to-domain-control-validation)』ページに詳しく説明されています。

CDN が「実行中」状況になったら、CDN ホスト名 CNAME レコードを DNS 内に保持する必要があります。 CNAME レコードが削除されると、CDN ホスト名が 3 日以内に SAN 証明書から削除される可能性があります。 そのような状況が発生した場合、その CDN ホスト名では HTTPS トラフィックが提供されなくなります。
{:note}

![SAN 証明書を使用する HTTPS の図](images/state-diagram-san.png)
