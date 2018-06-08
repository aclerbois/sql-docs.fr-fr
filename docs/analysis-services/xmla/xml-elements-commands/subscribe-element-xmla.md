---
title: Élément Subscribe (XMLA) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: f784cbe3c33eb6b2587b7b535668e793fac3b138
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34574401"
---
# <a name="subscribe-element-xmla"></a>Élément Subscribe (XMLA)
[!INCLUDE[ssas-appliesto-sqlas-aas](../../../includes/ssas-appliesto-sqlas-aas.md)]
  S’abonne à une trace et retourne un ensemble de lignes qui contient les événements de trace à partir d’une instance d’Analysis Services.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<Command>  
   <Subscribe>  
      <Object>...</Object>  
   </Subscribe>  
</Command>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l’élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Type de données et longueur|None|  
|Valeur par défaut|None|  
|Cardinalité|0-n : élément facultatif pouvant apparaître plusieurs fois.|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Élément|  
|------------------|-------------|  
|Éléments parents|[Commandee](../../../analysis-services/xmla/xml-elements-properties/command-element-xmla.md)|  
|Éléments enfants|[Objet](../../../analysis-services/xmla/xml-elements-properties/object-element-xmla.md)|  
  
## <a name="remarks"></a>Notes  
 Le **s’abonner** commande s’abonne à et flux de sauvegarder un ensemble de lignes à partir d’une trace spécifiée sur un [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] instance. Si un objet autre qu'une trace est spécifié dans l'élément **Object** , une erreur survient.  
  
 Si le **objet** élément n’est pas spécifié, une trace de session est définie et l’abonné sur le [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] instance. La trace de session retourne un ensemble fixe d'événements de trace à partir de la session en cours.  
  
 Le flux de l’ensemble de lignes retourné par cette commande est terminé si l’application cliente ferme la connexion à la [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] instance, ou si la session sur laquelle le **s’abonner** commande est exécutée est terminée.  
  
## <a name="see-also"></a>Voir aussi
 [Commandes &#40;XMLA&#41;](../../../analysis-services/xmla/xml-elements-commands/xml-elements-commands.md)  
  
  
