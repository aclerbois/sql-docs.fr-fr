---
title: Type de données ClrAssemblyFile (ASSL) | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: assl
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: db7ec0dea32fd0fbe0c0584b75fb42b783629daa
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="clrassemblyfile-data-type-assl"></a>Type de données ClrAssemblyFile (ASSL)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Définit un type de données primitif qui représente un des fichiers qui composent un [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] **Assembly** ([ClrAssembly](../../../analysis-services/scripting/data-type/clrassembly-data-type-assl.md) élément).  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<ClrAssemblyFile>  
   <Name>...</Name>  
      <Type>...</Type>  
      <Data>...</Data>  
</ClrAssemblyFile>  
```  
  
## <a name="data-type-characteristics"></a>Caractéristiques du type de données  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Types de données de base|Aucune|  
|Types de données dérivés|Aucune|  
  
## <a name="data-type-relationships"></a>Relations du type de données  
  
|Relation|Élément|  
|------------------|-------------|  
|Éléments parents|Aucune|  
|Éléments enfants|[Données](../../../analysis-services/scripting/objects/data-element-assl.md), [nom](../../../analysis-services/scripting/properties/name-element-assl.md), [Type](../../../analysis-services/scripting/properties/type-element-clrassemblyfile-assl.md)|  
|Éléments dérivés|[Fichier](../../../analysis-services/scripting/objects/file-element-assl.md) ([fichiers](../../../analysis-services/scripting/collections/files-element-assl.md) collection de [ClrAssembly](../../../analysis-services/scripting/data-type/clrassembly-data-type-assl.md))|  
  
## <a name="remarks"></a>Notes  
 L’élément correspondant dans le modèle d’objet objets AMO (Analysis Management) est <xref:Microsoft.AnalysisServices.ClrAssemblyFile>.  
  
## <a name="see-also"></a>Voir aussi  
 [Server, élément & #40 ; ASSL & #41 ;](../../../analysis-services/scripting/objects/server-element-assl.md)   
 [Élément de base de données &#40;ASSL&#41;](../../../analysis-services/scripting/objects/database-element-assl.md)   
 [Élément Assemblies &#40;ASSL&#41;](../../../analysis-services/scripting/collections/assemblies-element-assl.md)   
 [Assembly, élément & #40 ; ASSL & #41 ;](../../../analysis-services/scripting/objects/assembly-element-assl.md)   
 [Type de données DataBlock & #40 ; ASSL & #41 ;](../../../analysis-services/scripting/data-type/datablock-data-type-assl.md)   
 [Blocs élément & #40 ; ASSL & #41 ;](../../../analysis-services/scripting/collections/blocks-element-assl.md)   
 [Élément de bloc & #40 ; ASSL & #41 ;](../../../analysis-services/scripting/objects/block-element-assl.md)   
 [Type de données ComAssembly & #40 ; ASSL & #41 ;](../../../analysis-services/scripting/data-type/comassembly-data-type-assl.md)   
 [Analysis Services script des Types de données XML Language & #40 ; ASSL & #41 ;](../../../analysis-services/scripting/data-type/analysis-services-scripting-language-xml-data-types-assl.md)  
  
  
