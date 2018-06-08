---
title: Élément isolation (ASSL) | Documents Microsoft
ms.date: 5/8/2018
ms.prod: sql
ms.custom: assl
ms.reviewer: owend
ms.technology: analysis-services
ms.topic: reference
author: minewiskan
ms.author: owend
manager: kfile
ms.openlocfilehash: b8233ce2fc5d987dac6511fee978e45b213601a2
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="isolation-element-assl"></a>Élément Isolation (ASSL)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Indique le niveau d’isolation d’un élément qui est dérivé de la [source de données](../../../analysis-services/scripting/data-type/datasource-data-type-assl.md) type de données.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<DataSource>  
   ...  
   <Isolation>...</Isolation>  
   ...  
</DataSource>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l'élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Type de données et longueur|Chaîne (énumération)|  
|Valeur par défaut|*Lecture validée*|  
|Cardinalité|0-1: élément facultatif qui peut apparaître une fois et une seule.|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Élément|  
|------------------|-------------|  
|Élément parent|[DataSource](../../../analysis-services/scripting/data-type/datasource-data-type-assl.md)|  
|Éléments enfants|Aucune|  
  
## <a name="remarks"></a>Notes  
 La valeur de cet élément est limitée à l'une des chaînes du tableau suivant.  
  
|Valeur| Description|  
|-----------|-----------------|  
|*Lecture validée*|Spécifie que les instructions ne peuvent pas lire les données modifiées mais non validées par d'autres transactions. Cela permet d'éviter les lectures incorrectes. D'autres transactions peuvent modifier des données entre des instructions individuelles dans la transaction actuelle. Il en résulte des lectures non reproductibles ou des données fantômes. Il s'agit de la valeur par défaut de l'élément **Isolation** .|  
|*Snapshot*|Spécifie que les données lues par n'importe quelle instruction d'une transaction représenteront la version cohérente d'un point de vue transactionnel des données qui existaient au début de la transaction. La transaction ne peut détecter que les modifications de données qui ont été validées avant qu'elle ne commence. Les modifications de données effectuées par d'autres transactions après le démarrage de la transaction actuelle ne sont pas visibles pour les instructions qui s'exécutent dans la transaction actuelle. Tout se passe comme si les instructions d'une transaction obtenaient un instantané des données validées telles qu'elles existaient au début de cette transaction.|  
  
## <a name="see-also"></a>Voir aussi  
 [Propriétés & #40 ; ASSL & #41 ;](../../../analysis-services/scripting/properties/properties-assl.md)  
  
  
