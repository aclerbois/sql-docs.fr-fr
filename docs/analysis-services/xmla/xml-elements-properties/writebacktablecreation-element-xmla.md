---
title: Élément WritebackTableCreation (XMLA) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: e04f44d9caa3ae51ea141997e83de54524000fee
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34576851"
---
# <a name="writebacktablecreation-element-xmla"></a>Élément WritebackTableCreation (XMLA)
[!INCLUDE[ssas-appliesto-sqlas-aas](../../../includes/ssas-appliesto-sqlas-aas.md)]
  Détermine si une table d’écriture différée est créée au cours de la [processus](../../../analysis-services/xmla/xml-elements-commands/process-element-xmla.md) opération.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<Process>  
...  
   <WritebackTableCreation>...</WritebackTableCreation>  
...  
</Process>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l’élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Type de données et longueur|Chaîne (énumération)|  
|Valeur par défaut|None|  
|Cardinalité|0-1 : élément facultatif qui peut apparaître une fois et une seule.|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Élément|  
|------------------|-------------|  
|Éléments parents|[Traiter](../../../analysis-services/xmla/xml-elements-commands/process-element-xmla.md)|  
|Éléments enfants|None|  
  
## <a name="remarks"></a>Notes  
 Pour plus d’informations sur les options de traitement disponibles pour les objets sur une instance d’Analysis Services, consultez [du traitement d’un modèle multidimensionnel &#40;Analysis Services&#41;](../../../analysis-services/multidimensional-models/processing-a-multidimensional-model-analysis-services.md).  
  
 La valeur de l'élément **WritebackTableCreation** est limitée à l'une des chaînes répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|*Créer*|Création d'une nouvelle table d'écriture différée si aucune n'existe. Si une table d'écriture différée existe déjà, une erreur se produit.|  
|*CreateAlways*|Création d'une nouvelle table d'écriture différée et remplacement de toutes les tables d'écriture différée existantes.|  
|*UseExisting*|Utilisation de la table d'écriture différée existante s'il en existe déjà une. Si aucune n'existe, une erreur se produit.|  
  
## <a name="see-also"></a>Voir aussi
 [Propriétés &#40;XMLA&#41;](../../../analysis-services/xmla/xml-elements-properties/xml-elements-properties.md)  
  
  
