---
title: PARAMÈTRES (Transact-SQL) | Documents Microsoft
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
- PARAMETERS_TSQL
- PARAMETERS
dev_langs:
- TSQL
helpviewer_keywords:
- PARAMETERS view
- INFORMATION_SCHEMA.PARAMETERS view
ms.assetid: 06ded0ca-7d21-4400-864a-b801e855b257
caps.latest.revision: 35
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 9b73215f275f2171f72a70a71f1855d88d60c034
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="parameters-transact-sql"></a>PARAMETERS (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Retourne une ligne pour chaque paramètre d'une fonction définie par l'utilisateur ou d'une procédure stockée accessible à l'utilisateur actuel dans la base de données active. Pour les fonctions, cette vue renvoie également une ligne avec des informations sur les valeurs de retour.  
  
 Pour récupérer des informations à partir de ces vues, spécifiez le nom qualifié complet de **INFORMATION_SCHEMA. *** view_name*.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**SPECIFIC_CATALOG**|**nvarchar(** 128 **)**|Nom de catalogue de la routine pour laquelle cet élément constitue un paramètre|  
|**SPECIFIC_SCHEMA**|**nvarchar(** 128 **)**|Nom du schéma de la routine pour laquelle cet élément constitue un paramètre<br /><br /> **\*\* Important \* \***  n’utilisez pas les vues INFORMATION_SCHEMA pour déterminer le schéma d’un objet. La seule méthode fiable pour rechercher le schéma d’un objet est d’interroger l’affichage catalogue sys.objects.|  
|**SPECIFIC_NAME**|**nvarchar(** 128 **)**|Nom de la routine pour laquelle cet élément constitue un paramètre|  
|**ORDINAL_POSITION**|**int**|Position ordinale du paramètre en commençant à 1. Dans le cas de la valeur de retour d'une fonction, il s'agit d'un 0.|  
|**PARAMETER_MODE**|**nvarchar (** 10 **)**|Retourne IN pour un paramètre d'entrée, OUT pour un paramètre de sortie et INOUT pour un paramètre d'entrée/sortie.|  
|**IS_RESULT**|**nvarchar (** 10 **)**|Retourne YES s'il s'agit du résultat de la routine qui est une fonction. Dans le cas contraire, la valeur retournée est NO.|  
|**AS_LOCATOR**|**nvarchar (** 10 **)**|Retourne YES si l'élément est déclaré comme localisateur. Dans le cas contraire, la valeur retournée est NO.|  
|**NOM_PARAMÈTRE**|**nvarchar(** 128 **)**|Nom du paramètre. NULL si ceci correspond à la valeur retournée d'une fonction.|  
|**DATA_TYPE**|**nvarchar(** 128 **)**|Type de données fourni par le système.|  
|**CHARACTER_MAXIMUM_LENGTH**|**int**|Longueur maximale en caractères des données de type binaire ou caractère.<br /><br /> -1 pour **xml** et les données de type de valeur élevée. Dans le cas contraire, la valeur NULL est retournée.|  
|**CHARACTER_OCTET_LENGTH**|**int**|Longueur maximale en octets des données de type binaire ou caractère.<br /><br /> -1 pour **xml** et les données de type de valeur élevée. Dans le cas contraire, la valeur NULL est retournée.|  
|**COLLATION_CATALOG**|**nvarchar(** 128 **)**|Retourne toujours la valeur Null.|  
|**COLLATION_SCHEMA**|**nvarchar(** 128 **)**|Retourne toujours la valeur Null.|  
|**COLLATION_NAME**|**nvarchar(** 128 **)**|Nom du classement du paramètre. Retourne la valeur NULL si ce nom n'utilise pas l'un des types de caractères.|  
|**CHARACTER_SET_CATALOG**|**nvarchar(** 128 **)**|Nom de catalogue du jeu de caractères du paramètre. Retourne la valeur NULL si ce nom n'utilise pas l'un des types de caractères.|  
|**CHARACTER_SET_SCHEMA**|**nvarchar(** 128 **)**|Retourne toujours la valeur Null.|  
|**CHARACTER_SET_NAME**|**nvarchar(** 128 **)**|Nom du jeu de caractères du paramètre. Retourne la valeur NULL si ce nom n'utilise pas l'un des types de caractères.|  
|**NUMERIC_PRECISION**|**tinyint**|Précision des données numériques approchées ou exactes, des données de type entier ou monétaire. Dans le cas contraire, la valeur NULL est retournée.|  
|**NUMERIC_PRECISION_RADIX**|**smallint**|Base de précision des données numériques approchées ou exactes, des données de type entier ou monétaire. Dans le cas contraire, la valeur NULL est retournée.|  
|**NUMERIC_SCALE**|**tinyint**|Échelle des données numériques approchées ou exactes, des données de type entier ou monétaire. Dans le cas contraire, la valeur NULL est retournée.|  
|**DATETIME_PRECISION**|**smallint**|Précision en fractions de seconde si le type de paramètre est **datetime** ou **smalldatetime**. Dans le cas contraire, la valeur NULL est retournée.|  
|**INTERVAL_TYPE**|**nvarchar (** 30 **)**|NULL. Réservé pour un usage ultérieur.|  
|**INTERVAL_PRECISION**|**smallint**|NULL. Réservé pour un usage ultérieur.|  
|**USER_DEFINED_TYPE_CATALOG**|**nvarchar(** 128 **)**|NULL. Réservé pour un usage ultérieur.|  
|**USER_DEFINED_TYPE_SCHEMA**|**nvarchar(** 128 **)**|NULL. Réservé pour un usage ultérieur.|  
|**USER_DEFINED_TYPE_NAME**|**nvarchar(** 128 **)**|NULL. Réservé pour un usage ultérieur.|  
|**SCOPE_CATALOG**|**nvarchar(** 128 **)**|NULL. Réservé pour un usage ultérieur.|  
|**SCOPE_SCHEMA**|**nvarchar(** 128 **)**|NULL. Réservé pour un usage ultérieur.|  
|**SCOPE_NAME**|**nvarchar(** 128 **)**|NULL. Réservé pour un usage ultérieur.|  
  
## <a name="see-also"></a>Voir aussi  
 [Vues système &#40;Transact-SQL&#41;](http://msdn.microsoft.com/library/35a6161d-7f43-4e00-bcd3-3091f2015e90)   
 [Vues de schémas d’informations &#40;Transact-SQL&#41;](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [Sys.Parameters & #40 ; Transact-SQL & #41 ;](../../relational-databases/system-catalog-views/sys-parameters-transact-sql.md)  
  
  
