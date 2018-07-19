---

copyright:
  years: 2018
lastupdated: "2018-06-15"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Lancement de l'exécution de votre CDN

Apprenez comment exécuter votre CDN, en suivant les instructions ci-après. Ce document vous indique également comment démarrer et arrêter votre CDN.

## Lancement de l'exécution

Une fois que vous avez créé un CDN, il apparaît sur votre tableau de bord des CDN. Vous voyez le nom du CDN, son origine, son fournisseur et son statut.  

 ![Capture d'écran de la liste des mappages](images/mapping_list_cname.png)


Si vous avez commandé votre CDN avec un certificat de caractère générique HTTP ou HTTPS, vous pouvez passer à l'étape 1.

Si vous avez créé un CDN avec un certificat SAN DV HTTPS, des étapes supplémentaires peuvent s'avérer nécessaires pour vérifier votre domaine, décrites dans la page [Exécution de la validation DCV (Domain Control Validation) pour HTTPS](how-to-https.html#completing-domain-control-validation-for-https).

**Etape 1 :**

Une fois que vous avez commandé un CDN, vous devez configurer le **CNAME** avec votre fournisseur DNS. La plupart des fournisseurs DNS peuvent vous fournir des instructions sur la manière de définir ou de modifier le CNAME.

   * Pendant ce temps, le statut de votre CDN affichera **CNAME Configuration**. Contactez votre fournisseur DNS pour savoir quand seront prises en compte les modifications.

   ![CNAME config](images/cname-config.png)  

**Etape 2 :**

Après avoir configuré le CNAME avec votre fournisseur DNS, vous pouvez vérifier à tout moment son statut en sélectionnant **Get Status** à partir du menu déroulant dynamique situé à droite du statut de votre CDN.

  ![CNAME getStatus](images/cname-getstatus.png)  

**Etape 3 :**

Une fois le chaînage CNAME terminé, la sélection de l'option **Get Status** change le statut en *RUNNING* et le CDN est prêt à être utilisé.

Félicitations ! Votre CDN est désormais actif. Consultez ensuite la page [Gestion de votre CDN](how-to.html#manage-your-CDN) pour obtenir des informations supplémentaires sur les options de configuration, disponibles dans les rubriques [Durée de vie](how-to.html#setting-content-caching-time-using-time-to-live-), [Purge du contenu mis en cache](how-to.html#purging-cached-content) et [Ajout de détails sur le chemin d'origine](how-to.html#adding-origin-path-details).

## Démarrage du CDN

Le CDN ne peut être démarré que lorsque son statut est "Arrêté".  

**Etape 1 :**

Cliquez sur "Start CDN" à partir du menu déroulant dynamique, à savoir les 3 points à droite de la ligne du CDN.

  ![Menu déroulant dynamique](images/start_cdn.png)

**Etape 2 :**

Une boîte de dialogue plus grande s'ouvre, vous demandant de confirmer le démarrage du service. Sélectionnez **Confirm** pour poursuivre.

**Etape 3 :**

Si l'action aboutit, une boîte de dialogue apparaît dans l'angle supérieur droit de l'écran, indiquant que l'action a été effectuée avec succès, avec l'heure.

**Etape 4 :**

Cette étape change le statut en "CNAME Configuration".

**Etape 5 :**

Cliquez sur l'option "Get Status" du menu déroulant dynamique. Cette étape change le statut en "Running". Votre CDN devient opérationnel.

## Arrêt du CDN

UN CDN ne peut être arrêté que lorsque son statut est "Running".

**Etape 1 :**

Cliquez sur "Stop CDN" à partir du menu déroulant dynamique (3 points verticaux en regard du statut CDN).
 ![Menu déroulant dynamique](images/stop_cdn.png)

**Etape 2 :**

Une boîte de dialogue plus grande s'ouvre, vous demandant de confirmer l'arrêt du service. Sélectionnez **Confirm** pour poursuivre.

**Etape 3 :**

Après environ 5-15 secondes, le statut passe à "Stopped'

## Suppression du CDN

Pour supprimer un CDN, procédez comme suit :

**REMARQUE** : la sélection de `Delete` depuis le menu déroulant dynamique supprime uniquement le CDN, elle ne supprime pas votre compte.

**Etape 1 :**

Cliquez sur "Delete" à partir du menu déroulant dynamique.

 ![Menu déroulant dynamique avec commande Delete](images/delete_cdn.png)

**Etape 2 :**

Une boîte de dialogue plus grande s'ouvre, vous demandant de confirmer la suppression. Cliquez sur **Delete** pour poursuivre.

**Remarque** : si votre CDN est configuré en utilisant HTTPS avec un certificat SAN DV, le processus de suppression peut prendre jusqu'à 5 heures.

  ![Suppression avec avertissement](images/delete-with-warning.png)

**Etape 3 :**

Une fois les étapes 1 et 2 terminées, le statut de votre CDN sera `Deleting`. Quand le processus de suppression est terminé, en cliquant sur Get Status dans le menu déroulant dynamique, la ligne est retirée de la liste CDN. Si le processus de suppression n'est pas terminé, cette action n'a pas d'effet.
