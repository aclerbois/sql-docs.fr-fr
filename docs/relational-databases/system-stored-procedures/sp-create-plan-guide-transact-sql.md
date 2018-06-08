---
title: sp_create_plan_guide (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_create_plan_guide
- sp_create_plan_guide_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_create_plan_guide
ms.assetid: 5a8c8040-4f96-4c74-93ab-15bdefd132f0
caps.latest.revision: 82
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 79e07848de5827f172e298f96d2ec55d30422eb5
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spcreateplanguide-transact-sql"></a>sp_create_plan_guide (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Crée un repère de plan permettant d'associer des indicateurs de requête ou des plans de requête réels à des requêtes d'une base de données. Pour plus d'informations sur les repères de plan, consultez [Plan Guides](../../relational-databases/performance/plan-guides.md).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_create_plan_guide [ @name = ] N'plan_guide_name'  
    , [ @stmt = ] N'statement_text'  
    , [ @type = ] N'{ OBJECT | SQL | TEMPLATE }'  
    , [ @module_or_batch = ]  
      {   
                    N'[ schema_name. ] object_name'  
        | N'batch_text'  
        | NULL  
      }  
    , [ @params = ] { N'@parameter_name data_type [ ,...n ]' | NULL }   
    , [ @hints = ] { N'OPTION ( query_hint [ ,...n ] )'   
                 | N'XML_showplan'  
                 | NULL }  
```  
  
## <a name="arguments"></a>Arguments  
 [ @name =] N'*plan_guide_name*'  
 Est le nom du repère de plan. Les noms des repères de plan sont limités à la base de données active. *plan_guide_name* doivent respecter les règles pour [identificateurs](../../relational-databases/databases/database-identifiers.md) et ne peut pas commencer par le signe dièse (#). La longueur maximale de *plan_guide_name* est de 124 caractères.  
  
 [ @stmt =] N'*statement_text*'  
 Instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] permettant de créer un repère de plan. Lorsque le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] requête optimiseur reconnaît une requête qui correspond à *statement_text*, *plan_guide_name* en vigueur. Pour la création d’un repère de plan réussisse, *statement_text* doit apparaître dans le contexte spécifié par le @type, @module_or_batch, et @params paramètres.  
  
 *statement_text* doit être fourni d’une façon qui permet de faire correspondre à l’instruction correspondante fournie dans le lot ou le module identifié par l’optimiseur de requête @module_or_batch et @params. Pour plus d'informations, consultez la section « Remarques ». La taille de *statement_text* est limité uniquement par la mémoire disponible du serveur.  
  
 [@type =] N'{OBJET | SQL | MODÈLE} »  
 Est le type d’entité dans laquelle *statement_text* s’affiche. Elle spécifie le contexte pour la correspondance *statement_text* à *plan_guide_name*.  
  
 OBJECT  
 Indique *statement_text* s’affiche dans le contexte d’un [!INCLUDE[tsql](../../includes/tsql-md.md)] stockées procédure, fonction scalaire, fonction table multi-instructions, ou [!INCLUDE[tsql](../../includes/tsql-md.md)] déclencheur DML dans la base de données actuelle.  
  
 SQL  
 Indique *statement_text* s’affiche dans le contexte d’une instruction autonome ou un lot qui peut être envoyée qu’à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] via un mécanisme quelconque. [!INCLUDE[tsql](../../includes/tsql-md.md)] instructions soumises par des objets common language runtime (CLR) ou des procédures stockées étendues, ou à l’aide de EXEC N'*chaîne_sql*», sont traités comme des lots sur le serveur et, par conséquent, doivent être identifiés en tant que @type **=** 'SQL'. Si SQL est spécifié, l’indicateur de requête PARAMETERIZATION {FORCED | SIMPLE} ne peut pas être spécifié dans le @hints paramètre.  
  
 TEMPLATE  
 Indique que le repère de plan s’applique à toute requête qui paramètre selon le formulaire indiqué dans *statement_text*. Si le modèle est spécifié, seuls le paramétrage {FORCED | Indicateur de requête SIMPLE} peut être spécifié dans le @hints paramètre. Pour plus d’informations sur les repères de plan TEMPLATE, consultez [spécifier le comportement du paramétrage requête à l’aide de repères de Plan](../../relational-databases/performance/specify-query-parameterization-behavior-by-using-plan-guides.md).  
  
 [@module_or_batch =] {N'[ *nom_schéma*. ] *nom_objet*' | N'*batch_text*' | NULL}  
 Spécifie le nom de l’objet dans lequel *statement_text* s’affiche, ou le texte du lot dans lequel *statement_text* s’affiche. Le texte du lot ne peut pas inclure une utilisation*base de données* instruction.  
  
 Pour un repère de plan corresponde à un lot soumis à partir d’une application, *batch_tex*t doit être fourni dans le même format caractère pour caractère, qu’il est soumis à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Aucune conversion interne n'est effectuée pour faciliter cette correspondance. Pour plus d'informations, consultez la section Notes.  
  
 [*schema_name*.] *nom_objet* Spécifie le nom d’un [!INCLUDE[tsql](../../includes/tsql-md.md)] stockées procédure, fonction scalaire, fonction table multi-instructions, ou [!INCLUDE[tsql](../../includes/tsql-md.md)] déclencheur DML qui contient *statement_text*. Si *nom_schéma* n’est pas spécifié, *schema_name* utilise le schéma de l’utilisateur actuel. Si NULL est spécifié et @type = 'SQL', la valeur de @module_or_batch est définie à la valeur de @stmt. Si @type = ' modèle **'**, @module_or_batch doit être NULL.  
  
 [ @params =] {N' *@parameter_name data_type* [,*.. .n* ]' | NULL}  
 Spécifie les définitions de tous les paramètres qui sont incorporés dans *statement_text*. @params s’applique uniquement lorsque une des conditions suivantes est remplie :  
  
-   @type = 'SQL' ou 'TEMPLATE'. Si 'TEMPLATE', @params ne doit pas être NULL.  
  
-   *statement_text* est soumise à l’aide de sp_executesql et une valeur pour le @params paramètre est spécifié, ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] soumet en interne une instruction après l’avoir paramétrée. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] considère la soumission de requêtes paramétrables à partir des API de bases de données (y compris ODBC, OLE DB et ADO.NET) comme des appels à sp_executesql ou aux routines de curseur côté serveur d'API. Par conséquent, elles peuvent également être mises en correspondance par des repères de plan SQL ou TEMPLATE.  
  
 *@parameter_name data_type* doit être fourni dans exactement le même format qu’il est soumis à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’aide de sp_executesql ou soumis en interne après le paramétrage. Pour plus d'informations, consultez la section Notes. Si le traitement ne contient aucun paramètre, la valeur NULL doit être spécifiée. La taille de @params est limité uniquement par la mémoire du serveur disponible.  
  
 [@hints =] {N’option (*query_hint* [,*.. .n* ])' | N'*XML_showplan*' | NULL}  
 N’option (*query_hint* [,*.. .n* ])  
 Spécifie une clause OPTION à joindre à une requête qui correspond à @stmt. @hints doit être syntaxiquement identique à une clause OPTION dans une instruction SELECT et peut contenir toute séquence valide d’indicateurs de requête.  
  
 N'*XML_showplan*'  
 Plan de requête dans le format XML à appliquer comme un indicateur.  
  
 Nous vous recommandons d'assigner le plan d'exécution de requêtes XML à une variable ; sinon, vous devez isoler tout guillemet simple dans le plan d'exécution en le faisant précéder par un autre guillemet simple. Voir l'exemple E.  
  
 NULL  
 Indique qu'aucun indicateur existant spécifié dans la clause OPTION de la requête n'est appliqué à la requête. Pour plus d’informations, consultez [Clause OPTION &#40;Transact-SQL&#41;](../../t-sql/queries/option-clause-transact-sql.md).  
  
## <a name="remarks"></a>Notes  
 Les arguments de sp_create_plan_guide doivent être indiqués dans l'ordre affiché. Quand vous fournissez des valeurs pour les paramètres de **sp_create_plan_guide**, tous les noms de paramètres doivent être spécifiés explicitement, ou aucun nom ne doit être spécifié. Par exemple, si **@name =** est spécifié, alors **@stmt =**, **@type =**, etc.) doit l’être aussi. De même, si **@name =** est omis et que seule la valeur du paramètre est indiquée, les noms de paramètres restants doivent également être omis, et seules leurs valeurs doivent être indiquées. Les noms d'arguments sont utilisés à des fins descriptives uniquement, pour une meilleure compréhension de la syntaxe. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne vérifie pas que le nom de paramètre spécifié correspond au nom du paramètre à l'emplacement où le nom est utilisé.  
  
 Vous pouvez créer plusieurs repères de plan OBJECT ou SQL pour la même requête et le même lot ou module. Toutefois, un seul repère de plan peut être activé à un moment donné.  
  
 Vous ne pouvez pas créer de repère de plan de type OBJECT pour une valeur @module_or_batch qui fait référence à une procédure stockée, une fonction ou un déclencheur DML temporaire ou qui spécifie la clause WITH ENCRYPTION.  
  
 Si vous tentez de supprimer ou de modifier une fonction, une procédure stockée ou un déclencheur DML référencé par un repère de plan, qu'il soit activé ou désactivé, une erreur se produit. Une erreur se produit également si vous tentez de supprimer une table dont un des déclencheurs est référencé par un repère de plan.  
  
> [!NOTE]  
>  Les repères de plan ne peuvent pas être utilisés dans toutes les éditions de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prise en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md). Les repères de plan sont visibles dans n'importe quelle édition. En outre, vous pouvez attacher une base de données qui contient des repères de plan à n'importe quelle édition. Les repères de plan demeurent intacts lorsque vous restaurez ou attachez une base de données à une version mise à niveau de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Vous devez vérifier les avantages des repères de plan dans chaque base de données après avoir réalisé une mise à niveau de serveur.  
  
## <a name="plan-guide-matching-requirements"></a>Paramétrage de la mise en correspondance du repère de plan  
 Les repères de plan qui spécifient @type = 'SQL' ou @type = 'TEMPLATE' pour mettre en correspondance une requête, les valeurs de *batch_text* et  *@parameter_name data_type* [,*.. .n* ] doit être fourni exactement dans le même format que leurs homologues soumis par l’application. Par conséquent, vous devez indiquer le texte du traitement exactement de la même manière que le compilateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l'a reçu. Pour saisir le texte réel du traitement et du paramètre, vous pouvez utiliser [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]. Pour plus d’informations, consultez [utiliser SQL Server Profiler pour créer et les repères de Plan de Test](../../relational-databases/performance/use-sql-server-profiler-to-create-and-test-plan-guides.md).  
  
 Quand @type= 'SQL' et que @module_or_batch a la valeur NULL, la valeur définie pour de @module_or_batch est celle de @stmt. Cela signifie que la valeur de *statement_text* doit être fourni exactement dans le même format, caractère pour caractère, qu’il est soumis à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Aucune conversion interne n'est effectuée pour faciliter cette correspondance.  
  
 Lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] correspond à la valeur de *statement_text* à *batch_text* et  *@parameter_name data_type* [,*.. .n* ], ou si @type = **'** objet », au texte de la requête correspondante dans *nom_objet*, les éléments de chaîne suivants ne sont pas considérés comme :  
  
-   les espaces (tabulations, espaces, retours chariot ou sauts de ligne) à l'intérieur de la chaîne ;  
  
-   Commentaires (**--** ou **/ \* \* /**).  
  
-   les points-virgules situés à la fin.  
  
 Par exemple, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut correspondre à la *statement_text* chaîne `N'SELECT * FROM T WHERE a = 10'` à ce qui suit *batch_text*:  
  
 `N'SELECT *`  
  
 `FROM T`  
  
 `WHERE a=10'`  
  
 Toutefois, la même chaîne serait ne correspond à ce *batch_text*:  
  
 `N'SELECT * FROM T WHERE b = 10'`  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ignore les retours chariots, les sauts de lignes et les espaces au sein de la première requête. Dans la seconde, la séquence `WHERE b = 10` est interprétée différemment de `WHERE a = 10`. La mise en correspondance respecte la casse et les accents (même lorsque le classement de la base de données ne respecte pas la casse), sauf pour les mots clés, pour lesquels la casse n'est pas respectée. La mise en correspondance n'est en outre pas sensible aux formes abrégées des mots clés. Par exemple, les mots clés `EXECUTE`, `EXEC` et `execute` sont considérés comme équivalents.  
  
## <a name="plan-guide-effect-on-the-plan-cache"></a>Effet du repère de plan sur le cache du plan  
 La création d'un repère de plan sur un module supprime le plan de requête pour ce module du cache du plan. La création d'un repère de plan de type OBJET ou SQL sur un lot supprime le plan de requête pour un lot qui a la même valeur de hachage. La création d'un repère de plan de type TEMPLATE supprime tous les lots comprenant une instruction unique du cache du plan dans cette base de données.  
  
## <a name="permissions"></a>Autorisations  
 Pour créer un repère de plan de type OBJECT, il vous faut une autorisation ALTER sur l'objet référencé. Pour créer un repère de plan de type SQL ou TEMPLATE, il vous faut une autorisation ALTER pour la base de données active.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-creating-a-plan-guide-of-type-object-for-a-query-in-a-stored-procedure"></a>A. Création d'un repère de plan de type OBJECT pour une requête dans une procédure stockée  
 L'exemple ci-après crée un repère de plan qui correspond à une requête exécutée dans le contexte d'une procédure stockée basée sur une application et applique l'indicateur `OPTIMIZE FOR` à la requête.  
  
 Voici la procédure stockée :  
  
```  
IF OBJECT_ID(N'Sales.GetSalesOrderByCountry', N'P') IS NOT NULL  
    DROP PROCEDURE Sales.GetSalesOrderByCountry;  
GO  
CREATE PROCEDURE Sales.GetSalesOrderByCountry   
    (@Country_region nvarchar(60))  
AS  
BEGIN  
    SELECT *  
    FROM Sales.SalesOrderHeader AS h   
    INNER JOIN Sales.Customer AS c ON h.CustomerID = c.CustomerID  
    INNER JOIN Sales.SalesTerritory AS t   
        ON c.TerritoryID = t.TerritoryID  
    WHERE t.CountryRegionCode = @Country_region;  
END  
GO  
```  
  
 Et voici le repère de plan créé pour la requête dans la procédure stockée :  
  
```  
EXEC sp_create_plan_guide   
    @name =  N'Guide1',  
    @stmt = N'SELECT *  
              FROM Sales.SalesOrderHeader AS h   
              INNER JOIN Sales.Customer AS c   
                 ON h.CustomerID = c.CustomerID  
              INNER JOIN Sales.SalesTerritory AS t   
                 ON c.TerritoryID = t.TerritoryID  
              WHERE t.CountryRegionCode = @Country_region',  
    @type = N'OBJECT',  
    @module_or_batch = N'Sales.GetSalesOrderByCountry',  
    @params = NULL,  
    @hints = N'OPTION (OPTIMIZE FOR (@Country_region = N''US''))';  
```  
  
### <a name="b-creating-a-plan-guide-of-type-sql-for-a-stand-alone-query"></a>B. Création d'un repère de plan de type SQL pour une requête autonome  
 L'exemple suivant crée un repère de plan à mettre en correspondance avec une requête dans un lot soumis par une application qui utilise la procédure stockée système sp_executesql.  
  
 Voici le traitement :  
  
```  
SELECT TOP 1 * FROM Sales.SalesOrderHeader ORDER BY OrderDate DESC;  
```  
  
 Pour éviter la génération d'une exécution parallèle pour cette requête, créez le repère de plan suivant :  
  
```  
EXEC sp_create_plan_guide   
    @name = N'Guide1',   
    @stmt = N'SELECT TOP 1 *   
              FROM Sales.SalesOrderHeader   
              ORDER BY OrderDate DESC',   
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (MAXDOP 1)';  
```  
  
### <a name="c-creating-a-plan-guide-of-type-template-for-the-parameterized-form-of-a-query"></a>C. Création d'un repère de plan de type TEMPLATE pour la forme paramétrable d'une requête  
 L'exemple suivant crée un repère de plan correspondant à la requête qui paramètre selon une forme donnée, et commande à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d'imposer le paramétrage de la requête. La syntaxe des deux requêtes suivantes est équivalente, seules leurs valeurs littérales constantes diffèrent.  
  
```  
SELECT * FROM AdventureWorks2012.Sales.SalesOrderHeader AS h  
INNER JOIN AdventureWorks2012.Sales.SalesOrderDetail AS d   
    ON h.SalesOrderID = d.SalesOrderID  
WHERE h.SalesOrderID = 45639;  
  
SELECT * FROM AdventureWorks2012.Sales.SalesOrderHeader AS h  
INNER JOIN AdventureWorks2012.Sales.SalesOrderDetail AS d   
    ON h.SalesOrderID = d.SalesOrderID  
WHERE h.SalesOrderID = 45640;  
```  
  
 Voici le repère de plan pour la forme paramétrable de la requête :  
  
```  
EXEC sp_create_plan_guide   
    @name = N'TemplateGuide1',  
    @stmt = N'SELECT * FROM AdventureWorks2012.Sales.SalesOrderHeader AS h  
              INNER JOIN AdventureWorks2012.Sales.SalesOrderDetail AS d   
                  ON h.SalesOrderID = d.SalesOrderID  
              WHERE h.SalesOrderID = @0',  
    @type = N'TEMPLATE',  
    @module_or_batch = NULL,  
    @params = N'@0 int',  
    @hints = N'OPTION(PARAMETERIZATION FORCED)';  
```  
  
 Dans l'exemple précédent, la valeur du paramètre `@stmt` correspond à la forme paramétrable de la requête. La procédure stockée système [sp_get_query_template](../../relational-databases/system-stored-procedures/sp-get-query-template-transact-sql.md) est la seule méthode fiable pour obtenir cette valeur et pouvoir l’utiliser dans sp_create_plan_guide. Vous pouvez utiliser le script suivant à la fois pour obtenir la requête paramétrable et créer ensuite un repère de plan à partir de celle-ci.  
  
```  
DECLARE @stmt nvarchar(max);  
DECLARE @params nvarchar(max);  
EXEC sp_get_query_template   
    N'SELECT * FROM AdventureWorks2012.Sales.SalesOrderHeader AS h  
      INNER JOIN AdventureWorks2012.Sales.SalesOrderDetail AS d   
          ON h.SalesOrderID = d.SalesOrderID  
      WHERE h.SalesOrderID = 45639;',  
    @stmt OUTPUT,   
    @params OUTPUT  
EXEC sp_create_plan_guide N'TemplateGuide1',   
    @stmt,   
    N'TEMPLATE',   
    NULL,   
    @params,   
    N'OPTION(PARAMETERIZATION FORCED)';  
```  
  
> [!IMPORTANT]  
>  Les valeurs littérales constantes du paramètre `@stmt` transmises à sp_get_query_template peuvent affecter le type de données choisi pour le paramètre qui remplace le littéral. Ceci va également affecter la mise en correspondance du repère de plan. Vous devrez peut-être créer plusieurs repères de plan pour traiter plusieurs plages de valeurs.  
  
### <a name="d-creating-a-plan-guide-on-a-query-submitted-by-using-an-api-cursor-request"></a>D. Création d'un repère de plan pour une requête soumise à l'aide d'une requête de curseur API  
 Les repères de plan peuvent correspondre à des requêtes soumises à partir de routines de curseur côté serveur d'API. Ces routines incluent sp_cursorprepare, sp_cursorprepexec et sp_cursoropen. Les applications qui utilisent les API ADO, OLE DB et ODBC interagissent fréquemment avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] grâce à l'utilisation de curseurs côté serveur d'API. Pour voir l'invocation des routines de curseur côté serveur d'API dans les traces [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], il vous suffit de voir l'événement de trace du générateur de profils  RPC:Starting.  
  
 Supposez que les données suivantes apparaissent dans un événement de trace du générateur de profils RPC:Starting pour une requête que vous souhaitez ajuster au moyen d'un repère de plan :  
  
```  
DECLARE @p1 int;  
SET @p1=-1;  
DECLARE @p2 int;  
SET @p2=0;  
DECLARE @p5 int;  
SET @p5=4104;  
DECLARE @p6 int;  
SET @p6=8193;  
DECLARE @p7 int;  
SET @p7=0;  
EXEC sp_cursorprepexec @p1 output,@p2 output,N'@P1 varchar(255),@P2 varchar(255)',N'SELECT * FROM Sales.SalesOrderHeader AS h INNER JOIN Sales.SalesOrderDetail AS d ON h.SalesOrderID = d.SalesOrderID WHERE h.OrderDate BETWEEN @P1 AND @P2',@p5 OUTPUT,@p6 OUTPUT,@p7 OUTPUT,'20040101','20050101'  
SELECT @p1, @p2, @p5, @p6, @p7;  
```  
  
 Vous constatez que le plan de la requête `SELECT` dans l'appel à `sp_cursorprepexec` utilise une jointure de fusion mais vous souhaitez utiliser une jointure de hachage. La requête soumise à l'aide de `sp_cursorprepexec` est paramétrable, y compris une chaîne de requête et une chaîne de paramètre. Vous pouvez créer le repère de plan suivant pour choisir un autre plan en utilisant les chaînes de requête et de paramètre exactement comme elles apparaissent, au caractère près, dans l'appel à `sp_cursorprepexec`.  
  
```  
EXEC sp_create_plan_guide   
    @name = N'APICursorGuide',  
    @stmt = N'SELECT * FROM Sales.SalesOrderHeader AS h   
              INNER JOIN Sales.SalesOrderDetail AS d   
                ON h.SalesOrderID = d.SalesOrderID   
              WHERE h.OrderDate BETWEEN @P1 AND @P2',  
    @type = N'SQL',  
    @module_or_batch = NULL,  
    @params = N'@P1 varchar(255),@P2 varchar(255)',  
    @hints = N'OPTION(HASH JOIN)';  
```  
  
 Les exécutions ultérieures de cette requête seront affectées par ce repère de plan et une jointure de hachage sera utilisée pour traiter la requête.  
  
### <a name="e-creating-a-plan-guide-by-obtaining-the-xml-showplan-from-a-cached-plan"></a>E. Création d'un repère de plan en obtenant le plan d'exécution de requêtes XML à partir d'un plan mis en cache  
 L'exemple suivant crée un repère de plan pour une instruction SQL ad hoc simple. Le plan de requête souhaité pour cette instruction est fourni dans le repère de plan en spécifiant directement le plan d'exécution XML pour la requête dans le paramètre `@hints` . L'exemple exécute en premier l'instruction SQL pour générer un plan dans le cache du plan. Pour les besoins de cet exemple, il est supposé que le plan généré est le plan souhaité et qu'aucune analyse de requête supplémentaire n'est requise. Le plan d'exécution de requêtes XML pour la requête est obtenu en interrogeant les vues de gestion dynamique `sys.dm_exec_query_stats`, `sys.dm_exec_sql_text`et `sys.dm_exec_text_query_plan` , et est assigné à la variable `@xml_showplan` . La variable `@xml_showplan` est ensuite transmise à l'instruction `sp_create_plan_guide` dans le paramètre `@hints` . Vous pouvez aussi créer un repère de plan à partir d’un plan de requête dans le cache des plans à l’aide de la procédure stockée [sp_create_plan_guide_from_handle](../../relational-databases/system-stored-procedures/sp-create-plan-guide-from-handle-transact-sql.md) .  
  
```  
USE AdventureWorks2012;  
GO  
SELECT City, StateProvinceID, PostalCode FROM Person.Address ORDER BY PostalCode DESC;  
GO  
DECLARE @xml_showplan nvarchar(max);  
SET @xml_showplan = (SELECT query_plan  
    FROM sys.dm_exec_query_stats AS qs   
    CROSS APPLY sys.dm_exec_sql_text(qs.sql_handle) AS st  
    CROSS APPLY sys.dm_exec_text_query_plan(qs.plan_handle, DEFAULT, DEFAULT) AS qp  
    WHERE st.text LIKE N'SELECT City, StateProvinceID, PostalCode FROM Person.Address ORDER BY PostalCode DESC;%');  
  
EXEC sp_create_plan_guide   
    @name = N'Guide1_from_XML_showplan',   
    @stmt = N'SELECT City, StateProvinceID, PostalCode FROM Person.Address ORDER BY PostalCode DESC;',   
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints =@xml_showplan;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Repères de plan](../../relational-databases/performance/plan-guides.md)   
 [sp_control_plan_guide &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-control-plan-guide-transact-sql.md)   
 [sys.plan_guides &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-plan-guides-transact-sql.md)   
 [Procédures stockées du moteur de base de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sys.dm_exec_sql_text &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)   
 [Sys.dm_exec_cached_plans &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)   
 [sys.dm_exec_query_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)   
 [sp_create_plan_guide_from_handle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-create-plan-guide-from-handle-transact-sql.md)   
 [sys.fn_validate_plan_guide &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-validate-plan-guide-transact-sql.md)   
 [sp_get_query_template &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-get-query-template-transact-sql.md)  
  
  
