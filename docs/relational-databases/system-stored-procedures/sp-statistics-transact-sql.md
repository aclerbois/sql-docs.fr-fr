---
title: sp_statistics (Transact-SQL) | Documents Microsoft
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
- sp_statistics_TSQL
- sp_statistics
dev_langs:
- TSQL
helpviewer_keywords:
- sp_statistics
ms.assetid: 0bb6495f-258a-47ec-9f74-fd16671d23b8
caps.latest.revision: 32
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 0b4a24cce24a94fdd6c4f901974d0f4accf7ff1b
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spstatistics-transact-sql"></a>sp_statistics (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Retourne la liste de tous les index et statistiques d'une table ou d'une vue indexée.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
-- Syntax for SQL Server, Azure SQL Database, Azure SQL Data Warehouse, Parallel Data Warehouse  
  
sp_statistics [ @table_name = ] 'table_name'    
     [ , [ @table_owner = ] 'owner' ]   
     [ , [ @table_qualifier = ] 'qualifier' ]   
     [ , [ @index_name = ] 'index_name' ]   
     [ , [ @is_unique = ] 'is_unique' ]  
     [ , [ @accuracy = ] 'accuracy' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@table_name=** ] **'***table_name***'**  
 Spécifie la table utilisée pour retourner les informations de catalogue. *nom_table* est **sysname**, sans valeur par défaut. La recherche de correspondance avec des caractères génériques n'est pas prise en charge.  
  
 [  **@table_owner=** ] **'***propriétaire***'**  
 Nom du propriétaire de la table utilisée pour renvoyer des informations de catalogue. *TABLE_OWNER* est **sysname**, avec NULL comme valeur par défaut. La recherche de correspondance avec des caractères génériques n'est pas prise en charge. Si *propriétaire* n’est pas spécifié, les règles de visibilité de table par défaut du SGBD sous-jacent s’appliquent.  
  
 Si, dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], l'utilisateur actuel est propriétaire d'une table portant le nom spécifié, les index de la table sont retournés. Si *propriétaire* n’est pas spécifié et l’utilisateur actuel ne possède pas d’une table avec l’objet *nom*, cette procédure recherche une table avec l’objet *nom* appartenant au propriétaire de la base de données. Si cette table existe, ses index sont retournés.  
  
 [  **@table_qualifier=** ] **'***qualificateur***'**  
 Nom du qualificateur de la table. *qualificateur* est **sysname**, avec NULL comme valeur par défaut. Divers produits SGBD prennent en charge d’affectation de noms en trois parties pour les tables (*qualificateur ***.*** propriétaire ***.*** nom de*). Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ce paramètre représente le nom de la base de données. Dans d'autres produits, elle représente le nom du serveur de l'environnement de base de données de la table.  
  
 [  **@index_name=** ] **'***index_name***'**  
 Est le nom de l’index. *index_name* est **sysname**, avec % comme valeur par défaut. La recherche de correspondance avec des caractères génériques est prise en charge.  
  
 [  **@is_unique=** ] **'***is_unique***'**  
 Est si seuls les index uniques (si **Y**) doivent être retournées. *is_unique* est **char (1)**, avec une valeur par défaut **N**.  
  
 [  **@accuracy=** ] **'***précision***'**  
 Niveau de précision de la cardinalité et des pages pour les statistiques. *précision* est **char (1)**, avec une valeur par défaut **Q**. Spécifiez **E** pour vous assurer que les statistiques sont mises à jour afin que la cardinalité et des pages sont précis.  
  
 La valeur **E** (SQL_ENSURE) indique au pilote de récupérer les statistiques.  
  
 La valeur **Q** (SQL_QUICK) indique au pilote de récupérer la cardinalité et des pages uniquement si elles sont disponibles à partir du serveur. Dans ce cas, le pilote ne s'assure pas que les valeurs sont actuelles. Pour les applications écrites conformément au standard de groupe ouvert, seule l'option SQL_QUICK est disponible de la part des pilotes compatibles ODBC 3.x.  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**TABLE_QUALIFIER**|**sysname**|Nom du qualificateur de table. Cette colonne peut être NULL.|  
|**TABLE_OWNER**|**sysname**|Nom du propriétaire de la table. Cette colonne renvoie toujours une valeur.|  
|**NOM_TABLE**|**sysname**|Nom de la table. Cette colonne renvoie toujours une valeur.|  
|**NON_UNIQUE**|**smallint**|NON NULL.<br /><br /> 0 = Unique<br /><br /> 1 = Non unique|  
|**INDEX_QUALIFIER**|**sysname**|Nom de propriétaire d’index. Certains produits SGBD acceptent que des utilisateurs autres que le propriétaire de la table créent des index. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette colonne est toujours le même que **TABLE_NAME**.|  
|**INDEX_NAME**|**sysname**|Nom de l'index. Cette colonne renvoie toujours une valeur.|  
|**TYPE**|**smallint**|Cette colonne renvoie toujours une valeur :<br /><br /> 0 = Statistiques pour une table<br /><br /> 1 = ordonné en clusters<br /><br /> 2 = Haché<br /><br /> 3 = non cluster|  
|**SEQ_IN_INDEX**|**smallint**|Position de la colonne dans l'index|  
|**COLUMN_NAME**|**sysname**|Nom de colonne pour chaque colonne de la **TABLE_NAME** retourné. Cette colonne renvoie toujours une valeur.|  
|**COLLATION**|**char(1)**|Ordre utilisé dans les classements. Valeurs possibles :<br /><br /> A = Croissant<br /><br /> D = Décroissant<br /><br /> NULL = Non applicable|  
|**CARDINALITÉ**|**int**|Nombre de lignes dans la table ou de valeurs uniques dans l’index.|  
|**PAGES**|**int**|Nombre de pages pour stocker l'index ou la table.|  
|**FILTER_CONDITION**|**varchar(128)**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne retourne pas de valeur.|  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 Aucun  
  
## <a name="remarks"></a>Notes  
 Les index dans le jeu de résultats s’affichent dans l’ordre croissant en fonction des colonnes **NON_UNIQUE**, **TYPE**, **INDEX_NAME**, et **SEQ_IN_INDEX**.  
  
 Le type d'index cluster fait référence à un index dans lequel les données de la table sont stockées dans l'ordre de l'index. Cela correspond à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] index ordonnés en clusters.  
  
 Le type d'index haché accepte les recherches de concordance exacte ou d'intervalle, mais les recherches par critères spéciaux n'utilisent pas l'index.  
  
 **sp_statistics** équivaut à **SQLStatistics** dans ODBC. Les résultats obtenus sont triés par **NON_UNIQUE**, **TYPE**, **INDEX_QUALIFIER**, **INDEX_NAME**, et **SEQ_IN_INDEX**. Pour plus d’informations, consultez la [référence de l’API ODBC](http://go.microsoft.com/fwlink/?LinkId=68323).  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation SELECT sur le schéma.  
  
## <a name="example-includesssdwfullincludessssdwfull-mdmd-and-includesspdwincludessspdw-mdmd"></a>Exemple : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 L’exemple suivant retourne des informations sur la `DimEmployee` table.  
  
```  
-- Uses AdventureWorks  
  
EXEC sp_statistics DimEmployee;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées du catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/catalog-stored-procedures-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

