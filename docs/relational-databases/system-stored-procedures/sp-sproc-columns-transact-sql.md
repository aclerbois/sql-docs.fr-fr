---
title: sp_sproc_columns (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_sproc_columns
- sp_sproc_columns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_sproc_columns
ms.assetid: 62c18c21-35c5-4772-be0d-ffdcc19c97ab
caps.latest.revision: 26
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: b543a96117ad267022233403f798e83c85e7efad
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spsproccolumns-transact-sql"></a>sp_sproc_columns (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Fournit des informations de colonne pour une fonction définie par l'utilisateur ou une procédure stockée unique dans l'environnement courant.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
-- Syntax for SQL Server, Azure SQL Database, Azure SQL Data Warehouse, Parallel Data Warehouse  
  
sp_sproc_columns [[@procedure_name = ] 'name']   
    [ , [@procedure_owner = ] 'owner']   
    [ , [@procedure_qualifier = ] 'qualifier']   
    [ , [@column_name = ] 'column_name']  
    [ , [@ODBCVer = ] 'ODBCVer']  
    [ , [@fUsePattern = ] 'fUsePattern']  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@procedure_name =** ] **'***nom***'**  
 Nom de la procédure utilisée pour renvoyer des informations de catalogue. *nom* est **nvarchar (** 390 **)**, avec une valeur par défaut %, qui signifie que toutes les tables de la base de données actuelle. La recherche de correspondance avec des caractères génériques est prise en charge.  
  
 [  **@procedure_owner =**] **'***propriétaire***'**  
 Est le nom du propriétaire de la procédure. *propriétaire*est **nvarchar (** 384 **)**, avec NULL comme valeur par défaut. La recherche de correspondance avec des caractères génériques est prise en charge. Si *propriétaire* n’est pas spécifié, les règles de visibilité de procédure par défaut du SGBD sous-jacent s’appliquent.  
  
 Si l'utilisateur actuel possède une procédure du nom spécifié, les informations relatives à cette procédure sont retournées. Si *propriétaire*n’est pas spécifié et l’utilisateur actuel ne possède pas une procédure avec le nom spécifié, **sp_sproc_columns** rechercher une procédure portant le nom spécifié est possédée par le propriétaire de la base de données. Si la procédure existe, les informations relatives à ses colonnes sont renvoyées.  
  
 [  **@procedure_qualifier =**] **'***qualificateur***'**  
 Nom du qualificateur de la procédure. *qualificateur* est **sysname**, avec NULL comme valeur par défaut. Divers produits SGBD prennent en charge d’affectation de noms en trois parties pour les tables (*qualifier.owner.name*). Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ce paramètre représente le nom de la base de données. Dans d'autres produits, elle représente le nom du serveur de l'environnement de base de données de la table.  
  
 [  **@column_name =**] **'***column_name***'**  
 Colonne unique utilisée lorsqu'une seule colonne d'informations de catalogue est souhaitée. *column_name* est **nvarchar (** 384 **)**, avec NULL comme valeur par défaut. Si *column_name* est omis, toutes les colonnes sont renvoyées. La recherche de correspondance avec des caractères génériques est prise en charge. Pour assurer une interopérabilité maximale, le client de la passerelle ne doit utiliser que les modèles de comparaison standard ISO (caractères génériques % et _).  
  
 [  **@ODBCVer =**] **'***ODBCVer***'**  
 Est la version d’ODBC utilisée. *ODBCVer* est **int**, avec une valeur par défaut de 2, qui représente ODBC version 2.0. Pour plus d’informations sur la différence entre la version 2.0 et ODBC version 3.0 d’ODBC, consultez ODBC **SQLProcedureColumns** spécification pour ODBC version 3.0  
  
 [  **@fUsePattern =**] **'***fUsePattern***'**  
 Détermine si le trait de soulignement (_), le signe de pourcentage (%) et les crochets ([ ]) sont interprétés en tant que caractères génériques. Les valeurs valides sont 0 (critères spéciaux désactivés) et 1 (critères spéciaux activés). *fUsePattern* est **bits**, avec 1 comme valeur par défaut.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 Aucun  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**PROCEDURE_QUALIFIER**|**sysname**|Nom du qualificateur de procédure. Cette colonne peut être NULL.|  
