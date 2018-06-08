---
title: Sys.dm_exec_plan_attributes (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 10/20/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_exec_plan_attributes_TSQL
- dm_exec_plan_attributes_TSQL
- dm_exec_plan_attributes
- sys.dm_exec_plan_attributes
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_plan_attributes dynamic management function
ms.assetid: dacf3ab3-f214-482e-aab5-0dab9f0a3648
caps.latest.revision: 30
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 7a815fe805aeb1a1a4f919353225ba04f6e3fb41
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmexecplanattributes-transact-sql"></a>sys.dm_exec_plan_attributes (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne une ligne par attribut de plan pour le plan spécifié par le handle de plan. Cette fonction à valeur de table vous permet d'obtenir des informations sur un plan particulier, tel que les valeurs clé de cache ou le nombre d'exécutions simultanées en cours du plan.  
  
> [!NOTE]  
>  Certaines des informations retournées par cette fonction est mappé à la [sys.syscacheobjects](../../relational-databases/system-compatibility-views/sys-syscacheobjects-transact-sql.md) vue de compatibilité descendante.

## <a name="syntax"></a>Syntaxe  
```  
sys.dm_exec_plan_attributes ( plan_handle )  
```  
  
## <a name="arguments"></a>Arguments  
 *plan_handle*  
 Identifie de façon unique un plan de requête pour un lot exécuté et dont le plan réside dans le cache du plan. *plan_handle* est **varbinary(64)**. Le descripteur de plan peut être obtenu à partir de la [sys.dm_exec_cached_plans](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md) vue de gestion dynamique.  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|attribut|**varchar(128)**|Nom de l'attribut associé à ce plan. Le tableau situé immédiatement sous celle-ci répertorie les attributs possibles, leurs types de données et leurs descriptions.|  
|valeur|**sql_variant**|Valeur de l'attribut associé à ce plan.|  
|is_cache_key|**bit**|Indique si l'attribut est utilisé comme une partie de la clé de recherche en cache pour le plan.|  

Dans le tableau ci-dessus, **attribut** peut avoir les valeurs suivantes :

|Attribut|Type de données| Description|  
|---------------|---------------|-----------------|  
|set_options|**int**|Indique les valeurs d'option ayant servi à compiler le plan.|  
|objectid|**int**|Une des clés principales servant à rechercher un objet dans le cache. Il s’agit d’ID d’objet stocké dans [sys.objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md) pour les objets de base de données (procédures, vues, déclencheurs, etc.). Pour des plans de type « Adhoc » ou « Prepared », il s'agit d'un hachage interne du texte du lot.|  
|dbid|**int**|ID de la base de données contenant l'entité à laquelle le plan fait référence.<br /><br /> Pour des plans ad hoc ou préparés, il s'agit de l'ID de la base de données à partir duquel est exécuté le lot.|  
|dbid_execute|**int**|Pour les objets système stockés dans le **ressources** de base de données, l’ID de base de données à partir de laquelle le plan mis en cache est exécuté. Dans tous les autres cas, il est égal à 0.|  
|user_id|**int**|Une valeur de -2 indique que le lot soumis ne dépend pas de la résolution implicite des noms et peut être partagé entre différents utilisateurs. Cette méthode est recommandée. Toute autre valeur représente l'ID de l'utilisateur soumettant la requête dans la base de données.| 
|language_id|**smallint**|ID de la langue de la connexion qui a créé l'objet dans le cache. Pour plus d’informations, consultez [sys.syslanguages &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md).|  
|date_format|**smallint**|Format de date de la connexion qui a créé l'objet dans le cache Pour plus d’informations, consultez [SET DATEFORMAT &#40;Transact-SQL&#41;](../../t-sql/statements/set-dateformat-transact-sql.md).|  
|date_first|**tinyint**|Valeur date first. Pour plus d’informations, consultez [SET DATEFIRST &#40;Transact-SQL&#41;](../../t-sql/statements/set-datefirst-transact-sql.md).|  
|status|**int**|Bits d'état interne qui font partie de la clé de recherche en cache.|  
|required_cursor_options|**int**|Options de curseur spécifiées par l'utilisateur (type de curseur par exemple).|  
|acceptable_cursor_options|**int**|Options de curseur dans lesquelles [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut convertir implicitement afin de prendre en charge l'exécution de l'instruction. Par exemple, l'utilisateur peut spécifier un curseur dynamique, mais l'optimiseur de requête peut convertir ce type de curseur en curseur statique.|  
|inuse_exec_context|**int**|Nombre de lots en cours d'exécution qui font appel au plan de requête.|  
|free_exec_context|**int**|Nombre de contextes d'exécution en cache pour le plan de requête qui ne sont pas actuellement utilisés.|  
|hits_exec_context|**int**|Nombre d'obtention et de réutilisation du contexte d'exécution à partir du cache du plan évitant ainsi de recompiler l'instruction SQL. La valeur est une agrégation de toutes les exécutions de lot jusqu'à présent.|  
|misses_exec_context|**int**|Nombre de fois un contexte d'exécution était introuvable dans le cache de plan entraînant la création d'un nouveau contexte d'exécution pour l'exécution du lot.|  
|removed_exec_context|**int**|Nombre de contextes d'exécution ayant été supprimés en raison d'une mémoire insuffisante dans le plan en cache.|  
|inuse_cursors|**int**|Nombre de lots en cours d'exécution contenant un ou plusieurs curseurs qui font appel au plan en cache.|  
|free_cursors|**int**|Nombre de curseurs libres ou inactifs du plan en cache.|  
|hits_cursors|**int**|Nombre d'obtention et de réutilisation d'un curseur inactif à partir du plan en cache. La valeur est une agrégation de toutes les exécutions de lot jusqu'à présent.|  
|misses_cursors|**int**|Nombre de fois où un curseur inactif était introuvable dans le cache.|  
|removed_cursors|**int**|Nombre de curseurs ayant été supprimés en raison d'une mémoire insuffisante dans le plan en cache.|  
|sql_handle|**varbinary**(64)|Handle SQL du lot.|  
|merge_action_type|**smallint**|Type du plan d'exécution du déclencheur utilisé à la suite d'une instruction MERGE.<br /><br /> 0 indique un plan de non-déclencheur, un plan de déclencheur qui ne s'exécute pas à la suite d'une instruction MERGE ou un plan de déclencheur qui s'exécute à la suite d'une instruction MERGE qui spécifie uniquement une action DELETE.<br /><br /> 1 indique un plan de déclencheur INSERT qui s'exécute à la suite d'une instruction MERGE.<br /><br /> 2 indique un plan de déclencheur UPDATE qui s'exécute à la suite d'une instruction MERGE.<br /><br /> 3 indique un plan de déclencheur DELETE qui s'exécute à la suite d'une instruction MERGE contenant une action INSERT ou UPDATE correspondante.<br /><br /> Pour les déclencheurs imbriqués exécutés par des actions en cascade, cette valeur correspond à l'action de l'instruction MERGE qui a provoqué la cascade.|  
  
## <a name="permissions"></a>Autorisations  

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], nécessite `VIEW SERVER STATE` autorisation.   
Sur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], nécessite le `VIEW DATABASE STATE` autorisation dans la base de données.   

## <a name="remarks"></a>Notes  
  
## <a name="set-options"></a>Définir les options  
 Les copies du même plan compilé peuvent différer uniquement par la valeur dans la **set_options** colonne. Cela signifie que des connexions différentes font appel à différents jeux d'options SET pour la même requête. L'utilisation de différents jeux d'option n'est pas souhaitable généralement car elle est source de complications supplémentaires, d'une réutilisation insuffisante du plan et d'une augmentation du cache du plan en raison de la présence de plusieurs copies dans le cache.  
  
### <a name="evaluating-set-options"></a>Évaluation des options définies  
 Pour convertir la valeur retournée dans **set_options** dans les options ayant servi à compiler le plan, il faut soustraire les valeurs à partir de la **set_options** valeur, en commençant par la plus grande valeur possible, jusqu'à ce que vous atteigniez 0. Chaque valeur soustraite correspond à une option utilisée dans le plan de requête. Par exemple, si la valeur de **set_options** est 251, les options que le plan a été compilé avec ANSI_NULL_DFLT_ON (128), QUOTED_IDENTIFIER (64), ANSI_NULLS(32), ANSI_WARNINGS (16), CONCAT_NULL_YIELDS_NULL (8), Plan(2) parallèle et ANSI_PADDING (1).  
  
|Option|Valeur|  
|------------|-----------|  
|ANSI_PADDING|1|  
|Plan en parallèle|2|  
|FORCEPLAN|4|  
|CONCAT_NULL_YIELDS_NULL|8|  
|ANSI_WARNINGS|16|  
|ANSI_NULLS|32|  
|QUOTED_IDENTIFIER|64|  
|ANSI_NULL_DFLT_ON|128|  
|ANSI_NULL_DFLT_OFF|256|  
|NoBrowseTable<br /><br /> Indique que le plan n'utilise pas une table de travail pour implémenter une opération FOR BROWSE.|512|  
|TriggerOneRow<br /><br /> Indique que le plan contient une optimisation de ligne unique pour les tables delta de déclencheur AFTER.|1024|  
|ResyncQuery<br /><br /> Indique que la requête a été soumise par des procédures stockées système internes.|2048|  
|ARITH_ABORT|4096|  
|NUMERIC_ROUNDABORT|8192|  
|DATEFIRST|16384|  
|DATEFORMAT|32768|  
|LanguageID|65536|  
|UPON<br /><br /> Indique que l'option de base de données PARAMETERIZATION avait pour valeur FORCED lorsque le plan a été compilé.|131072|  
|ROWCOUNT|**S’applique à :** [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]<br /><br /> 262144|  
  
## <a name="cursors"></a>Curseurs  
 Les curseurs inactifs sont mis en cache dans un plan compilé pour que la mémoire utilisée pour stocker le curseur soit réutilisée par des utilisateurs simultanés des curseurs. Par exemple, supposez qu'un lot déclare et utilise un curseur sans le désallouer. Si deux utilisateurs exécutent le même lot, il y aura deux curseurs actifs. Une fois les curseurs désalloués (éventuellement dans des lots différents), la mémoire utilisée pour stocker le curseur est mise en cache et n'est pas libérée. Cette liste des curseurs inactifs est conservée dans le plan compilé. À la prochaine exécution du lot par un utilisateur, la mémoire de curseur en cache est réutilisée et initialisée correctement comme curseur actif.  
  
### <a name="evaluating-cursor-options"></a>Évaluation des options de curseur  
 Pour convertir la valeur retournée dans **required_cursor_options** et **acceptable_cursor_options** dans les options ayant servi à compiler le plan, il faut soustraire les valeurs à partir de la valeur de colonne, en commençant par la plus grande valeur possible, jusqu'à ce que vous atteigniez 0. Chaque valeur soustraite correspond à une option de curseur utilisée dans le plan de requête.  
  
|Option|Valeur|  
|------------|-----------|  
|Aucun|0|  
|INSENSITIVE|1|  
|SCROLL|2|  
|READ ONLY|4|  
|FOR UPDATE|8|  
|LOCAL|16|  
|GLOBAL|32|  
|FORWARD_ONLY|64|  
|KEYSET|128|  
|DYNAMIC|256|  
|SCROLL_LOCKS|512|  
|OPTIMISTIC|1024|  
|STATIC|2048|  
|FAST_FORWARD|4096|  
|IN PLACE|8192|  
|FOR *select_statement*|16384|  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-returning-the-attributes-for-a-specific-plan"></a>A. Retour des attributs pour un plan spécifique  
 L'exemple suivant retourne tous les attributs de plan pour un plan spécifié. Dans la première requête, la vue de gestion dynamique `sys.dm_exec_cached_plans` est interrogée pour obtenir le descripteur de plan du plan spécifié. Dans la deuxième requête, remplacez `<plan_handle>` par une valeur de descripteur de plan issue de la première requête.  
  
```sql  
SELECT plan_handle, refcounts, usecounts, size_in_bytes, cacheobjtype, objtype   
FROM sys.dm_exec_cached_plans;  
GO  
SELECT attribute, value, is_cache_key  
FROM sys.dm_exec_plan_attributes(<plan_handle>);  
GO  
```  
  
### <a name="b-returning-the-set-options-for-compiled-plans-and-the-sql-handle-for-cached-plans"></a>B. Retour des options SET pour les plans compilés et du handle SQL pour les plans en cache  
 L'exemple suivant retourne une valeur représentant les options ayant servi à compiler chaque plan. Il retourne également le handle SQL de tous les plans mis en cache.  
  
```sql  
SELECT plan_handle, pvt.set_options, pvt.sql_handle  
FROM (  
    SELECT plan_handle, epa.attribute, epa.value   
    FROM sys.dm_exec_cached_plans   
        OUTER APPLY sys.dm_exec_plan_attributes(plan_handle) AS epa  
    WHERE cacheobjtype = 'Compiled Plan') AS ecpa   
PIVOT (MAX(ecpa.value) FOR ecpa.attribute IN ("set_options", "sql_handle")) AS pvt;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Fonctions et vues de gestion dynamique liées à l’exécution &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [Sys.dm_exec_cached_plans &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)   
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)  
  
  

