---
title: sp_describe_undeclared_parameters (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_describe_undeclared_parameters
- sp_describe_undeclared_parameters_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_describe_undeclared_parameters
ms.assetid: 6f016da6-dfee-4228-8b0d-7cd8e7d5a354
caps.latest.revision: 22
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: fe290d9c7447524a9f1a7afe7f17294970280bb2
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spdescribeundeclaredparameters-transact-sql"></a>sp_describe_undeclared_parameters (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-asdb-xxxx-xxx-md.md)]

  Retourne un jeu de résultats qui contient des métadonnées sur les paramètres non déclarés dans un [!INCLUDE[tsql](../../includes/tsql-md.md)] lot. Considère chaque paramètre qui est utilisé dans le **@tsql** du lot, mais non déclaré dans **@params**. Le jeu de résultats retourné contient une ligne pour chaque paramètre de ce genre, avec les informations de type déduites pour ce paramètre. La procédure retourne un résultat vide, définissez si les **@tsql** lot d’entrée n’a aucun paramètre, à l’exception de ceux déclarés dans **@params**.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_describe_undeclared_parameters   
    [ @tsql = ] 'Transact-SQL_batch'   
    [ , [ @params = ] N'parameters' data type ] [, ...n]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@tsql =** ] **'***SQL_batch transact***'**  
 Une ou plusieurs instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] *Transact-SQL_batch* peut être **nvarchar (***n***)** ou **nvarchar (max)**.  
  
 [  **@params =** ] **N'***paramètres***'**  
 @params Fournit une chaîne de déclaration pour les paramètres pour le [!INCLUDE[tsql](../../includes/tsql-md.md)] fonctionne par lot, de même façon que sp_executesql. *Paramètres* peut être **nvarchar (***n***)** ou **nvarchar (max)**.  
  
 Est une chaîne qui contient les définitions de tous les paramètres qui ont été incorporés dans *Transact-SQL_batch*. Cette chaîne doit être une constante Unicode ou une variable Unicode. Chaque définition de paramètre se compose d'un nom de paramètre et d'un type de données. n correspond à un espace réservé pour d'autres définitions de paramètres. Si l’instruction Transact-SQL ou un lot dans l’instruction ne contient pas de paramètres, @params n’est pas obligatoire. La valeur par défaut de ce paramètre est NULL.  
  
 Type de données  
 Type de données du paramètre.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **sp_describe_undeclared_parameters** toujours retourne statut de retour de zéro en cas de réussite. Si la procédure génère une erreur et la procédure est appelée comme RPC, l’état de retour est rempli par le type d’erreur, comme décrit dans la colonne error_type de sys.dm_exec_describe_first_result_set. Si la procédure est appelée depuis [!INCLUDE[tsql](../../includes/tsql-md.md)], la valeur de retour est toujours égale à zéro, même en cas d'erreur.  
  
## <a name="result-sets"></a>Jeux de résultats  
 **sp_describe_undeclared_parameters** renvoie le jeu de résultats suivant.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**parameter_ordinal**|**int non NULL**|Contient la position ordinale du paramètre dans le jeu de résultats. La position du premier paramètre sera spécifiée comme 1.|  
