---
title: résultats Element (XMLA) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 7fc64d6b31f1b05d8bf5b4d1c80d75dff0583e86
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34576161"
---
# <a name="results-element-xmla"></a>Élément results (XMLA)
[!INCLUDE[ssas-appliesto-sqlas-aas](../../../includes/ssas-appliesto-sqlas-aas.md)]
  Contient une collection d'éléments [root](../../../analysis-services/xmla/xml-elements-properties/root-element-xmla.md) retournée par la méthode [Execute](../../../analysis-services/xmla/xml-elements-methods-execute.md) employée avec la commande [Batch](../../../analysis-services/xmla/xml-elements-commands/batch-element-xmla.md) .  
  
 **espace de noms** `http://schemas.microsoft.com/analysisservices/2003/xmla-multipleresults`  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<return>  
   <results>  
      <root>...</root>  
   </results>  
</return>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l’élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Type de données et longueur|None|  
|Valeur par défaut|None|  
|Cardinalité|0-1 : élément facultatif qui peut apparaître une fois et une seule.|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Élément|  
|------------------|-------------|  
|Éléments parents|[de retour](../../../analysis-services/xmla/xml-elements-properties/return-element-xmla.md)|  
|Éléments enfants|[racine](../../../analysis-services/xmla/xml-elements-properties/root-element-xmla.md)|  
  
## <a name="remarks"></a>Notes  
 Si une commande **Batch** est exécutée par la méthode **Execute** , l'élément **return** contient un élément **results** unique au lieu d'un élément **root** unique. Le contenu de l'élément **results** dépend des paramètres utilisés pour exécuter la commande **Batch** .  
  
 Pour les commandes **Batch** non transactionnelles, l'élément **results** contient un élément **root** pour chacune des commandes exécutées par la commande **Batch** , que ces commandes réussissent ou échouent. Pour les commandes **Batch** transactionnelles, l'élément **results** contient un seul élément **root** contenant les informations d'erreur de la commande qui a échoué dans la commande **Batch** .  
  
## <a name="see-also"></a>Voir aussi
 [Propriétés &#40;XMLA&#41;](../../../analysis-services/xmla/xml-elements-properties/xml-elements-properties.md)  
  
  
