---
title: Accéder aux schémas d’inventaire (AccessToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-access
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
helpviewer_keywords:
- columns table
- databases table
- exported metadata
- exporting, schemas of exported metadata
- foreign keys table
- forms table
- indexes, table
- inventory schemas
- macros table
- modules table
- queries table
- reference, inventory schemas
- reports table
- schemas of exported metadata
- schemas, exported metadata
- SSMA_Access_InventoryColumns
- SSMA_Access_InventoryDatabases
- SSMA_Access_InventoryForeignKeys
- SSMA_Access_InventoryForms
- SSMA_Access_InventoryIndexes
- SSMA_Access_InventoryMacros
- SSMA_Access_InventoryModules
- SSMA_Access_InventoryQueries
- SSMA_Access_InventoryReports
- SSMA_Access_InventoryTables
- tables, inventory
ms.assetid: fdd3cff2-4d62-4395-8acf-71ea8f17f524
caps.latest.revision: 17
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: db3476f35a5388d127d34ebb183364e0f63184f3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="access-inventory-schemas-accesstosql"></a>Accès aux schémas de stock (AccessToSQL)
Les sections suivantes décrivent les tables qui sont créés par SSMA lorsque vous exportez des schémas d’accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
## <a name="databases"></a>Bases de données  
Métadonnées de la base de données sont exportées vers le **SSMA_Access_InventoryDatabases** table. Cette table contient les colonnes suivantes :  
  
|Nom de la colonne|Type de données| Description|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|GUID qui identifie de façon unique chaque base de données. Cette colonne est également la clé primaire pour la table.|  
|**DatabaseName**|**nvarchar(4000)**|Le nom de la base de données Access.|  
|**ExportTime**|**datetime**|Date et heure de que création de ces métadonnées par SSMA.|  
|**Chemin d’accès**|**nvarchar(4000)**|Chemin d’accès et nom complet de la base de données Access.|  
|**FileSize**|**bigint**|La taille de la base de données Access en Ko.|  
|**FileOwner**|**nvarchar(4000)**|Le compte Windows qui est spécifié comme le propriétaire de la base de données Access.|  
|**DateCreated**|**datetime**|Date et heure de que création de la base de données Access.|  
|**DateModified**|**datetime**|Date et heure de que dernière modification de la base de données Access.|  
|**TablesCount**|**int**|Le nombre de tables dans la base de données Access.|  
|**QueriesCount**|**int**|Le nombre de requêtes dans la base de données Access.|  
|**FormsCount**|**int**|Le nombre de formulaires dans la base de données Access.|  
|**ModulesCount**|**int**|Le nombre de modules dans la base de données Access.|  
|**ReportsCount**|**int**|Le nombre de rapports dans la base de données Access.|  
|**MacrosCount**|**int**|Le nombre de macros dans la base de données Access.|  
|**AccessVersion**|**nvarchar(4000)**|La version de l’accès de la base de données.|  
|**Classement**|**nvarchar(4000)**|Le classement de la base de données Access. Les classements déterminent comment une base de données trie et compare les chaînes.|  
|**JetVersion**|**nvarchar(4000)**|La version du moteur de base de données Jet. Bases de données Access utilisent le moteur de base de données Jet sous-jacent.|  
|**IsUpdatable**|**bit**|Indique si la base de données peut être mis à jour. Si la valeur est 1, la base de données est modifiable. Si la valeur est 0, la base de données est en lecture seule.|  
|**QueryTimeout**|**int**|Valeur du délai d’attente de la requête ODBC configurée pour la base de données, en secondes. La valeur par défaut est 60 secondes.|  
  
## <a name="tables"></a>Tables  
Métadonnées de la table sont exportée vers le **SSMA_Access_InventoryTables** table. Cette table contient les colonnes suivantes :  
  
|Nom de la colonne|Type de données| Description|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|Identifie la base de données qui contient cette table.|  
|**TableId**|**uniqueidentifier**|GUID qui identifie de façon unique la table. Cette colonne est également la clé primaire pour la table.|  
|**TableName**|**nvarchar(4000)**|Nom de la table.|  
|**RowsCount**|**int**|Nombre de lignes dans la table.|  
|**ValidationRule**|**nvarchar(4000)**|La règle qui définit une entrée valide pour la table. S’il n’existe aucune règle de validation, le champ contient une chaîne vide.|  
|**LinkedTable**|**nvarchar(4000)**|Une autre table, si elle existe, qui est liée à la table. Lier des tables permet ajouts, suppressions et mises à jour à l’autre table à l’aide de cette table.|  
|**ExternalSource**|**nvarchar(4000)**|La source de données, si elle existe, qui est associé à la table. Si une table est liée, il a une source de données externe spécifiée dans ce champ.|  
  
## <a name="columns"></a>Columns  
Métadonnées de colonne sont exportée vers le **SSMA_Access_InventoryColumns** table. Cette table contient les colonnes suivantes :  
  
|Nom de la colonne|Type de données| Description|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|Identifie la base de données qui contient cette colonne.|  
|**TableId**|**uniqueidentifier**|Identifie la table qui contient cette colonne.|  
|**ColumnId**|**int**|Nombre s’incrémentant entier qui identifie la colonne. **ColumnId** est la clé primaire pour la table.|  
|**ColumnName**|**nvarchar(4000)**|Nom de la colonne.|  
|**IsNullable**|**bit**|Spécifie si la colonne peut contenir des valeurs null. Si la valeur est 1, la colonne peut contenir des valeurs null. Si la valeur est 0, la colonne ne peut pas contenir de valeurs null. Notez que la règle de validation peut également être utilisée pour éviter les valeurs null.|  
|**DataType**|**nvarchar(4000)**|Type de données Access de la colonne, tel que **texte** ou **Long**.|  
|**IsAutoIncrement**|**bit**|Spécifie si la colonne s’incrémente automatiquement des valeurs entières. Si la valeur est 1, les entiers sont à incrémentation automatique.|  
|**Ordinal**|**smallint**|L’ordre de la colonne dans la table, en commençant à zéro.|  
|**defaultValue**|**nvarchar(4000)**|Valeur par défaut de la colonne.|  
|**ValidationRule**|**nvarchar(4000)**|La règle est utilisée pour valider des données ajoutés ou mis à jour dans la colonne.|  
  
## <a name="indexes"></a>Index  
Métadonnées de l’index sont exportée vers le **SSMA_Access_InventoryIndexes** table. Cette table contient les colonnes suivantes :  
  
|Nom de la colonne|Type de données| Description|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|Identifie la base de données qui contient cet index.|  
|**TableId**|**uniqueidentifier**|Identifie la table qui contient cet index.|  
|**IndexId**|**int**|Entier incrémentation qui identifie l’index. Cette colonne est la clé primaire pour la table.|  
|**IndexName**|**nvarchar(4000)**|Le nom de l’index.|  
|**ColumnsIncluded**|**nvarchar(4000)**|Répertorie les colonnes qui sont incluses dans l’index. Les noms de colonnes sont séparées par un point-virgule.|  
|**IsUnique**|**bit**|Spécifie si chaque élément dans l’index doit être unique. Un index de plusieurs colonnes, la combinaison de valeurs doit être unique. Si la valeur est 1, l’index applique des valeurs uniques.|  
|**IsPK**|**bit**|Spécifie si l’index a été automatiquement créé en tant que partie de la définition de la clé primaire.|  
|**IsClustered**|**bit**|Spécifie si l’index est groupé. Un index cluster réorganise le stockage physique des données. Une table peut avoir qu’un seul index cluster.|  
  
## <a name="foreign-keys"></a>Clés étrangères  
Les métadonnées de clé étrangère sont exportée vers le **SSMA_Access_InventoryForeignKeys** table. Cette table contient les colonnes suivantes :  
  
|Nom de la colonne|Type de données| Description|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|Identifie la base de données qui contient cette clé FOREIGN KEY.|  
|**TableId**|**uniqueidentifier**|Identifie la table qui contient cette clé FOREIGN KEY.|  
|**ForeignKeyId**|**int**|Nombre s’incrémentant entier qui identifie la clé étrangère. Cette colonne est la clé primaire pour la table.|  
|**ForeignKeyName**|**nvarchar(4000)**|Le nom de l’index.|  
|**ReferencedTableId**|**uniqueidentifier**|Identifie la table qui contient les colonnes de la source.|  
|**SourceColumns**|**nvarchar(4000)**|Répertorie l’ou les colonnes clés étrangères.|  
|**ReferencedColumns**|**nvarchar(4000)**|Répertorie la colonne de clé primaire ou les colonnes qui sont référencées par la clé étrangère.|  
|**IsCascadeForUpdate**|**bit**|Spécifie que si la valeur de clé primaire est mise à jour, toutes les lignes qui font référence à cette valeur de clé sont également mis à jour.|  
|**IsCascadeForDelete**|**bit**|Spécifie que si la valeur de clé primaire est supprimée, toutes les lignes qui font référence à cette valeur de clé sont également supprimées.|  
|**IsEnforced**|**bit**|Spécifie que la contrainte de clé étrangère est appliquée.|  
  
## <a name="queries"></a>Requêtes  
Métadonnées de la requête sont exportée vers le **SSMA_Access_InventoryQueries** table. Cette table contient les colonnes suivantes :  
  
|Nom de la colonne|Type de données| Description|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|Identifie la base de données qui contient cette requête.|  
|**QueryId**|**int**|Nombre s’incrémentant entier qui identifie la requête. Cette colonne est la clé primaire pour la table.|  
|**QueryName**|**nvarchar(4000)**|Le nom de la requête.|  
|**QueryText**|**nvarchar(4000)**|Le code de requête SQL, telle qu’une instruction SELECT.|  
|**IsUpdateable**|**bit**|Spécifie si la requête est modifiable ou en lecture seule.|  
|**QueryType**|**nvarchar(4000)**|Spécifie le type de requête, tel que **sélectionnez** ou **SetOperation**.|  
|**ExternalSource**|**nvarchar(4000)**|Si la requête fait référence à une source de données externe, il est la chaîne de connexion utilisée par la requête.|  
  
## <a name="forms"></a>formulaires  
Métadonnées d’un formulaire sont exportée vers le **SSMA_Access_InventoryForms** table. Cette table contient les colonnes suivantes :  
  
|Nom de la colonne|Type de données| Description|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|Identifie la base de données qui contient ce formulaire.|  
|**FormId**|**int**|Nombre s’incrémentant entier qui identifie le formulaire. Cette colonne est la clé primaire pour la table.|  
|**FormName**|**nvarchar(4000)**|Nom du formulaire.|  
  
## <a name="macros"></a>Macros  
Métadonnées de la macro sont exportée vers le **SSMA_Access_InventoryMacros** table. Cette table contient les colonnes suivantes :  
  
|Nom de la colonne|Type de données| Description|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|Identifie la base de données qui contient la macro.|  
|**MacroId**|**int**|Nombre s’incrémentant entier qui identifie la macro. Cette colonne est la clé primaire pour la table.|  
|**Nom_macro**|**nvarchar(4000)**|Le nom de la macro.|  
  
## <a name="reports"></a>Rapports  
Métadonnées du rapport sont exportée vers le **SSMA_Access_InventoryReports** table. Cette table contient les colonnes suivantes :  
  
|Nom de la colonne|Type de données| Description|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|Identifie la base de données qui contient le rapport.|  
|**Identificateur ReportId**|**int**|Entier incrémentation qui identifie le rapport. Cette colonne est la clé primaire pour la table.|  
|**ReportName**|**nvarchar(4000)**|Nom du rapport.|  
  
## <a name="modules"></a>Modules  
Métadonnées du module sont exportées vers le **SSMA_Access_InventoryModules** table. Cette table contient les colonnes suivantes :  
  
|Nom de la colonne|Type de données| Description|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|Identifie la base de données qui contient le module.|  
|**ModuleId**|**int**|Nombre s’incrémentant entier qui identifie le module. Cette colonne est la clé primaire pour la table.|  
|**nom du module**|**nvarchar(4000)**|Le nom du module.|  
  
## <a name="see-also"></a>Voir aussi  
[Exportation d’un inventaire Access](http://msdn.microsoft.com/en-us/7e1941fb-3d14-4265-aff6-c77a4026d0ed)  
  
