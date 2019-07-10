---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-21"

keywords: domain, control, validation, https, san certificate, challenge, apache, nginx, redirect

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Exécution de la validation DCV (Domain Control Validation) pour HTTPS avec DV SAN
{: #completing-domain-control-validation-for-https-with-dv-san}

Le diagramme suivant présente les différents états possibles pour votre CDN entre le moment où il est créé et celui où il commence à fonctionner :

  ![Diagramme des états SAN](images/state-diagram-san.png)

## Etapes de validation DCV initiales
{: #initial-steps-to-domain-control-validation}

**Etape 1 :**

Une fois que vous avez commandé votre CDN avec un certificat SAN DV, le processus de demande de certificat commence. Au cours de ce processus, {{site.data.keyword.cloud}} CDN demande un certificat depuis Akamai. Quand un certificat est disponible, Akamai émet une demande vers l'autorité de certification.

  * Pendant cette période, le statut CDN affiche **Requesting Certificate**.

    ![Statut Requesting Certificate](images/requesting-cert.png)

**Etape 2 :**

Une fois que l'autorité de certification a reçu la demande, elle émet une demande d'authentification de validation de domaine.

  * A ce moment là, le statut de votre CDN passe à **Domain validation needed**.

    ![Domain Validation Needed](images/domain-validation-needed.png)

**Etape 3 :**

Cliquez sur le nom du CDN qui doit être validé. Sur la page de présentation qui s'ouvre, vous pouvez voir le statut général de votre CDN. En haut de la page, une alerte s'affiche pour vous rappeler qu'une validation du domaine est nécessaire. Sélectionnez le bouton **View domain validation** pour ouvrir une fenêtre qui vous indique les informations d'authentification requises pour l'exécution du processus de validation.

   ![Domain Validation Needed](images/view-domain-validation.png)

**Etape 4 :** une fois que vous avez effectué l'une des étapes de validation tirée de la section relative à la façon de répondre à une demande d'authentification de validation de domaine, votre CDN adopte le statut **Deploying certificate**. Pendant cette période, Akamai distribue votre certificat validé aux serveurs d'équilibrage des charges. Le déploiement d'un certificat peut prendre entre 2 et 4 heures.

  * Quand ce processus est terminé, tous les domaines, quelle que soit la méthode de validation utilisée, passent à l'état **CNAME Configuration**.

Vous trouverez des informations complémentaires relatives à l'exécution de la configuration CNAME ainsi qu'à la surveillance de votre CDN sur la page [Lancement de l'exécution](/docs/infrastructure/CDN?topic=CDN-getting-your-cdn-to-running-status#get-to-running).


## Validation DCV (Domain Control Validation)
{: #domain-control-validation}

Pour que votre nom de domaine CDN soit ajouté au certificat SAN, vous devez prouver que vous disposez d'un contrôle administratif sur votre domaine. Ce processus de validation est nommé DCV (Domain Control Validation). Vous avez un délai de 48 heures pour effectuer la validation DCV. Si vous ne le faites pas, votre demande expire et vous devez recommencer le processus de commande. Les sections suivantes décrivent les trois méthodes possibles pour effectuer la validation DCV.


### CNAME
{: #cname}

Cette méthode n'est recommandée que **QUE SI** votre CDN ne distribue **pas** le trafic en temps réel. Si votre domaine distribue le trafic en temps réel, nous vous recommandons d'utiliser la méthode Standard ou Redirect pour valider votre domaine.

Pour utiliser cette méthode, vous ajouterez un enregistrement CNAME pour votre domaine CDN dans votre configuration DNS. La valeur CNAME à utiliser est celle que vous avez utilisée lors de la création du CDN. Elle doit se terminer par le domaine `cdnedge.bluemix.net`. Aucune autre action n'est requise de votre part. La validation DCV progressera automatiquement à partir de là. Cette validation peut durer entre 2 et 4 heures. Une fois le certificat déployé, votre CDN passe directement au statut RUNNING.

La plupart des fournisseurs DNS peuvent vous fournir des instructions sur la manière de définir ou de modifier le CNAME. Voici un exemple d'enregistrement CNAME standard :

| **Type de ressource** | **Hôte** | **Pointe vers (CNAME)** | **TTL** |
|------------------|---------|-------------|----------------|
| CNAME | www.example.com | example.cdnedge.bluemix.net | 15 minutes |


---
### Méthode Standard
{: #standard}

Si vous choisissez la méthode Standard  pour la validation de domaine, la fenêtre affiche deux options, **Challenge URL** et **Challenge response**. Pour effectuer le processus de validation de domaine, ajoutez à votre serveur d'origine la réponse **Challenge response** fournie. Cela permettra à l'autorité de certification d'extraire la réponse **Challenge response** de votre serveur d'origine en utilisant l'URL spécifiée dans **Challenge URL**. Une fois votre serveur d'origine correctement configuré, la validation de domaine peut prendre entre 2 et 4 heures.

   ![Domain Validation - Standard](images/domain-validation-standard.png)

Pour effectuer la procédure de validation de domaine via la méthode Standard, vous devez configurer votre serveur d'origine d'une façon particulière. Les exemples de procédure pour  les serveurs _Apache(TM)_ et _Nginx(TM)_ sont présentés ci-après.

**Exemple de situation**
* Serveur d'origine : `www.example.com`
* Domaine CDN : `cdn.example.com`
* URL de demande d'authentification (option Challenge URL) : `http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject`
* Réponse à la demande d'authentification (option Challenge Response) : `examplechallenge`

#### Configuration Apache
{: #apache-configuration}

  * **Etape 1 :** connectez-vous à la machine en exécutant le serveur Apache2.

  * **Etape 2 :** créez le fichier pour la demande d'authentification sous `.well-known/acme-challenge/`, dans le répertoire de contenu de votre site web.  L'emplacement par défaut pour le contenu de site Web Apache2 est `/var/www/html/`. Pour cet exemple, la réponse à la demande d'authentification sera placée dans le répertoire `/var/www/html/.well-known/acme-challenge/`.

      ```
      mkdir -p /var/www/html/.well-known/acme-challenge
      printf "examplechallenge" > /var/www/html/.well-known/acme-challenge/examplechallenge-fileobject
      ```

  * **Etape 3 :** si nécessaire, ouvrez votre fichier de configuration de serveur Apache2. `/etc/apache2/apache2.conf` et `/etc/apache2/sites-enabled/` sont les emplacements par défaut pour les fichiers de configuration.

  * **Etape 4 :** si nécessaire, ajoutez votre domaine CDN en tant qu'alias **ServerAlias** supplémentaire dans votre hôte virtuel pour votre serveur d'origine.

  * **Etape 5 :** s'il vous fallait modifier votre configuration de serveur Apache2, redémarrez le serveur Apache2 avec un temps d'indisponibilité minimal en utilisant la commande suivante :

      ```
      apachectl -k graceful
      ```

  * **Etape 6 :** créez un enregistrement A dans votre DNS entre le domaine CDN et l'adresse IP du serveur d'origine.

#### Configuration Nginx
{: #nginx-configuration}

  * **Etape 1 :** connectez-vous à la machine en exécutant le serveur Nginx.

  * **Etape 2 :** créez le fichier pour la demande d'authentification sous `.well-known/acme-challenge/`, dans le répertoire de contenu de votre site web.  L'emplacement par défaut pour le contenu de site Nginx est `/usr/share/nginx/html/`.  Pour cet exemple, la réponse à la demande d'authentification sera placée dans le répertoire `/usr/share/nginx/html/.well-known/acme-challenge/`.
      ```
      mkdir -p /usr/share/nginx/html/.well-known/acme-challenge
      printf "examplechallenge" > /usr/share/nginx/html/.well-known/acme-challenge/examplechallenge-fileobject
      ```

  * **Etape 3 :** si nécessaire, ouvrez votre fichier de configuration de serveur Nginx. `/etc/nginx/nginx.conf` et`/etc/nginx/conf.d/` sont les emplacements par défaut pour les fichiers de configuration.

  * **Etape 4 :** si nécessaire, ajoutez votre domaine CDN en tant serveur **server_name** supplémentaire dans le bloc server pour votre serveur d'origine.

  * **Etape 5 :** s'il vous fallait modifier votre configuration de serveur Nginx, redémarrez le serveur Nginx avec un temps d'indisponibilité minimal en utilisant la commande suivante :

      ```
      nginx -s reload
      ```

  * **Etape 6 :** créez un enregistrement A dans votre DNS entre le domaine CDN et l'adresse IP du serveur d'origine.

#### Vérifiez que cette méthode Standard de réponse à la validation de domaine est prête pour l'autorité de certification
{: #verify-that-this-standard-method-to-address-domain-validation-is-ready-for-the-ca}

* Pour vérifier que cette méthode fonctionne via `curl`, exécutez la commande suivante pour l'URL de demande d'authentification.
    ```
    curl -v http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```
* Pour vérifier que cette méthode fonctionne via un navigateur, essayez d'accéder à l'URL de demande d'authentification depuis votre navigateur.

Dans les deux cas, vous devez être capable d'extraire la copie de l'objet fichier de demande d'authentification de validation de domaine stockée dans votre serveur d'origine.

#### Nettoyage pour la méthode Standard
{: #clean-up-for-the-standard-method}

Une fois que votre CDN a atteint le statut **Certificate deploying** :
1. Retirez le fichier `examplechallenge-fileobject` (facultatif).
1. Retirez l'alias ServerAlias ajouté (Apache2) ou le serveur server_name (Nginx) de la configuration de serveur, si nécessaire (facultatif).
1. Retirez l'enregistrement A entre le domaine CDN et l'adresse IP du serveur d'origine.

---
### Méthode Redirect
{: #redirect}

Cliquez sur l'onglet **Redirect** pour afficher toutes les informations permettant de répondre à la validation de domaine via une redirection. Ces informations permettent à l'autorité de certification d'extraire une copie de la **réponse à la demande d'authentification (option Challenge Response)** depuis Akamai via votre serveur d'origine. Une fois votre serveur correctement configuré, la validation de domaine peut prendre entre 2 et 4 heures.

   ![Domain Validation - Redirect](images/domain-validation-redirect.png)

Pour effectuer la procédure de validation de domaine via la méthode Redirect, vous devez configurer votre serveur Web d'une façon particulière. Les exemples de procédure pour les serveurs Apache et Nginx sont présentés dans les sections ci-après.

**Exemple de situation**
* Serveur d'origine : `www.example.com`
* Domaine CDN : `cdn.example.com`
* URL de demande d'authentification (option Challenge URL) : `http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject`
* Redirection d'URL : `http://dcv.akamai.com/.well-known/acme-challenge/examplechallenge-fileobject`

#### Configuration de la redirection Apache
{: #apache-redirect-configuration}

  * **Etape 1 :** connectez-vous à la machine en exécutant le serveur Apache2.

  * **Etape 2 :** ouvrez votre fichier de configuration de serveur Apache2. `/etc/apache2/apache2.conf` et `/etc/apache2/sites-enabled/` sont les emplacements par défaut du fichier de configuration.

  * **Etape 3 :** ajoutez une instruction de redirection à l'emplacement approprié du fichier de configuration. Si nécessaire, ajoutez votre domaine CDN en tant qu'alias **ServerAlias** supplémentaire dans votre hôte virtuel pour votre serveur d'origine.

    ```
    Redirect /.well-known/acme-challenge/examplechallenge-fileobject http://dcv.akamai.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```

  * **Etape 4 :** redémarrez le serveur Apache2 avec un temps d'indisponibilité minimal en utilisant la commande suivante :

    ```
    apachectl -k graceful
    ```

  * **Etape 5 :** créez un enregistrement A dans votre DNS entre le domaine CDN et l'adresse IP du serveur d'origine.

#### Configuration de la redirection Nginx
{: #nginx-redirect-confguration}

  * **Etape 1 :** connectez-vous à la machine en exécutant le serveur Nginx.

  * **Etape 2 :** ouvrez votre fichier de configuration de serveur Nginx. `/etc/nginx/nginx.conf` et `/etc/nginx/conf.d/` sont les emplacements par défaut pour les fichiers de configuration.

  * **Etape 3 :** vous disposez de deux méthodes valides pour cette étape.

    * Option 1 : (recommandée) ajoutez un bloc `location` avec une directive `return` pour effectuer la redirection dans le bloc `server` approprié. Si nécessaire, ajoutez votre domaine CDN en tant serveur **server_name** supplémentaire dans le bloc server pour votre serveur d'origine.

    ```
    server {
      listen 80;
      server_name www.example.com cdn.example.com;

      # Some server configuration directives
      # ...

      location = /.well-known/acme-challenge/examplechallenge-fileobject  {
          return 302 http://dcv.akamai.com$request_uri;
      }

      # Some more server configuration directives
      # ...
    }
    ```

   * Option 2 : ajoutez une directive `rewrite` dans le bloc `server`. Si nécessaire, ajoutez votre domaine CDN en tant serveur **server_name** supplémentaire dans le bloc server pour votre serveur d'origine.

    ```
    server {
      listen 80;
      server_name www.exmaple.com cdn.example.com;

      # Some server configuration directives
      # ...

      rewrite ^/(\.well-known/acme-challenge/examplechallenge-fileobject)$ http://dcv.akamai.com/$1 redirect;

      # Some more server configuration directives
      # ...
    }
    ```

  * **Etape 4 :** redémarrez le serveur Nginx avec un temps d'indisponibilité minimal en utilisant la commande suivante :

    ```
    nginx -s reload
    ```

  * **Etape 5 :** créez un enregistrement A dans votre DNS entre le domaine CDN et l'adresse IP du serveur d'origine.

#### Vérifiez que la redirection fonctionne correctement
{: #verify-that-the-redirect-is-occurring}

La procédure ci-dessous redirige le trafic _uniquement_ pour l'URL de demande d'authentification spécifique vers la redirection d'URL. Vous pouvez vérifier que la redirection a bien fonctionné via `curl` ou le navigateur.

* Pour vérifier que la redirection fonctionne via `curl`, exécutez la commande suivante pour l'URL de demande d'authentification.

    ```
    curl -vL http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```

* Pour vérifier que la redirection fonctionne via le navigateur, essayez d'atteindre l'URL de demande d'authentification depuis votre navigateur.

Dans les deux cas, vous devez être capable d'extraire la copie de l'objet fichier de demande d'authentification de validation de domaine depuis Akamai, dans le domaine `dcv.akamai.com`, où la demande d'origine a été redirigée.

#### Nettoyage pour la méthode Redirect
{: #clean-up-for-the-redirect-method}

Une fois que votre CDN a atteint le statut **Certificate deploying** :
1. Retirez les blocs ou instructions de redirection du fichier de configuration (facultatif).
1. Retirez l'alias ServerAlias ajouté (Apache2) ou le serveur server_name (Nginx) de la configuration de serveur, si nécessaire (facultatif).
1. Retirez l'enregistrement A entre le domaine CDN et l'adresse IP du serveur d'origine.
