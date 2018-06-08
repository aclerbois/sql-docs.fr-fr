---
title: La prise en charge des colonnes éparses dans le pilote OLE DB pour SQL Server | Documents Microsoft
description: Prise en charge des colonnes éparses dans le pilote OLE DB pour SQL Server
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: oledb|features
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- sparse columns, OLE DB Driver for SQL Server
- sparse columns, OLE DB
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: 616dfb431456099749250c7cc61540f6b36e7926
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sparse-columns-support-in-ole-db-driver-for-sql-server"></a>Prise en charge des colonnes éparses dans le pilote OLE DB pour SQL Server
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Pilote OLE DB pour SQL Server prend en charge les colonnes éparses. Pour plus d’informations sur les colonnes éparses dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], consultez [utiliser des colonnes éparses](../../../relational-databases/tables/use-sparse-columns.md) et [jeux de colonnes utilisez](../../../relational-databases/tables/use-column-sets.md).  
  
 Pour plus d’informations sur la prise en charge des colonnes éparses dans le pilote OLE DB pour SQL Server, [prend en charge les colonnes éparses &#40;OLE DB&#41;](../../oledb/ole-db/sparse-columns-support-ole-db.md).  
  
 Pour plus d’informations sur les exemples d’applications qui illustrent cette fonctionnalité, consultez [exemples de programmation de données SQL Server](http://msftdpprodsamples.codeplex.com/).  
  
## <a name="user-scenarios-for-sparse-columns-and-ole-db-driver-for-sql-server"></a>Scénarios utilisateur pour les colonnes éparses et le pilote OLE DB SQL Server  
 Le tableau suivant résume les scénarios utilisateur courants pour le pilote OLE DB pour les utilisateurs de SQL Server avec les colonnes éparses :  
  
|Scénario|Comportement|  
|--------------|--------------|  
|**Sélectionnez \* à partir de la table** ou IOpenRowset::OpenRowset.|Retourne toutes les colonnes qui ne sont pas membres d’éparse **column_set**, plus une colonne XML qui contient les valeurs de toutes les colonnes non null qui sont membres du éparse **column_set**.|  
|Référencer une colonne par nom.|La colonne peut être référencée quel que soit son état de la colonne éparse ou **column_set** l’appartenance.|  
|Accès **column_set** des colonnes de membre à une colonne XML calculée.|Colonnes qui sont membres d’éparse **column_set** est accessible en sélectionnant le **column_set** par nom et peuvent avoir les valeurs insérées et mises à jour en mettant à jour le code XML dans le **column_set** colonne.<br /><br /> La valeur doit être conforme au schéma pour **column_set** colonnes.|  
|Récupérer les métadonnées pour toutes les colonnes dans une table via l’ensemble de lignes de schéma DBSCHEMA_COLUMNS sans aucune restriction de colonne (OLE DB).|Retourne une ligne pour toutes les colonnes qui ne sont pas membres d’un **column_set**. Si la table possède un éparses **column_set**, une ligne sera retournée pour celui-ci.<br /><br /> Notez que cela ne retourne pas de métadonnées pour les colonnes qui sont membres d’un **column_set**.|  
|Récupérer des métadonnées pour toutes les colonnes, quel que soit le caractère éparse ou l’appartenance à un **column_set**. De très nombreuses lignes peuvent alors être retournées.|L’appel IDBSchemaRowset::GetRowset pour l’ensemble de lignes de schéma DBSCHEMA_COLUMNS_EXTENDED.|  
|Récupérer les métadonnées uniquement pour les colonnes qui sont membres d’un **column_set**. De très nombreuses lignes peuvent alors être retournées.|L’appel IDBSchemaRowset::GetRowset pour l’ensemble de lignes de schéma DBSCHEMA_SPARSE_COLUMN_SET.|  
|Déterminer si une colonne est éparse.|Consultez la colonne SS_IS_SPARSE de l'ensemble de lignes de schéma DBSCHEMA_COLUMNS (OLE DB).|  
|Déterminer si une colonne est un **column_set**.|Consultez la colonne SS_IS_COLUMN_SET de l'ensemble de lignes de schéma DBSCHEMA_COLUMNS Ou consultez *dwFlags* retournée par IColumnsinfo::GetColumnInfo ou bien DBCOLUMNFLAGS dans l’ensemble de lignes retourné par IColumnsRowset::GetColumnsRowset. Pour **column_set** colonnes, DBCOLUMNFLAGS_SS_ISCOLUMNSET est défini.|  
|Importer et exporter des colonnes éparses par l’utilitaire BCP pour une table sans aucune **column_set**.|Aucun changement de comportement des versions précédentes du pilote OLE DB pour SQL Server.|  
|Importer et exporter des colonnes éparses par l’utilitaire BCP pour une table avec un **column_set**.|Le **column_set** est importé et exporté de la même manière au format XML ; autrement dit, en tant que **varbinary (max)** si lié sous la forme d’un type binaire ou **nvarchar (max)** si liées en tant qu’un **char** ou **wchar** type.<br /><br /> Colonnes qui sont membres d’éparse **column_set** ne sont pas exportés en tant que colonnes distinctes ; elles sont uniquement exportées dans la valeur de la **column_set**.|  
|**queryout** comportement pour BCP.|Aucune modification de la gestion des colonnes explicitement nommées depuis les versions précédentes du pilote OLE DB pour SQL Server.<br /><br /> Les scénarios impliquant des opérations d'importation et d'exportation entre les tables avec des schémas différents peuvent nécessiter une gestion spéciale.<br /><br /> Pour plus d'informations sur BCP, consultez la section « Prise en charge de la copie en bloc (BCP) pour les colonnes éparses » plus loin dans cette rubrique.|  
  
## <a name="down-level-client-behavior"></a>Comportement de client de bas niveau  
 Les clients de bas niveau retournent des métadonnées uniquement pour les colonnes qui ne sont pas membres d’éparse **column_set** pour SQLColumns et DBSCHMA_COLUMNS.
  
 Les clients de bas niveau peuvent accéder à des colonnes qui sont membres du éparse **column_set** par nom et le **column_set** colonne sera accessible sous la forme d’une colonne XML à [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] les clients.  
  
## <a name="bulk-copy-bcp-support-for-sparse-columns"></a>Prise en charge de la copie en bloc (BCP) pour les colonnes éparses  
 Aucune modification à l’API BCP dans OLE DB pour les colonnes éparses ou **column_set** fonctionnalités.  
  
 Si une table possède un **column_set**, les colonnes éparses ne sont pas gérées en tant que colonnes distinctes. Les valeurs de toutes les colonnes éparses sont incluses dans la valeur de la **column_set**, qui est exporté dans la même façon que d’une colonne XML ; autrement dit, en tant que **varbinary (max)** si lié sous la forme d’un type binaire ou **nvarchar (max)** si liées en tant qu’un **char** ou **wchar** type). Lors de l’importation, le **column_set** valeur doit être conforme au schéma de la **column_set**.  
  
 Pour **queryout** opérations, il n’existe aucune modification à la façon dont les colonnes explicitement référencées sont gérées. **column_set** colonnes ont le même comportement que les colonnes XML et le caractère éparse n’a aucun effet sur la gestion des colonnes éparses nommées.  
  
 Toutefois, si **queryout** est utilisé pour l’exportation et que vous référencez des colonnes éparses qui sont membres du jeu par nom de colonnes éparses, vous ne pouvez effectuer aucune importation directe dans une table de structure similaire. Effet, BCP utilise des métadonnées cohérentes avec une **sélectionnez \***  opération pour l’importation et ne peut pas correspondre à **column_set** colonnes membres avec ces métadonnées. Pour importer **column_set** colonnes membres individuellement, vous devez définir une vue sur la table qui fait référence à l’élément **column_set** colonnes et que vous devez effectuer l’opération d’importation à l’aide de la vue.  
  
## <a name="see-also"></a>Voir aussi  
 [OLE DB Driver pour SQL Server](../../oledb/oledb-driver-for-sql-server.md)  
  
  
