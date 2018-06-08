---
title: Élément (ASSL) de contenu | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: assl
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: ee3c1babd828463e8654280c421d616535aca88e
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="content-element-assl"></a>Élément Content (ASSL)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Décrit le contenu de la colonne dans la [MiningStructure](../../../analysis-services/scripting/objects/miningstructure-element-assl.md) élément.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<ScalarMiningStructureColumn>  
   ...  
   <Content>...</Content>  
   ...  
</ScalarMiningStructureColumn>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l'élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Type de données et longueur|Chaîne (énumération)|  
|Valeur par défaut|Aucune|  
|Cardinalité|1-1 : élément obligatoire qui peut apparaître une fois et une seule.|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Élément|  
|------------------|-------------|  
|Élément parent|[ScalarMiningStructureColumn](../../../analysis-services/scripting/data-type/scalarminingstructurecolumn-data-type-assl.md)|  
|Éléments enfants|Aucune|  
  
## <a name="remarks"></a>Notes  
 Cette énumération décrit le type de contenu représenté par une colonne de structure d'exploration de données et peut être étendue autant que nécessaire par les fournisseurs d'algorithmes d'exploration de données. Pour plus d’informations sur les types de contenu, consultez [Types de contenu &#40;exploration de données&#41;](../../../analysis-services/data-mining/content-types-data-mining.md).  
  
 Les valeurs répertoriées dans le tableau suivant sont généralement prises en charge par les fournisseurs d'algorithmes d'exploration de données.  
  
|Valeur| Description|  
|-----------|-----------------|  
|*Discrètes*|La colonne contient des valeurs discrètes.|  
|*Continue*|Les valeurs de la colonne définissent un jeu continu de données numériques.|  
|*Discrétisé*|Les valeurs de la colonne représentent des groupes (ou compartiments) de valeurs dérivés d'une colonne continue.|  
|*commandée*|Les valeurs de la colonne définissent un jeu ordonné.|  
|*Cyclique*|Les valeurs de la colonne définissent un jeu ordonné cyclique.|  
|*Probabilité*|Les valeurs de la colonne spécifient une probabilité pour les colonnes contenues dans le [ClassifiedColumns](../../../analysis-services/scripting/collections/classifiedcolumns-element-assl.md) élément du parent **ScalarMiningStructureColumn**.|  
|*Variance*|Les valeurs de la colonne spécifient une variance pour les colonnes contenues dans le **ClassifiedColumns** élément du parent **ScalarMiningStructureColumn**.|  
|*StdDev*|Les valeurs de la colonne spécifient un écart type pour les colonnes contenues dans le **ClassifiedColumns** élément du parent **ScalarMiningStructureColumn**.|  
|*ProbabilityVariance*|Les valeurs de la colonne spécifient une variance de probabilité pour les colonnes contenues dans le **ClassifiedColumns** élément du parent **ScalarMiningStructureColumn**.|  
|*ProbabilityStdDev*|Les valeurs de la colonne spécifient un écart de probabilité pour les colonnes contenues dans le **ClassifiedColumns** élément du parent **ScalarMiningStructureColumn**.|  
|*Prise en charge*|Les valeurs de la colonne spécifient les informations de prise en charge pour la colonne contenue dans le **ClassifiedColumns** élément du parent **ScalarMiningStructureColumn**.<br /><br /> Remarque : Cette colonne est fournie en tant que partie de la norme pour les fournisseurs d’algorithmes d’exploration de données tiers. N’apportez pas des algorithmes fournis par Microsoft utilisent cette colonne.|  
|*Clé*|La colonne est une colonne clé.<br /><br /> Remarque : Ce type de contenu est uniquement applicable aux colonnes clés, dans lequel le **IsKey** a la valeur **True**.|  
  
 Outre ces valeurs standards, d’exploration de données des fournisseurs d’algorithmes inclus avec [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] prennent en charge les valeurs dans le tableau suivant.  
  
|Valeur| Description|  
|-----------|-----------------|  
|*Séquence de touches*|La colonne est une colonne clé, et les valeurs de la colonne représentent une séquence d'événements.<br /><br /> Remarque : Ce type de contenu est uniquement applicable aux colonnes clés, dans lequel le **IsKey** a la valeur **True**.|  
|*Temps clé*|La colonne est une colonne clé, et les valeurs de la colonne représentent des unités de mesure du temps.<br /><br /> Remarque : Ce type de contenu est uniquement applicable aux colonnes clés, dans lequel le **IsKey** a la valeur **True**.|  
|*Sequence*|Les valeurs de la colonne représentent une séquence d'événements.|  
|*Time*|Les valeurs de la colonne représentent des unités de mesure du temps.|  
  
 L’énumération qui correspond aux valeurs autorisées pour **contenu** dans l’objet d’objets AMO (Analysis Management) est modèle <xref:Microsoft.AnalysisServices.ScalarMiningStructureColumn>.  
  
## <a name="see-also"></a>Voir aussi  
 [Élément ClassifiedColumns &#40;ASSL&#41;](../../../analysis-services/scripting/collections/classifiedcolumns-element-assl.md)   
 [Propriétés & #40 ; ASSL & #41 ;](../../../analysis-services/scripting/properties/properties-assl.md)  
  
  
