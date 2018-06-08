---
title: Modifier une entité (Master Data Services)| Microsoft Docs
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
- entities [Master Data Services], changing name
ms.assetid: 6a5b9f14-6dfc-49d7-a771-e96461d4feae
caps.latest.revision: 8
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: 3f9a8a425e4bb44ac46f18f526df6fabf653581b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="edit-an-entity-master-data-services"></a>Modifier une entité (Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], vous pouvez modifier une entité.  
  
## <a name="prerequisites"></a>Conditions préalables requises  
 Pour effectuer cette procédure :  
  
-   Vous devez avoir l'autorisation d'accéder à la zone fonctionnelle **Administration de système** .  
  
-   Vous devez être administrateur de modèle. Pour plus d’informations, consultez [Administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
### <a name="to-edit-an-entity"></a>Pour modifier une entité  
  
1.  Dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], cliquez sur **Administration de système**.  
  
2.  Dans la page **Gérer le modèle** , sélectionnez un modèle dans la grille et cliquez sur **Entités**.  
  
3.  Dans la grille de la page **Gérer l’entité** , sélectionnez la ligne de l’entité à modifier, puis cliquez sur **Modifier**.  
  
4.  Dans la zone **Nom** , tapez le nom mis à jour de l’entité.  
  
5.  Dans le champ **Description** , tapez la description mise à jour de l’entité.  
  
6.  Dans le champ **Nom des tables intermédiaires** , tapez un nom mis à jour pour la table de mise en lots.  
  
7.  Pour le champ **Type du journal des transactions**, choisissez le type mis à jour du journal des transactions dans la liste déroulante.  
  
     Pour plus d’informations, consultez [Modifier le type du journal des transactions de l’entité &#40;Master Data Services&#41;](../master-data-services/change-the-entity-transaction-log-type-master-data-services.md).  
  
8.  Cochez ou décochez la case **Créer automatiquement des valeurs de code** .  
  
     Pour plus d’informations, consultez [Création automatique de code &#40;Master Data Services&#41;](../master-data-services/automatic-code-creation-master-data-services.md).  
  
9. Cochez la case **Autoriser la compression des données** . La compression de ligne est activée par défaut.  
  
     Pour plus d'informations, consultez [Data Compression](../relational-databases/data-compression/data-compression.md)  
  
## <a name="status"></a>État  
 La colonne d’état de la grille affiche l’état de l’opération sur l’entité. Lorsque vous cliquez sur **Enregistrer l’entité**, l’image ci-après s’affiche pour indiquer que l’entité est en cours de mise à jour.  
  
 ![Icône d’état de mise à jour](../master-data-services/media/mds-statusicon-updating.png "Icône d’état de mise à jour")  
  
 En cas d’erreur lors de la création ou de la modification d’une entité, l’image suivante apparaît.  
  
 ![Icône d’état d’erreur](../master-data-services/media/mds-statusicon-error.png "Icône d’état d’erreur")  
  
 Si l’état présente la valeur OK, l’image suivante apparaît.  
  
 ![Icône d’état OK](../master-data-services/media/mds-statusicon-ok.png "Icône d’état OK")  
  
## <a name="see-also"></a> Voir aussi  
 [Hiérarchies explicites &#40;Master Data Services&#41;](../master-data-services/explicit-hierarchies-master-data-services.md)   
 [Supprimer une entité &#40;Master Data Services&#41;](../master-data-services/delete-an-entity-master-data-services.md)   
 [Entités &#40;Master Data Services&#41;](../master-data-services/entities-master-data-services.md)  
  
  
