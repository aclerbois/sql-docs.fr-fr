---
title: sp_columns (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 10/17/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_columns_TSQL
- sp_columns
dev_langs:
- TSQL
helpviewer_keywords:
- sp_columns
ms.assetid: 2dec79cf-2baf-4c0f-8cbb-afb1a8654e1e
caps.latest.revision: 45
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 784811775a3364544e67d6591de203b091ab5402
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spcolumns-transact-sql"></a>MSdbms (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Retourne des informations de colonne pour les objets spécifiés qui peuvent être interrogés dans l'environnement actuel.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
-- Syntax for SQL Server, Azure SQL Database, Azure SQL Data Warehouse, Parallel Data Warehouse  
  
sp_columns [ @table_name = ] object  
     [ , [ @table_owner = ] owner ]   
     [ , [ @table_qualifier = ] qualifier ]   
     [ , [ @column_name = ] column ]   
     [ , [ @ODBCVer = ] ODBCVer ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@table_name=**] *objet*  
 Nom de l'objet utilisé pour retourner des informations de catalogue. *objet* peut être une table, vue ou un autre objet qui a des colonnes telles que les fonctions table. *objet* est **nvarchar (384)**, sans valeur par défaut. La recherche de correspondance avec des caractères génériques est prise en charge.  
  
 [  **@table_owner*** =**] *propriétaire*  
 Nom du propriétaire de l'objet utilisé pour retourner des informations de catalogue. *propriétaire* est **nvarchar (384)**, avec NULL comme valeur par défaut. La recherche de correspondance avec des caractères génériques est prise en charge. Si *propriétaire* n’est pas spécifié, les règles de visibilité d’objet par défaut du SGBD sous-jacent s’appliquent.  
  
 Si l'utilisateur actuel est propriétaire d'un objet portant le nom spécifié, les colonnes de cet objet sont retournées. Si *propriétaire* n’est pas spécifié et l’utilisateur actuel ne possède pas d’un objet avec l’objet *objet*, **sp_columns** recherche un objet avec l’objet *objet* appartenant au propriétaire de la base de données. S'il en existe un, les colonnes de cet objet sont retournées.  
  
 [  **@table_qualifier*** =**] *qualificateur*  
 Nom du qualificateur de l'objet. *qualificateur* est **sysname**, avec NULL comme valeur par défaut. Divers produits SGBD prennent en charge d’affectation de noms en trois parties pour les objets (*qualificateur ***.*** propriétaire ***.*** nom de*). Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette colonne représente le nom de la base de données. Dans certains produits, elle représente le nom du serveur de l'environnement de base de données de l'objet.  
  
 [  **@column_name=**] *colonne*  
 Colonne unique utilisée lorsqu'une seule colonne d'informations de catalogue est demandée. *colonne* est **nvarchar (384)**, avec NULL comme valeur par défaut. Si *colonne* est ne pas spécifié, toutes les colonnes sont renvoyées. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], *colonne* représente le nom de colonne indiqué dans le **syscolumns** table. La recherche de correspondance avec des caractères génériques est prise en charge. Pour assurer une interopérabilité maximale, le client de la passerelle ne doit utiliser que les modèles de comparaison standard de SQL-92 (caractères génériques % et _).  
  
 [  **@ODBCVer=**] *ODBCVer*  
 Version d'ODBC actuellement utilisée. *ODBCVer* est **int**, avec la valeur par défaut est 2. Cela indique ODBC version 2. Les valeurs valides sont 2 ou 3. Pour les différences de comportement entre les versions 2 et 3, consultez ODBC **SQLColumns** spécification.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 Aucun  
  
## <a name="result-sets"></a>Jeux de résultats  
 Le **sp_columns** procédure stockée de catalogue est équivalente à **SQLColumns** dans ODBC. Les résultats obtenus sont triés par **TABLE_QUALIFIER**, **TABLE_OWNER**, et **TABLE_NAME**.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**TABLE_QUALIFIER**|**sysname**|Nom du qualificateur d'objet. Ce champ peut contenir la valeur NULL.|  
