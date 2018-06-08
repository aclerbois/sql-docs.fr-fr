---
title: L’élément NullProcessing (ASSL) | Documents Microsoft
ms.date: 5/8/2018
ms.prod: sql
ms.custom: assl
ms.reviewer: owend
ms.technology: analysis-services
ms.topic: reference
author: minewiskan
ms.author: owend
manager: kfile
ms.openlocfilehash: 9e5fbc3682a614dc1599347ec030348ae10bb2c0
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="nullprocessing-element-assl"></a>NullProcessing Element (ASSL)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Définit le mode de traitement des valeurs NULL.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<DataItem>  
   ...  
   <NullProcessing>...</NullProcessing>  
   ...  
</DataItem>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l'élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Type de données et longueur|Chaîne (énumération)|  
|Valeur par défaut|*Automatique*|  
|Cardinalité|0-1: élément facultatif qui peut apparaître une fois et une seule.|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Élément|  
|------------------|-------------|  
|Élément parent|[DataItem](../../../analysis-services/scripting/data-type/dataitem-data-type-assl.md)|  
|Éléments enfants|Aucune|  
  
## <a name="remarks"></a>Notes  
 La valeur de cet élément est limitée à l'une des chaînes répertoriées dans le tableau suivant.  
  
|Valeur| Description|  
|-----------|-----------------|  
|*Conserver*|Conserve la valeur NULL.<br /><br /> Remarque : Cette valeur n’est pas prise en charge pour les mesures de comptage.|  
|*Erreur*|Déclenche une erreur de clé NULL. La valeur de [NullKeyNotAllowed](../../../analysis-services/scripting/properties/nullkeynotallowed-element-assl.md) détermine comment l’instance réagit à l’erreur.<br /><br /> Remarque : Cette valeur n’est pas prise en charge pour les mesures.|  
|*UnknownMember*|Génère un membre inconnu et déclenche une erreur de conversion de valeur NULL. La valeur de [NullKeyConvertedToUnknown](../../../analysis-services/scripting/properties/nullkeyconvertedtounknown-element-assl.md) détermine comment l’instance réagit à l’erreur.<br /><br /> Remarque : Cette valeur n’est pas prise en charge pour les colonnes associées aux mesures.|  
|*ZeroOrBlank*|Convertit la valeur NULL en zéro (pour les éléments de données numériques) ou en une chaîne vide (pour les éléments de données de type chaîne).<br /><br /> Remarque : Cette valeur assure la compatibilité avec les versions précédentes de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)].|  
|*Automatique*|Utilise le traitement par défaut approprié pour l'élément :<br /><br /> *ZeroOrBlank* pour les éléments de données OLAP.<br /><br /> *UnknownMember* pour les éléments de données d'exploration de données.|  
  
 L’énumération qui correspond aux valeurs autorisées pour **NullProcessing** dans l’objet d’objets AMO (Analysis Management) est modèle <xref:Microsoft.AnalysisServices.NullProcessing>.  
  
## <a name="see-also"></a>Voir aussi  
 [Propriétés & #40 ; ASSL & #41 ;](../../../analysis-services/scripting/properties/properties-assl.md)  
  
  
