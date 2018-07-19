---

copyright:
  years: 2018
lastupdated: "2018-06-17"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Exécution de la validation DCV (Domain Control Validation) pour HTTPS

## Etapes de validation DCV initiales 

**Etape 1 :**

Une fois que vous avez commandé votre CDN avec un certificat SAN DV, le processus de demande de certificat commence. Lors de ce processus, IBM Cloud CDN demande un certificat depuis Akamai. Quand un certificat est disponible, Akamai émet une demande vers l'autorité de certification.

  * Pendant cette période, le statut CDN affiche **Requesting Certificate**.

    ![Statut Requesting Certificate](images/requesting-cert.png)

**Etape 2 :**

Une fois que l'autorité de certification a reçu la demande, elle émet une demande d'authentification de validation de domaine.

  * A ce moment là, le statut de votre CDN passe à **Domain validation needed**.

    ![Domain Validation Needed](images/domain-validation-needed.png)

**Etape 3 :**

Cliquez sur le nom du CDN qui doit être validé. Sur la page de présentation qui s'ouvre, vous pouvez voir le statut général de votre CDN. En haut de la page, une alerte s'affiche vous rappelant qu'une validation du domaine est nécessaire. Sélectionnez le bouton **View domain validation** pour ouvrir une fenêtre qui vous indique les informations d'authentification requises pour l'exécution du processus de validation.

   ![Domain Validation Needed](images/view-domain-validation.png)

**Etape 4 :** une fois que vous avez effectué l'une des étapes de validation tirée de la section relative à la façon de répondre à une demande d'authentification de validation de domaine, votre CDN adopte le statut **Deploying certificate**. Pendant cette période, Akamai distribue votre certificat validé aux serveurs d'équilibrage des charges. Le déploiement d'un certificat peut prendre entre 2 et 4 heures.

  * Quand ce processus est terminé, tous les domaines, quelle que soit la méthode de validation utilisée, passent à l'état **CNAME Configuration**.

