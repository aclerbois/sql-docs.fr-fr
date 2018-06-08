---
title: Sys.fn_listextendedproperty (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: system-functions
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- fn_listextendedproperty
- fn_listextendedproperty_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- fn_listextendedproperty function
- displaying extended properties
- database extended properties [SQL Server]
- viewing extended properties
- column extended properties [SQL Server]
- sys.fn_listextendedproperties function
- database objects [SQL Server], extended properties
- extended properties [SQL Server], columns
- table extended properties [SQL Server]
ms.assetid: 59bbb91f-a277-4a35-803e-dcb91e847a49
caps.latest.revision: 32
author: rothja
ms.author: jroth
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 3484c32c00c5f94f084cd5c0e49837181054df40
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysfnlistextendedproperty-transact-sql"></a>sys.fn_listextendedproperty (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Retourne les valeurs de propriétés étendues d'objets de base de données.  
 
 
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn_listextendedproperty (   
    { default | 'property_name' | NULL }   
  , { default | 'level0_object_type' | NULL }   
  , { default | 'level0_object_name' | NULL }   
  , { default | 'level1_object_type' | NULL }   
  , { default | 'level1_object_name' | NULL }   
  , { default | 'level2_object_type' | NULL }   
  , { default | 'level2_object_name' | NULL }   
  )   
```  
  
## <a name="arguments"></a>Arguments  
 {valeur par défaut | '*property_name*' | NULL}  
 Est le nom de la propriété. *property_name* est **sysname**. Les entrées autorisées sont les valeurs par défaut, NULL, ou un nom de propriété.  
  
 {valeur par défaut | '*level0_object_type*' | NULL}  
 Type défini par l'utilisateur ou utilisateur. *level0_object_type* est **varchar (128)**, avec NULL comme valeur par défaut. Les entrées valides sont ASSEMBLY, CONTRACT, EVENT NOTIFICATION, FILEGROUP, MESSAGE TYPE, PARTITION FUNCTION, PARTITION SCHEME, REMOTE SERVICE BINDING, ROUTE, SCHEMA, SERVICE, USER, TRIGGER, TYPE, et NULL.  
  
> [!IMPORTANT]  
>  Les types de niveau 0 USER et TYPE seront éliminés dans une version ultérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Évitez d'utiliser ces fonctionnalités dans une nouvelle tâche de développement et prévoyez de modifier les applications qui les utilisent actuellement. À la place de USER, utilisez SCHEMA en tant que type de niveau 0. Pour TYPE, utilisez SCHEMA comme type de niveau 0 et TYPE comme type de niveau 1.  
  
 {valeur par défaut | '*level0_object_name*' | NULL}  
 Nom du type d'objet de niveau 0 spécifié. *level0_object_name* est **sysname** avec NULL comme valeur par défaut. Les entrées autorisées sont les valeurs par défaut, NULL, ou un nom d'objet.  
  
 {valeur par défaut | '*level1_object_type*' | NULL}  
 Type d'objet de niveau 1. *level1_object_type* est **varchar (128)** avec NULL comme valeur par défaut. Les entrées valides sont AGGREGATE, DEFAULT, FUNCTION, LOGICAL FILE NAME, PROCEDURE, QUEUE, RULE, SYNONYM, TABLE, TYPE, VIEW, XML SCHEMA COLLECTION et NULL.  
  
> [!NOTE]  
>  La valeur par défaut correspond à la valeur NULL, et la valeur 'default' est mappée sur le type d'objet DEFAULT.  
  
 {valeur par défaut | '*level1_object_name*' | NULL}  
 Nom du type d'objet de niveau 1 spécifié. *level1_object_name* est **sysname** avec NULL comme valeur par défaut. Les entrées autorisées sont les valeurs par défaut, NULL, ou un nom d'objet.  
  
 {valeur par défaut | '*level2_object_type*' | NULL}  
 Type d'objet de niveau 2. *level2_object_type* est **varchar (128)** avec NULL comme valeur par défaut. Les entrées valides sont DEFAULT, la valeur par défaut (qui correspond à la valeur NULL) et NULL. Les entrées valides pour *level2_object_type* sont des colonnes, contrainte, EVENT NOTIFICATION, INDEX, paramètre, TRIGGER et NULL.  
  
 {valeur par défaut | '*level2_object_name*' | NULL}  
 Nom du type d'objet de niveau 2 spécifié. *level2_object_name* est **sysname** avec NULL comme valeur par défaut. Les entrées autorisées sont les valeurs par défaut, NULL, ou un nom d'objet.  
  
## <a name="tables-returned"></a>Tables retournées  
 Le format des tables renvoyées par l'instruction fn_listextendedproperty est le suivant :  
  
|Nom de colonne|Type de données|  
|-----------------|---------------|  
|objtype|**sysname**|  
|objname|**sysname**|  
|name|**sysname**|  
|valeur|**sql_variant**|  
  
 Si la table renvoyée est vide, l'objet ne dispose pas de propriétés étendues, ou l'utilisateur n'est pas habilité à afficher la liste des propriétés étendues associées à cet objet. Lorsque les propriétés étendues de la base de données proprement dite sont retournées, les colonnes objtype et objname ont la valeur NULL.  
  
## <a name="remarks"></a>Notes  
 Si la valeur de *property_name* est NULL ou la valeur par défaut, fn_listextendedproperty retourne toutes les propriétés de l’objet spécifié.  
  
 Lorsque le type d'objet est spécifié et que la valeur du nom d'objet correspondant est NULL ou la valeur par défaut, fn_listextendedproperty retourne toutes les propriétés étendues de tous les objets du type spécifié.  
  
 Les objets se distinguent selon des niveaux : le niveau 0 étant le plus élevé et le niveau 2 le moins élevé. Si un type et un nom d'objet de niveau inférieur (niveau 1 ou 2) sont spécifiés, le type et le nom de l'objet parent ne doivent pas comporter la valeur NULL ou la valeur par défaut. Autrement, la fonction renvoie un jeu de résultats vide.  
  
 **objname** Latin1_General_CI_AI est fixe. Toutefois, vous pouvez contourner ce en substituant le classement dans la comparaison.  
  
```  
SELECT o.[object_id] AS 'table_id', o.[name] 'table_name',  
0 AS 'column_order', NULL AS 'column_name', NULL AS 'column_datatype',  
NULL AS 'column_length', Cast(e.value AS varchar(500)) AS 'column_description'  
FROM AdventureWorks.sys.objects AS o  
LEFT JOIN sys.fn_listextendedproperty(N'MS_Description', N'user',N'HumanResources',N'table', N'Employee', null, default) AS e  
    ON o.name = e.objname COLLATE SQL_Latin1_General_CP1_CI_AS  
WHERE o.name = 'Employee';  
```  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations d'affichage des propriétés étendues des objets peuvent varier selon le type d'objet.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-displaying-extended-properties-on-a-database"></a>A. Affichage des propriétés étendues d'une base de données  
 L'exemple suivant affiche toutes les propriétés étendues de l'objet de base de données lui-même.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT objtype, objname, name, value  
FROM fn_listextendedproperty(default, default, default, default, default, default, default);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `objtype    objname     name            value`  
  
 `---------  ---------   -----------     ----------------------------`  
  
 `NULL       NULL        MS_Description  AdventureWorks2008 Sample OLTP Database`  
  
 `(1 row(s) affected)`  
  
### <a name="b-displaying-extended-properties-on-all-columns-in-a-table"></a>B. Affichage des propriétés étendues de toutes les colonnes d'une table  
 L’exemple suivant répertorie les propriétés étendues des colonnes dans le `ScrapReason` table. Celle-ci est contenue dans le schéma `Production`.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT objtype, objname, name, value  
FROM fn_listextendedproperty (NULL, 'schema', 'Production', 'table', 'ScrapReason', 'column', default);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `objtype objname      name            value`  
  
 `------- -----------  -------------   ------------------------`  
  
 `COLUMN ScrapReasonID MS_Description  Primary key for ScrapReason records.`  
  
 `COLUMN Name          MS_Description  Failure description.`  
  
 `COLUMN ModifiedDate  MS_Description  Date the record was last updated.`  
  
 `(3 row(s) affected)`  
  
### <a name="c-displaying-extended-properties-on-all-tables-in-a-schema"></a>C. Affichage des propriétés étendues de toutes les tables d'un schéma  
 L’exemple suivant répertorie les propriétés étendues de toutes les tables contenues dans le `Sales` schéma.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT objtype, objname, name, value  
FROM fn_listextendedproperty (NULL, 'schema', 'Sales', 'table', default, NULL, NULL);  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_addextendedproperty &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql.md)   
 [sp_dropextendedproperty &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropextendedproperty-transact-sql.md)   
 [sp_updateextendedproperty &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-updateextendedproperty-transact-sql.md)   
 [Sys.extended_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/extended-properties-catalog-views-sys-extended-properties.md)  
  
  
