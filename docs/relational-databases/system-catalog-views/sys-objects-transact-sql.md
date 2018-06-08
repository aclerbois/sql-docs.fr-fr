---
title: Sys.Objects (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 05/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.objects_TSQL
- objects
- sys.objects
- objects_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.objects catalog view
- table-valued parameters, sys.objects catalog view
- user-defined table types [SQL Server]
- table types [SQL Server]
ms.assetid: f8d6163a-2474-410c-a794-997639f31b3b
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: cfbf6fa606834c9582392635670b5f04fe9995a3
ms.sourcegitcommit: 02c889a1544b0859c8049827878d66b2301315f8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/17/2018
---
# <a name="sysobjects-transact-sql"></a>sys.objects (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Contient une ligne pour chaque objet défini par l’utilisateur, la portée de schéma qui est créé dans une base de données, y compris la fonction scalaire définie par l’utilisateur compilée en mode natif.  
  
 Pour plus d’informations, consultez [Fonctions scalaires définies par l’utilisateur pour l’OLTP en mémoire](../../relational-databases/in-memory-oltp/scalar-user-defined-functions-for-in-memory-oltp.md).  
  
> [!NOTE]  
>  sys.objects n'affiche pas les déclencheurs DDL car ceux-ci ne sont pas définis avec une étendue de schéma. Tous les déclencheurs, DML et DDL, se trouvent dans [sys.triggers](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md). Sys.Triggers prend en charge un ensemble de règles de portée de nom pour les différents types de déclencheurs.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|name|**sysname**|Nom de l'objet.|  
|object_id|**int**|Numéro d'identification de l'objet. Unique dans une base de données.|  
|principal_id|**int**|ID du propriétaire, si celui-ci est différent du propriétaire du schéma. Par défaut, le propriétaire du schéma détient les objets contenus dans le schéma. Cependant, il est possible de spécifier un autre propriétaire à l'aide de l'instruction ALTER AUTHORIZATION qui permet de changer de propriétaire.<br /><br /> Prend la valeur NULL en l'absence d'un autre propriétaire.<br /><br /> Prend la valeur NULL si le type de l'objet est un des types suivants :<br /><br /> C = contrainte CHECK<br /><br /> D = DEFAULT (contrainte ou autonome)<br /><br /> F = Contrainte FOREIGN KEY<br /><br /> PK = Contrainte PRIMARY KEY<br /><br /> R = Règle (ancienne, autonome)<br /><br /> TA = Déclencheur d'assembly (intégration CLR)<br /><br /> TR = Déclencheur SQL<br /><br /> UQ = Contrainte UNIQUE|  
|schema_id|**int**|ID du schéma dans lequel se trouve l'objet.<br /><br /> Les objets système compris dans l'étendue du schéma sont toujours contenus dans les schémas sys ou INFORMATION_SCHEMA.|  
|parent_object_id|**int**|Identificateur de l'objet auquel appartient cet objet.<br /><br /> 0 = Il ne s'agit pas d'un objet enfant.|  
|Type|**char(2)**|Type d’objet :<br /><br /> AF = Fonction d'agrégation (CLR)<br /><br /> C = contrainte CHECK<br /><br /> D = DEFAULT (contrainte ou autonome)<br /><br /> F = Contrainte FOREIGN KEY<br /><br /> FN = Fonction scalaire SQL<br /><br /> FS = Fonction scalaire d'assembly (CLR)<br /><br /> FT = Fonction table d'assembly (CLR)<br /><br /> IF = Fonction table en ligne SQL<br /><br /> IT = table interne<br /><br /> P = Procédure stockée SQL<br /><br /> PC = procédure stockée d’Assembly (CLR)<br /><br /> PG = Repère de plan<br /><br /> PK = Contrainte PRIMARY KEY<br /><br /> R = Règle (ancienne, autonome)<br /><br /> RF = Procédure de filtre de réplication<br /><br /> S = Table de base système<br /><br /> SN = Synonyme<br /><br /> SO = Objet séquence<br /><br /> U = Table (définie par l'utilisateur)<br /><br /> V = Vue<br /><br /> <br /><br /> **S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> <br /><br /> SQ = File d'attente du service<br /><br /> TA = Déclencheur d'assembly DML (CLR)<br /><br /> TF = SQL-fonction table<br /><br /> TR = Déclencheur DML SQL<br /><br /> TT = Type de table<br /><br /> UQ = Contrainte UNIQUE<br /><br /> X = Procédure stockée étendue<br /><br /> <br /><br /> **S’applique aux**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] via [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].<br /><br /> <br /><br /> O = Table externe|  
|type_desc|**nvarchar(60)**|Description du type d'objet :<br /><br /> AGGREGATE_FUNCTION<br /><br /> CHECK_CONSTRAINT<br /><br /> CLR_SCALAR_FUNCTION<br /><br /> CLR_STORED_PROCEDURE<br /><br /> CLR_TABLE_VALUED_FUNCTION<br /><br /> CLR_TRIGGER<br /><br /> DEFAULT_CONSTRAINT<br /><br /> EXTENDED_STORED_PROCEDURE<br /><br /> FOREIGN_KEY_CONSTRAINT<br /><br /> INTERNAL_TABLE<br /><br /> PLAN_GUIDE<br /><br /> PRIMARY_KEY_CONSTRAINT<br /><br /> REPLICATION_FILTER_PROCEDURE<br /><br /> RULE<br /><br /> SEQUENCE_OBJECT<br /><br /> <br /><br /> **S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> <br /><br /> SERVICE_QUEUE<br /><br /> SQL_INLINE_TABLE_VALUED_FUNCTION<br /><br /> SQL_SCALAR_FUNCTION<br /><br /> SQL_STORED_PROCEDURE<br /><br /> SQL_TABLE_VALUED_FUNCTION<br /><br /> SQL_TRIGGER<br /><br /> SYNONYM<br /><br /> SYSTEM_TABLE<br /><br /> TABLE_TYPE<br /><br /> UNIQUE_CONSTRAINT<br /><br /> USER_TABLE<br /><br /> VIEW|  
|create_date|**datetime**|Date de création de l'objet.|  
|modify_date|**datetime**|Date de la dernière modification de l'objet avec l'instruction ALTER. Si l'objet est une table ou une vue, modify_date change également lorsqu'un index cluster sur la table ou la vue est créé ou modifié.|  
|is_ms_shipped|**bit**|Un composant [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] interne a créé l'objet.|  
|is_published|**bit**|L'objet est publié.|  
|is_schema_published|**bit**|Seul le schéma de l'objet est publié.|  
  
