---
title: Élément Security (XMLA) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: c4ea0e1f9bfab567c792bdd7b233bea038e87f54
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34576242"
---
# <a name="security-element-xmla"></a>Élément Security (XMLA)
[!INCLUDE[ssas-appliesto-sqlas-aas](../../../includes/ssas-appliesto-sqlas-aas.md)]
  Spécifie la manière de sauvegarder ou restaurer des définitions de sécurité, telles que les rôles et les autorisations, pendant un [sauvegarde](../../../analysis-services/xmla/xml-elements-commands/backup-element-xmla.md) ou [restaurer](../../../analysis-services/xmla/xml-elements-commands/restore-element-xmla.md) commande.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<Backup> <!-- or Restore -->  
   ...  
   <Security>...</Security>  
   ...  
</Backup>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l’élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Type de données et longueur|Chaîne (énumération)|  
|Valeur par défaut|*skipMembership*|  
|Cardinalité|0-1 : élément facultatif qui peut apparaître une fois et une seule.|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Élément|  
|------------------|-------------|  
|Éléments parents|[Sauvegarde](../../../analysis-services/xmla/xml-elements-commands/backup-element-xmla.md), [restaurer](../../../analysis-services/xmla/xml-elements-commands/restore-element-xmla.md)|  
|Éléments enfants|None|  
  
## <a name="remarks"></a>Notes  
 Le **sécurité** élément détermine si les définitions de sécurité, notamment les rôles et autorisations, définies sur une base de données Analysis Services sont sauvegardés ou restaurés lors de la, respectivement, un **sauvegarde** ou **Restaurer** commande. Cet élément détermine également si les comptes d'utilisateur et les groupes Windows définis comme membres des définitions de sécurité sont inclus dans le cadre de la commande **Backup** ou **Restore** .  
  
 La valeur de cet élément est limitée à l'une des chaînes répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|*skipMembership*|Inclut les définitions de sécurité, mais exclut les informations d'appartenance durant les commandes **Backup** ou **Restore** .|  
|*CopyAll*|Inclut les définitions de sécurité et les informations d'appartenance durant les commandes **Backup** ou **Restore** .|  
|*IgnoreSecurity*|Exclut les définitions de sécurité durant les commandes **Backup** ou **Restore** .|  
  
## <a name="see-also"></a>Voir aussi
 [Élément SynchronizeSecurity &#40;XMLA&#41;](../../../analysis-services/xmla/xml-elements-properties/synchronizesecurity-element-xmla.md)   
 [Propriétés &#40;XMLA&#41;](../../../analysis-services/xmla/xml-elements-properties/xml-elements-properties.md)  
  
  
