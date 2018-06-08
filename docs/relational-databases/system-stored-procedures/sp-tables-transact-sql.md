---
title: sp_tables (Transact-SQL) | Documents Microsoft
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
- sp_tables
- sp_tables_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_tables
ms.assetid: 787a2fa5-87a1-49bd-938b-6043c245f46b
caps.latest.revision: 43
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: b0a4e8b4ae1b78da17beb1a5289a90782979ad3c
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sptables-transact-sql"></a>sp_tables (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Retourne une liste d'objets qui peuvent être interrogés dans l'environnement actuel. Cela signifie toute table ou vue, à l'exception des objets synonymes.  
  
> [!NOTE]  
>  Pour déterminer le nom de l’objet de base d’un synonyme, interrogez la [sys.synonyms](../../relational-databases/system-catalog-views/sys-synonyms-transact-sql.md) affichage catalogue.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
-- Syntax for SQL Server, Azure SQL Database, Azure SQL Data Warehouse, Parallel Data Warehouse  
  
sp_tables [ [ @table_name = ] 'name' ]   
     [ , [ @table_owner = ] 'owner' ]   
     [ , [ @table_qualifier = ] 'qualifier' ]   
     [ , [ @table_type = ] "type" ]   
     [ , [@fUsePattern = ] 'fUsePattern'];  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@table_name=** ] **'***nom***'**  
 Table utilisée pour retourner les informations de catalogue. *nom* est **nvarchar (384)**, avec NULL comme valeur par défaut. La recherche de correspondance avec des caractères génériques est prise en charge.  
  
 [  **@table_owner=** ] **'***propriétaire***'**  
 Est le propriétaire de la table de la table utilisée pour retourner des informations de catalogue. *propriétaire* est **nvarchar (384)**, avec NULL comme valeur par défaut. La recherche de correspondance avec des caractères génériques est prise en charge. Si le propriétaire n'est pas précisé, les règles par défaut définissant la visibilité des tables du SGBD sous-jacent sont utilisées.  
  
 Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], si l'utilisateur actuel est propriétaire d'une table portant le nom spécifié, les colonnes de cette table sont renvoyées. Si le propriétaire n'est pas spécifié et que l'utilisateur actuel ne possède pas de table ayant le nom spécifié, la procédure recherche une table portant le nom spécifié, possédée par le propriétaire de la base de données. Si la recherche de la table aboutit, ce sont les colonnes de cette dernière qui sont retournées.  
  
 [  **@table_qualifier=** ] **'***qualificateur***'**  
 Nom du qualificateur de la table. *qualificateur* est **sysname**, avec NULL comme valeur par défaut. Divers produits SGBD prennent en charge d’affectation de noms en trois parties pour les tables (*qualificateur ***.*** propriétaire ***.*** nom de*). Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette colonne représente le nom de la base de données. Dans d'autres produits, elle représente le nom du serveur de l'environnement de base de données de la table.  
  
 [ **,** [  **@table_type=** ] **» «***type***'**, **'** type **» «** ]  
 Liste de valeurs séparées par des virgules, donnant des informations sur toutes les tables des types spécifiés. Ceux-ci incluent **TABLE**, **SYSTEMTABLE**, et **vue**. *type* est **varchar (100)**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  Chaque type de table doit être encadré de guillemets simples, l'ensemble du paramètre devant être encadré de guillemets doubles. Les types de table sont en lettres majuscules. Si SET QUOTED_IDENTIFIER est défini à ON, chaque guillemet simple doit devenir double et l'ensemble du paramètre doit être encadré de guillemets simples.  
  
 [  **@fUsePattern =** ] **'***fUsePattern***'**  
 Détermine si les caractères trait de soulignement (_), pourcentage (%) et crochet ([ ou ]) sont interprétés en tant que caractères génériques. Les valeurs valides sont 0 (critères spéciaux désactivés) et 1 (critères spéciaux activés). *fUsePattern* est **bits**, avec 1 comme valeur par défaut.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 Aucun  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**TABLE_QUALIFIER**|**sysname**|Nom du qualificateur de table. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette colonne représente le nom de la base de données. Ce champ peut contenir la valeur NULL.|  
|**TABLE_OWNER**|**sysname**|Nom du propriétaire de la table. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette colonne représente le nom de l'utilisateur de la base de données qui a créé la table. Ce champ retourne toujours une valeur.|  
|**NOM_TABLE**|**sysname**|Nom de la table. Ce champ retourne toujours une valeur.|  
|**TABLE_TYPE**|**varchar(32)**|Table, table système ou vue.|  
|**SECTION NOTES**|**varchar(254)**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne retourne pas de valeur pour cette colonne.|  
  
## <a name="remarks"></a>Notes  
 Pour assurer une interopérabilité maximale, le client de la passerelle ne doit utiliser que les critères spéciaux SQL standard de SQL-92 (caractères génériques % et _).  
  
 Les informations de privilège concernant l'accès en lecture/écriture de l'utilisateur actuel à une table spécifique ne sont pas toujours vérifiées. Par conséquent, l'accès n'est pas garanti. Ce jeu de résultats ne comprend pas uniquement des tables et des vues, mais également des synonymes et des noms d'alias, dans le cas des passerelles vers les SGBD qui gèrent ces types. Si l’attribut de serveur **sp_server_info** est Y dans le jeu de résultats pour **sp_server_info**, seules les tables qui sont accessibles par l’utilisateur actuel sont retournées.  
  
 **sp_tables** équivaut à **SQLTables** dans ODBC. Les résultats obtenus sont triés par **TABLE_TYPE**, **TABLE_QUALIFIER**, **TABLE_OWNER**, et **TABLE_NAME**.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation SELECT sur le schéma.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-returning-a-list-of-objects-that-can-be-queried-in-the-current-environment"></a>A. Retour d'une liste d'objets pouvant être sollicités dans l'environnement actuel  
 L’exemple suivant retourne une liste d’objets qui peuvent être des requêtes dans l’environnement actuel.  
  
```  
EXEC sp_tables ;  
```  
  
### <a name="b-returning-information-about-the-tables-in-a-specified-schema"></a>B. Retour d'informations sur les tables dans un schéma spécifié  
 L'exemple suivant retourne des informations sur les tables appartenant au schéma `Person` de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_tables   
   @table_name = '%',  
   @table_owner = 'Person',  
   @table_qualifier = 'AdventureWorks2012';  
```  
  
## <a name="examples-includesssdwfullincludessssdwfull-mdmd-and-includesspdwincludessspdw-mdmd"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-returning-a-list-of-objects-that-can-be-queried-in-the-current-environment"></a>C. Retour d'une liste d'objets pouvant être sollicités dans l'environnement actuel  
 L’exemple suivant retourne une liste d’objets qui peuvent être des requêtes dans l’environnement actuel.  
  
```  
EXEC sp_tables ;  
```  
  
### <a name="d-returning-information-about-the-tables-in-a-specified-schema"></a>D. Retour d'informations sur les tables dans un schéma spécifié  
 L’exemple suivant retourne des informations sur les tables de dimension dans le `AdventureWorksPDW201` base de données.  
  
```  
-- Uses AdventureWorks  
  
EXEC sp_tables   
   @table_name = 'Dim%',  
   @table_owner = 'dbo',  
   @table_qualifier = 'AdventureWorksPDW2012';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Sys.SYNONYMS &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-synonyms-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

