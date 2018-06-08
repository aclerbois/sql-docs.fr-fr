---
title: Créer un modèle (Master Data Services) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.component: non-specific
ms.reviewer: ''
ms.suite: sql
ms.technology:
- master-data-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- models [Master Data Services], creating models
- creating models [Master Data Services]
ms.assetid: 9bb3b3b3-bde8-44aa-ad62-eaae21188141
caps.latest.revision: 13
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: f1c2cb47c12d162fceb8b0b59fd6d493fbe29195
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-a-model-master-data-services"></a>Créer un modèle (Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], créez un modèle destiné à contenir des objets de modèle.  
  
## <a name="prerequisites"></a>Conditions préalables requises  
 Pour effectuer cette procédure :  
  
-   Vous devez avoir l'autorisation d'accéder à la zone fonctionnelle **Administration de système** .  
  
-   Vous devez être administrateur de modèle. Pour plus d’informations, consultez [Administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
### <a name="to-create-a-model"></a>Pour créer un modèle  
  
1.  Dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], cliquez sur **Administration de système**.  
  
2.  Dans la page **Vue du modèle** , dans la barre de menus, pointez sur **Gérer** , puis cliquez sur **Modèles**.  
  
3.  Sur la page **Gérer les modèles** , cliquez sur **Ajouter**. Un panneau s’affiche à droite.  
  
4.  Dans la zone **Nom** , tapez le nom du modèle.  
  
5.  (Facultatif) Dans le champ **Description** , saisissez la description du modèle.  
  
6.  Dans le champ **Nombre de jours de rétention du journal** , sélectionnez l’une des options permettant de conserver les données du journal. La valeur par défaut est **Paramètre système**, ce qui indique que la valeur est héritée des paramètres système dans le [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)]. Pour plus d’informations, consultez [Paramètres système &#40;Master Data Services&#41;](../master-data-services/system-settings-master-data-services.md).  
  
     Pour remplacer le paramètre système sans supprimer les données du journal des transactions, sélectionnez **NON**. Pour conserver uniquement les données du journal d’aujourd’hui et effacer celles des jours précédents, sélectionnez **OUI** et réglez le champ **Jours** sur 0. Pour conserver les données du journal pendant un nombre de jours déterminé, sélectionnez **OUI** et réglez le champ **Jours** sur le nombre de jours souhaité.  
  
7.  Sélectionnez éventuellement **Créer une entité portant le même nom que le modèle** pour créer une entité du même nom que le modèle.  
  
8.  Cliquez sur **Enregistrer le modèle**.  
  
 Pour chaque modèle créé, une ligne comportant huit colonnes est ajoutée à la grille. Ces huit colonnes sont les suivantes :  
  
-   **État**: l’état du modèle. Quand vous cliquez sur le bouton **Enregistrer le modèle**, l’image ![Mise à jour](../master-data-services/media/mds-model-status-updating.png "Mise à jour") s’affiche, ce qui signifie que le modèle est en cours de mise à jour. En cas d’erreur pendant la création ou la modification d’un modèle, l’image ![Erreur](../master-data-services/media/mds-model-status-error.png "Erreur") apparaît. Dans le cas contraire, le statut est OK et l’image ![OK](../master-data-services/media/mds-model-status-ok.png "OK") apparaît.  
  
-   **Nom**: le nom du modèle.  
  
-   **Description**: la description du modèle.  
  
-   **Nombre de jours de rétention du journal**: le nombre de jours de conservation du journal pour le modèle.  
  
-   **Créé par**: le nom de l’utilisateur qui a créé le modèle.  
  
-   **Date et heure de création**: la date et l’heure de création du modèle.  
  
-   **Mis à jour par**: le nom de l’utilisateur qui a effectué la dernière mise à jour du modèle.  
  
-   **Date et heure de mise à jour**: la date et l’heure de dernière mise à jour du modèle.  
  
## <a name="next-steps"></a>Next Steps  
  
-   [Créer une entité &#40;Master Data Services&#41;](../master-data-services/create-an-entity-master-data-services.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Modèles &#40;Master Data Services&#41;](../master-data-services/models-master-data-services.md)   
 [Entités &#40;Master Data Services&#41;](../master-data-services/entities-master-data-services.md)   
 [Supprimer un modèle &#40;Master Data Services&#41;](../master-data-services/delete-a-model-master-data-services.md)   
 [Modifier un modèle &#40;Master Data Services&#41;](../master-data-services/edit-model-master-data-services.md)   
 [Transactions &#40;Master Data Services&#41;](../master-data-services/transactions-master-data-services.md)  
  
  