## <a name="remarks"></a>Notes  
 Vous pouvez appliquer la [OBJECT_ID](../../t-sql/functions/object-id-transact-sql.md), [nom_objet](../../t-sql/functions/object-name-transact-sql.md), et [OBJECTPROPERTY](../../t-sql/functions/objectproperty-transact-sql.md)() des fonctions intégrées pour les objets affichés dans sys.objects.  
  
 Il existe une version de cette vue avec le même schéma, appelé [sys.system_objects](../../relational-databases/system-catalog-views/sys-system-objects-transact-sql.md), qui affiche les objets système. Il existe une autre vue appelée [sys.all_objects](../../relational-databases/system-catalog-views/sys-all-objects-transact-sql.md) qui affiche les objets système et utilisateur. Ces trois affichages catalogue ont la même structure.  
  
 Dans cette version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un index étendu, tel qu'un index XML ou un index spatial, est considéré comme une table interne dans sys.objects (type = IT et type_desc = INTERNAL_TABLE). Pour un index étendu :  
  
-   name correspond au nom interne de la table d'index.  
  
-   parent_object_id est le object_id de la table de base.  
  
-   Les colonnes is_ms_shipped, is_published et is_schema_published ont la valeur 0.  

**Vues système utiles connexes**  
Vous pouvez consulter des sous-ensembles des objets à l’aide de vues système pour un type spécifique d’objet, tel que :  
- [sys.tables](sys-tables-transact-sql.md)  
- [sys.views](sys-views-transact-sql.md)  
- [sys.procedures](sys-procedures-transact-sql.md)  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-returning-all-the-objects-that-have-been-modified-in-the-last-n-days"></a>A. Retour de tous les objets modifiés au cours des N derniers jours  
 Avant d'exécuter la requête suivante, remplacez `<database_name>` et `<n_days>` par des valeurs valides.  
  
