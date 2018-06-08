---
title: Fichier de l’élément (XMLA) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: eb80a091c9b9585a6efc29088d15139db7033efb
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34575191"
---
# <a name="file-element-xmla"></a>Élément File (XMLA)
[!INCLUDE[ssas-appliesto-sqlas-aas](../../../includes/ssas-appliesto-sqlas-aas.md)]
  Identifie un fichier à utiliser par le parent [sauvegarde](../../../analysis-services/xmla/xml-elements-commands/backup-element-xmla.md) ou [restaurer](../../../analysis-services/xmla/xml-elements-commands/restore-element-xmla.md) commande, ou par le parent [emplacement](../../../analysis-services/xmla/xml-elements-properties/location-element-xmla.md) élément.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<Backup> <!-- or one of the elements listed below in the Element relationships table -->  
   ...  
   <File>...</File>  
   ...  
</Backup>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l’élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Type de données et longueur|String|  
|Valeur par défaut|None|  
|Cardinalité|1-1 : élément requis qui apparaît une fois et une seule.|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Élément|  
|------------------|-------------|  
|Éléments parents|[Sauvegarde](../../../analysis-services/xmla/xml-elements-commands/backup-element-xmla.md), [emplacement](../../../analysis-services/xmla/xml-elements-properties/location-element-xmla.md), [restaurer](../../../analysis-services/xmla/xml-elements-commands/restore-element-xmla.md)|  
|Éléments enfants|None|  
  
## <a name="remarks"></a>Notes  
 Le **fichier** élément contient un nom de fichier UNC et que l’élément parent détermine l’utilisation de la **fichier** élément.  
  
 Pour **sauvegarde** commandes, le **fichier** élément détermine le nom du fichier de sauvegarde créé par le **sauvegarde** commande. Si un chemin d’accès n’est pas spécifié en tant que partie du nom de fichier, le chemin d’accès spécifié dans le **BackupDir** propriété de configuration pour l’instance d’Analysis Services est utilisée. Si le fichier spécifié existe déjà, une erreur se produit, sauf si le **AllowOverwrite** élément du parent **sauvegarde** commande est définie sur **True**.  
  
 Pour **restaurer** commandes, le **fichier** élément détermine le nom du fichier de sauvegarde à restaurer par le **restaurer** commande.  
  
 Pour **emplacement** éléments, le **fichier** élément décrit un fichier de sauvegarde à distance pour un [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] instance qui contient des partitions distantes. Pour plus d’informations sur la sauvegarde et restauration des partitions distantes, consultez [sauvegarde, restauration et synchronisation de bases de données de &#40;XMLA&#41;](../../../analysis-services/multidimensional-models-scripting-language-assl-xmla/backing-up-restoring-and-synchronizing-databases-xmla.md).  
  
## <a name="see-also"></a>Voir aussi
 [Élément AllowOverwrite &#40;XMLA&#41;](../../../analysis-services/xmla/xml-elements-properties/allowoverwrite-element-xmla.md)   
 [Propriétés &#40;XMLA&#41;](../../../analysis-services/xmla/xml-elements-properties/xml-elements-properties.md)  
  
  
