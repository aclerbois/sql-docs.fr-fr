---
title: Modifier les propriétés d’une Structure d’exploration de données | Documents Microsoft
ms.date: 05/01/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 8064fcc088c7ae9e7dd96c5c132b1246fe0ae2db
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="change-the-properties-of-a-mining-structure"></a>Modifier les propriétés d'une structure d'exploration de données
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Il existe deux types de propriétés sur une structure d'exploration de données, qui peuvent être modifiées :  
  
-   Propriétés de la structure d'exploration de données qui affectent la structure entière  
  
-   Propriétés sur des colonnes individuelles de la structure  
  
 Notez que certaines propriétés dépendent d'autres paramètres de propriété. Par exemple, vous ne pouvez pas définir les propriétés qui contrôlent le comportement de placement dans un conteneur (notamment <xref:Microsoft.AnalysisServices.ScalarMiningStructureColumn.DiscretizationMethod%2A> ou <xref:Microsoft.AnalysisServices.ScalarMiningStructureColumn.DiscretizationBucketCount%2A>) jusqu’à ce que vous ayez défini le type de données de la colonne sur **Discretized**.  
  
 Pour plus d’informations sur les propriétés de structure d’exploration de données, consultez [Structure d’exploration de données](../../analysis-services/data-mining/mining-structure-columns.md).  
  
### <a name="to-change-the-properties-of-a-mining-structure"></a>Pour modifier les propriétés d'une structure d'exploration de données  
  
1.  Sous l’onglet **Structure d’exploration de données** du Concepteur d’exploration de données, cliquez avec le bouton droit de la souris sur la structure d’exploration de données ou sur une colonne de la structure d’exploration de données, puis sélectionnez **Propriétés**.  
  
     La fenêtre **Propriétés** s’ouvre dans la partie droite de l’écran si elle n’était pas affichée.  
  
2.  Dans la fenêtre **Propriétés** , sélectionnez la valeur qui correspond à la propriété à modifier, puis entrez la nouvelle valeur.  
  
     La nouvelle valeur est appliquée lorsque vous sélectionnez un élément différent dans le concepteur.  
  
## <a name="see-also"></a>Voir aussi  
 [Tâches de la Structure d’exploration de données et procédures](../../analysis-services/data-mining/mining-structure-tasks-and-how-tos.md)  
  
  
