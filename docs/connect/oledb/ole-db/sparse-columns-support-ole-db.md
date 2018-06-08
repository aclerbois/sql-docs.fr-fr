---
title: Prise en charge des colonnes éparses (OLE DB) | Documents Microsoft
description: Prise en charge des colonnes éparses (OLE DB)
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: ole-db
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: reference
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: d505ef702a1fda4b3896b51efb23c2b3b761bf33
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sparse-columns-support-ole-db"></a>Prise en charge des colonnes éparses (OLE DB)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Cette rubrique fournit des informations sur le pilote OLE DB pour la prise en charge de SQL Server pour les colonnes éparses. Pour plus d’informations sur les colonnes éparses, consultez [prise en charge des colonnes éparses dans le pilote OLE DB pour SQL Server](../../oledb/features/sparse-columns-support-in-oledb-driver-for-sql-server.md). Pour obtenir un exemple, consultez [colonne d’affichage et de métadonnées de catalogue pour les colonnes éparses &#40;OLE DB&#41;](../../oledb/ole-db-how-to/display-column-and-catalog-metadata-for-sparse-columns-ole-db.md).  
  
## <a name="ole-db-statement-metadata"></a>Métadonnées d'instruction OLE DB  
 À partir de [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)], une nouvelle valeur d'indicateur DBCOLUMNFLAGS, DBCOLUMNFLAGS_SS_ISCOLUMNSET, est disponible. Cette valeur doit être définie pour les colonnes qui sont **column_set** valeurs. L’indicateur DBCOLUMNFLAGS peut être récupéré via la *dwFlags* paramètre de IColumnsInfo::GetColumnsInfo et la colonne DBCOLUMN_FLAGS de l’ensemble de lignes retourné par IColumnsRowset::GetColumnsRowset.  
  
## <a name="ole-db-catalog-metadata"></a>Métadonnées de catalogue OLE DB  
 Deux colonnes supplémentaires spécifiques à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ont été ajoutées à DBSCHEMA_COLUMNS.  
  
|Nom de colonne|Type de données|Valeur/commentaires|  
|-----------------|---------------|---------------------|  
|SS_IS_SPARSE|DBTYPE_BOOL|Si la colonne est une colonne éparse, la valeur est VARIANT_TRUE ; sinon, VARIANT_FALSE.|  
|SS_IS_COLUMN_SET|DBTYPE_BOOL|Si la colonne est éparse **column_set** colonne, la valeur est VARIANT_TRUE ; sinon, VARIANT_FALSE.|  
  
 Deux ensembles de lignes de schéma supplémentaires ont également été ajoutés. Ces ensembles de lignes ont la même structure que DBSCHEMA_COLUMNS mais retournent un contenu différent. DBSCHEMA_COLUMNS_EXTENDED retourne toutes les colonnes, indépendamment du **column_set** l’appartenance. DBSCHEMA_SPARSE_COLUMN_SET retourne uniquement les colonnes qui sont membres d’éparse **column_set**.  
  
## <a name="ole-db-datatypecompatibility-behavior"></a>Comportement OLE DB de DataTypeCompatibility  
 Comportement avec **DataTypeCompatibility = 80** (dans la chaîne de connexion) est cohérent avec un [!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)] client, comme suit :  
  
-   Les nouveaux ensembles de lignes de schéma ne sont pas visibles, et il n'y a pas de lignes pour eux dans l'ensemble de lignes d'ensembles de lignes de schéma.  
  
-   Les nouvelles colonnes de l'ensemble de lignes COLUMNS ne sont pas visibles.  
  
-   DBCOLUMNFLAGS_SS_ISCOLUMNSET n’est pas définie pour **column_set** colonnes.  
  
-   DBCOMPUTEMODE_NOTCOMPUTED est défini pour **column_set** colonnes.  
  
## <a name="ole-db-support-for-sparse-columns"></a>Prise en charge OLE DB des colonnes éparses  
 Les interfaces OLE DB suivantes ont été modifiés dans le pilote OLE DB pour SQL Server prendre en charge les colonnes éparses :  
  
|Type ou fonction membre| Description|  
|-----------------------------|-----------------|  
|IColumnsInfo::GetColumnsInfo|Un nouveau DBCOLUMNFLAGS indicateur valeur DBCOLUMNFLAGS_SS_ISCOLUMNSET est définie pour **column_set** dans les colonnes *dwFlags*.<br /><br /> DBCOLUMNFLAGS_WRITE est défini pour **column_set** colonnes.|  
|IColumsRowset::GetColumnsRowset|Une nouvelle valeur d’indicateur DBCOLUMNFLAGS, DBCOLUMNFLAGS_SS_ISCOLUMNSET, est définie pour **column_set** colonnes dans DBCOLUMN_FLAGS.<br /><br /> DBCOLUMN_COMPUTEMODE est défini à DBCOMPUTEMODE_DYNAMIC pour **column_set** colonnes.|  
|IDBSchemaRowset::GetSchemaRowset|DBSCHEMA_COLUMNS retourne deux nouvelles colonnes : SS_IS_COLUMN_SET et SS_IS_SPARSE.<br /><br /> DBSCHEMA_COLUMNS retourne uniquement les colonnes qui ne sont pas membres d’un **column_set**.<br /><br /> Deux nouveaux ensembles de lignes de schéma ont été ajoutés : DBSCHEMA_COLUMNS_EXTENDED retourne toutes les colonnes, quelle que soit la fragmentation de **column_set** l’appartenance. DBSCHEMA_SPARSE_COLUMN_SET retourne uniquement les colonnes qui sont membres d’un **column_set**. Ces nouveaux ensembles de lignes ont les mêmes colonnes et restrictions que DBSCHEMA_COLUMNS.|  
|IDBSchemaRowset::GetSchemas|IDBSchemaRowset::GetSchemas inclut les GUID des nouveaux ensembles de lignes DBSCHEMA_COLUMNS_EXTENDED et DBSCHEMA_SPARSE_COLUMN_SET dans la liste des ensembles de lignes de schéma disponibles.|  
|ICommand::Execute|Si **sélectionnez \* de** *table* est utilisé, il retourne toutes les colonnes qui ne sont pas membres d’éparse **column_set**, plus une colonne XML qui contient des valeurs de toutes les colonnes non null qui sont membres du éparse **column_set**, le cas échéant.|  
|IOpenRowset::OpenRowset|IOpenRowset::OpenRowset retourne un ensemble de lignes avec les mêmes colonnes que ICommand::Execute, avec un **sélectionnez \***  requête sur la même table.|  
|ITableDefinition|Il n’existe aucune modification à cette interface pour les colonnes éparses ou **column_set** colonnes. Les applications qui doivent effectuer des modifications de schéma doivent exécuter le [!INCLUDE[tsql](../../../includes/tsql-md.md)] approprié directement.|  
  
## <a name="see-also"></a>Voir aussi  
 [Programmation OLE DB Driver pour SQL Server](../../oledb/ole-db/oledb-driver-for-sql-server-programming.md)  
  
  
