---
title: Élément mode (XMLA) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 12e792c51b2f6feb3edb4e01c12dcf4809e2d4d4
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34575751"
---
# <a name="mode-element-xmla"></a>Élément Mode (XMLA)
[!INCLUDE[ssas-appliesto-sqlas-aas](../../../includes/ssas-appliesto-sqlas-aas.md)]
  Identifie le mode à utiliser par le parent [verrou](../../../analysis-services/xmla/xml-elements-commands/lock-element-xmla.md) élément lors de la création d’un verrou sur un objet spécifié.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<Lock>  
   ...  
   <Mode>...</Mode>  
   ...  
</Lock>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l’élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Type de données et longueur|Chaîne (énumération)|  
|Valeur par défaut|None|  
|Cardinalité|1-1 : élément requis qui apparaît une fois et une seule.|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Élément|  
|------------------|-------------|  
|Éléments parents|[Verrou](../../../analysis-services/xmla/xml-elements-commands/lock-element-xmla.md), [déverrouiller](../../../analysis-services/xmla/xml-elements-commands/unlock-element-xmla.md)|  
|Éléments enfants|None|  
  
## <a name="remarks"></a>Notes  
 Le parent **verrou** élément utilise le **Mode** élément afin de déterminer le type de verrou à créer sur un objet. La valeur de cet élément est limitée à l'une des chaînes répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|*CommitShared*|Un verrou partagé est établi sur l'objet spécifié. D'autres verrous partagés peuvent être créés pour le même objet.<br /><br /> Un verrou partagé empêche les transactions contenant des opérations d’écriture, telles qu’une [Execute](../../../analysis-services/xmla/xml-elements-methods-execute.md) appel de méthode en cours d’exécution un [Alter](../../../analysis-services/xmla/xml-elements-commands/alter-element-xmla.md) commande sur un objet spécifié, à partir de la validation jusqu'à ce que le verrou partagé est supprimé. Un verrou partagé n’empêche pas les transactions contenant des opérations de lecture, comme un [Discover](../../../analysis-services/xmla/xml-elements-methods-discover.md) appel de méthode ou un **Execute** appel de méthode en cours d’exécution un [instruction](../../../analysis-services/xmla/xml-elements-commands/statement-element-xmla.md) commande à partir de la validation.|  
|*CommitExclusive*|Un verrou exclusif est établi sur l'objet spécifié. Aucun autre verrou partagé ou exclusif ne peut être créé pour le même objet.<br /><br /> Jusqu'à sa suppression, un verrou exclusif empêche la validation des transactions contenant des opérations de lecture ou d'écriture sur un objet spécifié.|  
  
## <a name="see-also"></a>Voir aussi
 [Élément ID &#40;XMLA&#41;](../../../analysis-services/xmla/xml-elements-properties/id-element-xmla.md)   
 [Élément objet &#40;XMLA&#41;](../../../analysis-services/xmla/xml-elements-properties/object-element-xmla.md)   
 [Propriétés &#40;XMLA&#41;](../../../analysis-services/xmla/xml-elements-properties/xml-elements-properties.md)  
  
  
