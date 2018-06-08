---
title: Sys.dm_exec_describe_first_result_set_for_object (Transact-SQL) | Documents Microsoft
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
- sys.dm_exec_describe_first_result_set_for_object_TSQL
- sys.dm_exec_describe_first_result_set_for_object
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_describe_first_result_set_for_object catalog view
ms.assetid: 63b0fde7-95d7-4ad7-a219-a9feacf1bd89
caps.latest.revision: 22
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 8ac774a29be46e7be925141cd10b8dd7150e5724
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmexecdescribefirstresultsetforobject-transact-sql"></a>sys.dm_exec_describe_first_result_set_for_object (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-asdb-xxxx-xxx-md.md)]

  Cette fonction de gestion dynamique accepte un @object_id en tant que paramètre et décrit les premières métadonnées de résultat pour le module avec cet ID. Le @object_id spécifié peut être l’ID d’un [!INCLUDE[tsql](../../includes/tsql-md.md)] procédure stockée ou un [!INCLUDE[tsql](../../includes/tsql-md.md)] déclencheur. S'il s'agit de l'ID de tout autre objet (tel qu'une vue, une table, une fonction ou une procédure CLR), une erreur sera spécifiée dans les colonnes d'erreur du résultat.  
  
 **Sys.dm_exec_describe_first_result_set_for_object** a le même jeu de résultats en tant que définition de [sys.dm_exec_describe_first_result_set &#40;Transact-SQL&#41; ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-transact-sql.md) et est semblable à [sp_ describe_first_result_set &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.dm_exec_describe_first_result_set_for_object   
    ( @object_id , @include_browse_information )  
```  
  
## <a name="arguments"></a>Arguments  
 *@object_id*  
 Le @object_id d’un [!INCLUDE[tsql](../../includes/tsql-md.md)] procédure stockée ou un [!INCLUDE[tsql](../../includes/tsql-md.md)] déclencheur. @object_id est de type **int**.  
  
 *@include_browse_information*  
 @include_browse_information est de type **bits**. Lorsque la valeur 1 est définie, chaque requête est analysée comme si elle comportait une option FOR BROWSE sur la requête. Retourne des colonnes clés supplémentaires et des informations de table source.  
  
## <a name="table-returned"></a>Table retournée  
 Ces métadonnées communes sont retournées en tant que jeu de résultats avec une ligne pour chaque colonne dans les métadonnées de résultats. Chaque ligne décrit le type et la possibilité de valeur NULL de la colonne dans le format décrit dans la section suivante. S'il n'existe pas de première instruction pour chaque chemin d'accès de contrôle, un jeu de résultats avec des lignes nulles est retourné.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**is_hidden**|**bit**|Spécifie si la colonne est une colonne supplémentaire ajoutée à titre d'informations de navigation qui ne s'affiche pas réellement dans le jeu de résultats.|  
|**column_ordinal**|**int**|Contient la position ordinale de la colonne dans le jeu de résultats. Position de la première colonne sera spécifiée comme 1.|  
|**nom**|**sysname**|Contient le nom de la colonne si un nom peut être déterminé. Sinon, a la valeur NULL.|  
|**is_nullable**|**bit**|Contient la valeur 1 si la colonne autorise des valeurs NULL, 0 si la colonne n'autorise pas de valeurs NULL et 1 s'il est impossible de déterminer que la colonne autorise des valeurs NULL.|  
|**system_type_id**|**int**|Contient le system_type_id du type de données de la colonne comme spécifié dans sys.types. Pour les types CLR, bien que la colonne system_type_name retourne NULL, cette colonne retournera la valeur 240.|  
|**system_type_name**|**nvarchar (256)**|Contient le nom du type de données. Inclut des arguments (tels que la longueur, la précision, l'échelle) spécifiés pour le type de données de la colonne. Si le type de données est un type d'alias défini par l'utilisateur, le type de système sous-jacent est spécifié ici. S'il s'agit d'un type clr défini par l'utilisateur, NULL est retourné dans cette colonne.|  
|**max_length**|**smallint**|Longueur maximale (en octets) de la colonne.<br /><br /> -1 = la colonne est de type de données **varchar (max)**, **nvarchar (max)**, **varbinary (max)**, ou **xml**.<br /><br /> Pour **texte** des colonnes, le **max_length** valeur sera 16 ou à la valeur définie par **sp_tableoption 'text in row'**.|  
|**precision**|**tinyint**|Précision de la colonne si elle est numérique. Dans le cas contraire, retourne la valeur 0.|  
|**scale**|**tinyint**|Échelle de la colonne si elle est numérique. Dans le cas contraire, retourne la valeur 0.|  
|**collation_name**|**sysname**|Nom du classement de la colonne si elle est basée sur les caractères. Sinon, retourne NULL.|  
|**user_type_id**|**int**|Pour les types d'alias et CLR, contient l'information user_type_id du type de données de la colonne comme spécifié dans sys.types. Sinon, a la valeur NULL.|  
|**user_type_database**|**sysname**|Pour les types d'alias et CLR, contient le nom de la base de données dans laquelle le type est défini. Sinon, a la valeur NULL.|  
|**user_type_schema**|**sysname**|Pour les types d'alias et CLR, contient le nom du schéma dans lequel le type est défini. Sinon, a la valeur NULL.|  
|**user_type_name**|**sysname**|Pour les types d'alias et CLR, contient le nom du type. Sinon, a la valeur NULL.|  
|**assembly_qualified_type_name**|**nvarchar(4000)**|Pour les types CLR, retourne le nom de l'assembly et de la classe définissant le type. Sinon, a la valeur NULL.|  
|**xml_collection_id**|**int**|Contient l'information xml_collection_id du type de données de la colonne comme spécifié dans sys.columns. Cette colonne retournera NULL si le type retourné n'est pas associé à une collection de schémas XML.|  
|**xml_collection_database**|**sysname**|Contient la base de données dans laquelle la collection de schémas XML associée à ce type est définie. Cette colonne retournera NULL si le type retourné n'est pas associé à une collection de schémas XML.|  
|**xml_collection_schema**|**sysname**|Contient le schéma dans lequel la collection de schémas XML associée à ce type est définie. Cette colonne retournera NULL si le type retourné n'est pas associé à une collection de schémas XML.|  
|**xml_collection_name**|**sysname**|Contient le nom de la collection de schémas XML associé à ce type. Cette colonne retournera NULL si le type retourné n'est pas associé à une collection de schémas XML.|  
|**is_xml_document**|**bit**|Retourne 1 si le type de données retourné est XML et que ce type est garanti être un document XML complet (nœud racine compris), par opposition à un fragment XML. Dans le cas contraire, retourne la valeur 0.|  
|**is_case_sensitive**|**bit**|Retourne 1 si la colonne est d'un type chaîne sensible à la casse et 0 si ce n'est pas le cas.|  
|**is_fixed_length_clr_type**|**bit**|Retourne 1 si la colonne est d'un type CLR de longueur fixe et 0 si ce n'est pas le cas.|  
|**source_server**|**sysname**|Nom du serveur d'origine retourné par la colonne dans ce résultat (s'il provient d'un serveur distant). Le nom est donné tel qu’il apparaît dans sys.servers.  Retourne NULL si la colonne est initiée sur le serveur local, ou si elle ne peut pas être déterminé quel serveur elle provenance. Est fourni uniquement si les informations de navigation sont demandées.|  
|**source_database**|**sysname**|Nom de la base de données d'origine retourné par la colonne dans ce résultat. Retourne NULL si la base de données ne peut pas être déterminée. Est fourni uniquement si les informations de navigation sont demandées.|  
|**source_schema**|**sysname**|Nom du schéma d'origine retourné par la colonne dans ce résultat. Retourne NULL si le schéma ne peut pas être déterminé. Est fourni uniquement si les informations de navigation sont demandées.|  
|**source_table**|**sysname**|Nom de la table d'origine retourné par la colonne dans ce résultat. Retourne NULL si la table ne peut pas être déterminée. Est fourni uniquement si les informations de navigation sont demandées.|  
|**source_column**|**sysname**|Nom de la colonne d'origine retourné par la colonne dans ce résultat. Retourne NULL si la colonne ne peut pas être déterminée. Est fourni uniquement si les informations de navigation sont demandées.|  
|**is_identity_column**|**bit**|Retourne 1 si la colonne est une colonne d'identité et 0 dans le cas contraire. Retourne NULL s'il est impossible de déterminer que la colonne est une colonne d'identité.|  
|**is_part_of_unique_key**|**bit**|Retourne 1 si la colonne fait partie d'un index unique (notamment la contrainte unique et primaire) et 0 dans le cas contraire. Retourne NULL s'il est impossible de déterminer que la colonne fait partie d'un index unique. Fourni uniquement si les informations de navigation sont demandées.|  
|**is_updateable**|**bit**|Retourne 1 si la colonne peut être mise à jour et 0 dans le cas contraire. Retourne NULL s'il est impossible de déterminer que la colonne peut être mise à jour.|  
|**is_computed_column**|**bit**|Retourne 1 si la colonne est une colonne calculée et 0 dans le cas contraire. Retourne NULL s’il ne peut pas être déterminé que la colonne est une colonne calculée.|  
|**is_sparse_column_set**|**bit**|Retourne 1 si la colonne est une colonne éparse et 0 dans le cas contraire. Retourne NULL s'il est impossible de déterminer que la colonne fait partie d'un jeu de colonnes éparses.|  
|**ordinal_in_order_by_list**|**smallint**|Position de cette colonne dans la liste ORDER BY. Retourne NULL si la colonne ne s'affiche pas dans la liste ORDER BY ou si la liste ORDER BY ne peut pas être déterminée de manière unique.|  
|**order_by_list_length**|**smallint**|Longueur de la liste ORDER BY. Retourne NULL s'il n'existe aucune liste ORDER BY ou si la liste ORDER BY ne peut pas être déterminée de manière unique. Notez que cette valeur sera la même pour toutes les lignes retournées par sp_describe_first_result_set.|  
|**order_by_is_descending**|**smallint NULL**|Si la valeur ordinal_in_order_by_list n’est pas NULL, le **order_by_is_descending** colonne indique la direction de la clause ORDER BY pour cette colonne. Sinon, elle indique NULL.|  
|**error_number**|**int**|Contient le numéro d'erreur retourné par la fonction. Contient NULL si aucune erreur ne s'est produite dans la colonne.|  
|**error_severity**|**int**|Contient la gravité retournée par la fonction. Contient NULL si aucune erreur ne s'est produite dans la colonne.|  
|**error_state**|**int**|Contient le message d’état retourné par la fonction. Si aucune erreur ne s'est produite, la colonne contiendra NULL.|  
|**error_message**|**nvarchar(4096)**|Contient le message retourné par la fonction. Si aucune erreur ne s'est produite, la colonne contiendra NULL.|  
|**error_type**|**int**|Contient un entier qui représente l'erreur retournée. Mappé à error_type_desc. Consultez la liste sous les notes.|  
|**error_type_desc**|**nvarchar(60)**|Contient une chaîne majuscule courte qui représente l'erreur retournée. Mappé à error_type. Consultez la liste sous les notes.|  
  
## <a name="remarks"></a>Notes  
 Cette fonction utilise le même algorithme que **sp_describe_first_result_set**. Pour plus d’informations, consultez [sp_describe_first_result_set &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md).  
  
 Le tableau suivant répertorie les types d'erreur et leur description.  
  
|error_type|error_type| Description|  
|-----------------|-----------------|-----------------|  
|1|MISC|Toutes les erreurs qui ne font pas l'objet d'une description.|  
|2|SYNTAX|Une erreur de syntaxe s'est produite dans le lot.|  
|3|CONFLICTING_RESULTS|Le résultat n'a pas pu être déterminé en raison d'un conflit entre deux premières instructions possibles.|  
|4|DYNAMIC_SQL|Le résultat n'a pas pu être déterminé en raison du SQL dynamique qui pourrait éventuellement retourner le premier résultat.|  
|5|CLR_PROCEDURE|Le résultat n'a pas pu être déterminé parce qu'une procédure stockée clr pourrait éventuellement retourner le premier résultat.|  
|6|CLR_TRIGGER|Le résultat n'a pas pu être déterminé parce qu'un déclencheur CLR pourrait éventuellement retourner le premier résultat.|  
|7|EXTENDED_PROCEDURE|Le résultat n'a pas pu être déterminé parce qu'une procédure stockée étendue pourrait éventuellement retourner le premier résultat.|  
|8|UNDECLARED_PARAMETER|Le résultat n'a pas pu être déterminé car le type de données d'une ou plusieurs des colonnes du jeu de résultats dépend potentiellement d'un paramètre non déclaré.|  
|9|RECURSION|Le résultat n'a pas pu être déterminé car le lot contient une instruction récursive.|  
|10|TEMPORARY_TABLE|Le résultat n’a pas pu être déterminé car le lot contient une table temporaire n’est pas pris en charge par **sp_describe_first_result_set** .|  
|11|UNSUPPORTED_STATEMENT|Le résultat n’a pas pu être déterminé car le lot contient une instruction qui n’est pas pris en charge par **sp_describe_first_result_set** (par exemple, FETCH, REVERT etc.).|  
|12|OBJECT_ID_NOT_SUPPORTED|Le @object_id transmis à la fonction est pas pris en charge (autrement dit, pas une procédure stockée)|  
|13|OBJECT_ID_DOES_NOT_EXIST|Le @object_id transmis à la fonction est introuvable dans le catalogue système.|  
  
## <a name="permissions"></a>Autorisations  
 Requiert l’autorisation d’exécuter le @tsql argument.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-returning-metadata-with-and-without-browse-information"></a>A. Retour de métadonnées avec et sans informations de navigation  
 L’exemple suivant crée une procédure stockée nommée TestProc2 qui retourne deux jeux de résultats. Ensuite, l’exemple montre que **sys.dm_exec_describe_first_result_set** retourne des informations sur le premier jeu de résultats dans la procédure, avec et sans les informations de navigation.  
  
```  
CREATE PROC TestProc2  
AS  
SELECT object_id, name FROM sys.objects ;  
SELECT name, schema_id, create_date FROM sys.objects ;  
GO  
  
SELECT * FROM sys.dm_exec_describe_first_result_set_for_object(OBJECT_ID('TestProc2'), 0) ;  
SELECT * FROM sys.dm_exec_describe_first_result_set_for_object(OBJECT_ID('TestProc2'), 1) ;  
GO  
```  
  
### <a name="b-combining-the-sysdmexecdescribefirstresultsetforobject-function-and-a-table-or-view"></a>B. Combinaison de la fonction sys.dm_exec_describe_first_result_set_for_object avec une table ou une vue  
 L’exemple suivant utilise à la fois la vue de catalogue de système sys.procedures et **sys.dm_exec_describe_first_result_set_for_object** (fonction) pour afficher les métadonnées pour les jeux de résultats de toutes les procédures stockées dans le [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] base de données.  
  
```  
USE AdventureWorks2012;  
GO  
  
SELECT p.name, r.*   
FROM sys.procedures AS p  
CROSS APPLY sys.dm_exec_describe_first_result_set_for_object(p.object_id, 0) AS r;  
GO  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_describe_first_result_set &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md)   
 [sp_describe_undeclared_parameters &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-describe-undeclared-parameters-transact-sql.md)   
 [sys.dm_exec_describe_first_result_set &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-transact-sql.md)  
  
  