Vous trouverez des informations complémentaires relatives à l'exécution de la configuration CNAME ainsi qu'à la surveillance de votre CDN dans la page [Lancement de l'exécution](basic-functions.html#get-to-running).


## Demande d'authentification de validation de domaine

La demande d'authentification de validation de domaine prouve que vous êtes le propriétaire du domaine. Les trois méthodes dont vous disposez pour répondre à la demande d'authentification de validation de domaine sont présentées ci-dessous.

**Remarque** : si vous ne répondez pas à la demande d'authentification de validation de domaine dans les 48 heures, cette demande expire et vous devez recommencer le processus de commande.

### CNAME

Si votre enregistrement CNAME a été ajouté à votre fournisseur DNS avant de commander votre CDN, vous n'avez rien d'autre à faire. La validation de domaine est automatiquement gérée par IBM Cloud, Akamai et l'autorité de certification. Cette validation peut prendre entre 2 et 4 heures.

  * Si vous n'avez pas encore configuré votre enregistrement CNAME avec votre fournisseur DNS, vous devez le faire maintenant. La plupart des fournisseurs DNS peuvent vous fournir des instructions sur la manière de définir ou de modifier le CNAME.

   ![Domain Validation - CNAME](images/domain-validation-cname.png)

**Remarque** : cette méthode n'est recommandée que **QUE SI** votre CDN ne sert **pas** le trafic en temps réel. Sinon, il est conseillé d'utiliser la méthode Standard pour valider votre domaine.

---
### Méthode Standard

Si vous choisissez la méthode Standard  pour la validation de domaine, la fenêtre affiche deux options, **Challenge URL** et **Challenge response**. Pour effectuer le processus de validation de domaine, ajoutez à votre serveur d'origine la réponse **Challenge response** fournie, ce qui permet à l'autorité de certification d'extraire la réponse **Challenge response** de votre serveur d'origine en utilisant l'URL spécifiée dans **Challenge URL**. Une fois votre serveur d'origine correctement configuré, la validation de domaine peut prendre entre 2 et 4 heures.

   ![Domain Validation - Standard](images/domain-validation-standard.png)

Pour effectuer la procédure de validation de domaine via la méthode Standard, vous devez configurer votre serveur d'origine d'une façon particulière. Les procédures exemple pour les serveurs Apache et Nginx sont présentées ci-dessous.

**Situation exemple**
* Serveur d'origine : `www.example.com`
* Domaine CDN : `cdn.example.com`
* URL de demande d'authentification (option Challenge URL) : `http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject`
* Réponse à la demande d'authentification (option Challenge Response) : `examplechallenge`

#### Configuration Apache

  * **Etape 1 :** connectez-vous à la machine en exécutant le serveur Apache2.

  * **Etape 2 :** créez le fichier pour la demande d'authentification sous `.well-known/acme-challenge/`, dans le répertoire de contenu de votre site web. L'emplacement par défaut pour le contenu de site Web Apache2 est `/var/www/html/`. Pour cet exemple, la réponse à la demande d'authentification sera placée dans le répertoire `/var/www/html/.well-known/acme-challenge/`.

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

  * **Etape 1 :** connectez-vous à la machine en exécutant le serveur Nginx.

  * **Etape 2 :** créez le fichier pour la demande d'authentification sous `.well-known/acme-challenge/`, dans le répertoire de contenu de votre site web. L'emplacement par défaut pour le contenu de site Nginx est `/usr/share/nginx/html/`. Pour cet exemple, la réponse à la demande d'authentification sera placée dans le répertoire `/usr/share/nginx/html/.well-known/acme-challenge/`.
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

* Pour vérifier que cette méthode fonctionne via `curl`, exécutez la commande suivante pour l'URL de demande d'authentification.
    ```
    curl -v http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```
* Pour vérifier que cette méthode fonctionne via un navigateur, essayez d'accéder à l'URL de demande d'authentification depuis votre navigateur.

Dans les deux cas, vous devez être capable d'extraire la copie de l'objet fichier de demande d'authentification de validation de domaine stockée dans votre serveur d'origine.

#### Nettoyage pour la méthode Standard

Une fois que votre CDN a atteint le statut **Certificate deploying** :
1. Retirez le fichier `examplechallenge-fileobject` (facultatif).
1. Retirez l'alias ServerAlias ajouté (Apache2) ou le serveur server_name (Nginx) de la configuration de serveur, si nécessaire (facultatif).
1. Retirez l'enregistrement A entre le domaine CDN et l'adresse IP du serveur d'origine.

---
### Méthode Redirect

Cliquez sur l'onglet **Redirect** pour afficher toutes les informations permettant de répondre à la validation de domaine via une redirection. Ces informations permettent à l'autorité de certification d'extraire une copie de la **réponse à la demande d'authentification (option Challenge Response)** depuis Akamai via votre serveur d'origine. Une fois votre serveur correctement configuré, la validation de domaine peut prendre entre 2 et 4 heures.

   ![Domain Validation - Redirect](images/domain-validation-redirect.png)

Pour effectuer la procédure de validation de domaine via la méthode Redirect, vous devez configurer votre serveur Web d'une façon particulière. Les procédures exemple pour les serveurs Apache et Nginx sont présentées ci-dessous.

**Situation exemple**
* Serveur d'origine : `www.example.com`
* Domaine CDN : `cdn.example.com`
* URL de demande d'authentification (option Challenge URL) : `http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject`
* Redirection d'URL : `http://dcv.akamai.com/.well-known/acme-challenge/examplechallenge-fileobject`

#### Configuration de la redirection Apache

  * **Etape 1 :** connectez-vous à la machine en exécutant le serveur Apache2.

  * **Etape 2 :** ouvrez votre fichier de configuration de serveur Apache2. `/etc/apache2/apache2.conf` et `/etc/apache2/sites-enabled/` sont les emplacements par défaut pour le fichiers de configuration.

  * **Etape 3 :** ajoutez une instruction de redirection à l'emplacement approprié du fichier de configuration. Si nécessaire, ajoutez votre domaine CDN en tant qu'alias **ServerAlias** supplémentaire dans votre hôte virtuel pour votre serveur d'origine.
    ```
    Redirect http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject http://dcv.akamai.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```

  * **Etape 4 :** redémarrez le serveur Apache2 avec un temps d'indisponibilité minimal en utilisant la commande suivante :

    ```
    apachectl -k graceful
    ```

  * **Etape 5 :** créez un enregistrement A dans votre DNS entre le domaine CDN et l'adresse IP du serveur d'origine.

#### Configuration de la redirection Nginx

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

La procédure ci-dessous redirige le trafic uniquement pour l'URL de demande d'authentification spécifique vers la redirection d'URL. Vous pouvez vérifier le bon fonctionnement de la redirection via `curl` ou le navigateur.

* Pour vérifier que la redirection fonctionne via `curl`, exécutez la commande suivante pour l'URL de demande d'authentification.

    ```
    curl -vL http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```

* Pour vérifier que la redirection fonctionne via le navigateur, essayez d'accéder à l'URL de demande d'authentification depuis votre navigateur.

Dans les deux cas, vous devez être capable d'extraire la copie de l'objet fichier de demande d'authentification de validation de domaine depuis Akamai, dans le domaine dcv.akamai.com, où la demande d'origine a été redirigée.

#### Nettoyage pour la méthode Redirect

Une fois que votre CDN a atteint le statut **Certificate deploying** :
1. Retirez les blocs ou instructions de redirection du fichier de configuration (facultatif).
1. Retirez l'alias ServerAlias ajouté (Apache2) ou le serveur server_name (Nginx) de la configuration de serveur, si nécessaire (facultatif).
1. Retirez l'enregistrement A entre le domaine CDN et l'adresse IP du serveur d'origine.
