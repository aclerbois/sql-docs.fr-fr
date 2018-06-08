---
title: Sys.dm_exec_text_query_plan (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 10/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_exec_text_query_plan
- sys.dm_exec_text_query_plan_TSQL
- dm_exec_text_query_plan_TSQL
- sys.dm_exec_text_query_plan
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_text_query_plan dynamic management function
ms.assetid: 9d5e5f59-6973-4df9-9eb2-9372f354ca57
caps.latest.revision: 10
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: d8e7f577a0939312123b398e3be1ab5b4cce0cd8
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmexectextqueryplan-transact-sql"></a>sys.dm_exec_text_query_plan (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Retourne le plan d'exécution de requêtes au format texte pour un lot [!INCLUDE[tsql](../../includes/tsql-md.md)] ou pour une instruction spécifique dans le lot. Le plan de requête spécifié par le descripteur de plan peut être mis en cache ou en cours d’exécution. Cette fonction table est similaire à [sys.dm_exec_query_plan &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md), mais présente les différences suivantes :  
  
-   La sortie du plan de requête est au format texte.  
  
-   La taille de cette sortie n'est pas limitée.  
  
-   Les instructions individuelles dans le traitement peuvent être spécifiées.  
  
**S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu’à la [version actuelle](http://go.microsoft.com/fwlink/p/?LinkId=299658)), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sys.dm_exec_text_query_plan   
(   
    plan_handle   
    , { statement_start_offset | 0 | DEFAULT }  
        , { statement_end_offset | -1 | DEFAULT }  
)  
```  
  
## <a name="arguments"></a>Arguments  
*plan_handle*  
Identifie de façon univoque un plan de requête pour un traitement en cache ou en cours d'exécution. *plan_handle* est **varbinary(64)**.  
  
Le descripteur de plan peut être obtenu à partir des objets de gestion dynamiques suivants :  
  
-  [sys.dm_exec_cached_plans](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)  
  
-  [sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  
  
-  [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  
  
*statement_start_offset* | 0 | PAR DÉFAUT  
Indique, en octets, la position de début de la requête que la ligne décrit dans le texte de son traitement ou de son objet persistant. *statement_start_offset* est **int**. La valeur 0 indique le début du traitement. La valeur par défaut est 0 :  
  
Le décalage de début de l'instruction peut être obtenu à partir des objets de gestion dynamiques suivants :  
  
-  [sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  
  
-  [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  
  
*statement_end_offset* | -1 | PAR DÉFAUT  
Indique, en octets, la position de fin de la requête que la ligne décrit dans le texte de son traitement ou de son objet persistant.  
  
*statement_start_offset* est **int**.  
  
La valeur -1 indique la fin du traitement. La valeur par défaut est -1.  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**dbid**|**smallint**|ID de la base de données de contexte qui était en fonction lorsque l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] correspondant à ce plan a été compilée. Pour les instructions SQL ad hoc et préparées, l'ID de la base de données où les instructions ont été compilées.<br /><br /> Colonne acceptant la valeur NULL.|  
|**objectid**|**int**|ID de l'objet (par exemple, procédure stockée ou fonction définie par l'utilisateur) pour ce plan de requête. Pour les traitements ad hoc et préparées, cette colonne est **null**.<br /><br /> Colonne acceptant la valeur NULL.|  
|**number**|**smallint**|Entier servant à la numérotation des procédures stockées. Par exemple, un groupe de procédures pour le **commandes** application peut-être être appelée **orderproc ; 1**, **orderproc ; 2**, et ainsi de suite. Pour les traitements ad hoc et préparées, cette colonne est **null**.<br /><br /> Colonne acceptant la valeur NULL.|  
|**Chiffré**|**bit**|Indique si la procédure stockée correspondante est chiffrée.<br /><br /> 0 = Non chiffrée.<br /><br /> 1 = Chiffrée.<br /><br /> Colonne n'acceptant pas la valeur NULL.|  
|**query_plan**|**nvarchar(max)**|Contient la représentation sous forme de plan d’exécution lors de la compilation du plan de l’exécution de requête est spécifié avec *plan_handle*. Le Showplan est au format texte. Un plan est généré pour chaque traitement contenant par exemple des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] ad hoc, des appels de procédures stockées et des appels de fonctions définies par l'utilisateur.<br /><br /> Colonne acceptant la valeur NULL.|  
  
## <a name="remarks"></a>Notes  
 Dans les conditions suivantes, aucune sortie Showplan est retournée dans le **plan** colonne de la table retournée pour **sys.dm_exec_text_query_plan**:  
  
-   Si le plan de requête est spécifié à l’aide de *plan_handle* a été supprimé du cache du plan, le **query_plan** colonne de la table retournée est null. Par exemple, ceci peut se produire s’il existe un délai entre le moment où le descripteur de plan est capturé et lorsqu’il a été utilisé avec **sys.dm_exec_text_query_plan**.  
  
-   Certaines instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] ne sont pas mises en mémoire cache, par exemple les instructions d'opérations en bloc ou les instructions contenant des littéraux de chaîne dont la taille est supérieure à 8 Ko. Plans d’exécution pour de telles instructions ne peuvent pas être récupérées à l’aide de **sys.dm_exec_text_query_plan** car ils n’existent pas dans le cache.  
  
-   Si un [!INCLUDE[tsql](../../includes/tsql-md.md)] lot ou une procédure stockée contient un appel à une fonction définie par l’utilisateur ou un appel de code SQL dynamique, par exemple via la commande EXEC (*chaîne*), le compilé Showplan XML pour la fonction définie par l’utilisateur n’est pas incluse dans la table retournée par **sys.dm_exec_text_query_plan** du lot ou une procédure stockée. Vous devez procéder à un appel séparé à **sys.dm_exec_text_query_plan** pour le *plan_handle* qui correspond à la fonction définie par l’utilisateur.  
  
Lorsqu’une requête ad hoc utilise [simple](../../relational-databases/query-processing-architecture-guide.md#SimpleParam) ou [paramétrage forcé](../../relational-databases/query-processing-architecture-guide.md#ForcedParam), le **query_plan** colonne contiendra uniquement le texte de l’instruction et pas le plan de requête. Pour retourner le plan de requête, appelez **sys.dm_exec_text_query_plan** pour le descripteur de plan de la requête paramétrable préparée. Vous pouvez déterminer si la requête a été paramétrée en référençant la **sql** colonne de la [sys.syscacheobjects](../../relational-databases/system-compatibility-views/sys-syscacheobjects-transact-sql.md) vue ou la colonne de texte de la [sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md) vue de gestion dynamique.  
  
## <a name="permissions"></a>Autorisations  
 Pour exécuter **sys.dm_exec_text_query_plan**, un utilisateur doit être un membre de la **sysadmin** rôle serveur fixe ou disposer de l’autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-retrieving-the-cached-query-plan-for-a-slow-running-transact-sql-query-or-batch"></a>A. Récupération du plan de requête mis en mémoire cache pour un lot ou une requête Transact-SQL à exécution lente  
 Si une requête ou un traitement [!INCLUDE[tsql](../../includes/tsql-md.md)] s'exécute longtemps sur une connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] spécifique, récupérez le plan d'exécution de cette requête ou de ce traitement pour trouver la raison de ce retard. L'exemple suivant montre la récupération du plan d'exécution de requêtes pour une requête ou un traitement s'exécutant lentement.  
  
> [!NOTE]  
> Pour exécuter cet exemple, remplacez les valeurs de *session_id* et *plan_handle* avec des valeurs spécifiques à votre serveur.  
  
 Récupérez tout d'abord à l'aide de la procédure stockée `sp_who` l'ID de processus serveur (SPID) pour le processus exécutant la requête ou le traitement.  
  
```sql  
USE master;  
GO  
EXEC sp_who;  
GO  
```  
  
 Le jeu de résultats renvoyé par `sp_who` indique que le SPID est `54`. Utilisez cet identificateur avec la vue de gestion dynamique `sys.dm_exec_requests` pour récupérer le descripteur de plan via la requête suivante :  
  
```sql  
USE master;  
GO  
SELECT * FROM sys.dm_exec_requests  
WHERE session_id = 54;  
GO  
```  
  
 Le tableau retourné par **sys.dm_exec_requests** indique que le descripteur de plan pour la requête ou le lot à exécution lente est `0x06000100A27E7C1FA821B10600`. L'exemple suivant retourne le plan de requête pour le descripteur de plan spécifié et utilise les valeurs par défaut 0 et -1 pour retourner toutes les instructions dans la requête ou le lot.  
  
```sql  
USE master;  
GO  
SELECT query_plan   
FROM sys.dm_exec_text_query_plan (0x06000100A27E7C1FA821B10600,0,-1);  
GO  
```  
  
### <a name="b-retrieving-every-query-plan-from-the-plan-cache"></a>B. Récupération de chaque plan de requête à partir de la mémoire cache des plans  
 Pour récupérer un instantané de tous les plans de requête résidant dans la mémoire cache des plans, procurez-vous les descripteurs de tous les plans de requête dans la mémoire cache via une requête dans la vue de gestion dynamique `sys.dm_exec_cached_plans`. Les descripteurs de plan sont stockés dans la colonne `plan_handle` de `sys.dm_exec_cached_plans`. Utilisez ensuite l'opérateur CROSS APPLY pour transmettre les descripteurs à `sys.dm_exec_text_query_plan` comme suit. Le plan d’exécution de sortie pour chaque plan actuellement dans le cache du plan se trouve dans le `query_plan` colonne de la table qui est retournée.  
  
```sql  
USE master;  
GO  
SELECT *   
FROM sys.dm_exec_cached_plans AS cp   
CROSS APPLY sys.dm_exec_text_query_plan(cp.plan_handle, DEFAULT, DEFAULT);  
GO  
```  
  
### <a name="c-retrieving-every-query-plan-for-which-the-server-has-gathered-query-statistics-from-the-plan-cache"></a>C. La récupération de chaque plan de requête pour lequel le serveur a regroupé des statistiques de requête du cache du plan  
 Pour récupérer un instantané de tous les plans de requête pour lesquels le serveur a regroupé des statistiques actuellement dans la mémoire cache des plans, procurez-vous les descripteurs de ces plans dans la mémoire cache via une requête formulée dans la vue de gestion dynamique `sys.dm_exec_query_stats`. Les descripteurs de plan sont stockés dans la colonne `plan_handle` de `sys.dm_exec_query_stats`. Utilisez ensuite l'opérateur CROSS APPLY pour transmettre les descripteurs à `sys.dm_exec_text_query_plan` comme suit. La sortie du plan d'exécution de requêtes pour chaque plan est dans la colonne `query_plan` de la table retournée.  
  
```sql  
USE master;  
GO  
SELECT * FROM sys.dm_exec_query_stats AS qs   
CROSS APPLY sys.dm_exec_text_query_plan(qs.plan_handle, qs.statement_start_offset, qs.statement_end_offset);  
GO  
```  
  
### <a name="d-retrieving-information-about-the-top-five-queries-by-average-cpu-time"></a>D. Récupérer des informations sur les cinq premières requêtes par temps processeur moyen  
 L'exemple suivant retourne les plans de requête et le temps processeur moyen pour les cinq premières requêtes. Le **sys.dm_exec_text_query_plan** fonction spécifie les valeurs par défaut 0 et -1 pour retourner toutes les instructions dans le lot dans le plan de requête.  
  
```sql  
SELECT TOP 5 total_worker_time/execution_count AS [Avg CPU Time],  
Plan_handle, query_plan   
FROM sys.dm_exec_query_stats AS qs  
CROSS APPLY sys.dm_exec_text_query_plan(qs.plan_handle, 0, -1)  
ORDER BY total_worker_time/execution_count DESC;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sys.dm_exec_query_plan &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)  
