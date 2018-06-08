---
title: Sys.dm_sql_referenced_entities (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 11/09/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_sql_referenced_entities_TSQL
- dm_sql_referenced_entities
- sys.dm_sql_referenced_entities
- sys.dm_sql_referenced_entities_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_sql_referenced_entities dynamic management function
ms.assetid: 077111cb-b860-4d61-916f-bac5d532912f
caps.latest.revision: 46
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: a5eef60da505ab939908c22ce47311dfb5f070ea
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmsqlreferencedentities-transact-sql"></a>sys.dm_sql_referenced_entities (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Retourne une ligne pour chaque entité définie par l'utilisateur référencée par nom dans la définition de l'entité de référence spécifiée dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Une dépendance entre deux entités est créée lorsqu’une entité définie par l’utilisateur, appelée la *entité référencée*, apparaît par nom dans une expression SQL rendue persistante d’une autre entité définie par l’utilisateur, appelée la *entité de référence*. Par exemple, si une procédure stockée est l'entité de référence spécifiée, cette fonction retourne toutes les entités définies par l'utilisateur qui sont référencées dans la procédure stockée, telles que les tables, vues, types définis par l'utilisateur ou autres procédures stockées.  
  
 Vous pouvez utiliser cette fonction de gestion dynamique pour établir des rapports sur les types suivants d'entités référencées par l'entité de référence indiquée :  
  
-   Entités liées au schéma  
  
-   Entités non liées au schéma  
  
-   Entités des bases de données croisées et entre serveurs  
  
-   Dépendances au niveau des colonnes sur les entités liées au schéma et non liées au schéma  
  
-   Types définis par l'utilisateur (alias et CLR)  
  
-   collections de schémas XML  
  
-   Fonctions de partition  
  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] via [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]), [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sys.dm_sql_referenced_entities (  
    ' [ schema_name. ] referencing_entity_name ' , ' <referencing_class> ' )  
  
<referencing_class> ::=  
{  
    OBJECT  
  | DATABASE_DDL_TRIGGER  
  | SERVER_DDL_TRIGGER  
}  
```  
  
## <a name="arguments"></a>Arguments  
 [ *nom_schéma*. ] *referencing_entity_name*  
 Nom de l'entité de référence. *schema_name* est requis lorsque la classe de référence est OBJECT.  
  
 *schema_name.referencing_entity_name* est **nvarchar (517)**.  
  
 *< Referencing_class >* :: = {objet | DATABASE_DDL_TRIGGER | SERVER_DDL_TRIGGER}  
 Classe de l'entité de référence spécifiée. Une seule classe peut être spécifiée par instruction.  
  
 *< referencing_class >* est **nvarchar (60)**.  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|referencing_minor_id|**int**|ID de colonne lorsque l'entité de référence est une colonne ; sinon 0. N'accepte pas la valeur NULL.|  
|referenced_server_name|**sysname**|Nom du serveur de l'entité référencée.<br /><br /> Cette colonne est remplie pour les dépendances entre serveurs qui sont établies en spécifiant un nom en quatre parties valide. Pour plus d’informations sur les noms en plusieurs parties, consultez [Conventions de syntaxe Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).<br /><br /> NULL pour les dépendances non liées au schéma pour lesquelles l'entité a été référencée sans spécifier un nom en quatre parties.<br /><br /> NULL pour les entités liées au schéma, car ils doivent être dans la même base de données et par conséquent peuvent uniquement être définies à l’aide de deux parties (*schema.object*) nom.|  
|referenced_database_name|**sysname**|Nom de la base de données de l'entité référencée.<br /><br /> Cette colonne est remplie pour les références des bases de données croisées et entre serveurs qui sont établies en spécifiant un nom en trois ou quatre parties valide.<br /><br /> NULL pour les références non liées au schéma en cas de spécification à l'aide d'un nom en une ou deux parties.<br /><br /> NULL pour les entités liées au schéma, car ils doivent être dans la même base de données et par conséquent peuvent uniquement être définies à l’aide de deux parties (*schema.object*) nom.|  
|referenced_schema_name|**sysname**|Schéma auquel l'entité référencée appartient.<br /><br /> NULL pour les références non liées au schéma dans lesquelles l'entité a été référencée sans spécifier le nom de schéma.<br /><br /> Jamais NULL pour les références liées au schéma.|  
|referenced_entity_name|**sysname**|Nom de l'entité référencée. N'accepte pas la valeur NULL.|  
|referenced_minor_name|**sysname**|Nom de la colonne lorsque l'entité référencée est une colonne ; sinon NULL. Par exemple, referenced_minor_name est NULL dans la ligne qui répertorie l'entité référencée elle-même.<br /><br /> Une entité référencée est une colonne lorsqu'une colonne est identifiée par son nom dans l'entité de référence, ou lorsque l'entité parente est utilisée dans une instruction SELECT *.|  
|referenced_id|**int**|ID de l'entité référencée. Lorsque referenced_minor_id n'est pas égal à 0, referenced_id est l'entité dans laquelle la colonne est définie.<br /><br /> Toujours NULL pour les références entre serveurs.<br /><br /> NULL pour les références des bases de données croisées lorsque l'ID ne peut pas être déterminé, car la base de données est hors connexion ou l'entité ne peut pas être liée.<br /><br /> NULL pour les références dans la base de données si l'ID ne peut pas être déterminé. Pour les références non liées au schéma, l’ID ne peut pas être résolu lorsque l’entité référencée n’existe pas dans la base de données ou lors de la résolution de noms est dépend de l’appelant.  Dans ce cas, is_caller_dependent a la valeur 1.<br /><br /> Jamais NULL pour les références liées au schéma.|  
|referenced_minor_id|**int**|ID de colonne lorsque l'entité référencée est une colonne ; sinon 0. Par exemple, referenced_minor_name est 0 dans la ligne qui répertorie l'entité référencée elle-même.<br /><br /> Pour les références non liées au schéma, les dépendances de colonnes sont signalées uniquement lorsque toutes les entités référencées peuvent être liées. Si une entité référencée ne peut pas être liée, aucune dépendance au niveau des colonnes n'est signalée et referenced_minor_id est égal à 0. Voir l'exemple D.|  
|referenced_class|**tinyint**|Classe de l'entité référencée.<br /><br /> 1 = Objet ou colonne<br /><br /> 6 = Type<br /><br /> 10 = Collection du schéma XML<br /><br /> 21 = Fonction de partition|  
|referenced_class_desc|**nvarchar(60)**|Description de la classe de l'entité référencée.<br /><br /> OBJECT_OR_COLUMN<br /><br /> TYPE<br /><br /> XML_SCHEMA_COLLECTION<br /><br /> PARTITION_FUNCTION|  
|is_caller_dependent|**bit**|Indique que la liaison de schéma pour l'entité référencée se produit au moment de l'exécution ; par conséquent, la résolution de l'ID d'entité dépend du schéma de l'appelant. Cela se produit lorsque l'entité référencée est une procédure stockée, procédure stockée étendue ou fonction définie par l'utilisateur appelée dans une instruction EXECUTE.<br /><br /> 1 = l'entité référencée dépend de l'appelant et est résolue au moment de l'exécution. Dans ce cas, referenced_id a la valeur NULL.<br /><br /> 0 = l'ID de l'entité référencée ne dépend pas de l'appelant. Toujours 0 pour les références liées au schéma et pour les références des bases de données croisées et entre serveurs qui spécifient explicitement un nom de schéma. Par exemple, une référence à une entité au format `EXEC MyDatabase.MySchema.MyProc` ne dépend pas de l'appelant. Toutefois, une référence au format `EXEC MyDatabase..MyProc` dépend de l'appelant.|  
|is_ambiguous|**bit**|Indique la référence est ambiguë et peut être convertie au moment de l’exécution pour une fonction définie par l’utilisateur, un type défini par l’utilisateur (UDT) ou une référence xquery à une colonne de type **xml**. Par exemple, supposons que l’instruction `SELECT Sales.GetOrder() FROM Sales.MySales` est défini dans une procédure stockée. Jusqu'à ce que la procédure stockée soit exécutée, il n'est pas possible de savoir si `Sales.GetOrder()` est une fonction définie par l'utilisateur dans le schéma `Sales` ou une colonne nommée `Sales` de type défini par l'utilisateur avec une méthode nommée `GetOrder()`.<br /><br /> 1 = la référence à une fonction définie par l'utilisateur ou méthode de type défini par l'utilisateur de colonne est ambiguë.<br /><br /> 0 = la référence n'est pas équivoque ou l'entité peut être liée avec succès lorsque la fonction est appelée.<br /><br /> Toujours 0 pour les références liées au schéma.|  
|is_selected|**bit**|**S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> 1 = L'objet ou la colonne est sélectionné.|  
|is_updated|**bit**|**S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> 1 = L'objet ou la colonne est modifié.|  
|is_select_all|**bit**|**S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> 1 = L'objet est utilisé dans une clause SELECT * (au niveau de l'objet uniquement).|  
|is_all_columns_found|**bit**|**S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> 1 = Toutes les dépendances de colonne pour l'objet ont été trouvées.<br /><br /> 0 = Les dépendances de colonne pour l'objet n'ont pas été trouvées.|
|is_insert_all|**bit**|**S'applique à**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> 1 = l’objet est utilisé dans une instruction INSERT sans liste de colonnes (au niveau objet uniquement).|  
|is_incomplete|**bit**|**S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2 à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> 1 = l’objet ou de la colonne a une erreur de liaison et est incomplet.|
  
## <a name="exceptions"></a>Exceptions  
 Retourne un jeu de résultats vide sous chacune des conditions suivantes :  
  
-   Un objet système est spécifié.  
  
-   L'entité spécifiée n'existe pas dans la base de données active.  
  
-   L'entité spécifiée ne référence pas d'autres entités.  
  
-   Un paramètre non valide est passé.  
  
 Retourne une erreur lorsque l'entité de référence spécifiée est une procédure stockée numérotée.  
  
 Retourne l'erreur 2020 lorsque des dépendances de colonnes ne peuvent pas être résolues. Cette erreur n'empêche pas la requête de retourner des dépendances au niveau objet.  
  
## <a name="remarks"></a>Notes  
 Cette fonction peut être exécutée dans le contexte de n'importe quelle base de données pour retourner les entités qui référencent un déclencheur DDL au niveau du serveur.  
  
 Le tableau suivant répertorie les types des entités pour lesquelles les informations de dépendance sont créées et gérées. Les informations de dépendance ne sont pas créées ni gérées pour les règles, les valeurs par défaut, les tables temporaires, les procédures stockées temporaires ou les objets système.  
  
|Type d'entité|Entité de référence|Entité référencée|  
|-----------------|------------------------|-----------------------|  
|Table|Oui*|Oui|  
|Affichage|Oui|Oui|  
|Procédure stockée [!INCLUDE[tsql](../../includes/tsql-md.md)]**|Oui|Oui|  
|Procédure stockée CLR|non|Oui|  
|Fonction [!INCLUDE[tsql](../../includes/tsql-md.md)] définie par l'utilisateur|Oui|Oui|  
|Fonction CLR définie par l'utilisateur|non|Oui|  
|Déclencheur CLR (DML et DDL)|non|non|  
|Déclencheur DML [!INCLUDE[tsql](../../includes/tsql-md.md)]|Oui|non|  
|Déclencheur DDL au niveau de la base de données [!INCLUDE[tsql](../../includes/tsql-md.md)]|Oui|non|  
|Déclencheur DDL au niveau du serveur [!INCLUDE[tsql](../../includes/tsql-md.md)]|Oui|non|  
|Procédures stockées étendues|non|Oui|  
|File d'attente|non|Oui|  
|Synonyme|non|Oui|  
|Type (alias et type CLR défini par l'utilisateur)|non|Oui|  
|Collection de schémas XML|non|Oui|  
|Fonction de partition|non|Oui|  
  
 \* Une table est suivie comme entité de référence uniquement lorsqu’il fait référence à un [!INCLUDE[tsql](../../includes/tsql-md.md)] module, type défini par l’utilisateur ou collection de schémas XML dans la définition d’une colonne calculée, une contrainte CHECK ou une contrainte par défaut.  
  
 ** Les procédures stockées numérotées avec une valeur entière supérieure à 1 ne sont pas suivies en tant qu'entité de référence ou référencée.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation SELECT sur sys.dm_sql_referenced_entities et l'autorisation VIEW DEFINITION sur l'entité de référence. Par défaut, l'autorisation SELECT est accordée à public. Requiert l'autorisation VIEW DEFINITION sur la base de données ou l'autorisation ALTER DATABASE DDL TRIGGER sur la base de données lorsque l'entité de référence est un déclencheur DDL au niveau de la base de données. Requiert l'autorisation VIEW ANY DEFINITION sur le serveur lorsque l'entité de référence est un déclencheur DDL au niveau du serveur.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-returning-entities-that-are-referenced-by-a-database-level-ddl-trigger"></a>A. Retour d'entités qui sont référencées par un déclencheur DDL au niveau de la base de données  
 L'exemple suivant retourne les entités (tables et colonnes) qui sont référencées par le déclencheur DDL au niveau de la base de données `ddlDatabaseTriggerLog`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT referenced_schema_name, referenced_entity_name, referenced_minor_name,   
    referenced_minor_id, referenced_class_desc  
FROM sys.dm_sql_referenced_entities ('ddlDatabaseTriggerLog', 'DATABASE_DDL_TRIGGER');  
GO  
```  
  
### <a name="b-returning-entities-that-are-referenced-by-an-object"></a>B. Retour d'entités qui sont référencées par un objet  
 L'exemple suivant retourne les entités qui sont référencées par la fonction définie par l'utilisateur `dbo.ufnGetContactInformation`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT referenced_schema_name, referenced_entity_name, referenced_minor_name,   
    referenced_minor_id, referenced_class_desc, is_caller_dependent, is_ambiguous  
FROM sys.dm_sql_referenced_entities ('dbo.ufnGetContactInformation', 'OBJECT');  
GO  
```  
  
### <a name="c-returning-column-dependencies"></a>C. Retour de dépendances de colonnes  
 L'exemple suivant crée la table `Table1` avec la colonne calculée `c` définie comme étant la somme des colonnes `a` et `b`. La vue `sys.dm_sql_referenced_entities` est ensuite appelée. La vue retourne deux lignes, une pour chaque colonne définie dans la colonne calculée.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE TABLE dbo.Table1 (a int, b int, c AS a + b);  
GO  
SELECT referenced_schema_name AS schema_name,  
    referenced_entity_name AS table_name,  
    referenced_minor_name AS referenced_column,  
    COALESCE(COL_NAME(OBJECT_ID(N'dbo.Table1'),referencing_minor_id), 'N/A') AS referencing_column_name  
FROM sys.dm_sql_referenced_entities ('dbo.Table1', 'OBJECT');  
GO

-- Remove the table.  
DROP TABLE dbo.Table1;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 schema_name table_name referenced_column referencing_column  
 ----------- ---------- ----------------- ------------------  
 dbo         Table1     a                 c  
 dbo         Table1     b                 c  
```

### <a name="d-returning-non-schema-bound-column-dependencies"></a>D. Retour de dépendances de colonnes non liées au schéma  
 L'exemple suivant supprime `Table1`, puis crée `Table2` et la procédure stockée `Proc1`. La procédure référence `Table2` et la table `Table1` inexistante. La vue `sys.dm_sql_referenced_entities` est exécutée avec la procédure stockée spécifiée comme entité de référence. Le jeu de résultats montre une ligne pour `Table1` et 3 lignes pour `Table2`. Étant donné que `Table1` n'existe pas, les dépendances de colonnes ne peuvent pas être résolues et l'erreur 2020 est retournée. La colonne `is_all_columns_found` retourne 0 pour `Table1` indiquant qu'il y a des colonnes qui ne peuvent pas être découvertes.  
  
```sql  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID ( 'dbo.Table1', 'U' ) IS NOT NULL   
    DROP TABLE dbo.Table1;  
GO  
CREATE TABLE dbo.Table2 (c1 int, c2 int);  
GO  
CREATE PROCEDURE dbo.Proc1 AS  
    SELECT a, b, c FROM Table1;  
    SELECT c1, c2 FROM Table2;  
GO  
SELECT referenced_id, referenced_entity_name AS table_name, referenced_minor_name AS referenced_column_name, is_all_columns_found  
FROM sys.dm_sql_referenced_entities ('dbo.Proc1', 'OBJECT');  
GO  
  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 referenced_id table_name   referenced_column_name  is_all_columns_found  
 ------------- ------------ ----------------------- --------------------  
 935674381     Table2       NULL                    1  
 935674381     Table2       C1                      1  
 935674381     Table2       C2                      1  
 NULL          Table1       NULL                    0  

 Msg 2020, Level 16, State 1, Line 1The dependencies reported for entity "dbo.Proc1" might not include references to all columns. This is either because the entity references an object that does not exist or because of an error in one or more statements in the entity.  Before rerunning the query, ensure that there are no errors in the entity and that all objects referenced by the entity exist.
 ```
  
### <a name="e-demonstrating-dynamic-dependency-maintenance"></a>E. Illustration de la maintenance de dépendance dynamique  
 L'exemple suivant étend l'exemple D pour montrer que les dépendances sont maintenues dynamiquement. L'exemple recrée d'abord `Table1`, qui a été supprimé dans l'exemple D. Par la suite, `sys.dm_sql_referenced_entities` est à nouveau exécuté avec la procédure stockée spécifiée comme entité de référence. Le jeu de résultats indique que les tables et leurs colonnes respectives définies dans la procédure stockée sont retournées. En outre, la colonne `is_all_columns_found` retourne 1 pour tous les objets et colonnes.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE TABLE Table1 (a int, b int, c AS a + b);  
GO   
SELECT referenced_id, referenced_entity_name AS table_name, referenced_minor_name as column_name, is_all_columns_found  
FROM sys.dm_sql_referenced_entities ('dbo.Proc1', 'OBJECT');  
GO  
DROP TABLE Table1, Table2;  
DROP PROC Proc1;  
GO  
  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 referenced_id table_name   referenced_column_name  is_all_columns_found  
 ------------- ------------ ----------------------- --------------------  
 935674381     Table2       NULL                    1 
 935674381     Table2       c1                      1 
 935674381     Table2       c2                      1 
 967674495     Table1       NULL                    1 
 967674495     Table1       a                       1  
 967674495     Table1       b                       1  
 967674495     Table1       c                       1  
 ```
 
### <a name="f-returning-object-or-column-usage"></a>F. Obtenir l'utilisation de l'objet ou de la colonne  
 L'exemple suivant permet d'obtenir les dépendances d'objet et de colonne de la procédure stockée `HumanResources.uspUpdateEmployeePersonalInfo`. Cette procédure met à jour les colonnes `NationalIDNumber`, `BirthDate,``MaritalStatus`, et `Gender` de la `Employee` table selon un `BusinessEntityID` valeur. Une autre procédure stockée, `upsLogError` est définie dans un bloc TRY… CATCH pour capturer les erreurs d'exécution. Les colonnes `is_selected`, `is_updated` et `is_select_all` retournent des informations sur la façon dont ces objets et colonnes sont utilisés dans l'objet de référence. La table et les colonnes qui sont modifiées sont indiquées par un 1 dans la colonne is_updated. Seule la colonne `BusinessEntityID` est sélectionnée et la procédure stockée `uspLogError` n'est ni sélectionnée ni modifiée.  
  
**S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
```sql  
SELECT referenced_entity_name AS table_name, referenced_minor_name as column_name, is_selected, is_updated, is_select_all  
FROM sys.dm_sql_referenced_entities ('HumanResources.uspUpdateEmployeePersonalInfo', 'OBJECT');  
  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 table_name    column_name         is_selected is_updated is_select_all  
 ------------- ------------------- ----------- ---------- -------------  
 uspLogError   NULL                0           0          0  
 Employee      NULL                0           1          0  
 Employee      BusinessEntityID    1           0          0  
 Employee      NationalIDNumber    0           1          0  
 Employee      BirthDate           0           1          0  
 Employee      MaritalStatus       0           1          0  
 Employee      Gender              0           1          0
 ```
  
## <a name="see-also"></a>Voir aussi  
 [sys.dm_sql_referencing_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md)   
 [sys.sql_expression_dependencies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)  
  
  
