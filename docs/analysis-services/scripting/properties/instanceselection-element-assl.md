---
title: Élément InstanceSelection (ASSL) | Documents Microsoft
ms.date: 5/8/2018
ms.prod: sql
ms.custom: assl
ms.reviewer: owend
ms.technology: analysis-services
ms.topic: reference
author: minewiskan
ms.author: owend
manager: kfile
ms.openlocfilehash: 1f00127f58bb1f8c5a6792bec3ddcb8420f0d24b
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="instanceselection-element-assl"></a>Élément InstanceSelection (ASSL)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Fournit un indicateur aux applications clientes qui suggère le mode d'affichage d'une liste d'éléments d'après le nombre d'éléments attendus dans la liste.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<DimensionAttribute>  
   ...  
   <InstanceSelection>...</InstanceSelection>  
   ...  
</DimensionAttribute>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l'élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Type de données et longueur|Chaîne (énumération)|  
|Valeur par défaut|*Aucun*|  
|Cardinalité|0-1: élément facultatif qui peut apparaître une fois et une seule.|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Élément|  
|------------------|-------------|  
|Élément parent|[DimensionAttribute](../../../analysis-services/scripting/data-type/dimensionattribute-data-type-assl.md)|  
|Éléments enfants|Aucune|  
  
## <a name="remarks"></a>Notes  
 La valeur de cet élément est limitée à l'une des chaînes suivantes :  
  
|Valeur|Description|  
|-----------|-----------------|  
|*Aucun*|Ne pas afficher de liste de sélection. Autoriser les utilisateurs à entrer des valeurs directement.|  
|*Liste déroulante*|Le nombre d'éléments est suffisamment faible pour permettre l'affichage dans une liste déroulante.|  
|*Listee*|Le nombre d'éléments est trop important pour une liste déroulante mais ne nécessite pas de filtrage.|  
|*FilteredList*|le nombre d'éléments est suffisamment important pour demander aux utilisateurs de filtrer les éléments à afficher.|  
|*MandatoryFilter*|Le nombre d'éléments est tellement important que l'affichage doit en permanence être filtré.|  
  
 L’énumération qui correspond aux valeurs autorisées pour **InstanceSelection** dans l’objet d’objets AMO (Analysis Management) est modèle <xref:Microsoft.AnalysisServices.InstanceSelection>.  
  
## <a name="see-also"></a>Voir aussi  
 [Propriétés & #40 ; ASSL & #41 ;](../../../analysis-services/scripting/properties/properties-assl.md)  
  
  
