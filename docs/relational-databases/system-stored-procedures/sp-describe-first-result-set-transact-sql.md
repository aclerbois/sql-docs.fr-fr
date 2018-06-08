---
title: sp_describe_first_result_set (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/17/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_describe_first_result_set
- sp_describe_first_result_set_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_describe_first_result_set
ms.assetid: f2355a75-3a8e-43e6-96ad-4f41038f6d22
caps.latest.revision: 22
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 12890dc6282f879259730530b3ff8f03fc6de8b9
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spdescribefirstresultset-transact-sql"></a>sp_describe_first_result_set (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-all-md](../../includes/tsql-appliesto-ss2012-all-md.md)]

  Retourne les métadonnées pour la première possible de jeu de résultats de la [!INCLUDE[tsql](../../includes/tsql-md.md)] par lots. Retourne un jeu de résultats vide si le lot ne retourne pas de résultats. Génère une erreur si le [!INCLUDE[ssDE](../../includes/ssde-md.md)] ne peut pas déterminer les métadonnées pour la première requête qui sera exécutée en effectuant une analyse statique. La vue de gestion dynamique [sys.dm_exec_describe_first_result_set &#40;Transact-SQL&#41; ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-transact-sql.md) retourne les mêmes informations.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_describe_first_result_set [ @tsql = ] N'Transact-SQL_batch'   
    [ , [ @params = ] N'parameters' ]   
    [ , [ @browse_information_mode = ] <tinyint> ] ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@tsql =** ] **'***SQL_batch transact***'**  
 Une ou plusieurs instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] *Transact-SQL_batch* peut être **nvarchar (***n***)** ou **nvarchar (max)**.  
  
 [  **@params =** ] **N'***paramètres***'**  
 @params Fournit une chaîne de déclaration pour les paramètres pour le [!INCLUDE[tsql](../../includes/tsql-md.md)] lot, qui est similaire à sp_executesql. Les paramètres peuvent être **nvarchar (n)** ou **nvarchar (max)**.  
  
 Est une chaîne qui contient les définitions de tous les paramètres qui ont été incorporés dans le [!INCLUDE[tsql](../../includes/tsql-md.md)] *_batch*. Cette chaîne doit être une constante Unicode ou une variable Unicode. Chaque définition de paramètre se compose d'un nom de paramètre et d'un type de données. *n* est un espace réservé qui indique les définitions de paramètres supplémentaires. Chaque paramètre spécifié dans l’instruction doit être défini dans @params. Si le [!INCLUDE[tsql](../../includes/tsql-md.md)] lot dans l’instruction ou l’instruction ne contient pas de paramètres, @params n’est pas obligatoire. NULL est la valeur par défaut pour ce paramètre.  
  
 [  **@browse_information_mode =** ] *tinyint*  
 Spécifie si des colonnes clés supplémentaires et les informations de table source sont retournées. Si la valeur 1 est définie, chaque requête est analysée comme si elle incluait une option FOR BROWSE sur la requête. Des colonnes clés supplémentaires et les informations de table source sont retournées.  
  
-   Si la valeur est 0, aucune information n'est retournée.  
  
-   Si la valeur 1 est définie, chaque requête est analysée comme si elle incluait une option FOR BROWSE sur la requête. Retourne les noms de tables de base comme informations de colonne source.  
  
-   Si la valeur 2 est définie, chaque requête est analysée comme si elle était utilisée lors de la préparation ou de l'exécution d'un curseur. Retourne les noms de vues comme informations de colonne source.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **sp_describe_first_result_set** retourne toujours un état de zéro en cas de réussite. Si la procédure génère une erreur et la procédure est appelée comme RPC, l’état de retour est remplie par le type de message d’erreur décrit dans la colonne error_type de sys.dm_exec_describe_first_result_set. Si la procédure est appelée de [!INCLUDE[tsql](../../includes/tsql-md.md)], la valeur de retour est toujours de zéro, même en présence d'une erreur.  
  
