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

# Règles et conventions de dénomination
{: #rules-and-naming-conventions}

## Quelles sont les règles applicables au nom d'hôte de CDN ?
{: #what-are-the-rules-for-the-cdn-hostname}

La chaîne d'entrée `Hostname` de CDN **doit** :
  * être composée de caractères alphanumériques
  * comprendre moins de 254 caractères
  * se terminer par un nom de domaine de niveau supérieur valide
  * **ne pas** contenir plus de 10 libellés
  * **ne pas** se terminer par `cdnedge.bluemix.net` (cette terminaison, qui est utilisée pour les enregistrements CNAME, est réservée)

Pour plus de détails, veuillez vous référer à la RFC 1035, section 2.3.4. 

En outre, il est vivement recommandé d'utiliser un nom de domaine complet comme nom d'hôte de CDN. Choisissez un nom au format `www.example.com` plutôt qu'un nom de domaine racine (également appelé domaine Zone Apex ou Naked) au format `example.com`. Vous devrez créer un enregistrement CNAME pour le nom d'hôte de CDN que vous utilisez, et la norme RFC DNS 1033 exige que l'enregistrement de domaine racine soit un enregistrement A et non un enregistrement CNAME. Des explications supplémentaires sont fournies dans le document RFC 2181, section 10.1

## Que sont les conventions de dénomination CNAME personnalisées ?
{: #what-are-the-custom-cname-naming-conventions}

La chaîne d'entrée `CNAME` doit être conforme aux règles suivantes :
  * elle **doit** être unique (elle ne peut pas être utilisée par un autre service IBM Cloud CDN)
  * elle doit contenir moins de 64 caractères alphanumériques
  * elle ne doit **pas** comporter les caractères spéciaux `! @ # $ % ^ & *`
  * elle ne doit **pas** comporter des traits de soulignement (`_`)
  * elle ne doit **pas** comporter de point (`.`)
  * les tirets (`-`) sont autorisés, mais le CNAME ne peut pas commencer ou se terminer par un tiret

## Quelles sont les règles applicables aux noms de compartiment ?
{: #what-are-the-rules-for-bucket-names}

Les noms de compartiment :
  * **doivent** comporter un minimum de 1 caractère
  * doivent faire moins de 200 caractères
  * doivent contenir des lettres (les lettres majuscules et minuscules sont autorisées), des chiffres et des traits d'union
  * ne doivent **pas** être formatés en tant qu'adresse IP (par exemple, 127.0.0.1)
  * ne peuvent **pas** commencer par un point (.)
  * peuvent se terminer par un point (.)
  * un seul point (.) est autorisé entre les libellés (my..ibmcloud.bucket n'est pas autorisé, par exemple).

Bien que les lettres en majuscules et les points puissent être validés, nous recommandons de toujours utiliser des noms de compartiment conformes au DNS.
{: note}

## Quelles sont les règles applicables à la chaîne de chemin d'accès pour l'origine ?
{: #what-are-the-rules-for-the-path-string-for-the-origin}

Le chemin d'accès est facultatif lorsque vous créez votre CDN. Cependant, s'il est fourni, le chemin d'accès **doit** :
  * faire moins de 1000 caractères de long
  * commencer par `/`

## Quelles sont les règles applicables à la chaîne de chemin d'accès pour la purge ?
{: #what-are-the-rules-for-the-path-string-for-purge}

Le chemin de purge doit :
  * faire moins de 1000 caractères de long
  * commencer par `/`
  * ne peut pas se terminer par `/`
  * ne peut pas se terminer par un point (.)
  * ne peut pas contenir `*`

La purge n'est autorisée que pour les fichiers uniques. La purge de niveau répertoire n'est pas prise en charge pour le moment.
{; note}

## Dans le cadre de la commande **Add Origin**, quelles sont les règles à suivre pour spécifier la chaîne de chemin d'accès ?
{: #for-the-add-origin-command-are-there-any-rules-to-follow-for-the-path-string}

Pour **Add Origin**, le chemin est **obligatoire**. De même, si votre CDN a été créé avec un chemin d'accès, le chemin d'accès spécifié sous **Add Origin** doit commencer par le chemin du CDN comme préfixe. Ainsi, si le chemin du CDN a été spécifié sous la forme `/storage` et que vous voulez appeler la commande **Add Origin** avec un chemin appelé `/examplePath`, le chemin fourni doit être `/storage/examplePath`.

## Quelles sont les règles applicables à la fourniture des extensions de fichier ?
{: #what-are-the-rules-for-providing-file-extensions}

Lors de la création d'une origine avec stockage d'objets, les extensions de fichier doivent être séparées par des virgules. Ainsi, `txt, jpg, xml` est une liste valide. Toute extension particulière ne peut dépasser 10 caractères.
