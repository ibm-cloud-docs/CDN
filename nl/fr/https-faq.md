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

# Foire aux questions pour HTTPS
{: #faq-for-https}

## Pour mon CDN, quelle est la différence entre HTTPS avec un certificat de caractère générique et HTTPS avec un certificat SAN ?
{: #for-my-cdn-what-is-the-difference-between-https-with-wildcard-certificate-and-https-with-san-certificate}
{:faq}

Avec des certificats de caractère générique, tous les clients utilisent le même certificat déployé sur les réseaux CDN du fournisseur. L'enregistrement CNAME, incluant le suffixe `.cdnedge.bluemix.net`, doit être utilisé pour accéder au service. Exemple : `https://www.example-cname.cdnedge.bluemix.net`

Dans le cas d'un certificat SAN, des domaines client multiples partagent un certificat SAN unique en ajoutant leurs noms de domaine dans les entrées SAN. L'accès au service peut ensuite s'effectuer en utilisant le nom d'hôte, `https://www.example.com`, par exemple.

## Comment s'effectue la validation de domaine avec redirection ?
{: #how-is-domain-validation-with-redirect-accomplished}
{:faq}

Cela dépend de votre serveur. La procédure d'exécution de la validation de domaine pour les serveurs Apache et Nginx est présentée sur la page [Exécution de la validation DCV (Domain Control Validation) pour HTTPS](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#redirect).

## Combien de temps dure la validation de domaine ?
{: #how-long-does-domain-validation-take}
{:faq}

La validation de domaine prend généralement entre 2 et 4 heures, selon la méthode choisie. La validation de domaine avec enregistrement CNAME est la procédure la plus rapide, elle dure généralement moins d'une heure. La validation de domaine utilisant les méthodes Standard et Redirect prend environ 4 heures, une fois qu'une réponse a été apportée à la demande d'authentification.

## Combien de temps prend la création et l'activation de HTTPS pour mon CDN avec un certificat SAN DV ?
{: #how-long-does-it-take-to-create-and-enable-https-for-my-cdn-with-a-dv-san-certificate}
{:faq}

Une demande normale d'activation de HTTPS prend entre 3 et 9 heures en moyenne, depuis la demande initiale jusqu'à l'exécution.

## Combien de temps prend la suppression d'un CDN avec un certificat SAN DV ?
{: #how-long-does-it-take-to-delete-a-cdn-with-a-dv-san-certificate}
{:faq}

La suppression de votre CDN nécessite que votre domaine soit retiré du certificat sur tous les serveurs d'équilibrage des charges. Ce processus peut prendre jusqu'à 8 heures pour s'exécuter.

## Il y a-t-il des frais supplémentaires associés à l'utilisation d'un certificat SAN DV pour mon CDN ?
{: #is-there-any-additional-cost-associated-with-using-a-dv-san-certificate-for-my-cdn}
{:faq}

Non. Les configurations de certificats SAN DV sont fournies gratuitement, à la différence de HTTP ou HTTPS avec certificat de caractère générique.

## Est-ce que mon CDN créé en utilisant HTTPS avec certificat de caractère générique peut être mis à jour pour utiliser un certificat SAN DV ?
{: #can-my-cdn-created-using-https-with-wildcard-be-updated-to-use-a-dv-san-certificate}
{:faq}

Non, un mappage de certificat de caractère générique ne peut pas être changé en certificat SAN.

## Qu'est-ce qu'une autorité de certification ?
{: #what-is-a-certificate-authority}
{:faq}

Une autorité de certification est une entité qui émet des certificats numériques.

## Quelle autorité de certification le service {{site.data.keyword.cloud}} CDN utilise-t-il pour émettre un certificat SAN DV ?
{: #which-ca-does-ibm-cloud-cdn-service-use-for-issuing-a-dv-san-certificate}
{:faq}

Le service IBM Cloud CDN se sert de l'autorité de certification LetsEncrypt.

## Quels sont les certificats SSL pris en charge pour IBM Cloud CDN ?
{: #what-ssl-certificates-are-supported-for-ibm-cloud-cdn}
{:faq}

Les certificats SSL pris en charge sont les suivants : certificat de caractère générique et certificat SAN (Subject Alternate Name) DV (Domain Validation). Le certificat SAN est partagé entre des clients multiples. IBM Cloud CDN ne prend pas en charge le téléchargement des certificats personnalisés.

## J'ai reçu un e-mail me demandant de répondre à une demande d'authentification de validation de domaine liée à mon CDN. Que dois-je faire maintenant ?
{: #i-received-an-email-asking-me-to-address-a-domain-validation-challenge-related-to-my-cdn}
{:faq}

Vous disposez de trois méthodes pour répondre : CNAME, Standard ou Redirect.

Pour plus de détails sur l'utilisation de l'une de ces méthodes, consultez le document [Exécution de la validation DCV (Domain Control Validation) pour HTTPS](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#initial-steps-to-domain-control-validation).

## Que se passe-t-il si je ne réponds pas à la demande d'authentification de validation de domaine de mon CDN ?
{: #what-will-happen-if-i-dont-address-the-challenge-for-domain-validation-of-my-cdn}
{:faq}

Si l'état du mappage reste à DOMAIN_VALIDATION_PENDING pendant plus de 48 heures, la création du mappage est annulée et l'état du mappage passe à CREATE_ERROR. Dans cet état, vous pouvez choisir de réessayer la création ou de supprimer le mappage.

## Un certificat de caractère générique doit-il valider un domaine pour mon CDN ?
{: #does-a-wildcard-certificate-need-to-validate-a-domain-for-my-cdn}
{:faq}

Non, mais vous pouvez uniquement utiliser l'enregistrement CNAME pour extraire du contenu de votre serveur d'origine. `https://www.example-cname.cdnedge.bluemix.net`

## J'ai reçu un e-mail indiquant que mes domaines ne pointent pas sur IBM CDN CNAME. Que dois-je faire maintenant ?
{: #i-received-an-email-indicating-that-my-domain-is-not-pointed-to-IBM-CDN-CNAME}
{:faq}

Cet e-mail signifie que votre CDN n'est pas utilisé. Pour utiliser le CDN et activer le ou les domaines, vous devez définir le ou les enregistrements DNS CNAME répertoriés sur votre système fournisseur DNS.
Si vous effectuez cette action sous 7 jours, le trafic HTTP et HTTPS sera restauré pour votre CDN et celui-ci aura le statut RUNNING. Si le CDN est toujours inutilisé après 7 jours, nous devrons désactiver HTTPS de manière définitive votre domaine CDN afin d'empêcher que votre domaine inutilisé ne bloque de nouvelles demandes de domaine CDN d'être ajoutées au certificat SAN partagé. L'accès au trafic HTTP via le CDN pourra néanmoins être restauré ultérieurement via l'ajout d'un enregistrement CNAME pour votre domaine.
Pour plus de détails sur la façon de résoudre cette situation, consultez le document [Exécution de la validation DCV (Domain Control Validation) pour HTTPS](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#cname).

## Si j'utilise le type de certificat SAN pour mon CDN, puis-je toujours me servir de CNAME pour accéder à mon service ?
{: #if-i-use-a-san-certificate-type-for-my-cdn-can-i-still-use-the-cname-for-access-to-my-service}
{:faq}

Non. Pour le certificat SAN, vous ne pouvez utiliser que le domaine personnalisé pour accéder au contenu depuis le serveur d'origine.

## Tous mes domaines IBM Cloud CDN sont-ils ajoutés dans un seul certificat ?
{: #are-all-of-my-ibm-cloud-cdn-domains-added-into-one-certificate}
{:faq}

Pas nécessairement. La sélection des certificats est gérée par Akamai, afin de garantir que l'état des certificats est le plus efficace possible. Les domaines étant ajoutés dans différents certificats de façon proportionnée, il est impossible de garantir que tous vos domaines se trouveront dans le même certificat.

## Pourquoi "wildcard" s'affiche-t-il quand j'exécute une action `dig` ou que j'essaie d'accéder au contenu alors que le statut de mon CDN est `Requesting Certificate`, `Domain Validation pending` ou `Deploying Certificate` ?
{: #why-do-i-see-wildcard}
{:faq}

Lors du processus de demande d'un certificat SAN DV, la chaîne d'enregistrement DNS pour votre CDN est chaînée à un certificat de caractère générique, temporairement. Jusqu'à ce que le processus soit terminé, le contenu est temporairement distribué via ce certificat de caractère générique. Une fois le processus de demande terminé, la chaîne d'enregistrement DNS est mise à jour pour chaîner le certificat SAN DV de votre CDN.
