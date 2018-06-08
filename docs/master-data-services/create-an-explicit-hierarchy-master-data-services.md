---
title: Créer une hiérarchie explicite (Master Data Services) | Microsoft Docs
ms.custom: ''
ms.date: 04/01/2016
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
- creating explicit hierarchies [Master Data Services]
- explicit hierarchies, creating
ms.assetid: ba768393-6990-4eda-8cb0-d58cb3cfc2e2
caps.latest.revision: 10
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: d59004304b46944e2865391cd2da8983e15aca13
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-an-explicit-hierarchy-master-data-services"></a>Créer une hiérarchie explicite (Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], créez une hiérarchie explicite lorsque vous avez besoin d'une hiérarchie déséquilibrée dans laquelle les membres peuvent exister à tous les niveaux. Les hiérarchies explicites contiennent des membres d'une entité unique.  
  
 Après avoir créé une hiérarchie explicite, vous pouvez lui ajouter des membres dans la zone fonctionnelle **Explorateur** .  
  
## <a name="prerequisites"></a>Conditions préalables requises  
 Pour effectuer cette procédure :  
  
-   Vous devez avoir l'autorisation d'accéder à la zone fonctionnelle **Administration de système** .  
  
-   Vous devez être administrateur de modèle. Pour plus d’informations, consultez [Administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
-   L'entité doit être activée pour les hiérarchies et collections explicites.  
  
### <a name="to-create-an-explicit-hierarchy"></a>Pour créer une hiérarchie explicite  
  
1.  Dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], cliquez sur **Administration de système**.  
  
2.  Dans la page **Gérer le modèle** , sélectionnez un modèle dans la grille et cliquez sur **Entités**.  
  
3.  Dans la grille de la page **Gérer l’entité** , sélectionnez la ligne de l’entité pour laquelle vous souhaitez créer une hiérarchie explicite.  
  
4.  Cliquez sur **Hiérarchies explicites**.  
  
5.  Dans la page **Gérer la hiérarchie explicite** , cliquez sur **Ajouter**.  
  
6.  Dans la zone **Nom** , tapez un nom pour la hiérarchie.  
  
7.  Décochez éventuellement la case **Hiérarchie obligatoire** pour créer la hiérarchie comme une hiérarchie non obligatoire. Pour plus d’informations sur les types de hiérarchies, consultez [Hiérarchies explicites &#40;Master Data Services&#41;](../master-data-services/explicit-hierarchies-master-data-services.md).  
  
8.  Cliquez sur **Enregistrer**.  
  
## <a name="grid-columns"></a>Colonnes de la grille  
 Pour chaque hiérarchie explicite que vous créez, une ligne comportant sept colonnes est ajoutée à la grille. Ces différentes colonnes sont décrites ci-après.  
  
|Nom   |Description|  
|----------|-----------------|  
|État|État de l’entité. Lorsque vous cliquez sur **Enregistrer** , l’image ci-après s’affiche pour indiquer que l’entité est en cours de mise à jour.<br /><br /> ![Icône d’état de mise à jour](../master-data-services/media/mds-statusicon-updating.png "Icône d’état de mise à jour")<br /><br /> En cas d’erreur lors de la création ou de la modification d’une entité, l’image suivante apparaît.<br /><br /> ![Icône d’état d’erreur](../master-data-services/media/mds-statusicon-error.png "Icône d’état d’erreur")<br /><br /> Si l’état présente la valeur OK, l’image ci-dessous s’affiche.<br /><br /> ![Icône d’état OK](../master-data-services/media/mds-statusicon-ok.png "Icône d’état OK")|  
|Nom   |Nom de hiérarchie explicite.|  
|Obligatoire|Indique si la hiérarchie explicite est ou non obligatoire.|  
|Date de création|Nom de l’utilisateur ayant créé la hiérarchie explicite.|  
|Créée le|Date et heure de création de la hiérarchie explicite.|  
|Mise à jour par|Nom de l’utilisateur ayant effectué la dernière mise à jour de la hiérarchie explicite.|  
|Mise à jour le|Date et heure de la dernière mise à jour de la hiérarchie explicite.|  
  
## <a name="next-steps"></a>Next Steps  
  
-   [Créer un membre consolidé &#40;Master Data Services&#41;](../master-data-services/create-a-consolidated-member-master-data-services.md)  
  
  
  
## <a name="see-also"></a> Voir aussi  
 [Hiérarchies explicites &#40;Master Data Services&#41;](../master-data-services/explicit-hierarchies-master-data-services.md)   
 [Hiérarchies dérivées avec des niveaux supérieurs d’une hiérarchie dérivée&#40;Master Data Services&#41;](../master-data-services/derived-hierarchies-with-explicit-caps-master-data-services.md)   
 [Modifier le nom d’une hiérarchie explicite &#40;Master Data Services&#41;](../master-data-services/change-an-explicit-hierarchy-name-master-data-services.md)  
  
  

