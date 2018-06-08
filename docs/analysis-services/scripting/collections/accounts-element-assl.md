---
title: Comptes d’élément (ASSL) | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: assl
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: bf3d3b56e9cc2b299ddcdec1c0a506e0168f8741
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="accounts-element-assl"></a>Élément Accounts (ASSL)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Contient la collection des types de compte sont définies dans un [base de données](../../../analysis-services/scripting/objects/database-element-assl.md) élément.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<Database>  
   ...  
   <Accounts>  
      <Account>...</Account>  
   </Accounts>  
   ...  
</Database>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l'élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Type de données et longueur|Aucun (collection)|  
|Valeur par défaut|Aucun (collection)|  
|Cardinalité|0-1: élément facultatif qui peut apparaître une fois et une seule.|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Élément|  
|------------------|-------------|  
|Éléments parents|[Base de données](../../../analysis-services/scripting/objects/database-element-assl.md)|  
|Éléments enfants|[Compte](../../../analysis-services/scripting/objects/account-element-assl.md)|  
  
## <a name="remarks"></a>Notes  
 Dimensions, dont [Type](../../../analysis-services/scripting/properties/type-element-dimension-assl.md) a la valeur *comptes*, peuvent avoir un attribut qui spécifie le type de compte, par exemple recettes, dépenses, etc., représenté par les membres de la dimension. Le type de compte est ensuite utilisé par [mesure](../../../analysis-services/scripting/objects/measure-element-assl.md) éléments, dont [AggregationFunction](../../../analysis-services/scripting/properties/aggregatefunction-element-assl.md) a la valeur *ByAccount*, afin de déterminer la fonction d’agrégation à utiliser pour regrouper les membres de cette dimension. L'élément **Accounts** contient une collection d'éléments **Account** , qui représentent des types de compte et la fonction d'agrégation à utiliser pour chacun d'eux.  
  
 Un type de compte doit être répertorié si la fonction d’agrégation est différente de la valeur par défaut utilisée par [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] pour chaque type de compte.  
  
 L'ensemble des types de compte valides est fixe.  
  
 L’élément correspondant dans le modèle d’objet objets AMO (Analysis Management) est <xref:Microsoft.AnalysisServices.AccountCollection>.  
  
## <a name="see-also"></a>Voir aussi  
 [Élément AccountType &#40;ASSL&#41;](../../../analysis-services/scripting/properties/accounttype-element-assl.md)   
 [Collections de & #40 ; ASSL & #41 ;](../../../analysis-services/scripting/collections/collections-assl.md)  
  
  
