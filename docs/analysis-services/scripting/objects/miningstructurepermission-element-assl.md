---
title: Élément MiningStructurePermission (ASSL) | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: assl
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: d933d5051a9280d779c23eeb60832aee286dc131
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="miningstructurepermission-element-assl"></a>Élément MiningStructurePermission (ASSL)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Définit les autorisations que les membres d’un [rôle](../../../analysis-services/scripting/objects/role-element-assl.md) élément avoir un individu [MiningStructure](../../../analysis-services/scripting/objects/miningstructure-element-assl.md) élément.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<MiningStructurePermissions>  
   <MiningStructurePermission xsi:type="Permission">  
      <AllowDrillthrough>...</AllowDrillthrough>  
  
      <!-- This element also inherits all the child elements listed in Permission -->  
   </MiningStructurePermission>  
</MiningStructurePermissions>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l'élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Type de données et longueur|[Autorisation](../../../analysis-services/scripting/data-type/permission-data-type-assl.md)|  
|Valeur par défaut|Aucune|  
|Cardinalité|0-n : élément facultatif pouvant apparaître plusieurs fois.|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Élément|  
|------------------|-------------|  
|Éléments parents|[MiningStructurePermissions](../../../analysis-services/scripting/collections/miningstructurepermissions-element-assl.md)|  
|Éléments enfants|Aucune|  
  
## <a name="remarks"></a>Notes  
 L’élément correspondant dans le modèle d’objet objets AMO (Analysis Management) est <xref:Microsoft.AnalysisServices.MiningStructurePermission>.  
  
 Dans [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)], l’autorisation **AllowDrillthrough** a été étendu pour appliquer à une structure d’exploration de données. Lorsque vous assignez cette autorisation à un rôle, tout utilisateur qui est membre de ce rôle peut interroger directement la structure d'exploration de données en utilisant la syntaxe suivante :  
  
```  
SELECT <structure column list> FROM <structure>.CASES  
```  
  
 De plus, si **AllowDrillthrough** est activé sur la structure d'exploration de données et le modèle d'exploration de données, les utilisateurs peuvent interroger les colonnes de structure qui n'étaient pas incluses dans le modèle d'exploration de données en utilisant la syntaxe suivante :  
  
```  
SELECT StructureColumn('<structure column name>' FROM <model>.CASES  
```  
  
 Par exemple, vous créez un modèle qui utilise uniquement des colonnes pour la clé des clients, les revenus des clients et les achats des clients. En utilisant l'extraction, un utilisateur peut retourner d'autres colonnes de structure qui n'étaient pas incluses dans le modèle d'exploration de données, telles que les informations de contact client.  
  
 Par conséquent, pour protéger des données sensibles ou des informations personnelles, vous devez construire votre vue de source de données de manière à masquer les informations personnelles et accorder l'autorisation **AllowDrillthrough** sur une structure uniquement si cela est nécessaire.  
  
 Pour plus d’informations, consultez [Requêtes d’extraction &#40;exploration de données&#41;](../../../analysis-services/data-mining/drillthrough-queries-data-mining.md).  
  
## <a name="see-also"></a>Voir aussi  
 <xref:Microsoft.AnalysisServices.MiningModel.AllowDrillThrough%2A>   
 <xref:Microsoft.AnalysisServices.AdomdClient.MiningModel.AllowDrillThrough%2A>   
 [Objets & #40 ; ASSL & #41 ;](../../../analysis-services/scripting/objects/objects-assl.md)  
  
  
