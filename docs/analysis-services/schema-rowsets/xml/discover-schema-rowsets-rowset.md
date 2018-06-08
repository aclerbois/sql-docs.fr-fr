---
title: Ensemble de lignes DISCOVER_SCHEMA_ROWSETS | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: schema-rowsets
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 7349a3e4877813003212b125de31e7b98343313d
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="discoverschemarowsets-rowset"></a>Ensemble de lignes DISCOVER_SCHEMA_ROWSETS
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Retourne les noms, restrictions, description et autres informations pour toutes les valeurs d'énumération et toutes les valeurs d'énumération supplémentaires spécifiques au fournisseur prises en charge par le fournisseur [!INCLUDE[msCoName](../../../includes/msconame-md.md)] XMLA (XML for Analysis).  
  
 Si vous appelez le [Discover](../../../analysis-services/xmla/xml-elements-methods-discover.md) méthode avec la **DISCOVER_SCHEMA_ROWSETS** valeur d’énumération dans le [RequestType](../../../analysis-services/xmla/xml-elements-properties/requesttype-element-xmla.md) élément, le **Discover** méthode retourne la **DISCOVER_SCHEMA_ROWSETS** ensemble de lignes.  
  
## <a name="rowset-columns"></a>Colonnes de l'ensemble de lignes  
 L'ensemble de lignes DISCOVER_SCHEMA_ROWSETS contient les colonnes suivantes.  
  
|Nom de colonne|Indicateur de type|Longueur| Description|  
|-----------------|--------------------|------------|-----------------|  
|**schemaName**|**DBTYPE_WSTR**||Nom du schéma ou de la requête. Cette requête retourne les valeurs de l'énumération *RequestTypes* .|  
|**SchemaGuid**|**DBTYPE_GUID**||GUID du schéma.|  
|**Restrictions**|**DBTYPE_HCHAPTER**||Tableau des restrictions prises en charge par le fournisseur.|  
|**Description**|**DBTYPE_WSTR**||Description localisable du schéma.|  
|**RestrictionsMask**|**DBTYPE_UI8**|||  
  
 Cet ensemble de lignes de schéma n'est pas trié.  
  
 Pour un fournisseur qui prend en charge trois restrictions pour l'ensemble de lignes de schéma DBSCHEMA_MEMBERS, le tableau **Restrictions** peut retourner le résultat suivant. Les éléments du résultat font référence aux noms de colonne dans le schéma.  
  
```  
<Restrictions>  
      <CATALOG_NAME type="string" />   
      <SCHEMA_NAME type="string" />   
      <CUBE_NAME type="string" />   
</Restrictions>  
```  
  
## <a name="restriction-columns"></a>Colonnes de restriction  
 L'ensemble de lignes **DISCOVER_SCHEMA_ROWSETS** peut être restreint sur les colonnes répertoriées dans le tableau suivant.  
  
|Nom de colonne|Indicateur de type|État de la restriction|  
|-----------------|--------------------|-----------------------|  
|**schemaName**|**DBTYPE_WSTR**||  
  
## <a name="see-also"></a>Voir aussi  
 [XML for Analysis ensembles de lignes de schéma](../../../analysis-services/schema-rowsets/xml/xml-for-analysis-schema-rowsets.md)  
  
  
