---
title: sp_special_columns_100 (entrepôt de données SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: ''
ms.prod_service: sql-data-warehouse, pdw
ms.service: sql-data-warehouse
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 5774fadc-77cc-46f8-8f9f-a0f9efe95e21
caps.latest.revision: 9
author: ronortloff
ms.author: rortloff
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || = sqlallproducts-allversions'
ms.openlocfilehash: 38d573b0190b57fef666e393035678265204ffdf
ms.sourcegitcommit: d2573a8dec2d4102ce8882ee232cdba080d39628
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="spspecialcolumns100-sql-data-warehouse"></a>sp_special_columns_100 (entrepôt de données SQL)
[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md.md)]

  Retourne le jeu optimal de colonnes qui identifie de façon unique une ligne d'une table. Retourne également les colonnes automatiquement mises à jour lorsqu'une transaction met à jour une valeur dans la ligne.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône Lien de rubrique") [Conventions de la syntaxe Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
-- Syntax for Azure SQL Data Warehouse and Parallel Data Warehouse  
  
sp_special_columns_100 [ @table_name = ] 'table_name'     
     [ , [ @table_owner = ] 'table_owner' ]   
     [ , [ @qualifier = ] 'qualifier' ]   
     [ , [ @col_type = ] 'col_type' ]   
     [ , [ @scope = ] 'scope' ]  
     [ , [ @nullable = ] 'nullable' ]   
     [ , [ @ODBCVer = ] 'ODBCVer' ]   
[ ; ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @table_name=] '*table_name*'  
 Nom de la table utilisée pour renvoyer des informations de catalogue. *nom* est **sysname**, sans valeur par défaut. La recherche de correspondance avec des caractères génériques n'est pas prise en charge.  
  
 [ @table_owner=] '*table_owner*'  
 Est le propriétaire de la table de la table utilisée pour retourner des informations de catalogue. *propriétaire* est **sysname**, avec NULL comme valeur par défaut. La recherche de correspondance avec des caractères génériques n'est pas prise en charge. Si *propriétaire* n’est pas spécifié, les règles de visibilité de table par défaut du SGBD sous-jacent s’appliquent.  
  
 Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], si l'utilisateur actuel est propriétaire d'une table portant le nom spécifié, les colonnes de cette table sont renvoyées. Si *propriétaire* n’est pas spécifié et l’utilisateur actuel ne possède pas d’une table de l’objet *nom*, cette procédure recherche une table de l’objet *nom* appartenant au propriétaire de la base de données. Si la table existe, ses colonnes sont retournées.  
  
 [ @qualifier=] '*qualificateur*'  
 Nom du qualificateur de la table. *qualificateur* est **sysname**, avec NULL comme valeur par défaut. Divers produits SGBD prennent en charge d’affectation de noms en trois parties pour les tables (*qualifier.owner.name*). Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette colonne représente le nom de la base de données. Dans certains produits, elle représente le nom du serveur de l'environnement de base de données de la table.  
  
 [ @col_type=] '*type_colonne*'  
 Est le type de colonne. *type_colonne* est **char (** 1 **)**, avec un défaut Type R retourne la colonne ou jeu optimal de colonnes qui, en récupérant des valeurs à partir de l’ou les colonnes, permet de n’importe quelle ligne spécifié table pour identifier de manière unique. Une colonne peut être soit une colonne virtuelle spécifiquement créée à cet effet, soit la ou les colonnes d'un index unique de la table. Le type V fournit la ou les colonnes de la table spécifiée qui, le cas échéant, sont automatiquement mises à jour par la source de données lorsqu'une valeur dans la ligne est mise à jour par une transaction.  
  
 [ @scope=] '*étendue*'  
 Étendue minimale requise de l'identificateur de ligne ROWID. *étendue* est **char (** 1 **)**, avec t comme valeur par défaut Spécifie que le ROWID est valide uniquement lorsqu’il est positionné sur cette ligne. L'étendue T indique que le ROWID est valide pour l'ensemble de la transaction.  
  
 [ @nullable=] '*nullable*'  
 Permet de savoir si les colonnes spéciales peuvent accepter ou non une valeur NULL. *Nullable* est **char (** 1 **)**, avec une valeur par défaut U. O identifie les colonnes spéciales qui n’acceptent pas les valeurs null. U identifie les colonnes qui acceptent partiellement des valeurs NULL.  
  
 [ @ODBCVer=] '*ODBCVer*'  
 Est la version ODBC utilisée. *ODBCVer* est **int (** 4 **)**, avec la valeur par défaut est 2. Cela indique ODBC version 2.0. Pour plus d’informations sur la différence entre la version 2.0 et ODBC version 3.0, consultez la spécification ODBC SQLSpecialColumns pour ODBC version 3.0.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 Aucun  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|SCOPE|**smallint**|Étendue réelle de l'identificateur de ligne. Peut prendre les valeurs 0, 1 ou 2. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Retourne toujours 0. Ce champ retourne toujours une valeur.<br /><br /> 0 = SQL_SCOPE_CURROW. La validité du numéro d'identification de ligne est garantie uniquement pendant qu'il est positionné sur cette ligne. Il se peut qu'une sélection ultérieure qui utiliserait le numéro d'identification de la ligne ne retourne pas de ligne si celle-ci a été mise à jour ou supprimée au cours d'une autre transaction.<br /><br /> 1 = SQL_SCOPE_TRANSACTION. La validité du numéro d'identification de ligne est garantie pour la durée de la transaction en cours.<br /><br /> 2 = SQL_SCOPE_SESSION. L'identificateur de ligne est valide pour la durée de la session (parmi les limites de transaction).|  
|COLUMN_NAME|**sysname**|Nom de colonne pour chaque colonne de la *table*retourné. Ce champ retourne toujours une valeur.|  
|DATA_TYPE|**smallint**|Type de données ODBC SQL.|  
|TYPE_NAME|**sysname**|Nom du type de données dépendant de la source de données ; par exemple, **char**, **varchar**, **money**, ou **texte**.|  
|PRECISION|**Int**|Précision de la colonne dans la source de données. Ce champ retourne toujours une valeur.|  
|LENGTH|**Int**|Longueur, en octets, nécessaire pour le type de données sous forme binaire dans la source de données, par exemple, 10 pour **char (** 10 **)**, 4 pour **entier**et 2 pour **smallint**.|  
|SCALE|**smallint**|Échelle de la colonne dans la source de données. La valeur NULL est retournée pour les types de données pour lesquels l'échelle n'est pas applicable.|  
|PSEUDO_COLUMN|**smallint**|Indique si la colonne est une colonne virtuelle. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne toujours 1 :<br /><br /> 0 = SQL_PC_UNKNOWN<br /><br /> 1 = SQL_PC_NOT_PSEUDO<br /><br /> 2 = SQL_PC_PSEUDO|  
  
## <a name="remarks"></a>Notes  
 sp_special_columns est équivalente à SQLSpecialColumns dans ODBC. Les résultats obtenus sont triés par SCOPE.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation SELECT sur le schéma.  
  
## <a name="examples-includesssdwfullincludessssdwfull-mdmd-and-includesspdwincludessspdw-mdmd"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 Cet exemple retourne des informations sur la colonne qui identifie les lignes de manière unique dans la table `FactFinance`.  
  
```  
-- Uses AdventureWorks  
  
EXEC sp_special_columns_100 @table_name = 'FactFinance';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Les procédures stockées de l’entrepôt de données SQL](../../relational-databases/system-stored-procedures/sql-data-warehouse-stored-procedures.md)  
  
  