```sql  
USE <database_name>;  
GO  
SELECT name AS object_name   
  ,SCHEMA_NAME(schema_id) AS schema_name  
  ,type_desc  
  ,create_date  
  ,modify_date  
FROM sys.objects  
WHERE modify_date > GETDATE() - <n_days>  
ORDER BY modify_date;  
GO  
```  
  
### <a name="b-returning-the-parameters-for-a-specified-stored-procedure-or-function"></a>B. Retour des paramètres d'une fonction ou d'une procédure stockée spécifique  
 Avant d'exécuter la requête suivante, remplacez `<database_name>` et `<schema_name.object_name>` par des noms valides.  
  
```sql  
USE <database_name>;  
GO  
SELECT SCHEMA_NAME(schema_id) AS schema_name  
    ,o.name AS object_name  
    ,o.type_desc  
    ,p.parameter_id  
    ,p.name AS parameter_name  
    ,TYPE_NAME(p.user_type_id) AS parameter_type  
    ,p.max_length  
    ,p.precision  
    ,p.scale  
    ,p.is_output  
FROM sys.objects AS o  
INNER JOIN sys.parameters AS p ON o.object_id = p.object_id  
WHERE o.object_id = OBJECT_ID('<schema_name.object_name>')  
ORDER BY schema_name, object_name, p.parameter_id;  
GO  
```  
  
### <a name="c-returning-all-the-user-defined-functions-in-a-database"></a>C. Retour de toutes les fonctions définies par l'utilisateur dans une base de données  
 Avant d'exécuter la requête suivante, remplacez `<database_name>` par un nom de base de données valide.  
  
```sql  
USE <database_name>;  
GO  
SELECT name AS function_name   
  ,SCHEMA_NAME(schema_id) AS schema_name  
  ,type_desc  
  ,create_date  
  ,modify_date  
FROM sys.objects  
WHERE type_desc LIKE '%FUNCTION%';  
GO  
```  
  
### <a name="d-returning-the-owner-of-each-object-in-a-schema"></a>D. Retour du propriétaire de chaque objet dans un schéma.  
 Avant d'exécuter la requête suivante, remplacez toutes les occurrences de `<database_name>` et `<schema_name>` par des noms valides.  
  
```sql  
USE <database_name>;  
GO  
SELECT 'OBJECT' AS entity_type  
    ,USER_NAME(OBJECTPROPERTY(object_id, 'OwnerId')) AS owner_name  
    ,name   
FROM sys.objects WHERE SCHEMA_NAME(schema_id) = '<schema_name>'  
UNION   
SELECT 'TYPE' AS entity_type  
    ,USER_NAME(TYPEPROPERTY(SCHEMA_NAME(schema_id) + '.' + name, 'OwnerId')) AS owner_name  
    ,name   
FROM sys.types WHERE SCHEMA_NAME(schema_id) = '<schema_name>'   
UNION  
SELECT 'XML SCHEMA COLLECTION' AS entity_type   
    ,COALESCE(USER_NAME(xsc.principal_id),USER_NAME(s.principal_id)) AS owner_name  
    ,xsc.name   
FROM sys.xml_schema_collections AS xsc JOIN sys.schemas AS s  
    ON s.schema_id = xsc.schema_id  
WHERE s.name = '<schema_name>';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Sys.all_objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-all-objects-transact-sql.md)   
 [sys.system_objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-system-objects-transact-sql.md)   
 [sys.triggers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md)   
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Interrogation des catalogues système SQL Server FAQ](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [Sys.internal_tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-internal-tables-transact-sql.md)  
  
  
