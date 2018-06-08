---
title: Sys.dm_exec_query_plan (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 08/02/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_exec_query_plan_TSQL
- sys.dm_exec_query_plan
- dm_exec_query_plan
- sys.dm_exec_query_plan_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_query_plan dynamic management function
ms.assetid: e26f0867-9be3-4b2e-969e-7f2840230770
caps.latest.revision: 33
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 36eb4273ebdc689320ff831268a508fa977997fb
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmexecqueryplan-transact-sql"></a>sys.dm_exec_query_plan (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne le plan d'exécution de requêtes au format XML pour le traitement spécifié par le descripteur de plan. Le plan spécifié par le descripteur de plan peut être en cache ou en cours d'exécution.  
  
 Le schéma XML pour le Showplan est publié et disponible sur [ce site Web Microsoft](http://go.microsoft.com/fwlink/?linkid=43100&clcid=0x409). Vous le trouverez également dans le répertoire d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.dm_exec_query_plan ( plan_handle )  
```  
  
## <a name="arguments"></a>Arguments  
 *plan_handle*  
 Identifie de façon univoque un plan de requête pour un traitement en cache ou en cours d'exécution.  
  
 *plan_handle* est **varbinary(64)**. *plan_handle* peut être obtenu à partir des objets de gestion dynamique suivants :  
  
 [sys.dm_exec_cached_plans](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)  
  
 [sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  
  
 [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**dbid**|**smallint**|ID de la base de données de contexte qui était en fonction lorsque l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] correspondant à ce plan a été compilée. Pour les instructions SQL ad hoc et préparées, l'ID de la base de données où les instructions ont été compilées.<br /><br /> Colonne acceptant la valeur NULL.|  
|**objectid**|**int**|ID de l'objet (par exemple, procédure stockée ou fonction définie par l'utilisateur) pour ce plan de requête. Pour les traitements ad hoc et préparées, cette colonne est **null**.<br /><br /> Colonne acceptant la valeur NULL.|  
|**number**|**smallint**|Entier servant à la numérotation des procédures stockées. Par exemple, un groupe de procédures pour le **commandes** application peut-être être appelée **orderproc ; 1**, **orderproc ; 2**, et ainsi de suite. Pour les traitements ad hoc et préparées, cette colonne est **null**.<br /><br /> Colonne acceptant la valeur NULL.|  
|**Chiffré**|**bit**|Indique si la procédure stockée correspondante est chiffrée.<br /><br /> 0 = Non chiffrée.<br /><br /> 1 = Chiffrée.<br /><br /> Colonne n'acceptant pas la valeur NULL.|  
|**query_plan**|**xml**|Contient la représentation sous forme de plan d’exécution lors de la compilation du plan de l’exécution de requête est spécifié avec *plan_handle*. Le plan d'exécution de requêtes est au format XML. Un plan est généré pour chaque traitement contenant par exemple des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] ad hoc, des appels de procédures stockées et des appels de fonctions définies par l'utilisateur.<br /><br /> Colonne acceptant la valeur NULL.|  
  
## <a name="remarks"></a>Notes  
 Dans les conditions suivantes, aucune sortie Showplan est retournée dans le **query_plan** colonne de la table retournée pour **sys.dm_exec_query_plan**:  
  
-   Si le plan de requête est spécifié à l’aide de *plan_handle* a été supprimé du cache du plan, le **query_plan** colonne de la table retournée est null. Par exemple, ceci peut se produire s’il existe un délai entre le moment où le descripteur de plan est capturé et lorsqu’il a été utilisé avec **sys.dm_exec_query_plan**.  
  
-   Certaines instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] ne sont pas mises en mémoire cache, par exemple les instructions d'opérations en bloc ou les instructions contenant des littéraux de chaîne dont la taille est supérieure à 8 Ko. Impossible de récupérer à l’aide de plans d’exécution XML pour de telles instructions **sys.dm_exec_query_plan** , sauf si le lot en cours d’exécution, car ils n’existent pas dans le cache.  
  
-   Si un [!INCLUDE[tsql](../../includes/tsql-md.md)] lot ou une procédure stockée contient un appel à une fonction définie par l’utilisateur ou un appel de code SQL dynamique, par exemple via la commande EXEC (*chaîne*), le compilé Showplan XML pour la fonction définie par l’utilisateur n’est pas incluse dans la table retournée par **sys.dm_exec_query_plan** du lot ou une procédure stockée. Vous devez procéder à un appel séparé à **sys.dm_exec_query_plan** pour le descripteur de plan qui correspond à la fonction définie par l’utilisateur.  
  
 Lorsqu’une requête ad hoc utilise un paramétrage simple ou forcé, le **query_plan** colonne contiendra uniquement le texte de l’instruction et pas le plan de requête. Pour retourner le plan de requête, appelez **sys.dm_exec_query_plan** pour le descripteur de plan de la requête paramétrable préparée. Vous pouvez déterminer si la requête a été paramétrée en référençant la **sql** colonne de la [sys.syscacheobjects](../../relational-databases/system-compatibility-views/sys-syscacheobjects-transact-sql.md) vue ou la colonne de texte de la [sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md) vue de gestion dynamique.  
  
 En raison d’une limitation du nombre de niveaux imbriqués autorisés dans les **xml** type de données, **sys.dm_exec_query_plan** ne peut pas retourner des plans de requête qui correspondent ou sont supérieurs à 128 niveaux d’éléments imbriqués. Dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette condition empêchait les retours par le plan de requête et générait l'erreur 6335. Dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 2 et versions ultérieures, le **query_plan** colonne renvoie la valeur NULL. Vous pouvez utiliser la [sys.dm_exec_text_query_plan &#40;Transact-SQL&#41; ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-text-query-plan-transact-sql.md) fonction de gestion dynamique pour retourner la sortie du plan de requête au format texte.  
  
## <a name="permissions"></a>Autorisations  
 Pour exécuter **sys.dm_exec_query_plan**, un utilisateur doit être un membre de la **sysadmin** rôle serveur fixe ou disposer de l’autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="examples"></a>Exemples  
 Les exemples suivants montrent comment utiliser le **sys.dm_exec_query_plan** vue de gestion dynamique.  
  
 Pour afficher les plans de requête XML, exécutez les requêtes suivantes dans l’éditeur de requête de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], puis cliquez sur **ShowPlanXML** dans les **query_plan** colonne de la table retournée par **sys.dm_exec_query_plan**. Le plan d'exécution de requêtes XML s'affiche dans le volet de résumé de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. Pour enregistrer le Showplan XML dans un fichier, avec le bouton droit **ShowPlanXML** dans les **query_plan** colonne, cliquez sur **enregistrer les résultats sous**, nommez le fichier au format \< *nom_fichier*> .sqlplan ; par exemple, MyXMLShowplan.sqlplan.  
  
### <a name="a-retrieve-the-cached-query-plan-for-a-slow-running-transact-sql-query-or-batch"></a>A. Récupération du plan de requête mis en mémoire cache pour un traitement ou une requête Transact-SQL à exécution lente  
 Les plans de requête pour divers types de lots [!INCLUDE[tsql](../../includes/tsql-md.md)], par exemple les procédures stockées, les fonctions définies par l'utilisateur et les lots appropriés, sont mis en cache dans une zone de la mémoire appelée le cache de plan. Chaque plan de requête mis dans cette mémoire cache est différencié par un identificateur unique appelé descripteur de plan. Vous pouvez spécifier ce descripteur avec la **sys.dm_exec_query_plan** vue de gestion dynamique pour récupérer le plan d’exécution pour un particulier [!INCLUDE[tsql](../../includes/tsql-md.md)] requête ou le lot.  
  
 Si une requête ou un traitement [!INCLUDE[tsql](../../includes/tsql-md.md)] s'exécute longtemps sur une connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] spécifique, récupérez le plan d'exécution de cette requête ou de ce traitement pour trouver la raison de ce retard. L'exemple suivant montre la récupération du plan d'exécution de requêtes XML pour une requête ou un traitement s'exécutant lentement.  
  
> [!NOTE]  
>  Pour exécuter cet exemple, remplacez les valeurs de *session_id* et *plan_handle* avec des valeurs spécifiques à votre serveur.  
  
 Récupérez tout d'abord à l'aide de la procédure stockée `sp_who` l'ID de processus serveur (SPID) pour le processus exécutant la requête ou le traitement.  
  
```  
USE master;  
GO  
exec sp_who;  
GO  
```  
  
 Le jeu de résultats renvoyé par `sp_who` indique que le SPID est `54`. Utilisez cet identificateur avec la vue de gestion dynamique `sys.dm_exec_requests` pour récupérer le descripteur de plan via la requête suivante :  
  
```  
USE master;  
GO  
SELECT * FROM sys.dm_exec_requests  
WHERE session_id = 54;  
GO  
```  
  
 Le tableau retourné par **sys.dm_exec_requests** indique que le descripteur de plan pour la requête ou le lot à exécution lente est `0x06000100A27E7C1FA821B10600`, que vous spécifiez en tant que le *plan_handle* argument avec `sys.dm_exec_query_plan` pour récupérer le plan d’exécution au format XML comme suit. Le plan d’exécution au format XML pour la requête ou le lot à exécution lente est contenu dans le **query_plan** colonne de la table retournée par `sys.dm_exec_query_plan`.  
  
```  
USE master;  
GO  
SELECT * FROM sys.dm_exec_query_plan (0x06000100A27E7C1FA821B10600);  
GO  
```  
  
### <a name="b-retrieve-every-query-plan-from-the-plan-cache"></a>B. Récupération de chaque plan de requête du cache du plan  
 Pour récupérer un instantané de tous les plans de requête résidant dans la mémoire cache des plans, procurez-vous les descripteurs de tous les plans de requête dans la mémoire cache via une requête dans la vue de gestion dynamique `sys.dm_exec_cached_plans`. Les descripteurs de plan sont stockés dans la colonne `plan_handle` de `sys.dm_exec_cached_plans`. Utilisez ensuite l'opérateur CROSS APPLY pour transmettre les descripteurs à `sys.dm_exec_query_plan` comme suit. La sortie du plan d'exécution de requêtes XML pour chaque plan actuellement dans la mémoire cache des plans se trouve dans la colonne `query_plan` de la table retournée.  
  
```  
USE master;  
GO  
SELECT * FROM sys.dm_exec_cached_plans cp CROSS APPLY sys.dm_exec_query_plan(cp.plan_handle);  
GO  
```  
  
### <a name="c-retrieve-every-query-plan-for-which-the-server-has-gathered-query-statistics-from-the-plan-cache"></a>C. Récupération dans la mémoire cache des plans de chaque plan de requête pour lequel le serveur a regroupé des statistiques de requête  
 Pour récupérer un instantané de tous les plans de requête pour lesquels le serveur a regroupé des statistiques actuellement dans la mémoire cache des plans, procurez-vous les descripteurs de ces plans dans la mémoire cache via une requête formulée dans la vue de gestion dynamique `sys.dm_exec_query_stats`. Les descripteurs de plan sont stockés dans la colonne `plan_handle` de `sys.dm_exec_query_stats`. Utilisez ensuite l'opérateur CROSS APPLY pour transmettre les descripteurs à `sys.dm_exec_query_plan` comme suit. La sortie du plan d'exécution de requêtes XML pour chaque plan pour lequel le serveur a regroupé des statistiques actuellement dans la mémoire cache des plans se trouve dans la colonne `query_plan` de la table retournée.  
  
```  
USE master;  
GO  
SELECT * FROM sys.dm_exec_query_stats qs CROSS APPLY sys.dm_exec_query_plan(qs.plan_handle);  
GO  
```  
  
### <a name="d-retrieve-information-about-the-top-five-queries-by-average-cpu-time"></a>D. Récupération d'informations sur les cinq premières requêtes d'après le temps processeur moyen  
 L'exemple suivant retourne les plans et le temps processeur moyen pour les cinq premières requêtes.  
  
```  
SELECT TOP 5 total_worker_time/execution_count AS [Avg CPU Time],  
Plan_handle, query_plan   
FROM sys.dm_exec_query_stats AS qs  
CROSS APPLY sys.dm_exec_query_plan(qs.plan_handle)  
ORDER BY total_worker_time/execution_count DESC;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Sys.dm_exec_cached_plans &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)   
 [sys.dm_exec_query_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)   
 [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)   
 [sp_who &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)   
 [Guide de référence des opérateurs Showplan logiques et physiques](../../relational-databases/showplan-logical-and-physical-operators-reference.md)   
 [sys.dm_exec_text_query_plan &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-text-query-plan-transact-sql.md)  
  
  

