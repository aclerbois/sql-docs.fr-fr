---
title: Ensemble de lignes MDSCHEMA_CUBES | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: schema-rowsets
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: da0fc34fedcd6980f8dd19e199314fc1295c78bb
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="mdschemacubes-rowset"></a>Ensemble de lignes MDSCHEMA_CUBES
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Décrit la structure des cubes d'une base de données.  
  
## <a name="rowset-columns"></a>Colonnes de l'ensemble de lignes  
 L'ensemble de lignes **MDSCHEMA_CUBES** contient les colonnes suivantes.  
  
|Nom de colonne|Indicateur de type| Description|  
|-----------------|--------------------|-----------------|  
|**CATALOG_NAME**|**DBTYPE_WSTR**|Nom de la base de données.|  
|**SCHEMA_NAME**|**DBTYPE_WSTR**|Non pris en charge.|  
|**CUBE_NAME**|**DBTYPE_WSTR**|Nom du cube ou de la dimension. Les noms de dimension sont précédés d'un symbole dollar ($).<br /><br /> Remarque : Seuls administrateurs de serveur et base de données disposer d’autorisations pour afficher les cubes créés à partir d’une dimension.|  
|**CUBE_TYPE**|**DBTYPE_WSTR**|Type du cube. Les valeurs valides sont :<br /><br /> **CUBE**<br /><br /> **DIMENSION**|  
|**CUBE_GUID**|**DBTYPE_GUID**|Non pris en charge.|  
|**CREATED_ON**|**DBTYPE_DBTIMESTAMP**|Non pris en charge.|  
|**LAST_SCHEMA_UPDATE**|**DBTYPE_DBTIMESTAMP**|Heure du dernier traitement du cube.|  
|**SCHEMA_UPDATED_BY**|**DBTYPE_WSTR**|Non pris en charge.|  
|**LAST_DATA_UPDATE**|**DBTYPE_DBTIMESTAMP**|Heure du dernier traitement du cube.|  
|**DATA_UPDATED_BY**|**DBTYPE_WSTR**|Non pris en charge.|  
|**DESCRIPTION**|**DBTYPE_WSTR**|Description conviviale du cube.|  
|**IS_DRILLTHROUGH_ENABLED**|**DBTYPE_BOOL**|Valeur booléenne qui retourne toujours True.|  
|**IS_LINKABLE**|**DBTYPE_BOOL**|Valeur booléenne qui indique si un cube peut être utilisé dans un cube lié.|  
|**IS_WRITE_ENABLED**|**DBTYPE_BOOL**|Valeur booléenne indiquant si un cube est activé en écriture.|  
|**IS_SQL_ENABLED**|**DBTYPE_BOOL**|Valeur booléenne qui indique si SQL peut être utilisé sur le un cube.|  
|**CUBE_CAPTION**|**DBTYPE_WSTR**|Légende du cube.|  
|**BASE_CUBE_NAME**|**DBTYPE_WSTR**|Nom du cube source s'il s'agit d'un cube de perspective.|  
|**ANNOTATIONS**|**DBTYPE_WSTR**|(Facultatif) Jeu de notes, au format XML.|  
  
 L'ensemble de lignes est trié selon **CATALOG_NAME**, **SCHEMA_NAME**, **CUBE_NAME**.  
  
## <a name="restriction-columns"></a>Colonnes de restriction  
 L'ensemble de lignes **MDSCHEMA_CUBES** peut être restreint sur les colonnes répertoriées dans le tableau suivant.  
  
|Nom de colonne|Indicateur de type|État de la restriction|  
|-----------------|--------------------|-----------------------|  
|**CATALOG_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**SCHEMA_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**CUBE_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**CUBE_SOURCE**|**DBTYPE_UI2**|(Facultatif) Restriction par défaut est une valeur de 1. Une image bitmap avec l’une des valeurs valides suivantes :<br /><br /> 1 CUBE<br /><br /> 2 DIMENSION|  
|**Cube_Name de base**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
  
## <a name="see-also"></a>Voir aussi  
 [OLE DB pour OLAP Schema Rowsets](../../../analysis-services/schema-rowsets/ole-db-olap/ole-db-for-olap-schema-rowsets.md)  
  
  
