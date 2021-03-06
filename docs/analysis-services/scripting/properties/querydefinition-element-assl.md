---
title: "Élément QueryDefinition (ASSL) | Documents Microsoft"
ms.custom: 
ms.date: 03/06/2017
ms.prod: analysis-services
ms.prod_service: analysis-services
ms.service: 
ms.component: 
ms.reviewer: 
ms.suite: pro-bi
ms.technology: 
ms.tgt_pltfrm: 
ms.topic: reference
apiname: QueryDefinition Element
apilocation: http://schemas.microsoft.com/analysisservices/2003/engine
apitype: Schema
applies_to: SQL Server 2016 Preview
f1_keywords: QueryDefinition
helpviewer_keywords: QueryDefinition element
ms.assetid: 25bf0e93-d5c5-41df-b310-a253a4ace80e
caps.latest.revision: "34"
author: Minewiskan
ms.author: owend
manager: kfile
ms.workload: Inactive
ms.openlocfilehash: e829e3013b078ac8f267a3cf493d9a54ddf712c4
ms.sourcegitcommit: f486d12078a45c87b0fcf52270b904ca7b0c7fc8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/08/2018
---
# <a name="querydefinition-element-assl"></a>Élément QueryDefinition (ASSL)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]Contient une expression opaque pour une requête associée à un [DataSource](../../../analysis-services/scripting/objects/datasource-element-assl.md) élément dans une [QueryBinding](../../../analysis-services/scripting/data-type/querybinding-data-type-assl.md) élément.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<QueryBinding>  
   ...  
   <QueryDefinition>...</QueryDefinition>  
   ...  
</QueryBinding>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l'élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Type de données et longueur|String|  
|Valeur par défaut|None|  
|Cardinalité|1-1 : élément requis qui apparaît une fois et une seule.|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Élément|  
|------------------|-------------|  
|Élément parent|[QueryBinding](../../../analysis-services/scripting/data-type/querybinding-data-type-assl.md)|  
|Éléments enfants|None|  
  
## <a name="remarks"></a>Notes   
 L’élément qui correspond au parent de **QueryDefinition** dans l’objet d’objets AMO (Analysis Management) est modèle <xref:Microsoft.AnalysisServices.QueryBinding>.  
  
## <a name="see-also"></a>Voir aussi  
 [Propriétés &#40; ASSL &#41;](../../../analysis-services/scripting/properties/properties-assl.md)  
  
  
