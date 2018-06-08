---
title: Ensemble de lignes DISCOVER_PROPERTIES | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: schema-rowsets
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 8752af7b67b69659ec3d9a348b4ec7c647f41c55
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="discoverproperties-rowset"></a>Ensemble de lignes DISCOVER_PROPERTIES
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Retourne une liste d'informations et de valeurs sur les propriétés standard et spécifiques au fournisseur prises en charge par le fournisseur XML for Analysis (XMLA) [!INCLUDE[msCoName](../../../includes/msconame-md.md)] pour la source de données spécifiée. Les propriétés non prises en charge ne sont pas répertoriées dans le jeu de résultats retourné.  
  
 Si vous appelez le [Discover](../../../analysis-services/xmla/xml-elements-methods-discover.md) méthode avec la **DISCOVER_PROPERTIES** valeur d’énumération dans le [RequestType](../../../analysis-services/xmla/xml-elements-properties/requesttype-element-xmla.md) élément, le **Discover** méthode retourne la **DISCOVER_PROPERTIES** ensemble de lignes...  
  
## <a name="rowset-columns"></a>Colonnes de l'ensemble de lignes  
 L'ensemble de lignes **DISCOVER_PROPERTIES** contient les colonnes suivantes.  
  
|Nom de colonne|Type|Longueur| Description|  
|-----------------|----------|------------|-----------------|  
|**propertyName**|**DBTYPE_WSTR**||Nom de la propriété.|  
|**PropertyDescription**|**DBTYPE_WSTR**||Description (texte) localisable de la propriété. Peut retourner **NULL**.|  
|**PropertyType**|**DBTYPE_WSTR**||Type de données XML de la propriété.<br /><br /> Peut retourner **NULL**.|  
|**PropertyAccessType**|**DBTYPE_WSTR**||Accès pour la propriété. La valeur peut être **Read**, **Write**ou **ReadWrite**.|  
|**IsRequired**|**DBTYPE_BOOL**||Valeur booléenne qui indique si une propriété est obligatoire.<br /><br /> True si une propriété est obligatoire ; false dans le cas contraire.<br /><br /> Peut retourner **NULL**.|  
|**Value**|**DBTYPE_WSTR**||Valeur actuelle de la propriété.<br /><br /> Peut retourner **NULL**.|  
  
 Cet ensemble de lignes de schéma n'est pas trié.  
  
## <a name="restriction-columns"></a>Colonnes de restriction  
 L'ensemble de lignes **DISCOVER_PROPERTIES** peut être restreint sur la colonne répertoriée dans le tableau suivant.  
  
|Nom de colonne|Indicateur de type|État de la restriction|  
|-----------------|--------------------|-----------------------|  
|**propertyName**|**DBTYPE_WSTR**||  
  
## <a name="see-also"></a>Voir aussi  
 [XML for Analysis ensembles de lignes de schéma](../../../analysis-services/schema-rowsets/xml/xml-for-analysis-schema-rowsets.md)  
  
  
