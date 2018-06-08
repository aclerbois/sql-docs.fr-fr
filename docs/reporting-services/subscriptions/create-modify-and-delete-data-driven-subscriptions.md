---
title: Créer, modifier ou supprimer des abonnements pilotés par les données | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: subscriptions
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- query-based subscriptions [Reporting Services]
- queries [Reporting Services], data-driven subscriptions
- subscriptions [Reporting Services], data-driven
- data-driven subscriptions
ms.assetid: 0ba2093e-9393-4eb6-af06-9da10988cfaf
caps.latest.revision: 51
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: cb4b22c712593429a1ad3485e643af48626c5734
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-modify-and-delete-data-driven-subscriptions"></a>Créer, modifier ou supprimer des abonnements pilotés par les données
  Un abonnement piloté par les données est un abonnement qui a recours à une requête pour obtenir les valeurs de données qui seront utilisées dans le traitement de l'abonnement au moment de l'exécution. Lorsque l'abonnement est déclenché, une requête est traitée pour récupérer des informations récentes sur les destinataires, les options de remise de rapport, les formats de rendu et les valeurs de paramètre. Les résultats de la requête sont combinées à la définition de l'abonnement pour créer un abonnement dynamique utilisant les données que vous avez conservées dans une base de données employés, une base de données clients ou dans toute autre base de données contenant des informations utilisables comme données d'abonnés.  
  
 Pour créer un abonnement piloté par les données ou modifier un abonnement existant, recourez aux pages Créer un abonnement piloté par les données dans le Gestionnaire de rapports. Ces pages vous guident tout au long du processus de création ou de modification d'un abonnement. Pour accéder à un abonnement après qu'il a été créé, utilisez la page Mes abonnements et la liste Abonnements d'un rapport. Pour savoir comment créer un abonnement piloté par les données, consultez [Créer un abonnement piloté par les données &#40;didacticiel SSRS&#41;](../../reporting-services/create-a-data-driven-subscription-ssrs-tutorial.md).  
  
 Dans cette rubrique :  
  