## <a name="result-sets"></a>Jeux de résultats  
 Ces métadonnées communes sont retournées en tant que jeu de résultats avec une ligne pour chaque colonne dans les métadonnées de résultats. Chaque ligne décrit le type et la possibilité de valeur NULL de la colonne dans le format décrit dans la section suivante. S'il n'existe pas de première instruction pour chaque chemin d'accès de contrôle, un jeu de résultats avec des lignes nulles est retourné.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**is_hidden**|**bit non NULL**|Indique que la colonne est une colonne supplémentaire ajoutée à titre d'informations de navigation et qu'elle ne s'affiche pas réellement dans le jeu de résultats.|  
|**column_ordinal**|**int non NULL**|Contient la position ordinale de la colonne dans le jeu de résultats. La position de la première colonne sera spécifiée comme 1.|  
|**nom**|**sysname NULL**|Contient le nom de la colonne si un nom peut être déterminé. Sinon, il contiendra NULL.|  
|**is_nullable**|**bit non NULL**|Contient la valeur 1 si la colonne autorise des valeurs NULL, 0 si la colonne n'autorise pas de valeurs NULL, et 1 s'il n'est pas possible de déterminer si la colonne autorise des valeurs NULL.|  
|**system_type_id**|**int non NULL**|Contient le system_type_id du type de données de la colonne comme spécifié dans sys.types. Pour les types CLR, bien que la colonne system_type_name retourne NULL, cette colonne retournera la valeur 240.|  
|**system_type_name**|**nvarchar (256) NULL**|Contient le nom et les arguments (tels que la longueur, la précision, l'échelle) spécifiés pour le type de données de la colonne. Si le type de données est un type d'alias défini par l'utilisateur, le type de système sous-jacent est spécifié ici. S'il s'agit d'un type clr défini par l'utilisateur, NULL est retourné dans cette colonne.|  
|**max_length**|**Smallint non NULL**|Longueur maximale (en octets) de la colonne.<br /><br /> -1 = la colonne est de type de données **varchar (max)**, **nvarchar (max)**, **varbinary (max)**, ou **xml**.<br /><br /> Pour **texte** des colonnes, le **max_length** valeur sera 16 ou à la valeur définie par **sp_tableoption 'text in row'**.|  
|**precision**|**tinyint non NULL**|Précision de la colonne si elle est numérique. Dans le cas contraire, retourne la valeur 0.|  
|**scale**|**tinyint non NULL**|Échelle de la colonne si elle est numérique. Dans le cas contraire, retourne la valeur 0.|  
|**collation_name**|**sysname NULL**|Nom du classement de la colonne si elle est basée sur les caractères. Sinon, retourne NULL.|  
|**user_type_id**|**int NULL**|Pour les types d'alias et CLR, contient l'information user_type_id du type de données de la colonne comme spécifié dans sys.types. Sinon, a la valeur NULL.|  
|**user_type_database**|**sysname NULL**|Pour les types d'alias et CLR, contient le nom de la base de données dans laquelle le type est défini. Sinon, a la valeur NULL.|  
|**user_type_schema**|**sysname NULL**|Pour les types d'alias et CLR, contient le nom du schéma dans lequel le type est défini. Sinon, a la valeur NULL.|  
|**user_type_name**|**sysname NULL**|Pour les types d'alias et CLR, contient le nom du type. Sinon, a la valeur NULL.|  
|**assembly_qualified_type_name**|**nvarchar(4000)**|Pour les types CLR, retourne le nom de l'assembly et de la classe définissant le type. Sinon, a la valeur NULL.|  
|**xml_collection_id**|**int NULL**|Contient l'information xml_collection_id du type de données de la colonne comme spécifié dans sys.columns. Cette colonne retournera NULL si le type retourné n'est pas associé à une collection de schémas XML.|  
|**xml_collection_database**|**sysname NULL**|Contient la base de données dans laquelle la collection de schémas XML associée à ce type est définie. Cette colonne retournera NULL si le type retourné n'est pas associé à une collection de schémas XML.|  
|**xml_collection_schema**|**sysname NULL**|Contient le schéma dans lequel la collection de schémas XML associée à ce type est définie. Cette colonne retournera NULL si le type retourné n'est pas associé à une collection de schémas XML.|  
|**xml_collection_name**|**sysname NULL**|Contient le nom de la collection de schémas XML associé à ce type. Cette colonne retournera NULL si le type retourné n'est pas associé à une collection de schémas XML.|  
|**is_xml_document**|**bit non NULL**|Retourne 1 si le type de données retourné est XML et que ce type est garanti être un document XML complet (nœud racine compris), par opposition à un fragment XML. Dans le cas contraire, retourne la valeur 0.|  
|**is_case_sensitive**|**bit non NULL**|Retourne 1 si la colonne est un type chaîne sensible à la casse et 0 si ce n'est pas le cas.|  
|**is_fixed_length_clr_type**|**bit non NULL**|Retourne 1 si la colonne est un type CLR de longueur fixe et 0 si ce n'est pas le cas.|  
|**source_server**|**sysname**|Nom du serveur d'origine retourné par la colonne dans ce résultat (s'il provient d'un serveur distant). Le nom est donné tel qu’il apparaît dans sys.servers. Retourne NULL si la colonne provient du serveur local, ou s'il est impossible de déterminer la provenance du serveur. Est fourni uniquement si les informations de navigation sont demandées.|  
|**source_database**|**sysname**|Nom de la base de données d'origine retourné par la colonne dans ce résultat. Retourne NULL si la base de données ne peut pas être déterminée. Est fourni uniquement si les informations de navigation sont demandées.|  
|**source_schema**|**sysname**|Nom du schéma d'origine retourné par la colonne dans ce résultat. Retourne NULL si le schéma ne peut pas être déterminé. Est fourni uniquement si les informations de navigation sont demandées.|  
|**source_table**|**sysname**|Nom de la table d'origine retourné par la colonne dans ce résultat. Retourne NULL si la table ne peut pas être déterminée. Est fourni uniquement si les informations de navigation sont demandées.|  
|**source_column**|**sysname**|Nom de la colonne d'origine retourné par la colonne de résultat. Retourne NULL si la colonne ne peut pas être déterminée. Est fourni uniquement si les informations de navigation sont demandées.|  
|**is_identity_column**|**bits NULL**|Retourne 1 si la colonne est une colonne d'identité et 0 dans le cas contraire. Retourne NULL s'il est impossible de déterminer que la colonne est une colonne d'identité.|  
|**is_part_of_unique_key**|**bits NULL**|Retourne 1 si la colonne fait partie d'un index unique (notamment la contrainte unique et primaire) et 0 dans le cas contraire. Retourne NULL s'il est impossible de déterminer que la colonne fait partie d'un index unique. Fourni uniquement si les informations de navigation sont demandées.|  
|**is_updateable**|**bits NULL**|Retourne 1 si la colonne peut être mise à jour et 0 dans le cas contraire. Retourne NULL s'il est impossible de déterminer que la colonne peut être mise à jour.|  
|**is_computed_column**|**bits NULL**|Retourne 1 si la colonne est une colonne calculée et 0 dans le cas contraire. Retourne NULL s’il ne peut pas être déterminé que la colonne est une colonne calculée.|  
|**is_sparse_column_set**|**bits NULL**|Retourne 1 si la colonne est une colonne éparse et 0 dans le cas contraire. Retourne NULL s’il ne peut pas être déterminé que la colonne fait partie d’un jeu de colonnes éparses.|  
|**ordinal_in_order_by_list**|**smallint NULL**|Position de cette colonne dans la liste ORDER BY. Retourne NULL si la colonne n’apparaît pas dans la liste ORDER BY ou si la liste ORDER BY ne peut pas être déterminée de manière unique.|  
|**order_by_list_length**|**smallint NULL**|Longueur de la liste ORDER BY. Retourne NULL s'il n'existe aucune liste ORDER BY ou si la liste ORDER BY ne peut pas être déterminée de manière unique. Notez que cette valeur sera la même pour toutes les lignes retournées par **sp_describe_first_result_set.**|  
|**order_by_is_descending**|**smallint NULL**|Si la valeur ordinal_in_order_by_list n’est pas NULL, le **order_by_is_descending** colonne indique la direction de la clause ORDER BY pour cette colonne. Sinon, elle indique NULL.|  
|**tds_type_id**|**int non NULL**|À usage interne uniquement.|  
|**tds_length**|**int non NULL**|À usage interne uniquement.|  
|**tds_collation_id**|**int NULL**|À usage interne uniquement.|  
|**tds_collation_sort_id**|**tinyint NULL**|À usage interne uniquement.|  
  
## <a name="remarks"></a>Notes  
 **sp_describe_first_result_set** garantit que si la procédure retourne les premières métadonnées de jeu de résultats pour (un hypothétique) lot A et si ce lot (A) s’il est ensuite exécutée ensuite le lot sera (1) génère une erreur au moment de l’optimisation, (2) déclenche une erreur d’exécution, (3) ne retourne aucun résultat défini ou (4) retourne un premier jeu de résultats avec les mêmes métadonnées décrites par **sp_describe_first_result_set**.  
  
 Le nom, la possibilité de valeur NULL et le type de données peuvent différer. Si **sp_describe_first_result_set** renvoie un jeu de résultats vide, la garantie est que l’exécution du lot retournera des jeux sans résultats.  
  
 Cette garantie suppose qu'aucune modification de schéma pertinente n'a lieu sur le serveur. Modifications de schéma pertinentes sur le serveur de ne pas inclure la création de tables temporaires ou variables de table dans le lot A entre le moment qui **sp_describe_first_result_set** est appelée et le moment où le jeu de résultats est retourné pendant l’exécution, y compris les modifications de schéma effectuées par le lot B.  
  
 **sp_describe_first_result_set** renvoie une erreur dans un des cas suivants.  
  
-   Si l’entrée @tsql n’est pas valide [!INCLUDE[tsql](../../includes/tsql-md.md)] lot. Validité est déterminée en analysant le [!INCLUDE[tsql](../../includes/tsql-md.md)] lot. Toutes les erreurs provoquées par le traitement au cours de l’optimisation des requêtes ou lors de l’exécution ne sont pas considérés lors de la détermination de si le [!INCLUDE[tsql](../../includes/tsql-md.md)] lot est valide.  
  
-   Si @params n’est pas NULL et contient une chaîne qui n’est pas une chaîne de déclaration de syntaxe valide pour les paramètres, ou si elle contient une chaîne qui déclare un paramètre plusieurs fois.  
  
-   Si l’entrée [!INCLUDE[tsql](../../includes/tsql-md.md)] lot déclare une variable locale du même nom qu’un paramètre déclaré dans @params.  
  
-   Si l'instruction utilise une table temporaire.  
  
-   La requête inclut la création d'une table permanente qui est alors interrogée.  
  
 Si tous les autres contrôles réussissent, tous les chemins d'accès de flux de contrôle possibles à l'intérieur du lot d'entrée sont pris en compte. Cette prend également en compte tous les instructions de flux (GOTO, IF/ELSE, WHILE et [!INCLUDE[tsql](../../includes/tsql-md.md)] blocs TRY/CATCH) ainsi que toutes les procédures, dynamiques [!INCLUDE[tsql](../../includes/tsql-md.md)] traitements ou des déclencheurs appelées à partir du lot d’entrée par une instruction EXEC, une instruction DDL qui entraîne des déclencheurs DDL à être activé ou une instruction DML qui provoque l’activation des déclencheurs sur une table cible ou sur une table qui a été modifiée en raison d’une action en cascade sur une contrainte de clé étrangère. Dans le cas de nombreux chemins d'accès de contrôle possibles, un algorithme s'arrête à un point donné.  
  
 Pour chaque chemin d’accès de flux de contrôle, la première instruction (le cas échéant) qui retourne un jeu de résultats est déterminé par **sp_describe_first_result_set**.  
  
 Lorsque plusieurs premières instructions possibles sont trouvées dans un lot, leurs résultats peuvent différer selon le nombre de colonnes, le nom de colonne, la possibilité de valeur NULL et le type de données. La gestion de ces différences fait l'objet d'une description plus détaillée dans cette rubrique :  
  
-   Si le nombre de colonnes diffère, une erreur est générée et aucun résultat n'est retourné.  
  
-   Si le nom de colonne diffère, le nom de colonne retourné est défini sur NULL.  
  
-   Si la possibilité de valeur NULL diffère, la possibilité de valeur NULL retournée autorisera des valeurs NULL.  
  
-   Si le type de données diffère, une erreur est générée et aucun résultat n'est retourné à l'exception des cas suivants :  
  
    -   **varchar(a)** à **varchar(a')** où un ' > un.  
  
    -   **varchar(a)** à **varchar (max)**  
  
    -   **nvarchar(a)** à **nvarchar(a')** où un ' > un.  
  
    -   **nvarchar(a)** à **nvarchar (max)**  
  
    -   **varbinary(a)** à **varbinary(a')** où un ' > un.  
  
    -   **varbinary(a)** à **varbinary (max)**  
  
 **sp_describe_first_result_set** ne prend pas en charge la récursivité indirecte.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l’autorisation d’exécuter le @tsql argument.  
  
## <a name="examples"></a>Exemples  
  
### <a name="typical-examples"></a>Exemples types  
  
#### <a name="a-simple-example"></a>A. Exemple simple  
 L'exemple suivant décrit le jeu de résultats retourné par une requête unique.  
  
```  
sp_describe_first_result_set @tsql = N'SELECT object_id, name, type_desc FROM sys.indexes'  
```  
  
 L'exemple suivant affiche le jeu de résultats retourné par une requête unique qui contient un paramètre.  
  
```  
sp_describe_first_result_set @tsql =   
N'SELECT object_id, name, type_desc   
FROM sys.indexes   
WHERE object_id = @id1'  
, @params = N'@id1 int'  
```  
  
#### <a name="b-browse-mode-examples"></a>B. Exemples de modes de navigation  
 Les trois exemples suivants illustrent la principale différence entre les différents modes de navigation. Seules les colonnes appropriées ont été incluses dans les résultats de la requête.  
  
 L'exemple qui utilise 0 indique qu'aucune information est retournée.  
  
```sql  
CREATE TABLE dbo.t (a int PRIMARY KEY, b1 int);  
GO  
CREATE VIEW dbo.v AS SELECT b1 AS b2 FROM dbo.t;  
GO  
EXEC sp_describe_first_result_set N'SELECT b2 AS b3 FROM dbo.v', null, 0;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
|is_hidden|column_ordinal|name|source_schema|source_table|source_column|is_part_of_unique_key|  
|----------------|---------------------|----------|--------------------|-------------------|--------------------|-------------------------------|  
|0|1|b3|NULL|NULL|NULL|NULL|  
  
 L'exemple qui utilise 1 indique qu'il retourne les informations comme s'il incluait une option FOR BROWSE sur la requête.  
  
```sql  
EXEC sp_describe_first_result_set N'SELECT b2 AS b3 FROM v', null, 1  
  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
|is_hidden|column_ordinal|name|source_schema|source_table|source_column|is_part_of_unique_key|  
|----------------|---------------------|----------|--------------------|-------------------|--------------------|-------------------------------|  
|0|1|b3|dbo|t|B1|0|  
|1|2|a|dbo|t|a|1|  
  
 L'exemple qui utilise 2 indique une analyse comme si vous prépariez un curseur.  
  
```sql  
EXEC sp_describe_first_result_set N'SELECT b2 AS b3 FROM v', null, 2  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
|is_hidden|column_ordinal|name|source_schema|source_table|source_column|is_part_of_unique_key|  
|----------------|---------------------|----------|--------------------|-------------------|--------------------|-------------------------------|  
|0|1|B3|dbo|v|B2|0|  
|1|2|ROWSTAT|NULL|NULL|NULL|0|  
  
### <a name="examples-of-problems"></a>Exemples de problèmes  
 Les exemples suivants utilisent deux tables pour tous les exemples. Exécutez les instructions suivantes pour créer les tables d'exemples.  
  
```  
CREATE TABLE dbo.t1 (a int NULL, b varchar(10) NULL, c nvarchar(10) NULL);  
CREATE TABLE dbo.t2 (a smallint NOT NULL, d varchar(20) NOT NULL, e int NOT NULL);  
```  
  
#### <a name="error-because-the-number-of-columns-differ"></a>Erreur car le nombre de colonnes diffère  
 Le nombre de colonnes dans les premiers jeux de résultats possibles diffère dans cet exemple.  
  
```  
sp_describe_first_result_set @tsql =   
N'  
IF(1=1)  
    SELECT a FROM t1;  
ELSE  
    SELECT a, b FROM t1;  
SELECT * FROM t; -- Ignored, not a possible first result set.'  
  
```  
  
#### <a name="error-because-the-data-types-differ"></a>Erreur car les types de données diffèrent  
 Les types de colonnes diffèrent dans les premiers jeux de résultats possibles.  
  
```  
sp_describe_first_result_set @tsql =   
N'  
IF(1=1)  
    SELECT a FROM t1;  
ELSE  
    SELECT a FROM t2;  
```  
  
 Résultat : Erreur, types discordants (**int** et **smallint**).  
  
#### <a name="column-name-cannot-be-determined"></a>Le nom de colonne ne peut pas être déterminé  
 Les colonnes dans les premiers jeux de résultats possibles diffèrent de par la longueur pour le même type de longueur variable, la possibilité de valeur NULL et les noms de colonne :  
  
```  
sp_describe_first_result_set @tsql =   
N'  
IF(1=1)  
    SELECT b FROM t1;  
ELSE  
    SELECT d FROM t2; '  
```  
  
 Résultat : \<nom de colonne inconnu > **varchar (20) NULL**  
  
#### <a name="column-name-forced-to-be-identical-through-aliasing"></a>Nom de colonne forcé à être identique par crénelage  
 Identique à l'exemple précédent, mais les colonnes ont le même nom via le crénelage de colonne.  
  
```  
sp_describe_first_result_set @tsql =   
N'  
IF(1=1)  
    SELECT b FROM t1;  
ELSE  
    SELECT d AS b FROM t2;'  
```  
  
 Résultat : b **varchar (20) NULL**  
  
#### <a name="error-because-column-types-cannot-be-matched"></a>Erreur car les types de colonne ne peuvent pas être mis en correspondance  
 Les types de colonnes diffèrent possibles premiers jeux de résultats.  
  
```  
sp_describe_first_result_set @tsql =   
N'  
IF(1=1)  
    SELECT b FROM t1;  
ELSE  
    SELECT c FROM t1;'  
```  
  
 Résultat : Erreur, types discordants (**varchar (10)** et **nvarchar (10)**).  
  
#### <a name="result-set-can-return-an-error"></a>Le jeu de résultats peut retourner une erreur  
 Le premier jeu de résultats est soit une erreur, soit un jeu de résultats.  
  
```  
sp_describe_first_result_set @tsql =   
N'  
IF(1=1)  
    RAISERROR(''Some Error'', 16, 1);  
  
ELSE  
    SELECT a FROM t1;  
SELECT e FROM t2; -- Ignored, not a possible first result set.;'  
```  
  
 Résultat : un **intNULL**  
  
#### <a name="some-code-paths-return-no-results"></a>Certains chemins de code ne retournent pas de résultats  
 Le premier jeu de résultats est soit Null, soit un jeu de résultats.  
  
```  
sp_describe_first_result_set @tsql =   
N'  
IF(1=1)  
    RETURN;  
SELECT a FROM t1;'  
```  
  
 Résultat : un **intNULL**  
  
#### <a name="result-from-dynamic-sql"></a>Résultat du SQL dynamique  
 Le premier jeu de résultats est SQL dynamique qui est détectable car il s'agit d'une chaîne littérale.  
  
```  
sp_describe_first_result_set @tsql =   
N'EXEC(N''SELECT a FROM t1'');'  
```  
  
 Résultat : un **NULL de type INT**  
  
#### <a name="result-failure-from-dynamic-sql"></a>Échec du résultat du SQL dynamique  
 Le premier jeu de résultats est indéfini en raison du SQL dynamique.  
  
```  
sp_describe_first_result_set @tsql =   
N'  
DECLARE @SQL NVARCHAR(max);  
SET @SQL = N''SELECT a FROM t1 WHERE 1 = 1 '';  
IF(1=1)  
    SET @SQL += N'' AND e > 10 '';  
EXEC(@SQL); '  
```  
  
 Résultat : Erreur. Le résultat n'est pas détectable en raison du SQL dynamique.  
  
#### <a name="result-set-specified-by-user"></a>Jeu de résultats spécifié par l'utilisateur  
 Le premier jeu de résultats est spécifié manuellement par l'utilisateur.  
  
```  
sp_describe_first_result_set @tsql =   
N'  
DECLARE @SQL NVARCHAR(max);  
SET @SQL = N''SELECT a FROM t1 WHERE 1 = 1 '';  
IF(1=1)  
    SET @SQL += N'' AND e > 10 '';  
EXEC(@SQL)  
    WITH RESULT SETS(  
        (Column1 BIGINT NOT NULL)  
    ); '  
```  
  
 Résultat : Column1 **bigint non NULL**  
  
#### <a name="error-caused-by-a-ambiguous-result-set"></a>Erreur provoquée par un jeu de résultats ambigu  
 Cet exemple suppose qu’un autre utilisateur nommé user1 a une table nommée t1 dans le schéma par défaut s1 avec des colonnes (une **int non NULL**).  
  
```  
sp_describe_first_result_set @tsql =   
N'  
    IF(@p > 0)  
    EXECUTE AS USER = ''user1'';  
    SELECT * FROM t1;'  
, @params = N'@p int'  
```  
  
 Résultat : Erreur. T1 peut être dbo.t1 ou s1.t1, chacun avec un nombre différent de colonnes.  
  
#### <a name="result-even-with-ambiguous-result-set"></a>Résultat même avec un jeu de résultats ambigu  
 Utilisez les mêmes hypothèses que l'exemple précédent.  
  
```  
sp_describe_first_result_set @tsql =   
N'  
    IF(@p > 0)  
    EXECUTE AS USER = ''user1'';  
    SELECT a FROM t1;'  
```  
  
 Résultat : un **int NULL** car dbo.t1.a et s1.t1.a ont un type **int** et des valeurs NULL différentes.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_describe_undeclared_parameters &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-describe-undeclared-parameters-transact-sql.md)   
 [Sys.dm_exec_describe_first_result_set &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-transact-sql.md)   
 [sys.dm_exec_describe_first_result_set_for_object &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-for-object-transact-sql.md)  
 
