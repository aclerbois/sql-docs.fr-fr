---
title: Ensemble de lignes DISCOVER_XML_METADATA | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: schema-rowsets
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 74b0c33570c9a7f6889e9754eeac7426f72c98ac
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="discoverxmlmetadata-rowset"></a>Ensemble de lignes DISCOVER_XML_METADATA
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Retourne un document XML qui décrit un objet demandé. L'ensemble de lignes qui est retourné se compose toujours d'une ligne et d'une colonne.  
  
 Si vous appelez le [Discover](../../../analysis-services/xmla/xml-elements-methods-discover.md) méthode avec la **DISCOVER_XML_METATDATA** valeur d’énumération dans le [RequestType](../../../analysis-services/xmla/xml-elements-properties/requesttype-element-xmla.md) élément, le **Discover** méthode retourne la **DISCOVER_XML_METATDATA** ensemble de lignes.  
  
## <a name="rowset-columns"></a>Colonnes de l'ensemble de lignes  
 L'ensemble de lignes **DISCOVER_XML_METADATA** contient la colonne suivante.  
  
|Nom de colonne|Indicateur de type|Longueur| Description|  
|-----------------|--------------------|------------|-----------------|  
|**MÉTADONNÉES**|**DBTYPE_WSTR**||Document XML qui décrit l'objet demandé par la restriction.|  
  
 Cet ensemble de lignes de schéma n'est pas trié.  
  
> [!IMPORTANT]  
>  L'ensemble de lignes **DISCOVER_XML_METADATA** ne peut pas être interrogé à l'aide de la syntaxe de commande SELECT. Toutefois, le **DISCOVER_XML_METADATA** ensemble de lignes peut être interrogée à l’aide de <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection.GetSchemaDataSet%2A>.  
  
## <a name="restriction-columns"></a>Colonnes de restriction  
 L'ensemble de lignes **DISCOVER_XML_METADATA** peut être restreint sur les colonnes répertoriées dans le tableau suivant.  
  
|Nom de colonne|Indicateur de type|État de la restriction|  
|-----------------|--------------------|-----------------------|  
|**DatabaseID**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**DimensionID**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**CubeID**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**MeasureGroupID**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**ID de partition**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**PerspectiveID**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**DimensionPermissionID**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**RoleID**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**DatabasePermissionID**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**MiningModelID**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**MiningModelPermissionID**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**DataSourceID**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**MiningStructureID**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**AggregationDesignID**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**TraceID**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**MiningStructurePermissionID**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**CubePermissionID**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**ID d’assembly**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**MdxScriptID**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**DataSourceViewID**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**DataSourcePermissionID**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**ObjectExpansion**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
  
 La restriction, **ObjectExpansion**, est disponible pour chaque objet principal de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]. Le client utilise généralement des restrictions pour décrire les objets OLAP pour lesquels la DDL doit être retournée, et il utilise la restriction **ObjectExpansion** pour définir le degré d'expansion dans la DDL retournée. Le tableau suivant indique si la valeur d’énumération est autorisée pour [modifier un élément &#40;XMLA&#41; ](../../../analysis-services/xmla/xml-elements-commands/alter-element-xmla.md) commandes.  
  
|Valeur d'énumération| Description|  
|-----------------------|-----------------|  
|**ReferenceOnly**|Retourne uniquement le nom/ID/horodatage/état demandé pour les objets demandés et tous les objets principaux descendants.|  
|**ObjectProperties**|Développe l'objet demandé sans référence aux objets qu'il contient (inclut les objets secondaires contenus développés).|  
|**ExpandObject**|Identique à *ObjectProperties*, mais retourne également le nom, l'ID et l'horodateur pour les objets principaux contenus.|  
|**ExpandFull**|Développe entièrement l'objet demandé de manière récursive jusqu'au bas de chaque objet qu'il contient.|  
  
## <a name="see-also"></a>Voir aussi  
 [XML for Analysis ensembles de lignes de schéma](../../../analysis-services/schema-rowsets/xml/xml-for-analysis-schema-rowsets.md)  
  
  
