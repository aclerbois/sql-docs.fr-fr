---
title: Sys.dm_sql_referencing_entities (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_sql_referencing_entities
- dm_sql_referencing_entities_TSQL
- sys.dm_sql_referencing_entities_TSQL
- dm_sql_referencing_entities
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_sql_referencing_entities dynamic management function
ms.assetid: c16f8f0a-483f-4feb-842e-da90426045ae
caps.latest.revision: 33
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: adf5f058b8eb39f4eecfd13d922ba723664a73a0
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmsqlreferencingentities-transact-sql"></a>sys.dm_sql_referencing_entities (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Retourne une ligne pour chaque entité dans la base de données actuelle qui référence une autre entité définie par l'utilisateur par nom. Une dépendance entre deux entités est créée lorsqu’une entité, appelée la *entité référencée*, apparaît par nom dans une expression SQL rendue persistante d’une autre entité, appelée la *entité de référence*. Par exemple, si un type défini par l'utilisateur est spécifié comme entité référencée, cette fonction retourne chaque entité définie par l'utilisateur qui référence ce type par nom dans sa définition. La fonction ne retourne pas les entités dans d'autres bases de données qui peuvent référencer l'entité spécifiée. Cette fonction doit être exécutée dans le contexte de la base de données master pour retourner un déclencheur DDL au niveau du serveur comme une entité de référence.  
  
 Vous pouvez utiliser cette fonction de gestion dynamique pour établir un rapport sur les types d'entités suivants dans la base de données actuelle qui référencent l'entité spécifiée :  
  
-   Entités liées au schéma ou non liées au schéma  
  
-   Déclencheurs DDL au niveau de la base de données  
  
-   Déclencheurs DDL au niveau du serveur  
  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] via [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]), [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sys.dm_sql_referencing_entities (  
    ' schema_name.referenced_entity_name ' , ' <referenced_class> ' )  
  
<referenced_class> ::=  
{  
    OBJECT  
  | TYPE  
  | XML_SCHEMA_COLLECTION  
  | PARTITION_FUNCTION  
}  
```  
  
## <a name="arguments"></a>Arguments  
 *schema_name.Referenced*_*nom_entité*  
 Est le nom de l’entité référencée.  
  
 *schema_name* est obligatoire sauf lorsque la classe référencée est PARTITION_FUNCTION.  
  
 *schema_name.referenced_entity_name* est **nvarchar (517)**.  
  
 *< Referenced_class >* :: = {objet | TYPE | XML_SCHEMA_COLLECTION | PARTITION_FUNCTION}  
 Classe de l'entité référencée. Une seule classe peut être spécifiée par instruction.  
  
 *< referenced_class >* est **nvarchar**(60).  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|referencing_schema_name|**sysname**|Schéma auquel l'entité de référence appartient. Autorise la valeur NULL.<br /><br /> NULL pour les déclencheurs DDL au niveau de la base de données et au niveau du serveur.|  
|referencing_entity_name|**sysname**|Nom de l’entité de référence. N'accepte pas la valeur NULL.|  
|referencing_id|**int**|ID de l'entité de référence. N'accepte pas la valeur NULL.|  
|referencing_class|**tinyint**|Classe de l'entité de référence. N'accepte pas la valeur NULL.<br /><br /> 1 = objet<br /><br /> 12 = déclencheur DDL au niveau de la base de données<br /><br /> 13 = déclencheur DDL au niveau du serveur|  
|referencing_class_desc|**nvarchar(60)**|Description de la classe d’entité de référence.<br /><br /> OBJECT<br /><br /> DATABASE_DDL_TRIGGER<br /><br /> SERVER_DDL_TRIGGER|  
|is_caller_dependent|**bit**|Indique que la résolution de l'ID d'entité référencée se produit au moment de l'exécution, car elle dépend du schéma de l'appelant.<br /><br /> 1 = l'entité de référence a la possibilité de référencer l'entité ; toutefois, la résolution de l'ID d'entité référencée dépend de l'appelant et ne peut pas être déterminée. Cela se produit uniquement pour les références non liées au schéma à une procédure stockée, procédure stockée étendue ou fonction définie par l'utilisateur appelée dans une instruction EXECUTE.<br /><br /> 0 = l'entité référencée ne dépend pas de l'appelant.|  
  
## <a name="exceptions"></a>Exceptions  
 Retourne un jeu de résultats vide sous chacune des conditions suivantes :  
  
-   Un objet système est spécifié.  
  
-   L'entité spécifiée n'existe pas dans la base de données active.  
  
-   L'entité spécifiée ne référence pas d'autres entités.  
  
-   Un paramètre non valide est passé.  
  
 Retourne une erreur lorsque l'entité référencée spécifiée est une procédure stockée numérotée.  
  
## <a name="remarks"></a>Notes  
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
  
### <a name="includesskatmaiincludessskatmai-mdmd--includesssql11includessssql11-mdmd"></a>[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] – [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
  
-   Requiert l'autorisation CONTROL sur l'objet référencé. Lorsque l'entité référencée est une fonction de partition, l'autorisation CONTROL sur la base de données est requise.  
  
-   Requiert l’autorisation SELECT sur sys.dm_sql_referencing_entities. Par défaut, l'autorisation SELECT est accordée à public.  
  
### <a name="includesssql14includessssql14-mdmd---includesscurrentincludessscurrent-mdmd"></a>[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] - [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]  
  
-   Ne requiert aucune autorisation sur l'objet référencé. Des résultats partiels peuvent être retournés si l'utilisateur a l'autorisation VIEW DEFINITION uniquement sur certaines entités de référence.  
  
-   Requiert VIEW DEFINITION sur l'objet lorsque l'entité de référence est un objet.  
  
-   Requiert VIEW ANY DEFINITION sur la base de données lorsque l'entité de référence est un déclencheur DDL de niveau base de données.  
  
-   Requiert VIEW ANY DEFINITION sur le serveur lorsque l'entité de référence est un déclencheur DDL au niveau du serveur.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-returning-the-entities-that-refer-to-a-given-entity"></a>A. Retour des entités qui font référence à une entité donnée  
 L'exemple suivant retourne les entités dans la base de données active qui font référence à la table spécifiée.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT referencing_schema_name, referencing_entity_name, referencing_id, referencing_class_desc, is_caller_dependent  
FROM sys.dm_sql_referencing_entities ('Production.Product', 'OBJECT');  
GO  
```  
  
### <a name="b-returning-the-entities-that-refer-to-a-given-type"></a>B. Retour des entités qui font référence à un type donné  
 L'exemple suivant retourne les entités qui référencent le type d'alias `dbo.Flag`. Le jeu de résultats montre que deux procédures stockées utilisent ce type. Le `dbo.Flag` type est également utilisé dans la définition de plusieurs colonnes dans le `HumanResources.Employee` table ; Toutefois, étant donné que le type n’est pas dans la définition d’une colonne calculée, une contrainte CHECK ou une contrainte par défaut dans la table, ne retourne aucune ligne pour le `HumanResources.Employee` table.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT referencing_schema_name, referencing_entity_name, referencing_id, referencing_class_desc, is_caller_dependent  
FROM sys.dm_sql_referencing_entities ('dbo.Flag', 'TYPE');  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 referencing_schema_name referencing_entity_name   referencing_id referencing_class_desc is_caller_dependent  
 ----------------------- -------------------------  ------------- ---------------------- -------------------  
 HumanResources          uspUpdateEmployeeHireInfo  1803153469    OBJECT_OR_COLUMN       0  
 HumanResources          uspUpdateEmployeeLogin     1819153526    OBJECT_OR_COLUMN       0  
 (2 row(s) affected)`  
 ``` 
 
## <a name="see-also"></a>Voir aussi  
 [sys.dm_sql_referenced_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md)   
 [sys.sql_expression_dependencies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)  
  
  