|**TABLE_OWNER**|**sysname**|Nom du propriétaire de l'objet. Ce champ retourne toujours une valeur.|  
|**NOM_TABLE**|**sysname**|Nom de l'objet. Ce champ retourne toujours une valeur.|  
|**COLUMN_NAME**|**sysname**|Nom de colonne, pour chaque colonne de la **TABLE_NAME** retourné. Ce champ retourne toujours une valeur.|  
|**DATA_TYPE**|**smallint**|Code entier pour le type de données ODBC. S’il s’agit d’un type de données qui ne peut pas être mappé à un type ODBC, sa valeur est NULL. Le nom de type de données natif est renvoyé dans le **TYPE_NAME** colonne.|  
|**TYPE_NAME**|**sysname**|Chaîne représentant un type de données. Le SGBD sous-jacent dispose de ce nom de type de données.|  
|**PRECISION**|**int**|Nombre de chiffres significatifs. La valeur de retour pour la **précision** colonne est en base 10.|  
|**LENGTH**|**int**|Taille de transfert des données. <sup>1</sup>|  
|**MISE À L’ÉCHELLE**|**smallint**|Nombre de chiffres situés à droite du séparateur décimal.|  
|**RADIX**|**smallint**|Base pour les types de données numériques.|  
|**ACCEPTE LES VALEURS NULL**|**smallint**|Spécifie la possibilité de contenir une valeur NULL.<br /><br /> 1 = les valeurs NULL sont autorisées.<br /><br /> 0 = pas de valeur NULL.|  
|**SECTION NOTES**|**varchar(254)**|Ce champ renvoie toujours la valeur NULL.|  
|**COLUMN_DEF**|**nvarchar(4000)**|Valeur par défaut de la colonne.|  
|**SQL_DATA_TYPE**|**smallint**|Valeur du type de données SQL tel qu'il apparaît dans le champ TYPE du descripteur. Cette colonne est le même que le **DATA_TYPE** colonne, à l’exception de la **datetime** et SQL-92 **intervalle** des types de données. Cette colonne renvoie toujours une valeur.|  
|**SQL_DATETIME_SUB**|**smallint**|Code de sous-type pour **datetime** et SQL-92 **intervalle** des types de données. Pour les autres types de données, cette colonne renvoie la valeur NULL.|  
|**CHAR_OCTET_LENGTH**|**int**|Longueur maximale, en octets, d'une colonne de type de données caractère ou entier. Pour tous les autres types de données, cette colonne renvoie une valeur NULL.|  
|**ORDINAL_POSITION**|**int**|Numéro d'ordre de la colonne dans l'objet. La première colonne de l'objet est la colonne 1. Cette colonne renvoie toujours une valeur.|  
|**IS_NULLABLE**|**varchar(254)**|Possibilité de valeur Null dans la colonne de l'objet. Les règles ISO sont utilisées pour déterminer la possibilité de valeur Null. Un SGBD compatible avec la norme ISO SQL ne peut pas renvoyer de chaîne vide.<br /><br /> YES = la colonne peut inclure des valeurs NULL.<br /><br /> NO = la colonne ne peut pas inclure de valeurs NULL.<br /><br /> Cette colonne renvoie une chaîne de longueur zéro si la possibilité de valeurs Null n'est pas connue.<br /><br /> La valeur retournée pour cette colonne est différente de celle renvoyée pour la **NULLABLE** colonne.|  
|**SS_DATA_TYPE**|**tinyint**|Type de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisé par les procédures stockées étendues. Pour plus d’informations, consultez [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md).|  
  
 <sup>1</sup> pour plus d’informations, consultez la documentation de Microsoft ODBC.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite des autorisations SELECT et VIEW DEFINITION sur le schéma.  
  
## <a name="remarks"></a>Notes  
 **sp_columns** respecte la configuration requise pour les identificateurs délimités. Pour plus d'informations, consultez [Database Identifiers](../../relational-databases/databases/database-identifiers.md).  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne les informations de colonne pour une table spécifique.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_columns @table_name = N'Department',  
   @table_owner = N'HumanResources';  
```  
  
## <a name="examples-includesssdwfullincludessssdwfull-mdmd-and-includesspdwincludessspdw-mdmd"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 L'exemple suivant retourne les informations de colonne pour une table spécifique.  
  
```  
-- Uses AdventureWorks  
  
EXEC sp_columns @table_name = N'DimEmployee',  
   @table_owner = N'dbo';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_tables &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-tables-transact-sql.md)   
 [Procédures stockées du catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/catalog-stored-procedures-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  