-   [Gestion et suppression d'un abonnement piloté par les données](#bkmk_manage_and_delete)  
  
-   [Création et modification d'un abonnement piloté par les données](#bkmk_create_and_modify)  
  
-   [Définition d'une requête qui extrait les informations d'abonnement](#bkmk_define_query)  
  
-   [Exécution de l'abonnement](#bkmk_run_subscription)  
  
##  <a name="bkmk_manage_and_delete"></a> Gestion et suppression d'un abonnement piloté par les données  
 Un abonnement piloté par les données en cours d'exécution ne peut pas être arrêté ou supprimé via la page Gérer les travaux du Gestionnaire de rapports. Par conséquent, il est préférable d'utiliser une planification partagée pour déclencher l'abonnement piloté par les données. Si vous voulez empêcher temporairement l'exécution d'un abonnement, vous pouvez suspendre la planification qui le déclenche. Pour plus d’informations, consultez [old_Créer et gérer des abonnements pour les serveurs de rapports en mode natif](http://msdn.microsoft.com/en-us/7f46cbdb-5102-4941-bca2-5e0ff9012c6b).  
  
 Pour supprimer un abonnement piloté par les données, sélectionnez-le dans la page Mes abonnements ou dans la page Abonnements d’un rapport, puis cliquez sur **Supprimer**.  
  
 Pour obtenir des instructions sur l’annulation d’un abonnement piloté par les données, consultez [Gérer un processus en cours d’exécution](../../reporting-services/subscriptions/manage-a-running-process.md).  
  
##  <a name="bkmk_create_and_modify"></a> Création et modification d'un abonnement piloté par les données  
 Pour créer un abonnement piloté par les données, sélectionnez un rapport qui utilise des informations d'identification stockées ou aucune information d'identification. Lorsque vous créez l'abonnement piloté par les données, nous vous conseillons d'utiliser une convention d'affectation de noms pour le champ de description, afin de pouvoir différencier facilement les abonnements standard des abonnements pilotés par les données.  
  
#### <a name="to-create-a-data-driven-subscription-native-mode"></a>Pour créer un abonnement piloté par les données (mode natif)  
  
1.  Dans le Gestionnaire de rapports, naviguez jusqu'au dossier qui contient le rapport, pointez sur le rapport, ouvrez le menu d'options et cliquez sur **Gérer**.  
  
2.  Cliquez sur l'onglet **Abonnements** .  
  
3.  Cliquez sur le bouton **Nouvel abonnement piloté par les données** .  
  
#### <a name="to-create-a-data-driven-subscription-sharepoint-mode"></a>Pour créer un abonnement piloté par les données (mode SharePoint)  
  
1.  Dans la bibliothèque de documents SharePoint, pointez sur le rapport, ouvrez le menu d'options et cliquez sur **Gérer les abonnements**.  
  
2.  Cliquez sur **Ajouter un abonnement piloté par les données**.  
  
#### <a name="to-modify-an-existing-data-driven-subscription-native-mode"></a>Pour modifier un abonnement piloté par les données existant (mode natif)  
  
1.  Dans le Gestionnaire de rapports, naviguez jusqu'au dossier qui contient le rapport, pointez sur le rapport, ouvrez le menu d'options et cliquez sur **Gérer**.  
  
2.  Cliquez sur l'onglet **Abonnements** . Vous pouvez également cliquer sur le lien **Mes abonnements** situé en haut du gestionnaire de rapports.  
  
3.  Sélectionnez l'abonnement à modifier. L’icône suivante indique un abonnement piloté par les données : ![Icône Abonnement piloté par les données](../../reporting-services/subscriptions/media/hlp-16subscriptiondd.gif "Icône Abonnement piloté par les données")  
  
#### <a name="to-modify-an-existing-data-driven-subscription-sharepoint-mode"></a>Pour modifier un abonnement piloté par les données existant (mode SharePoint)  
  
1.  Dans la bibliothèque de documents SharePoint, pointez sur le rapport, ouvrez le menu d'options et cliquez sur **Gérer les abonnements**.  
  
2.  Sélectionnez l'abonnement à modifier.  
  
> [!NOTE]  
>  Vous pouvez modifier n'importe quelle valeur déjà spécifiée. Toutes les valeurs sont présentées comme elles ont été créées, à l'exception du mot de passe qui est utilisé pour accéder à la banque de données des abonnés. Vous devez retaper le mot de passe chaque fois que vous modifiez des valeurs dans la deuxième page ou dans les pages suivantes.  
  
 Avant de créer un abonnement piloté par les données, assurez-vous que les conditions suivantes sont remplies :  
  
-   **Conditions requises liées au rapport**. Le rapport doit utiliser des informations d'identification stockées ou ne pas en utiliser du tout pour être en mesure d'extraire les données au moment de l'exécution. Vous ne pouvez pas vous abonner à un rapport qui utilise des informations d'identification déléguées ou empruntées pour vous connecter à une source de données externe ; les informations d'identification de l'utilisateur qui crée ou possède l'abonnement ne seront pas disponibles lorsque l'abonnement sera traité. Les informations d'identification stockées peuvent être un compte Windows ou un compte d'utilisateur de base de données. Pour plus d’informations, consultez [Spécifier des informations d’identification et de connexion pour les sources de données de rapport](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md).  
  
     Vous ne pouvez pas vous abonner à un rapport du Générateur de rapports qui utilise un modèle comme source de données si le modèle contient des paramètres de sécurité de l'élément de modèle. Seuls les rapports qui utilisent la sécurité de l'élément de modèle sont inclus dans cette restriction.  
  
     Vous ne pouvez pas créer un abonnement piloté par les données pour un rapport qui contient l'expression `User!UserID` .  
  
-   **Conditions requises liées aux données**. Vous devez posséder une source de données externe et accessible contenant des données d'abonnés.  
  
-   **Conditions requises liées à l'utilisateur**. L'auteur de l'abonnement doit être autorisé à « Gérer les rapports » et « Gérer tous les abonnements ». Pour plus d’informations sur les autorisations d’exécution de tâches au niveau élément, consultez [Tâches et autorisations](../../reporting-services/security/tasks-and-permissions.md). L'auteur doit également posséder les informations d'identification requises pour accéder à la source de données externe qui contient les données des abonnés.  
  
##  <a name="bkmk_define_query"></a> Définition d'une requête qui extrait les informations d'abonnement  
 Un abonnement piloté par les données doit spécifier une requête ou une commande qui permet d'extraire les données des abonnés. La requête doit produire une ligne pour chaque abonné. Si vous utilisez l'extension de remise par messagerie électronique, la requête doit retourner un alias de messagerie pour chaque abonné. Le nombre de remises effectuées est basé sur le nombre de lignes retournées par la requête. Si le jeu de lignes contient 10 000 lignes, l'abonnement remet 10 000 rapports.  
  
 Si l'exécution de la requête est trop longue, vous pouvez augmenter la valeur du délai d'expiration pour permettre un temps de traitement supplémentaire.  
  
 Pour cette étape, la requête doit être validée avant que vous continuiez. La validation ne traite pas la requête mais retourne la liste de toutes les colonnes qui se trouvent dans l'ensemble de lignes, ce qui vous permet de référencer les colonnes lors de sélections ultérieures. Si la validation de la requête échoue, il vous est impossible de continuer. Une requête n'est pas validée si sa syntaxe est incorrecte ou si la connexion à la source de données n'est pas valide. Utilisez le bouton **Précédent** pour effectuer les corrections qui s'imposent sur la source de données.  
  
##  <a name="bkmk_run_subscription"></a> Exécution de l'abonnement  
 Vous devez indiquer les conditions du traitement de l'abonnement. Vous pouvez spécifier une planification ou déclencher l'abonnement de façon à ce qu'il coïncide avec la mise à jour de l'instantané d'exécution de rapport. Le traitement des abonnements pilotés par les données est identique au traitement des abonnements standard.  
  
## <a name="see-also"></a> Voir aussi  
 [Créer et gérer des abonnements pour les serveurs de rapports en mode natif](../../reporting-services/subscriptions/create-and-manage-subscriptions-for-native-mode-report-servers.md)   
 [Abonnements et remise &#40;Reporting Services&#41;](../../reporting-services/subscriptions/subscriptions-and-delivery-reporting-services.md)   
 [Gestionnaire de rapports &#40;SSRS en mode natif&#41;](http://msdn.microsoft.com/library/80949f9d-58f5-48e3-9342-9e9bf4e57896)   
 [old_Créer et gérer des abonnements pour les serveurs de rapports en mode natif](http://msdn.microsoft.com/en-us/7f46cbdb-5102-4941-bca2-5e0ff9012c6b)   
 [Page Abonnements &#40;Gestionnaire de rapports&#41;](http://msdn.microsoft.com/library/cf3a6bd0-e0b2-4875-a532-63ef34cfa860)   
 [Page Mes abonnements &#40;Gestionnaire de rapports&#41;](http://msdn.microsoft.com/library/491a85a3-f323-4155-a0a8-de2779899995)  
  
  