|**nom**|**sysname non NULL**|Contient le nom du paramètre.|  
|**suggested_system_type_id**|**int non NULL**|Contient le **system_type_id** du type de données du paramètre comme spécifié dans sys.types.<br /><br /> Pour les types CLR, bien que le **system_type_name** colonne retourne NULL, cette colonne renvoie la valeur 240.|  
|**suggested_system_type_name**|**nvarchar (256) NULL**|Contient le nom du type de données. Inclut des arguments (tels que la longueur, la précision, l'échelle) spécifiés pour le type de données du paramètre. Si le type de données est un type d'alias défini par l'utilisateur, le type de système sous-jacent est spécifié ici. S'il s'agit d'un type de données CLR défini par l'utilisateur, NULL est retourné dans cette colonne. Si le type du paramètre ne peut pas être déduit, NULL est retourné.|  
|**suggested_max_length**|**Smallint non NULL**|Consultez sys.columns. pour **max_length** description de la colonne.|  
|**suggested_precision**|**tinyint non NULL**|Consultez sys.columns. pour la description de la colonne de précision.|  
|**suggested_scale**|**tinyint non NULL**|Consultez sys.columns. pour la description de la colonne d'échelle.|  
|**suggested_user_type_id**|**int NULL**|Pour les types d'alias et CLR, contient l'information user_type_id du type de données de la colonne comme spécifié dans sys.types. Sinon, a la valeur NULL.|  
|**suggested_user_type_database**|**sysname NULL**|Pour les types d'alias et CLR, contient le nom de la base de données dans laquelle le type est défini. Sinon, a la valeur NULL.|  
|**suggested_user_type_schema**|**sysname NULL**|Pour les types d'alias et CLR, contient le nom du schéma dans lequel le type est défini. Sinon, a la valeur NULL.|  
|**suggested_user_type_name**|**sysname NULL**|Pour les types d'alias et CLR, contient le nom du type. Sinon, a la valeur NULL.|  
|**suggested_assembly_qualified_type_name**|**nvarchar (4000) NULL**|Pour les types CLR, retourne le nom de l'assembly et de la classe qui définit le type. Sinon, a la valeur NULL.|  
|**suggested_xml_collection_id**|**int NULL**|Contient l’information xml_collection_id du type de données du paramètre comme spécifié dans sys.columns. Cette colonne retournera NULL si le type retourné n'est pas associé à une collection de schémas XML.|  
|**suggested_xml_collection_database**|**sysname NULL**|Contient la base de données dans laquelle la collection de schémas XML associée à ce type est définie. Cette colonne retournera NULL si le type retourné n'est pas associé à une collection de schémas XML.|  
|**suggested_xml_collection_schema**|**sysname NULL**|Contient le schéma dans lequel la collection de schémas XML associée à ce type est définie. Cette colonne retournera NULL si le type retourné n'est pas associé à une collection de schémas XML.|  
|**suggested_xml_collection_name**|**sysname NULL**|Contient le nom de la collection de schémas XML associé à ce type. Cette colonne retournera NULL si le type retourné n'est pas associé à une collection de schémas XML.|  
|**suggested_is_xml_document**|**bit non NULL**|Retourne 1 si le type qui est retourné est XML et que ce type est garanti être un document XML. Dans le cas contraire, retourne la valeur 0.|  
|**suggested_is_case_sensitive**|**bit non NULL**|Retourne 1 si la colonne est d'un type chaîne sensible à la casse et 0 si ce n'est pas le cas.|  
|**suggested_is_fixed_length_clr_type**|**bit non NULL**|Retourne 1 si la colonne est d'un type CLR de longueur fixe et 0 si ce n'est pas le cas.|  
|**suggested_is_input**|**bit non NULL**|Retourne 1 si le paramètre est utilisé n'importe où autre que le côté gauche d'une attribution. Dans le cas contraire, retourne la valeur 0.|  
|**suggested_is_output**|**bit non NULL**|Retourne 1 si le paramètre est utilisé du côté gauche d'une attribution ou est passé à un paramètre de sortie d'une procédure stockée. Dans le cas contraire, retourne la valeur 0.|  
|**formal_parameter_name**|**sysname NULL**|Si le paramètre est un argument d'une procédure stockée ou une fonction définie par l'utilisateur, retourne le nom du paramètre formel correspondant. Sinon, retourne NULL.|  
|**suggested_tds_type_id**|**int non NULL**|À usage interne uniquement.|  
|**suggested_tds_length**|**int non NULL**|À usage interne uniquement.|  
  
## <a name="remarks"></a>Notes  
 **sp_describe_undeclared_parameters** retourne retourne toujours 0.  
  
 Le cas d'utilisation le plus courant est celui d'une application qui reçoit une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] pouvant contenir des paramètres et devant les traiter d'une certaine façon. Un exemple est une interface utilisateur (par exemple, ODBCTest ou RowsetViewer) où l’utilisateur fournit une requête avec la syntaxe de paramètre ODBC. L'application doit découvrir dynamiquement le nombre de paramètres et inviter l'utilisateur à fournir chacun d'eux.  
  
 Autre exemple : en l'absence d'entrée de la part de l'utilisateur, une application doit faire une boucle sur les paramètres et obtenir les données pour ces derniers depuis un autre emplacement (tel qu'une table). Dans ce cas, l'application ne doit pas passer l'ensemble des informations de paramètre à la fois. À la place, l'application peut obtenir toutes les informations de paramètres du fournisseur et obtenir les données proprement dites de la table. À l’aide de code **sp_describe_undeclared_parameters** est plus générique et est susceptible de nécessiter une modification si la structure de données modifications ultérieurement.  
  
 **sp_describe_undeclared_parameters** renvoie une erreur dans un des cas suivants.  
  
