---
title: COLONNES (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- COLUMNS
- COLUMNS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- COLUMNS view
- INFORMATION_SCHEMA.COLUMNS view
ms.assetid: bbf7ac4a-7444-4351-a590-a9f71e0bc495
caps.latest.revision: 48
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 54467131c2360d961421dd90b43024febbf06883
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="columns-transact-sql"></a>COLUMNS (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Renvoie une ligne pour chaque colonne accessible à l'utilisateur actuel dans la base de données actuelle.  
  
 Pour récupérer des informations à partir de ces vues, spécifiez le nom qualifié complet de **INFORMATION_SCHEMA ***.** view_name *.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**TABLE_CATALOG**|**nvarchar(** 128 **)**|Qualificateur de la table.|  
|**TABLE_SCHEMA**|**nvarchar(** 128 **)**|Nom du schéma qui contient la table.<br /><br /> **\*\* Important \* \***  n’utilisez pas les vues INFORMATION_SCHEMA pour déterminer le schéma d’un objet. La seule méthode fiable pour rechercher le schéma d’un objet est d’interroger l’affichage catalogue sys.objects.|  
|**NOM_TABLE**|**nvarchar(** 128 **)**|Nom de la table.|  
|**COLUMN_NAME**|**nvarchar(** 128 **)**|Nom de colonne.|  
|**ORDINAL_POSITION**|**int**|Numéro d’identification de la colonne.|  
|**COLUMN_DEFAULT**|**nvarchar (** 4000 **)**|Valeur par défaut de la colonne.|  
|**IS_NULLABLE**|**varchar (** 3 **)**|Valeur NULL possible dans la colonne. Si cette colonne accepte des valeurs NULL, elle renvoie YES. Dans le cas contraire, elle renvoie NO.|  
|**DATA_TYPE**|**nvarchar(** 128 **)**|Type de données fourni par le système.|  
|**CHARACTER_MAXIMUM_LENGTH**|**int**|Longueur maximale (en caractères) des données de type binaire, caractère, texte et image.<br /><br /> -1 pour **xml** et les données de type de valeur élevée. Renvoie NULL dans les autres cas. Pour plus d’informations, consultez [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md).|  
|**CHARACTER_OCTET_LENGTH**|**int**|Longueur maximale, en octets, des données de type binaire, caractère, texte et image.<br /><br /> -1 pour **xml** et les données de type de valeur élevée. Renvoie NULL dans les autres cas.|  
|**NUMERIC_PRECISION**|**tinyint**|Précision des données numériques approchées ou exactes, des données de type entier ou monétaire. Renvoie NULL dans les autres cas.|  
|**NUMERIC_PRECISION_RADIX**|**smallint**|Base de précision des données numériques approchées ou exactes, des données de type entier ou monétaire. Renvoie NULL dans les autres cas.|  
|**NUMERIC_SCALE**|**int**|Échelle des données numériques approchées ou exactes, des données de type entier ou monétaire. Renvoie NULL dans les autres cas.|  
|**DATETIME_PRECISION**|**smallint**|Code de sous-type pour **datetime** et ISO **intervalle** des types de données. Renvoie NULL pour les autres types de données.|  
|**CHARACTER_SET_CATALOG**|**nvarchar(** 128 **)**|Retourne **master**. Cela indique la base de données dans laquelle se trouve le jeu de caractères, si la colonne est de données de type caractère ou **texte** type de données. Renvoie NULL dans les autres cas.|  
|**CHARACTER_SET_SCHEMA**|**nvarchar(** 128 **)**|Retourne toujours la valeur Null.|  
|**CHARACTER_SET_NAME**|**nvarchar(** 128 **)**|Retourne le nom unique pour le jeu de caractères si cette colonne est de données de type caractère ou **texte** type de données. Renvoie NULL dans les autres cas.|  
|**COLLATION_CATALOG**|**nvarchar(** 128 **)**|Retourne toujours la valeur Null.|  
|**COLLATION_SCHEMA**|**nvarchar(** 128 **)**|Retourne toujours la valeur Null.|  
|**COLLATION_NAME**|**nvarchar(** 128 **)**|Retourne le nom unique pour le classement si la colonne est de données de type caractère ou **texte** type de données. Renvoie NULL dans les autres cas.|  
|**DOMAIN_CATALOG**|**nvarchar(** 128 **)**|Si la colonne est un type de données alias, elle correspond au nom de la base de données dans laquelle le type de données défini par l'utilisateur a été créé. Renvoie NULL dans les autres cas.|  
|**DOMAIN_SCHEMA**|**nvarchar(** 128 **)**|Si la colonne est un type de données défini par l'utilisateur, elle renvoie le nom du schéma du type de données défini par l'utilisateur. Renvoie NULL dans les autres cas.<br /><br /> **\*\* Important \* \***  n’utilisez pas les vues INFORMATION_SCHEMA pour déterminer le schéma d’un type de données. La seule méthode fiable pour rechercher le schéma d'un type est d'utiliser la fonction TYPEPROPERTY.|  
|**NOM_DOMAINE**|**nvarchar(** 128 **)**|Si la colonne est un type de données défini par l'utilisateur, elle représente le nom du type de données défini par l'utilisateur. Renvoie NULL dans les autres cas.|  
  
## <a name="remarks"></a>Notes  
 Le **ORDINAL_POSITION** colonne de la **INFORMATION_SCHEMA. COLONNES** vue n’est pas compatible avec le modèle binaire des colonnes retournées par la fonction COLUMNS_UPDATED. Pour obtenir un modèle binaire qui est compatible avec COLUMNS_UPDATED, vous devez référencer le **ColumnID** propriété de la fonction système COLUMNPROPERTY lorsque vous interrogez le **INFORMATION_SCHEMA. COLONNES** vue. Par exemple :  
  
```  
USE AdventureWorks2012;  
GO  
SELECT TABLE_NAME, COLUMN_NAME, COLUMNPROPERTY(OBJECT_ID(TABLE_SCHEMA + '.' + TABLE_NAME), COLUMN_NAME, 'ColumnID') AS COLUMN_ID  
FROM AdventureWorks2012.INFORMATION_SCHEMA.COLUMNS  
WHERE TABLE_NAME = 'Person';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Vues système &#40;Transact-SQL&#41;](http://msdn.microsoft.com/library/35a6161d-7f43-4e00-bcd3-3091f2015e90)   
 [Vues de schémas d’informations &#40;Transact-SQL&#41;](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [Sys.syscharsets &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-syscharsets-transact-sql.md)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [sys.configurations &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.types &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-types-transact-sql.md)   
 [COLUMNS_UPDATED &#40;Transact-SQL&#41;](../../t-sql/functions/columns-updated-transact-sql.md)  
  
  
