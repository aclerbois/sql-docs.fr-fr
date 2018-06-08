---
title: Historique de révision de membre (Master Data Services) | Microsoft Docs
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
ms.assetid: 113069c5-12e6-48ec-b443-b42e14f77308
caps.latest.revision: 7
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: 17f6aedcddb4b42f76ecd9da5de0f9fa47f7b84b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="member-revision-history-master-data-services"></a>Historique de révision de membre (Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Un historique de révision de membre est enregistré à chaque modification d’un membre, si le journal des transactions entité est de type membre.  
  
 Pour plus d’informations sur les types de journaux de transactions, consultez [Modifier le type du journal des transactions de l’entité &#40;Master Data Services&#41;](../master-data-services/change-the-entity-transaction-log-type-master-data-services.md).  
  
 Les historiques de révision de membre sont enregistrés dès lors qu’interviennent les modifications suivantes :  
  
-   Création, suppression, réactivation ou purge des membres.  
  
-   Modification des valeurs d’attribut.  
  
-   Déplacement des membres dans une hiérarchie ou une collection.  
  
## <a name="view-and-manage-revision-history-by-entity"></a>Affichage et gestion de l’historique de révision par entité  
 Dans la zone fonctionnelle Explorateur, vous pouvez afficher les révisions pour tous les membres de l’entité. Si vous disposez des autorisations de mise à jour, vous pouvez restaurer le membre à une révision précédente.  
  
 **Afficher et gérer l’historique de révision**  
  
1.  Dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], sélectionnez le modèle et la version, puis cliquez sur **Explorateur**.  
  
2.  Sélectionnez l’entité dans le menu **Entités** .  
  
3.  Cliquez sur **Afficher l’historique** pour afficher toutes les données historiques de l’entité.  
  
4.  Cliquez sur **Filtrer** pour filtrer les données.  
  
5.  Cliquez sur l’en-tête de colonne pour trier les données.  
  
6.  Si vous disposez des autorisations de mise à jour, cliquez sur **Rétablir le membre** pour revenir à la version sélectionnée.  
  
## <a name="view-and-manage-revision-history-by-member"></a>Affichage et gestion de l’historique de révision par membre  
 Dans la zone fonctionnelle Explorateur, vous pouvez afficher les révisions associées à un membre si vous disposez des autorisations de lecture sur ce membre. Si vous disposez des autorisations de mise à jour, vous pouvez restaurer le membre à une révision précédente ou ajouter des annotations à la révision.  
  
1.  Dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], sélectionnez le modèle et la version, puis cliquez sur **Explorateur**.  
  
2.  Sélectionnez l’entité dans le menu **Entités** .  
  
3.  Sélectionnez le membre.  
  
4.  Dans le volet de droite, cliquez sur **Afficher l’historique** .  
  
## <a name="log-retention-setting"></a>Paramètres de rétention des journaux  
 Vous pouvez configurer la durée de conservation des données d’historique en définissant la propriété **Durée de conservation des journaux en jours** dans les paramètres système de la base de données [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] et en configurant le paramètre **Jours de conservation des journaux** lorsque vous créez ou modifiez un modèle.  
  
## <a name="related-task"></a>Tâche connexe  
  
|Description de la tâche|Rubrique|  
|----------------------|-----------|  
|Rétablissement de l’historique de révision de membre|[Rétablissement de l’historique de révision de membre &#40;Master Data Services&#41;](../master-data-services/rollback-member-revision-history-master-data-services.md)|  
  
## <a name="see-also"></a> Voir aussi  
 [Créer un modèle &#40;Master Data Services&#41;](../master-data-services/create-a-model-master-data-services.md)   
 [Paramètres système &#40;Master Data Services&#41;](../master-data-services/system-settings-master-data-services.md)  
  
  
