---
title: Approbation requise (Master Data Services) | Microsoft Docs
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
ms.assetid: b475a53d-269d-49f3-bb42-965c555f80be
caps.latest.revision: 7
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: 45b968dec37c80c2a0bc435edbe60b0226eac8d7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="approval-required-master-data-services"></a>Approbation requise (Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], l’administrateur peut définir une entité sur Approbation requise. Toutes les modifications apportées à cette entité nécessitent que l’un des administrateurs d’entité les vérifie et les approuve.  
  
> [!NOTE]  
>  Les modifications effectuées sur les membres feuille exigent une approbation. Les modifications apportées aux hiérarchies et aux collections explicitement déconseillées contournent l’approbation.  
>   
>  Les modifications apportées au travers du processus de table de mise en lots contournent l’approbation.  
>   
>  Les modifications apportées par une règle d’entreprise contournent l’approbation.  
  
## <a name="prerequisites"></a>Conditions préalables requises  
 Pour effectuer cette procédure :  
  
-   Vous devez avoir l’autorisation d’accéder à la zone fonctionnelle Administration de système  
  
-   Vous devez être administrateur de modèle. Pour plus d’informations, consultez [Administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
-   Une entité doit exister. Pour plus d’informations, consultez [Créer une entité &#40;Master Data Services&#41;](../master-data-services/create-an-entity-master-data-services.md).  
  
## <a name="to-enable-approval-required-for-an-entity"></a>Pour activer l’option Approbation requise pour une entité  
  
1.  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], cliquez sur **Administration de système**.  
  
2.  Dans la grille de la page **Gérer le modèle** , sélectionnez un modèle et cliquez sur **Entités**.  
  
3.  Dans la grille de la page **Gérer l’entité** , sélectionnez la ligne de l’entité pour laquelle vous souhaitez activer  **Approbation requise** .  
  
4.  Cliquez sur **Modifier**, sélectionnez **Approbation requise**, puis cliquez sur **Enregistrer**.  
  
## <a name="see-also"></a> Voir aussi  
 [Ensembles de modifications &#40;Master Data Services&#41;](../master-data-services/changesets-master-data-services.md)  
  
  
