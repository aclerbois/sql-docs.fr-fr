---
title: Créer des requêtes d’extraction à l’aide de DMX | Documents Microsoft
ms.date: 05/01/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 6616f90f475da91f3f5c38c0f5922d16d72a5408
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="create-drillthrough-queries-using-dmx"></a>Créer des requêtes d'extraction à l'aide de DMX
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Pour tous les modèles qui prennent en charge l'extraction, vous pouvez récupérer les données de cas et les données de structure en créant une requête DMX dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou dans tout autre client qui prend en charge DMX.  
  
> [!WARNING]  
>  Pour afficher les données, l'extraction doit avoir été activée, et vous devez disposer des autorisations nécessaires.  
  
## <a name="specifying-drillthrough-options"></a>Spécification des options d'extraction  
 La syntaxe générale pour extraire les cas de modèles et les cas de structure est la suivante :  
  
```  
SELECT <model column list>, StructureColumn('<structure column name') FROM <modelname>.CASES  
```  
  
 Pour plus d’informations sur l’utilisation de requêtes DMX pour retourner des données de cas, consultez [SELECT FROM &#60;modèle&#62;.CASES &#40;DMX&#41;](../../dmx/select-from-model-cases-dmx.md) et [SELECT FROM &#60;structure&#62;.CASES](../../dmx/select-from-structure-cases.md).  
  
## <a name="examples"></a>Exemples  
 La requête DMX suivante retourne les données de cas pour une série de produit spécifique, à partir d'un modèle de série chronologique. La requête retourne également la colonne **Amount**, qui n'a pas été utilisée dans le modèle, mais qui est disponible dans la structure d'exploration de données.  
  
```  
SELECT [DateSeries], [Model Region], Quantity, StructureColumn('Amount') AS [M200 Pacific Amount]  
FROM Forecasting.CASES  
WHERE [Model Region] = 'M200 Pacific'  
```  
  
 Notez que, dans cet exemple, un alias a été utilisé pour renommer la colonne de structure. Si vous n'assignez pas d'alias à la colonne de structure, la colonne est retournée avec le nom 'Expression'. Il s'agit du comportement par défaut pour toutes les colonnes sans nom.  
  
## <a name="see-also"></a>Voir aussi  
 [Requêtes d’extraction & #40 ; exploration de données & #41 ;](../../analysis-services/data-mining/drillthrough-queries-data-mining.md)   
 [Extraction sur les Structures d’exploration de données](../../analysis-services/data-mining/drillthrough-on-mining-structures.md)  
  
  