-   Si l’entrée @tsql n’est pas valide [!INCLUDE[tsql](../../includes/tsql-md.md)] lot. Validité est déterminée en analysant le [!INCLUDE[tsql](../../includes/tsql-md.md)] lot. Toutes les erreurs provoquées par le traitement au cours de l’optimisation des requêtes ou lors de l’exécution ne sont pas considérés lors de la détermination de si le [!INCLUDE[tsql](../../includes/tsql-md.md)] lot est valide.  
  
-   Si @params n’est pas NULL et contient une chaîne qui n’est pas une chaîne de déclaration de syntaxe valide pour les paramètres, ou si elle contient une chaîne qui déclare un paramètre plusieurs fois.  
  
-   Si l’entrée [!INCLUDE[tsql](../../includes/tsql-md.md)] lot déclare une variable locale du même nom qu’un paramètre déclaré dans @params.  
  
-   Si l'instruction crée des tables temporaires.  
  
 Si @tsql n’a aucun paramètre, autres que ceux déclarés dans @params, la procédure retourne un jeu de résultats vide.  
  
## <a name="parameter-selection-algorithm"></a>Algorithme de sélection du paramètre  
 Dans le cas d'une requête avec des paramètres non déclarés, la déduction de type de données pour les paramètres non déclarés s'effectue en trois étapes.  
  
 **Étape 1**  
  
 La première étape dans la déduction du type de données pour une requête avec des paramètres non déclarés consiste à rechercher les types de données de toutes les sous-expressions dont les types de données ne dépendent pas des paramètres non déclarés. Le type peut être déterminé pour les expressions suivantes :  
  
-   Colonnes, constantes, variables et paramètres déclarés.  
  
-   Résultats d'un appel à une fonction définie par l'utilisateur (UDF).  
  
