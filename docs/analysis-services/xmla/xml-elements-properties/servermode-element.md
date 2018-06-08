---
title: Élément ServerMode | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 21e9344ef945311b3af07398e6e927482718f5ff
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34576261"
---
# <a name="servermode-element"></a>Élément ServerMode
[!INCLUDE[ssas-appliesto-sqlas-aas](../../../includes/ssas-appliesto-sqlas-aas.md)]
  Le **ServerMode** élément serveur Spécifie le mode de fonctionne dans le serveur.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<Server>  
...  
   <ddl300:ServerMode>...</ddl300:ServerMode>  
...  
</Server>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l’élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Type de données et longueur|Chaîne (énumération)|  
|Valeur par défaut|(aucun)|  
|Cardinalité|0-1 : élément facultatif qui ne peut apparaître qu'une seule fois.|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Élément|  
|------------------|-------------|  
|Éléments parents|[Server](../../../analysis-services/scripting/objects/server-element-assl.md)|  
|Éléments enfants|None|  
  
## <a name="remarks"></a>Notes  
 Le serveur s'exécute dans l'un des modes suivants :  
  
|Valeur|Description|  
|-----------|-----------------|  
|*Multidimensionnel*|Mode multidimensionnel et d'exploration de données|  
|*Sous forme de tableau*|Mode tabulaire|  
|*SharePoint*|Mode SharePoint|  
  
## <a name="see-also"></a>Voir aussi
 [Server](../../../analysis-services/scripting/objects/server-element-assl.md)  
  
  
