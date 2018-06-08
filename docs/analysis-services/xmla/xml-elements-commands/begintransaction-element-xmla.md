---
title: Élément BeginTransaction (XMLA) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: eb4a4dea3658ad03fd6205f9076bd1cb65ca9ba7
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34574321"
---
# <a name="begintransaction-element-xmla"></a>Élément BeginTransaction (XMLA)
[!INCLUDE[ssas-appliesto-sqlas-aas](../../../includes/ssas-appliesto-sqlas-aas.md)]
  Commence une transaction sur la session active avec une instance d’Analysis Services.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<Command>  
   <BeginTransaction />  
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
|Éléments enfants|None|  
  
## <a name="remarks"></a>Notes  
 La commande **BeginTransaction** démarre une transaction active dans la session active. Si une transaction active existe déjà, l'instance [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] incrémente le nombre de référence de transactions pour la session active. Dans le cas inverse, l'instance entame une nouvelle transaction et définit le décompte de références de la session active à 1. Si une transaction active est définie de manière explicite par le biais de la commande **BeginTransaction** , toutes les commandes suivantes sont exécutées à l'intérieur de la transaction explicitement définie.  
  
 Lorsque la session active arrive à son terme et que le nombre de référence de transactions est supérieur à zéro, toutes les transactions actives sont restaurées.  
  
 Si aucune transaction active n'est explicitement définie sur la session active, toutes les commandes émises sur cette session sont exécutées à l'intérieur d'une transaction implicitement définie. La transaction implicite est validée si la commande réussit ou est restaurée en cas d'échec de la commande.  
  
## <a name="see-also"></a>Voir aussi
 [Élément Cancel &#40;XMLA&#41;](../../../analysis-services/xmla/xml-elements-commands/cancel-element-xmla.md)   
 [Élément CommitTransaction &#40;XMLA&#41;](../../../analysis-services/xmla/xml-elements-commands/committransaction-element-xmla.md)   
 [Élément RollbackTransaction &#40;XMLA&#41;](../../../analysis-services/xmla/xml-elements-commands/rollbacktransaction-element-xmla.md)   
 [Commandes &#40;XMLA&#41;](../../../analysis-services/xmla/xml-elements-commands/xml-elements-commands.md)  
  
  