-   Expression dont les types de données ne dépendent pas des paramètres non déclarés pour toutes les entrées.  
  
 Par exemple, envisagez la requête `SELECT dbo.tbl(@p1) + c1 FROM t1 WHERE c2 = @p2 + 2`. Les expressions dbo.tbl (@p1) + c1 et c2 ont des types de données et expression @p1 et @p2 + 2 ne sont pas.  
  
 Après cette étape, si une expression (autre qu'un appel à une fonction UDF) compte deux arguments sans types de données, la déduction du type se solde par une erreur. Les exemples suivants entraînent tous des erreurs :  
  
```  
SELECT * FROM t1 WHERE @p1 = @p2  
SELECT * FROM t1 WHERE c1 = @p1 + @p2  
SELECT * FROM t1 WHERE @p1 = SUBSTRING(@p2, 2, 3)  
```  
  
 L'exemple suivant ne génère pas d'erreur :  
  
```  
SELECT * FROM t1 WHERE @p1 = dbo.tbl(c1, @p2, @p3)  
```  
  
 **Étape 2**  
  
 Pour un paramètre non déclaré donné @p, l’algorithme de déduction de type recherche l’expression la plus profonde E (@p) qui contient @p et est une des opérations suivantes :  
  
-   Argument d'une comparaison ou d'opérateur d'assignation.  
  
-   Argument d'une fonction définie par l'utilisateur (notamment une fonction UDF table), d'une procédure ou d'une méthode.  
  
-   Argument d’un **valeurs** clause d’une **insérer** instruction.  
  
-   Argument d’un **CAST** ou **convertir**.  
  
 L’algorithme de déduction de type recherche un type de données cible TT (@p) pour E (@p). Les types de données cibles des exemples précédents sont les suivants :  
  
-   Type de données de l'autre côté de la comparaison ou de l'attribution.  
  
-   Type de données déclaré du paramètre auquel cet argument est passé.  
  
-   Type de données de la colonne dans laquelle cette valeur est insérée.  
  
-   Type de données vers lequel l'instruction effectue la conversion.  
  
 Par exemple, envisagez la requête `SELECT * FROM t WHERE @p1 = dbo.tbl(@p2 + c1)`. Puis E (@p1) = @p1, E (@p2) = @p2 + c1, TT (@p1) est le type de données de retour déclaré de dbo.tbl et TT (@p2) est le type de données de paramètre déclaré pour dbo.tbl.  
  
 Si @p n’est pas contenue dans une expression répertoriée au début de l’étape 2, l’algorithme de déduction de type détermine que E (@p) est la plus grande expression scalaire qui contient @p, et l’algorithme de déduction de type ne calcule pas un type de données cible TT (@p) pour E (@p). Par exemple, si la requête est SELECT `@p + 2` E puis (@p) = @p + 2, et il n’existe aucun TT (@p).  
  
 **Étape 3**  
  
 Maintenant que E (@p) et TT (@p) sont identifiés, l’algorithme de déduction de type déduit un type de données pour @p dans un des deux manières suivantes :  
  
-   Déduction simple  
  
     Si E (@p) = @p et TT (@p) existe, par exemple, si @p est directement un argument à une des expressions répertoriées au début de l’étape 2, l’algorithme de déduction de type déduit le type de données @p être TT (@p). Par exemple :  
  
    ```  
    SELECT * FROM t WHERE c1 = @p1 AND @p2 = dbo.tbl(@p3)  
    ```  
  
     Le type de données @p1, @p2, et @p3 sera le type de données de c1, le type de données de retour de dbo.tbl et le type de données de paramètre pour dbo.tbl respectivement.  
  
     Comme un cas particulier, si @p est un argument d’un \<, >, \<=, ou > =, opérateur, la déduction simple des règles ne s’appliquent pas. L'algorithme de la déduction du type utilisera les règles de déduction générales expliquées dans la section suivante. Par exemple, si c1 est une colonne de type de données char(30), considérez les deux requêtes suivantes :  
  
    ```  
    SELECT * FROM t WHERE c1 = @p  
    SELECT * FROM t WHERE c1 > @p  
    ```  
  
     Dans le premier cas, l’algorithme de déduction de type déduit **char (30)** comme type de données de @p conformément aux règles précédemment dans cette rubrique. Dans le deuxième cas, l’algorithme de déduction de type déduit **varchar(8000)** selon les règles de déduction générales décrites dans la section suivante.  
  
-   Déduction générale  
  
     Si la déduction simple ne s'applique pas, les types de données suivants sont considérés pour les paramètres non déclarés :  
  
    -   Types de données Integer (**bits**, **tinyint**, **smallint**, **int**, **bigint**)  
  
    -   Types de données Money (**smallmoney**, **money**)  
  
    -   Types de données à virgule flottante (**float**, **réel**)  
  
    -   **numérique (38, 19)** -autres types de données décimal ou numérique ne sont pas pris en compte.  
  
    -   **varchar(8000)**, **varchar (max)**, **nvarchar (4000)**, et **nvarchar (max)** - les autres types de données string (tels que **texte**, **char (8000)**, **nvarchar (30)**, etc.) ne sont pas considérés.  
  
    -   **varbinary (8000)** et **varbinary (max)** -autres types de données binaires ne sont pas considérés (tels que **image**, **binary(8000)**, **varbinary (30)** , etc..).  
  
    -   **date**, **Time (7)**, **smalldatetime**, **datetime**, **datetime2 (7)**, **datetimeoffset(7)**  - Les autres date et heure de types, tels que **time(4)**, ne sont pas considérés.  
  
    -   **sql_variant**  
  
    -   **xml**  
  
    -   Types CLR définis par le système (**hierarchyid**, **geometry**, **geography**)  
  
    -   Types CLR définis par l’utilisateur  
  
### <a name="selection-criteria"></a>Critères de sélection  
 Parmi les types de données candidats, tout type de données qui invaliderait la requête est rejeté. Parmi les types de données candidats restants, l'algorithme de déduction du type en sélectionne un d'après les règles suivantes.  
  
1.  Le type de données qui produit le plus petit nombre de conversions implicites dans E (@p) est sélectionné. Si un type de données particulier produit un type de données pour E (@p) qui est différent de TT (@p), l’algorithme de déduction de type considère qu’il s’agit d’une conversion implicite supplémentaire du type de données de E (@p) à TT (@p).  
  
     Par exemple :  
  
    ```  
    SELECT * FROM t WHERE Col_Int = Col_Int + @p  
    ```  
  
     Dans ce cas, E (@p) est Col_Int + @p et TT (@p) est **int**. **int** est choisi pour @p car il ne produit pas de conversions implicites. Tout autre choix de type de données produit au moins une conversion implicite.  
  
2.  Si plusieurs types de données sont liés pour le plus petit nombre de conversions, le type de données dont la priorité est supérieure est utilisé. Par exemple  
  
    ```  
    SELECT * FROM t WHERE Col_Int = Col_smallint + @p  
    ```  
  
     Dans ce cas, **int** et **smallint** entraînent une conversion. Chaque autre type de données entraîne plusieurs conversions. Étant donné que **int** est prioritaire sur **smallint**, **int** est utilisé pour @p. Pour plus d’informations sur la priorité des types de données, consultez [priorité des types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-type-precedence-transact-sql.md).  
  
     Cette règle s'applique uniquement s'il existe une conversion implicite entre chaque type de données lié d'après la règle 1 et le type de données présentant la priorité la plus élevée. S'il n'existe aucune conversion implicite, la déduction du type de données se solde par une erreur. Par exemple, dans la requête `SELECT @p FROM t`, Échec de la déduction de type de données, car tout type de données pour @p serait également correcte par rapport. Par exemple, il n’existe aucune conversion implicite de **int** à **xml**.  
  
3.  Si deux types de données semblables sont liés sous la règle 1, par exemple **varchar(8000)** et **varchar (max)**, le plus petit type de données (**varchar(8000)**) est choisie. Le même principe s’applique aux **nvarchar** et **varbinary** des types de données.  
  
4.  Pour les besoins de la règle 1, l'algorithme de la déduction du type préfère certaines conversions plutôt que d'autres. Conversions classées de la meilleure à la pire :  
  
    1.  Conversion entre un même type de données de base de longueur différente.  
  
    2.  Conversion entre la version de longueur fixe et de longueur variable de mêmes types de données (par exemple, **char** à **varchar**).  
  
    3.  Conversion entre **NULL** et **int**.  
  
    4.  Toute autre conversion.  
  
 Par exemple, pour la requête `SELECT * FROM t WHERE [Col_varchar(30)] > @p`, **varchar(8000)** est choisi car la conversion (a) est préférable. Pour la requête `SELECT * FROM t WHERE [Col_char(30)] > @p`, **varchar(8000)** est encore choisi car il provoque une conversion de type (b) et un autre choix (tel que **varchar (4000)**) provoque une conversion de type (d).  
  
 En dernier exemple, considérons une requête `SELECT NULL + @p`, **int** est choisi pour @p , car il en résulte une conversion de type (c).  
  
## <a name="permissions"></a>Autorisations  
 Requiert l’autorisation d’exécuter le @tsql argument.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne des informations telles que le type de données attendu pour les paramètres non déclarés `@id` et `@name`.  
  
```  
sp_describe_undeclared_parameters @tsql =   
N'SELECT object_id, name, type_desc   
FROM sys.indexes  
WHERE object_id = @id OR name = @name'  
  
```  
  
 Lorsque le paramètre `@id` est fourni comme référence `@params`, le paramètre `@id` est omis du jeu de résultats et seul le paramètre `@name` est décrit.  
  
```  
sp_describe_undeclared_parameters @tsql =   
N'SELECT object_id, name, type_desc   
FROM sys.indexes  
WHERE object_id = @id OR NAME = @name',  
@params = N'@id int'  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_describe_first_result_set &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md)   
 [Sys.dm_exec_describe_first_result_set &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-transact-sql.md)   
 [sys.dm_exec_describe_first_result_set_for_object &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-for-object-transact-sql.md)  
  
  
