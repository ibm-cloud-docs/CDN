---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-21"

keywords: running status, additional steps, stop cdn, learn, configure cname, delete cdn, start cdn

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:warning: .warning}
{:download: .download}

# Lancement de l'exécution de votre CDN
{: #getting-your-cdn-to-running-status}

Apprenez comment exécuter votre CDN, en suivant les instructions ci-après. Ce document vous indique également comment démarrer et arrêter votre CDN.

## Lancement de l'exécution
{: #get-to-running}

Une fois que vous avez créé un CDN, il apparaît sur votre tableau de bord des CDN. Vous voyez le nom du CDN, son origine, son fournisseur et son statut.  

 ![Capture d'écran de la liste des mappages](images/mapping-list.png)


Si vous avez commandé votre CDN avec un certificat de caractère générique HTTP ou HTTPS, vous pouvez passer à l'étape 1.

Si vous avez créé un CDN avec un certificat SAN DV HTTPS, des étapes supplémentaires peuvent s'avérer nécessaires pour vérifier votre domaine, décrites dans la page [Exécution de la validation DCV (Domain Control Validation) pour HTTPS](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#completing-domain-control-validation-for-https).

**Etape 1 :**

Une fois que vous avez commandé un CDN, vous devez configurer le **CNAME** avec votre fournisseur DNS. La plupart des fournisseurs DNS peuvent vous fournir des instructions sur la manière de définir ou de modifier le CNAME.

   * Pendant ce temps, le statut de votre CDN affichera **CNAME Configuration**. Contactez votre fournisseur DNS pour savoir quand seront prises en compte les modifications.

   ![CNAME config](images/cname-config.png)  

**Etape 2 :**

Après avoir configuré le CNAME avec votre fournisseur DNS, vous pouvez vérifier à tout moment son statut en sélectionnant **Get Status** à partir du menu déroulant dynamique situé à droite du statut de votre CDN.

  ![CNAME getStatus](images/cname-getstatus.png)  

**Etape 3 :**

Une fois le chaînage CNAME terminé, la sélection de l'option **Get Status** change le statut en *RUNNING* et le CDN est prêt à être utilisé.

Félicitations ! Votre CDN est désormais actif. Consultez ensuite la page [Gestion de votre CDN](/docs/infrastructure/CDN?topic=CDN-manage-your-cdn#manage-your-cdn) pour obtenir des informations supplémentaires sur les options de configuration, disponibles dans les rubriques [Durée de vie](docs/infrastructure/CDN?topic=CDN-manage-your-cdn#setting-content-caching-time-using-time-to-live-), [Purge du contenu mis en cache](/docs/infrastructure/CDN?topic=CDN-manage-your-cdn#purging-cached-content) et [Ajout de détails sur le chemin d'origine](/docs/infrastructure/CDN?topic=CDN-manage-your-cdn#adding-origin-path-details).

## Démarrage du CDN
{: #starting-cdn}

Lorsque vous démarrez votre CDN, le serveur de noms de domaine est informé qu'il doit diriger le trafic depuis votre origine vers le serveur d'équilibrage des charges Akamai. Une fois le mappage démarré, le cache DNS peut encore diriger le trafic vers l'origine, par conséquent, il se peut que la fonctionnalité ne soit pas visible du domaine juste après le démarrage du mappage. Le temps nécessaire à la mise à jour dépend de la fréquence de régénération du cache DNS, qui varie en fonction d'un fournisseur DNS à un autre.

Un CDN peut être démarré uniquement s'il a pour statut `Stopped`.
{: note}

**Etape 1 :**

Cliquez sur **Start CDN** à partir du menu déroulant dynamique, à savoir les 3 points à droite de la ligne du CDN.

  ![Menu déroulant dynamique](images/start_cdn.png)

**Etape 2 :**

Une boîte de dialogue plus grande s'ouvre, vous demandant de confirmer le démarrage du service. Sélectionnez **Confirm** pour poursuivre.

**Etape 3 :**

Si l'action aboutit, une boîte de dialogue apparaît dans l'angle supérieur droit de l'écran, indiquant que l'action a été effectuée avec succès, avec l'heure.

**Etape 4 :**

Cette étape remplace le statut par `CNAME Configuration`

**Etape 5 :**

Cliquez sur **Get Status** dans le menu déroulant dynamique. Cette étape remplace le statut par `Running`. Votre CDN devient opérationnel.

## Arrêt d'un CDN
{: #stopping-a-cdn}

La fonctionnalité STOP CDN est destinée aux fenêtres de maintenance ne dépassant pas 7 jours. Au-delà de 7 jours, le CDN doit être démarré ou bien il sera désactivé et le trafic vers le CDN CNAME ne sera pas redirigé vers le serveur d'origine.
{: important}

Une fois qu'un mappage est arrêté, la recherche DNS est basculée vers l'origine. Le trafic ignore les serveurs d'équilibrage des charges CDN et le contenu est extrait directement de l'origine. Une fois qu'un mappage est arrêté, votre contenu peut ne pas être accessible pendant un bref laps de temps. Cela est dû au fait qu'il se peut que le cache DNS soit encore en train de diriger le trafic vers les serveurs d'équilibrage des charges Akamai. Toutefois, durant ce laps de temps, le serveur d'équilibrage des charges Akamai refusera le trafic pour le domaine. Ce laps de temps dépend de la fréquence de régénération du cache DNS, qui varie en fonction d'un fournisseur DNS à un autre.

UN CDN ne peut être arrêté que lorsque son statut est `Running`.
{: note}

Il n'est **PAS** recommandé d'arrêter un CDN qui est configuré avec un certificat SAN HTTPS, car il se peut que le trafic HTTPS ne fonctionne pas lorsque vous ramenez le CDN au statut `Running`.
{: important}

L'arrêt d'un CDN pour un domaine de caractère générique n'est **PAS** autorisé pour le moment.
{: important}

**Etape 1 :**

Cliquez sur "Stop CDN" à partir du menu déroulant dynamique (3 points verticaux en regard du statut CDN).
 ![Menu déroulant dynamique](images/stop_cdn.png)

**Etape 2 :**

Une boîte de dialogue plus grande s'ouvre, vous demandant de confirmer l'arrêt du service. Sélectionnez **Confirm** pour poursuivre.

**Etape 3 :**

Après environ 5-15 secondes, le statut passe à 'Stopped'

## Suppression d'un CDN
{: #deleting-a-cdn}

Pour supprimer un CDN, procédez comme suit :

La sélection de `Delete` depuis le menu déroulant dynamique supprime uniquement le CDN, elle ne supprime pas votre compte.
{: note}

**Etape 1 :**

Cliquez sur "Delete" à partir du menu déroulant dynamique.

 ![Menu déroulant dynamique avec commande Delete](images/delete_cdn.png)

**Etape 2 :**

Une boîte de dialogue plus grande s'ouvre, vous demandant de confirmer la suppression. Cliquez sur **Delete** pour poursuivre.

Si votre CDN est configuré en utilisant HTTPS avec un certificat SAN DV, le processus de suppression peut prendre jusqu'à 5 heures.
{: note}

  ![Suppression avec avertissement](images/delete-with-warning.png)

**Etape 3 :**

Une fois les étapes 1 et 2 terminées, le statut de votre CDN sera `Deleting`. Lorsque le processus de suppression est terminé, lorsque vous cliquez sur 'Get Status' dans le menu déroulant dynamique, la ligne est retirée de la liste CDN. Si le processus de suppression n'est pas terminé, cette action n'a pas d'effet.
