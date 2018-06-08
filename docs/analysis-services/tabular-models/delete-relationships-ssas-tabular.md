---
title: Supprimer des relations | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 115d72bd0b833d1f392b2349d3ed4e4970f58453
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="delete-relationships"></a>Supprimer des relations 
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Vous pouvez supprimer des relations existantes à l'aide du générateur de modèles dans la vue de diagramme ou à l'aide de la boîte de dialogue Gérer les relations. Pour plus d’informations sur l’utilisation des relations dans les modèles tabulaires, consultez [relations](../../analysis-services/tabular-models/relationships-ssas-tabular.md).  
  
## <a name="considerations-for-deleting-relationships"></a>Considérations relatives à la suppression de relations  
 Gardez à l'esprit les points suivants lorsque vous décidez de supprimer ou non une relation :  
  
-   Il n'existe aucune méthode permettant d'annuler la suppression d'une relation. Vous pouvez recréer la relation, mais cette opération requiert le recalcul complet des formules dans le modèle. Par conséquent, effectuez toujours des vérifications avant de supprimer une relation utilisée dans des formules.  
  
-   La suppression d'une relation entre deux tables peut provoquer des erreurs dans les formules qui référencent ces tables.  
  
-   La fonction DAX (Data Analysis Expression) RELATED utilise les relations entre des tables pour rechercher des valeurs associées dans une autre table. Elle génèrera des résultats différents après la suppression de la relation. Pour plus d'informations, consultez la fonction RELATED (DAX).  
  
-   Outre la modification des résultats des tableaux croisés dynamiques et des formules, la création et la suppression de relations entraînent le recalcul du classeur, ce qui peut prendre du temps.  
  
## <a name="delete-relationships"></a>Supprimer des relations  
  
#### <a name="to-delete-a-relationship-by-using-diagram-view"></a>Pour supprimer une relation à l'aide de la vue de diagramme  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], cliquez sur le menu **Modèle** , puis pointez sur **Vue du modèle**, et enfin sur **Vue de diagramme**.  
  
2.  Cliquez avec le bouton droit sur une ligne de relation entre deux tables, puis cliquez sur **Supprimer**.  
  
#### <a name="to-delete-a-relationship-by-using-the-manage-relationships-dialog-box"></a>Pour supprimer une relation à l'aide de la boîte de dialogue Gérer les relations.  
  
1.  Dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], cliquez sur le menu **Table** , puis sur **Gérer les relations**.  
  
2.  Dans la boîte de dialogue **Gérer les relations** , sélectionnez une ou plusieurs relations dans la liste.  
  
     Pour sélectionner plusieurs relations, maintenez la touche Ctrl enfoncée pendant que vous cliquez sur chaque relation.  
  
3.  Cliquez sur **Supprimer la relation**.  
  
4.  Dans la boîte de dialogue **Gérer les relations** , cliquez sur **Fermer**.  
  
## <a name="see-also"></a>Voir aussi  
 [Relations](../../analysis-services/tabular-models/relationships-ssas-tabular.md)   
 [Créer une relation](../../analysis-services/tabular-models/create-a-relationship-between-two-tables-ssas-tabular.md)  
  
  
