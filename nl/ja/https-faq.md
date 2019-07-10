---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-05"

keywords: faq, https, wildcard, certificate, san certificate, domain validation, redirect, domains, challenge

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:faq: data-hd-content-type='faq'}

# HTTPS についてのよくある質問
{: #faq-for-https}

## CDN にとって、ワイルドカード証明書を使用する HTTPS と SAN 証明書を使用する HTTPS との違いは何ですか?
{: #for-my-cdn-what-is-the-difference-between-https-with-wildcard-certificate-and-https-with-san-certificate}
{:faq}

ワイルドカード証明書の場合、すべてのカスタマーが、ベンダーの CDN ネットワークにデプロイされた同じ証明書を使用します。 サービスにアクセスするために、IBM サフィックス `.cdnedge.bluemix.net` を含む CNAME が使用される必要があります。 例えば、`https://www.example-cname.cdnedge.bluemix.net` です。

SAN 証明書の場合、複数のカスタマー・ドメインが、それぞれのドメイン名を SAN エントリーに追加することによって、単一の SAN 証明書を共有します。 そうすると、ホスト名 (例えば `https://www.example.com`) を使用してサービスにアクセスできます。

## リダイレクトでのドメイン検証はどのように行われるのですか
{: #how-is-domain-validation-with-redirect-accomplished}
{:faq}

それはご使用のサーバーによって異なります。 Apache サーバーおよび Nginx サーバーのドメイン検証実行手順は、『[HTTPS のためのドメイン制御検証の実行](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#redirect)』ページに記載されています。

## ドメイン検証にはどのくらいの時間がかかりますか
{: #how-long-does-domain-validation-take}
{:faq}

ドメイン検証には通常は 2 時間から 4 時間かかりますが、検証に選択された方式によって異なります。 CNAME 検証方式での DV が最も速く、通常は 1 時間以内です。 標準方式およびリダイレクト方式を使用する DV では、チャレンジに対処した後、通常は最大 4 時間かかります。

## DV SAN 証明書を使用する CDN で HTTPS を作成して使用可能にするにはどのくらいの時間がかかりますか?
{: #how-long-does-it-take-to-create-and-enable-https-for-my-cdn-with-a-dv-san-certificate}
{:faq}

HTTPS を使用可能にするための標準的な要求にかかる時間は、最初の要求から実行中になるまで、平均 3 時間から 9 時間です。

## DV SAN 証明書を使用する CDN を削除するにはどのくらいの時間がかかりますか?
{: #how-long-does-it-take-to-delete-a-cdn-with-a-dv-san-certificate}
{:faq}

お客様の CDN を削除するには、すべてのエッジ・サーバーで証明書からお客様のドメインが削除される必要があります。 このプロセスの完了には最大 8 時間かかる可能性があります。

## CDN での DV SAN 証明書の使用に関連する追加コストはありますか?
{: #is-there-any-additional-cost-associated-with-using-a-dv-san-certificate-for-my-cdn}
{:faq}

いいえ。DV SAN 証明書構成は、HTTP、およびワイルドカード証明書を使用する HTTPS の場合と同様に、追加料金なしで提供されます。

## ワイルドカードの HTTPS を使用して作成された CDN を DV SAN 証明書を使用するように更新することはできますか?
{: #can-my-cdn-created-using-https-with-wildcard-be-updated-to-use-a-dv-san-certificate}
{:faq}

いいえ。ワイルドカード・マッピングを SAN 証明書に変更することはできません。

## 認証局とは何ですか
{: #what-is-a-certificate-authority}
{:faq}

認証局 (CA) は、デジタル証明書を発行する機関です。

## {{site.data.keyword.cloud}} CDN サービスが DV SAN 証明書を発行するために使用するのはどの CA ですか?
{: #which-ca-does-ibm-cloud-cdn-service-use-for-issuing-a-dv-san-certificate}
{:faq}

IBM Cloud CDN サービスは、Let's Encrypt 認証局を使用します。

## IBM Cloud CDN でサポートされている SSL 証明書は何ですか?
{: #what-ssl-certificates-are-supported-for-ibm-cloud-cdn}
{:faq}

サポートされている SSL 証明書は、ワイルドカード証明書と、ドメイン検証 (DV) サブジェクト代替名 (SAN) 証明書です。 SAN 証明書は、複数のカスタマーで共有されます。 IBM Cloud CDN は、カスタム証明書のアップロードをサポートしていません。

## CDN に関連したドメイン検証チャレンジに対処するように求める E メールを受信しました。 この後、どうすればよいですか
{: #i-received-an-email-asking-me-to-address-a-domain-validation-challenge-related-to-my-cdn}
{:faq}

ドメイン検証に対処する方法は、CNAME、標準、またはダイレクトの 3 つがあります。

これらの対処方法について詳しくは、『[HTTPS のためのドメイン制御検証の実行](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#initial-steps-to-domain-control-validation)』文書を参照してください。

## CDN のドメイン検証のチャレンジに対処しないとどうなるのですか?
{: #what-will-happen-if-i-dont-address-the-challenge-for-domain-validation-of-my-cdn}
{:faq}

マッピングの状態が 48 時間を超えて DOMAIN_VALIDATION_PENDING 状態である場合、マッピング作成は取り消され、マッピングの状態は CREATE_ERROR になります。 この状態では、作成の再試行またはマッピングの削除を行うことを選択できます。

## ワイルドカード証明書は CDN のドメインを検証する必要がありますか?
{: #does-a-wildcard-certificate-need-to-validate-a-domain-for-my-cdn}
{:faq}

いいえ。ただし、CNAME の使用によってのみ、オリジン `https://www.example-cname.cdnedge.bluemix.net` からコンテンツを取得できます。

## 自分のドメインが IBM CDN CNAME を指していないことを示す E メールを受け取りました。この後、どうすればよいですか
{: #i-received-an-email-indicating-that-my-domain-is-not-pointed-to-IBM-CDN-CNAME}
{:faq}

このメールは、自分の CDN が使用されていないことを意味しています。CDN を使用して証明書内でドメインをアクティブにするには、リストされている CNAME DNS レコードを DNS プロバイダー・システムで設定する必要があります。7 日以内にこのアクションを完了すると、CDN の HTTP トラフィックと HTTPS トラフィックの両方が復元され、CDN は「実行中」状況になります。7 日経っても CDN が未使用のままである場合は、共有 SAN 証明書に追加される新しい CDN ドメイン要求を未使用のドメインがブロックしないように、CDN ドメインの HTTPS を永続的に無効にする必要があります。CDN を介した HTTP トラフィック・アクセスは、ドメインの CNAME レコードを追加することによって後で復元することができます。
この状態の対処方法について詳しくは、『[HTTPS のためのドメイン制御検証の実行](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#cname)』文書を参照してください。

## CDN に SAN 証明書タイプを使用していても、サービスへのアクセスに CNAME を使用できますか?
{: #if-i-use-a-san-certificate-type-for-my-cdn-can-i-still-use-the-cname-for-access-to-my-service}
{:faq}

いいえ。SAN 証明書の場合、カスタム・ドメインを使用してのみ、オリジンからのコンテンツにアクセスできます。

## IBM Cloud CDN ドメインのすべてが 1 つの証明書に追加されますか?
{: #are-all-of-my-ibm-cloud-cdn-domains-added-into-one-certificate}
{:faq}

必ずしもそうではありません。 証明書の選択は、証明書が最も効率のよい状態になるように、Akamai によって処理されます。 ドメインは、均整のとれた方法で別々の証明書に追加されるため、お客様のドメインのすべてが同じ証明書にあることは保証されません。

## CDN の状況が 「証明書要求中 (`Requesting Certificate`)」、「ドメイン検証保留中 (`Domain Validation pending`)」、または「証明書デプロイ中 (`Deploying Certificate`)」の場合に、コンテンツにアクセスしようとするか `dig` を実行すると「ワイルドカード」が表示されるのはなぜですか
{: #why-do-i-see-wildcard}
{:faq}

DV SAN 証明書要求プロセス中に、CDN の DNS レコード・チェーンが一時的にワイルドカード証明書にチェーニングされます。 このプロセスが完了するまで、コンテンツは一時的にこのワイルドカード証明書を通して提供されます。 要求プロセスが完了すると、DNS レコード・チェーンは、CDN の DV SAN 証明書にチェーニングするように更新されます。
