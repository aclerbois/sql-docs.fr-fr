---
title: Modifiez la valeur de délai d’attente pour les requêtes d’exploration de données | Documents Microsoft
ms.date: 05/01/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: c67744ea3862185b59d1a0af3e1bb144eba57e23
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="change-the-time-out-value-for-data-mining-queries"></a>Modifier la valeur du délai d'attente pour les requêtes d'exploration de données
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Lorsque vous générez un graphique de courbes d'élévation ou que vous exécutez une requête de prédiction, quelquefois, la génération de toutes les données requises pour la prédiction peut prendre du temps. Pour empêcher l'expiration du délai pour la requête, vous pouvez modifier la valeur qui contrôle le délai d'attente du serveur [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] pour achever une requête.  
  
 La valeur par défaut est 15 ; toutefois, si vos modèles sont complexes ou que la source de données est importante, cela risque de ne pas suffire. Le cas échéant, vous pouvez augmenter cette valeur de manière significative, afin de consacrer assez de temps pour le traitement. Si, par exemple, vous définissez **Délai de requête** avec la valeur 600, l'exécution de la requête peut continuer jusqu'à 10 minutes.  
  
 Pour plus d’informations sur les requêtes de prédiction, consultez [Tâches de requête d’exploration de données et procédures](../../analysis-services/data-mining/data-mining-query-tasks-and-how-tos.md).  
  
### <a name="configure-the-time-out-value-for-data-mining-queries"></a>Configurer la valeur du délai d'attente pour les requêtes d'exploration de données  
  
1.  Dans le menu [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]Outils **de** , sélectionnez **Options**.  
  
2.  Dans le volet **Options** , développez **Concepteurs Business Intelligence**.  
  
3.  Cliquez sur la zone de texte **Délai de requête** et tapez une valeur pour le nombre de secondes.  
  
## <a name="see-also"></a>Voir aussi  
 [Tâches de requête d’exploration de données et de procédures](../../analysis-services/data-mining/data-mining-query-tasks-and-how-tos.md)   
 [Requêtes d’exploration de données](../../analysis-services/data-mining/data-mining-queries.md)  
  
  
