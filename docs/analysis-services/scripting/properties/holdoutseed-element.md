---
title: Élément HoldoutSeed | Documents Microsoft
ms.date: 5/8/2018
ms.prod: sql
ms.custom: assl
ms.reviewer: owend
ms.technology: analysis-services
ms.topic: reference
author: minewiskan
ms.author: owend
manager: kfile
ms.openlocfilehash: f0fbf102dcfc731f02195964c6657244e9bf0b70
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="holdoutseed-element"></a>Élément HoldoutSeed
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Spécifie la valeur initiale d’une partition d’exclusion répétée qui contient le jeu de tests d’un [MiningStructure](../../../analysis-services/scripting/objects/miningstructure-element-assl.md) élément. Cette valeur de départ garantit que le contenu du modèle reste le même pendant le retraitement. Si non spécifié ou la valeur 0, [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] crée une valeur initiale à l’aide d’un algorithme de hachage sur le nom de la structure d’exploration de données.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<MiningStructure>  
   ...  
   <ddl100_100:HoldoutSeed>...</ddl100_100:HoldoutSeed>  
   ...  
</MiningStructure>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l'élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Type de données et longueur|Long|  
|Valeur par défaut|0|  
|Cardinalité|0-1 : Élément facultatif qui peut se produire une seule fois seulement.|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Élément|  
|------------------|-------------|  
|Élément parent|[MiningStructure](../../../analysis-services/scripting/objects/miningstructure-element-assl.md)|  
|Éléments enfants|Aucune|  
  
## <a name="remarks"></a>Notes  
 Lors de la création initiale d'une structure d'exploration de données, l'ID et le nom sont identiques. Toutefois, vous pouvez modifier le nom de la structure d'exploration de données. Par conséquent, si vous souhaitez veiller à ce que la partition soit répétée, vous ne devez pas compter sur la valeur de départ créée par le nom, mais définir une valeur de départ explicitement.  
  
 En outre, lorsque vous créez une copie d’une structure d’exploration de données à l’aide de la **exporter** instruction, [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] conserve le nom de la nouvelle structure d’exploration de données mais génère automatiquement un nouveau code. Par conséquent, il est possible d'avoir deux structures d'exploration de données qui partagent le même nom mais qui ont des ID différents. Deux structures d'exploration de données qui portent le même nom ont la même valeur de départ. Toutefois, comme le partitionnement des données dépend également des données source, le contenu réel des partitions dans chaque structure peut être différent.  
  
 Les nouvelles propriétés **HoldoutMaxCases**, **HoldoutMaxPercent**, **HoldoutSeed**, ou **HoldoutActualSize** sont uniquement disponibles dans [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] et versions ultérieures. Par conséquent, vous devez préfixer ces propriétés avec le nouvel espace de noms comme indiqué dans la description de la syntaxe sans quoi [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] retourne une erreur.  
  
 **Remarque** dans [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] ne prenait pas en charge l’utilisation de partitions d’exclusion sur une structure d’exploration de données. Par conséquent, [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] des instructions de langage de script (ASSL) qui contiennent les paramètres d’exclusion **HoldoutMaxCases**, **HoldoutMaxPercent**, **HoldoutSeed**, ou **HoldoutActualSize** ne peut pas être utilisé dans [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]. Si vous utilisez l'un de ces paramètres d'exclusion dans une instruction ASSL dans [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] retourne une erreur.  
  
 L’élément qui correspond au parent de **HoldoutSeed** dans l’objet d’objets AMO (Analysis Management) est modèle <xref:Microsoft.AnalysisServices.MiningStructure>.  
  
## <a name="see-also"></a>Voir aussi  
 [Propriétés &#40;ASSL&#41;](../../../analysis-services/scripting/properties/properties-assl.md)   
 [HoldoutActualSize (élément)](../../../analysis-services/scripting/properties/holdoutactualsize-element.md)   
 [Holdoutmaxpercent, élément](../../../analysis-services/scripting/properties/holdoutmaxpercent-element.md)   
 [HoldoutMaxCases, élément](../../../analysis-services/scripting/properties/holdoutmaxcases-element.md)  
  
  
