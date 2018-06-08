---
title: Sessions utilisateur dans le système de plateforme Analytique | Documents Microsoft »
description: Sessions utilisateur Parallel Data Warehouse du système de plateforme Analytique.
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: fc2e759d77953f739d77f6ad4eb371cc9747efdc
ms.sourcegitcommit: 056ce753c2d6b85cd78be4fc6a29c2b4daaaf26c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="user-sessions-in-analytics-platform-system"></a>Sessions utilisateur dans le système de plateforme Analytique
Une connexion avec les autorisations appropriées permettre gérer les sessions de toutes les connexions sur un dispositif de SQL Server PDW, y compris l’exécution de ces actions :  
  
-   Afficher les sessions actives sur le matériel, y compris les sessions actives et inactives.  
  
-   Afficher les requêtes actives et récentes pour une session.  
  
-   Fin des sessions actives.  
  
Ces actions peuvent être effectuées à l’aide du [contrôler le matériel à l’aide de la Console d’administration](monitor-the-appliance-by-using-the-admin-console.md) ou [vues système](tsql-system-views.md) grâce aux commandes SQL, comme indiqué ci-dessous.  
  
Les autorisations requises pour gérer les sessions à l’aide de ces deux méthodes sont les mêmes et sont décrites dans [accorder des autorisations pour gérer les connexions, utilisateurs et rôles de base de données](grant-permissions.md#grant-permissions-to-manage-logins-users-and-database-roles).  
  
## <a name="manage-sessions-by-using-the-admin-console"></a>Gérer les Sessions à l’aide de la Console d’administration  
  
### <a name="to-view-current-sessions-by-using-the-admin-console"></a>Pour afficher les sessions en cours à l’aide de la Console d’administration  
  
1.  Dans le menu supérieur, cliquez sur **Sessions**.  
  
2.  La liste résultante affiche toutes les sessions récentes. Pour afficher uniquement les sessions de « Actif » ou « Inactif », cliquez sur le **état** en-tête de colonne pour trier les résultats par état.  
  
### <a name="to-view-active-and-recent-queries-for-a-session-by-using-the-admin-console"></a>Pour afficher les requêtes actives et récentes pour une session à l’aide de la Console d’administration  
  
1.  Dans le menu supérieur, cliquez sur **Sessions**.  
  
2.  Dans la liste des résultats, cliquez sur l’ID de session de la session de votre choix.  
  
3.  La liste résultante de requêtes affiche les requêtes récentes pour la session. Pour plus d’informations sur l’affichage des détails de la requête, consultez [analyse les requêtes actives](monitoring-active-queries.md).  
  
### <a name="to-end-sessions-by-using-the-admin-console"></a>À la fin des sessions à l’aide de la Console d’administration  
  
1.  Dans le menu supérieur, cliquez sur **Sessions**.  
  
2.  Rechercher l’ID de session pour la session à annuler.  
  
3.  Cliquez sur la croix rouge **X** à gauche de l’ID de session à la fin de la session. Sessions avec l’état « Actif » ou « Inactif » aura un rouge **X**; uniquement ces sessions peuvent être terminées.  
  
## <a name="manage-sessions-by-using-system-views-and-sql-commands"></a>Gérer les Sessions à l’aide des commandes et les vues système  
  
### <a name="to-view-current-sessions-by-using-system-views"></a>Pour afficher les Sessions en cours à l’aide de vues système  
Utilisez [sys.dm_pdw_exec_sessions](../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-sessions-transact-sql.md) pour générer une liste de sessions en cours.  
  
Cet exemple renvoie l’ID de session, login_name et l’état de toutes les sessions avec l’état 'Active' ou 'Inactive'.  
  
```sql  
SELECT session_id, login_name, status FROM sys.dm_pdw_exec_sessions WHERE status='Active' OR status='Idle';  
```  
  
### <a name="to-view-active-and-recent-queries-for-a-session-by-using-system-views"></a>Pour afficher les requêtes actives et récentes pour une Session à l’aide de vues système  
Pour afficher les requêtes actifs et terminés récemment associés à une session, vous utilisez la [sys.dm_pdw_exec_sessions](../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-sessions-transact-sql.md) et [sys.dm_pdw_exec_requests](../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql.md) vues. Cette requête retourne une liste de toutes les sessions actives ou inactives, ainsi que toutes les requêtes actives ou récentes associées à chaque ID de session.  
  
```sql  
SELECT es.session_id, es.login_name, es.status AS sessionStatus,   
er.request_id, er.status AS requestStatus, er.command   
FROM sys.dm_pdw_exec_sessions es   
LEFT OUTER JOIN sys.dm_pdw_exec_requests er   
ON (es.session_id=er.session_id)   
WHERE (es.status='Active' OR es.status='Idle') AND   
(er.status!= 'Completed' AND er.status!= 'Failed' AND er.status!= 'Cancelled');  
```  
  
### <a name="to-end-sessions-by-using-sql-commands"></a>À la fin des Sessions à l’aide de commandes SQL  
Utilisez le [KILL](../t-sql/language-elements/kill-transact-sql.md) commande pour mettre fin à une session en cours. Vous devez l’ID de session pour la fin du processus, qui peut être obtenu à l’aide de la [sys.dm_pdw_exec_sessions](../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-sessions-transact-sql.md) vue.  
  
Dans cet exemple, sélectionnez login_name, ID de session et les valeurs d’état pour trouver une session en fonction du nom de connexion.  
  
```sql  
SELECT session_id, login_name, status FROM sys.dm_pdw_exec_sessions;  
```  
  
Sessions dont l’état « Actif » ou « Inactif » peuvent être terminées à l’aide de la commande KILL.  
  
```sql  
KILL 'SID137';  
```  
  
<!-- MISSING LINKS 
## See Also  
[Common Metadata Query Examples &#40;SQL Server PDW&#41;](../sqlpdw/common-metadata-query-examples-sql-server-pdw.md)  
-->
  
