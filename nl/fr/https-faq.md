---

copyright:
  years: 2018
lastupdated: "2018-06-19"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Foire aux questions pour HTTPS

## Quelle est la différence entre HTTPS avec un certificat de caractère générique et HTTPS avec un certificat SAN ?

Avec un certificat de caractère générique, tous les clients utilisent le même certificat déployé sur les réseaux CDN du fournisseur. L'enregistrement CNAME, incluant le suffixe `.cdnedge.bluemix.net`, doit être utilisé pour accéder au service. Exemple : `https://www.example-cname.cdnedge.bluemix.net`

Dans le cas d'un certificat SAN, des domaines client multiples partagent un certificat SAN unique en ajoutant leurs noms de domaine dans les entrées SAN. L'accès au service peut ensuite s'effectuer en utilisant le nom d'hôte, `https://www.example.com`, par exemple.

## Comment s'effectue la validation de domaine avec redirection ?

Cela dépend de votre serveur. La procédure d'exécution de la validation de domaine pour les serveurs Apache et Nginx est présentée sur la page [Exécution de la validation DCV (Domain Control Validation) pour HTTPS](how-to-https.html#redirect-).

## Combien de temps dure la validation de domaine ?

La validation de domaine prend généralement entre 2 et 4 heures, selon la méthode choisie. La validation de domaine avec enregistrement CNAME est la procédure la plus rapide, elle dure généralement moins d'une heure. La validation de domaine utilisant les méthodes Standard et Redirect prend environ 4 heures, une fois qu'une réponse a été apportée à la demande d'authentification.

## Combien de temps dure le processus de création avec un certificat SAN DV ?

Une demande normale d'activation de HTTPS prend entre 3 et 9 heures en moyenne, depuis la demande initiale jusqu'à l'exécution.

## Combien de temps prend la suppression d'un CDN avec un certificat SAN DV ?

La suppression de votre CDN nécessite que votre domaine soit retiré du certificat sur tous les serveurs d'équilibrage des charges. Ce processus peut prendre jusqu'à 8 heures pour s'exécuter.

## Il y a-t-il des frais supplémentaires associés à l'utilisation d'un certificat SAN DV ?

Non. Les configurations de certificats SAN DV sont fournies gratuitement, à la différence de HTTP ou HTTPS avec certificat de caractère générique.

## Est-ce que mon CDN créé en utilisant HTTPS avec certificat de caractère générique peut être mis à jour pour utiliser un certificat SAN DV ?

Non, un mappage de certificat de caractère générique ne peut pas être changé en certificat SAN pour l'instant.

## Qu'est-ce qu'une autorité de certification ?

Une autorité de certification est une entité qui émet des certificats numériques.

## Quelle autorité de certification le service IBM Cloud CDN utilise-t-il pour émettre un certificat SAN DV ?

Le service IBM Cloud CDN se sert de l'autorité de certification LetsEncrypt.

## Quels sont les certificats SSL pris en charge ?

Les certificats SSL pris en charge sont les suivants : certificat de caractère générique et certificat SAN (Subject Alternate Name) DV (Domain Validation). Le certificat SAN est partagé entre des clients multiples. IBM Cloud CDN ne prend pas en charge le téléchargement des certificats personnalisés.

## J'ai reçu un e-mail me demandant de répondre à une demande d'authentification de validation de domaine. Que dois-je faire maintenant ?

Vous disposez de trois méthodes pour répondre : CNAME, Standard ou Redirect.

Pour plus de détails sur l'utilisation de l'une de ces méthodes, consultez le document [Exécution de la validation DCV (Domain Control Validation) pour HTTPS](how-to-https.html#how-to-https.html#initial-steps-to-domain-control-validation).

## Que se passe-t-il si je ne réponds pas à la demande d'authentification de validation de domaine ?

Si l'état du mappage reste à DOMAIN_VALIDATION_PENDING pendant plus de 48 heures, la création du mappage est annulée et l'état du mappage passe à CREATE_ERROR. Dans cet état, vous pouvez choisir de réessayer la création ou de supprimer le mappage.

## Les certificats de caractère générique doivent-ils valider un domaine ?

Non, mais vous pouvez uniquement utiliser l'enregistrement CNAME pour extraire du contenu de votre serveur d'origine. `https://www.example-cname.cdnedge.bluemix.net`

## Si j'utilise le type de certificat SAN, puis-je toujours me servir de CNAME pour accéder à mon service ?

Non. Pour le certificat SAN, vous ne pouvez utiliser que le domaine personnalisé pour accéder au contenu depuis le serveur d'origine. `https://www.example.com`

## Tous mes domaines sont-ils ajoutés dans un seul certificat ?

Pas nécessairement. La sélection des certificats est gérée par Akamai, afin de garantir que l'état des certificats est le plus efficace possible. Les domaines étant ajoutés dans différents certificats de façon proportionnée, il est impossible de garantir que tous vos domaines se trouveront dans le même certificat.

## Pourquoi "wildcard" s'affiche-t-il quand j'exécute une action `dig` ou que j'essaie d'accéder au contenu alors que le statut de mon CDN est `Requesting Certificate`, `Domain Validation pending` ou `Deploying Certificate` ?

Lors du processus de demande d'un certificat SAN DV, la chaîne d'enregistrement DNS pour votre CDN est chaînée à un certificat de caractère générique, temporairement. Jusqu'à ce que le processus soit terminé, le contenu est temporairement servi via ce certificat de caractère générique. Une fois le processus de demande terminé, la chaîne d'enregistrement DNS est mise à jour pour chaîner le certificat SAN DV de votre CDN.