|**PROCEDURE_OWNER**|**sysname**|Nom du propriétaire de la procédure. Cette colonne renvoie toujours une valeur.|  
|**NOM_PROCÉDURE**|**nvarchar (** 134 **)**|Nom de la procédure. Cette colonne renvoie toujours une valeur.|  
|**COLUMN_NAME**|**sysname**|Nom de colonne pour chaque colonne de la **TABLE_NAME** retourné. Cette colonne renvoie toujours une valeur.|  
|**COLUMN_TYPE**|**smallint**|Ce champ retourne toujours une valeur :<br /><br /> 0 = SQL_PARAM_TYPE_UNKNOWN<br /><br /> 1 = SQL_PARAM_TYPE_OUTPUT<br /><br /> 2 = SQL_PARAM_TYPE_OUTPUT<br /><br /> 3 = SQL_RESULT_COL<br /><br /> 4 = SQL_PARAM_OUTPUT<br /><br /> 5 = SQL_RETURN_VALUE|  
|**DATA_TYPE**|**smallint**|Code entier d'un type de données ODBC. Si ce type de données ne peut pas être mappé à un type ISO, la valeur est NULL. Le nom de type de données natif est renvoyé dans le **TYPE_NAME** colonne.|  
|**TYPE_NAME**|**sysname**|Représentation sous forme de chaîne du type de données. Nom du type de données, tel qu'il existe dans le SGBD sous-jacent.|  
|**PRECISION**|**int**|Nombre de chiffres significatifs. La valeur de retour pour la **précision** colonne est en base 10.|  
|**LENGTH**|**int**|Taille de transfert des données.|  
|**MISE À L’ÉCHELLE**|**smallint**|Nombre de chiffres situés à droite du séparateur décimal.|  
|**RADIX**|**smallint**|Base des types numériques.|  
|**ACCEPTE LES VALEURS NULL**|**smallint**|Précise la possibilité de valeur nulle.<br /><br /> 1 = Le type de données peut être créé en autorisant des valeurs NULL.<br /><br /> 0 = les valeurs NULL ne sont pas autorisées.|  
|**SECTION NOTES**|**varchar (** 254 **)**|Description de la colonne de procédure. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne retourne pas de valeur pour cette colonne.|  
|**COLUMN_DEF**|**nvarchar (** 4000 **)**|Valeur par défaut de la colonne.|  
|**SQL_DATA_TYPE**|**smallint**|Valeur du type de données SQL telle qu’elle apparaît dans le **TYPE** champ du descripteur. Cette colonne est le même que le **DATA_TYPE** colonne, à l’exception de la **datetime** et ISO **intervalle** des types de données. Cette colonne renvoie toujours une valeur.|  
|**SQL_DATETIME_SUB**|**smallint**|Le **datetime** ISO **intervalle** sous-code si la valeur de **SQL_DATA_TYPE** est **SQL_DATETIME** ou **SQL_INTERVAL**. Pour les données les types autres que **datetime** et ISO **intervalle**, ce champ est NULL.|  
|**CHAR_OCTET_LENGTH**|**int**|Longueur maximale en octets d’un **caractère** ou **binaire** colonne de type de données. Pour tous les autres types de données, cette colonne retourne une valeur NULL.|  
|**ORDINAL_POSITION**|**int**|Numéro d'ordre de la colonne dans la table. La première colonne de la table est la colonne 1. Cette colonne renvoie toujours une valeur.|  
|**IS_NULLABLE**|**varchar(254)**|Possibilité de valeurs NULL dans la colonne de la table. Les règles ISO sont utilisées pour déterminer la possibilité de valeur Null. Un SGBD compatible avec la norme ISO ne peut pas renvoyer de chaîne vide.<br /><br /> Affiche YES si la colonne peut comprendre des valeurs NULL et NO dans le cas contraire.<br /><br /> Cette colonne renvoie une chaîne de longueur zéro si la possibilité de valeurs Null n'est pas connue.<br /><br /> La valeur renvoyée pour cette colonne est différente de celle renvoyée pour la colonne NULLABLE.|  
|**SS_DATA_TYPE**|**tinyint**|Type de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisé par les procédures stockées étendues. Pour plus d’informations, consultez [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md).|  
  
## <a name="remarks"></a>Notes  
 **sp_sproc_columns** équivaut à **SQLProcedureColumns** dans ODBC. Les résultats obtenus sont triés par **PROCEDURE_QUALIFIER**, **PROCEDURE_OWNER**, **nom_procédure**et l’ordre que les paramètres apparaissent dans la définition de procédure.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation SELECT sur le schéma.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées du catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/catalog-stored-procedures-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
