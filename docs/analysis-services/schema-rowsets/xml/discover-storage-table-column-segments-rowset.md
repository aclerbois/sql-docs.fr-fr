---
title: Ensemble de lignes DISCOVER_STORAGE_TABLE_COLUMN_SEGMENTS | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: schema-rowsets
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: bd9c481ea5d34dcf4b8dd478000c564ff6c996ab
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="discoverstoragetablecolumnsegments-rowset"></a>Ensemble de lignes DISCOVER_STORAGE_TABLE_COLUMN_SEGMENTS
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Fournit des informations au niveau de la colonne et du segment sur les tables de stockage utilisé par une base de données Analysis Services en cours d’exécution tabulaire ou [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] mode. Cet ensemble de lignes est utilisé principalement pour le dépannage et l'analyse.  
  
 **S'applique à :** modèles tabulaires  
  
## <a name="rowset-columns"></a>Colonnes de l'ensemble de lignes  
 L'ensemble de lignes **DISCOVER_STORAGE_TABLE_COLUMN_SEGMENTS** contient les colonnes suivantes.  
  
|**Nom de colonne**|**Indicateur de type**|**Restriction**|**Description**|  
|---------------------|------------------------|---------------------|---------------------|  
|**DATABASE_NAME**|**DBTYPE_WSTR**|Oui|Spécifie la base de données tabulaire.<br /><br /> L'ensemble de lignes **DISCOVER_STORAGE_TABLE_COLUMN_SEGMENTS** peut être restreint à l'aide de cette colonne. Si omis, le nom de la base de données active est utilisé.|  
|**CUBE_NAME**|**DBTYPE_WSTR**|Oui|Nom du modèle.<br /><br /> L'ensemble de lignes **DISCOVER_STORAGE_TABLES** peut être restreint à l'aide de cette colonne.|  
|**NOM_GROUPE_MESURES**|**DBTYPE_WSTR**|Oui|Nom du groupe de mesures.|  
|**NOM_PARTITION**|**DBTYPE_WSTR**|Oui|Nom de la partition.|  
|**DIMENSION_NAME**|**DBTYPE_WSTR**||Nom de la dimension.|  
|**TABLE_ID**|**DBTYPE_WSTR**||ID interne du segment de table.|  
|**COLUMN_ID**|**DBTYPE_WSTR**||ID interne de la colonne.|  
|**_NOMBRE DE SEGMENT**|**DBTYPE_I8**||Nombre ordinal du segment de table.|  
|**TABLE_PARTTION_NUMBER**|**DBTYPE_I8**||Nombre ordinal de la partition.|  
|**RECORDS_COUNT**|**DBTYPE_I8**||Nombre d'enregistrements dans la partition.|  
|**ALLOCATED_SIZE**|**DBTYPE_UI8**||Taille allouée en octets au segment de colonne.|  
|**USED_SIZE**|**DBTYPE_UI8**||Taille en octets utilisée par le segment de colonne.|  
|**COMPRESSION_TYPE**|**DBTYPE_WSTR**||Type de compression appliqué au segment de colonne. Cette valeur est destinée uniquement à une utilisation interne et de support technique. Microsoft ne publie pas de valeurs valides ou de descriptions pour cette colonne.|  
|**BITS_COUNT**|**DBTYPE_I8**||Nombre de bits.|  
|**BOOKMARK_BITS_COUNT**|**DBTYPE_I8**||Nombre de bits de signet.|  
|**VERTIPAQ_STATE**|**DBTYPE_WSTR**||État de compression VertiPaq pour ce segment de colonne. Les valeurs possibles sont les suivantes :<br /><br /> SKIPPED – la compression VertiPaq a été ignorée.<br /><br /> COMPLETED – la compression VertiPaq s'est terminée correctement.<br /><br /> TIMEBOXED - un délai a été imparti pour la compression VertiPaq.|  
  
## <a name="using-adomdnet-to-return-the-rowset"></a>Utilisation d'ADOMD.NET pour retourner l'ensemble de lignes  
 Lorsque vous utilisez ADOMD.NET et l'ensemble de lignes de schéma pour récupérer des métadonnées, vous pouvez utiliser un GUID ou une chaîne pour référencer un objet d'ensemble de lignes de schéma dans la méthode GetSchemaDataSet. Pour plus d'informations, consultez [Working with Schema Rowsets in ADOMD.NET](../../../analysis-services/multidimensional-models-adomd-net-client/retrieving-metadata-working-with-schema-rowsets.md).  
  
 Le tableau suivant fournit le GUID et les valeurs de chaîne qui identifient cet ensemble de lignes.  
  
|Argument|Valeur|  
|--------------|-----------|  
|GUID|a07ccd45-8148-11d0-87bb-00c04fc33942|  
|ADOMDNAME|StorageSegments|  
  
## <a name="example"></a>Exemple  
 La requête suivante retourne les segments de la table stockage associés à l'attribut de modèle LastName, dans la base de données actuelle.  
  
```  
SELECT DISTINCT TABLE_ID, COLUMN_ID   
FROM $system.DISCOVER_STORAGE_TABLE_COLUMN_SEGMENTS  
WHERE COLUMN_ID = 'LastName'  
ORDER BY TABLE_ID  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Ensembles de lignes de schéma de Analysis Services](../../../analysis-services/schema-rowsets/analysis-services-schema-rowsets.md)  
  
  
