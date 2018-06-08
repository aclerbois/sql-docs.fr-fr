---
title: Élément (XMLA) de sauvegarde | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 4e00a4991226779a91e16806dbe15973d946ec69
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34574211"
---
# <a name="backup-element-xmla"></a>Élément Backup (XMLA)
[!INCLUDE[ssas-appliesto-sqlas-aas](../../../includes/ssas-appliesto-sqlas-aas.md)]
  Permet de sauvegarder une base de données Analysis Services dans un fichier de sauvegarde.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<Command>  
   <Backup>  
      <Object>...</Object>  
      <File>...</File>  
      <Security>...</Security>  
      <ApplyCompression>...</ApplyCompression>  
      <AllowOverwrite>...</AllowOverwrite>  
      <Password>...</Password>  
      <BackupRemotePartitions>...</BackupRemotePartitions>  
      <Locations>...</Locations>  
   </Backup>  
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
|Éléments enfants|[AllowOverwrite](../../../analysis-services/xmla/xml-elements-properties/allowoverwrite-element-xmla.md), [ApplyCompression](../../../analysis-services/xmla/xml-elements-properties/applycompression-element-xmla.md), [BackupRemotePartitions](../../../analysis-services/xmla/xml-elements-properties/backupremotepartitions-element-xmla.md), [fichier](../../../analysis-services/xmla/xml-elements-properties/file-element-xmla.md), [emplacements](../../../analysis-services/xmla/xml-elements-properties/locations-element-xmla.md), [ Objet](../../../analysis-services/xmla/xml-elements-properties/object-element-xmla.md), [mot de passe](../../../analysis-services/xmla/xml-elements-properties/password-element-xmla.md), [sécurité](../../../analysis-services/xmla/xml-elements-properties/security-element-xmla.md)|  
  
## <a name="remarks"></a>Notes  
 Le **sauvegarde** commande sauvegarde le [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] base de données spécifiée dans le [objet](../../../analysis-services/xmla/xml-elements-properties/object-element-xmla.md) élément à un fichier de sauvegarde et sauvegarde éventuellement les partitions distantes pour les fichiers de sauvegarde distants. Si le **objet** élément fait référence à un objet autre qu’un [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] de base de données, une erreur se produit.  
  
 Les informations sauvegardées par la commande **Backup** dépendent du mode de stockage utilisé par les objets dans la base de données. Le tableau suivant répertorie les informations sauvegardées selon le mode de stockage utilisé.  
  
|Mode de stockage|Informations sauvegardées|  
|------------------|-----------------------------------|  
|OLAP multidimensionnel (MOLAP)|Données sources, agrégations et métadonnées|  
|Hybrid OLAP (HOLAP)|Agrégations et métadonnées|  
|OLAP relationnel (ROLAP)|Métadonnées|  
  
 Pendant un **sauvegarde** de commande, un verrou partagé est placé sur le [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] base de données spécifiée dans le **objet** élément. Le verrou partagé est libéré lorsque la commande **Backup** est terminée.  
  
 Plusieurs **sauvegarde** commandes peuvent être exécutées en parallèle, si les commandes sont inclus dans le [parallèles](../../../analysis-services/xmla/xml-elements-properties/parallel-element-xmla.md) collection d’un [lot](../../../analysis-services/xmla/xml-elements-commands/batch-element-xmla.md) commande. La collection **Parallel** permet de sauvegarder une base de données dans plusieurs fichiers de sauvegarde simultanément.  
  
 Pour plus d’informations sur la sauvegarde et restauration des bases de données, consultez [sauvegarde, restauration et synchronisation de bases de données de &#40;XMLA&#41;](../../../analysis-services/multidimensional-models-scripting-language-assl-xmla/backing-up-restoring-and-synchronizing-databases-xmla.md).  
  
> [!IMPORTANT]  
>  Pour chaque fichier de sauvegarde, l'utilisateur qui exécute la commande de sauvegarde doit avoir l'autorisation d'écrire dans l'emplacement de sauvegarde spécifié pour chaque fichier. L’utilisateur doit aussi avoir l’un des rôles suivants : membre d’un rôle serveur pour l’instance [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] ou membre d’un rôle de base de données avec les autorisations de contrôle total (Administrateur) sur la base de données à sauvegarder.  
  
## <a name="see-also"></a>Voir aussi
 [Élément Restore &#40;XMLA&#41;](../../../analysis-services/xmla/xml-elements-commands/restore-element-xmla.md)   
 [Élément Synchronize &#40;XMLA&#41;](../../../analysis-services/xmla/xml-elements-commands/synchronize-element-xmla.md)   
 [Commandes &#40;XMLA&#41;](../../../analysis-services/xmla/xml-elements-commands/xml-elements-commands.md)  
  
  
