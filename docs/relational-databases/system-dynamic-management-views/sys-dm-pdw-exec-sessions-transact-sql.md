---
title: Sys.dm_pdw_exec_sessions (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/07/2017
ms.prod: ''
ms.prod_service: sql-data-warehouse, pdw
ms.service: sql-data-warehouse
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 31c262b3-7e4d-44c4-af71-aaef0fd1a980
caps.latest.revision: 8
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || = sqlallproducts-allversions'
ms.openlocfilehash: ce411196b28f658789769cd53aa86693d747ef47
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmpdwexecsessions-transact-sql"></a>Sys.dm_pdw_exec_sessions (Transact-SQL)
[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md.md)]

  Contient des informations sur toutes les sessions actuellement ou récemment ouverte sur l’appareil. Il répertorie une ligne par session.  
  
|Nom de la colonne|Type de données| Description|Plage|  
|-----------------|---------------|-----------------|-----------|  
|session_id|**nvarchar(32)**|L’id de la requête en cours de la dernière requête exécuter (si la session est terminée et que la requête en cours d’exécution au moment de l’arrêt). Clé pour cette vue.|Unique dans toutes les sessions dans le système.|  
|status|**nvarchar(10)**|Pour les sessions en cours, identifie si la session est actuellement active ou inactive. Pour des sessions précédentes, la session d’état peut être fermé ou supprimé des (si la session a été fermée).|'ACTIVE', 'FERMÉ', 'INACTIVE', 'TERMINÉ'|  
|request_id|**nvarchar(32)**|L’id de la requête actuelle ou de la dernière exécution de la requête.|Le point d’entrée unique pour toutes les demandes dans le système. Null si aucune n’a été exécutée.|  
|security_id|**varbinary(85)**|ID de sécurité de l’entité de la session en cours d’exécution.||  
|login_name|**nvarchar(128)**|Le nom de connexion du principal qui exécute la session.|Toute chaîne conforme aux conventions de nom d’utilisateur.|  
|login_time|**datetime**|Date et heure à laquelle l’utilisateur connecté et cette session a été créée.|Valide **datetime** avant l’heure actuelle.|  
|query_count|**int**|Capture le nombre de requêtes/requeststhis session s’est exécutée depuis la création.|Supérieur ou égal à 0.|  
|is_transactional|**bit**|Indique si une session est actuellement dans une transaction ou non.|0 pour la validation automatique, 1 pour transactionnelle.|  
|client_id|**nvarchar(255)**|Capture des informations sur le client pour la session.|N’importe quelle chaîne valide.|  
|app_name|**nvarchar(255)**|Capture des informations de nom d’application éventuellement définie en tant que partie du processus de connexion.|N’importe quelle chaîne valide.|  
|sql_spid|**int**|Numéro d’identification du SPID. Utilisez le `session_id` cette session. Utilisez le `sql_spid` colonne pour joindre au **sys.dm_pdw_nodes_exec_sessions**.<br /><br /> **\*\* Avertissement \* \***  cette colonne contient les SPID fermés.||  
  
 Pour plus d’informations sur le nombre maximal de lignes conservées par cette vue, consultez la section valeurs de la vue système Maximum dans la [valeurs minimale et maximale (SQL Server PDW)](http://msdn.microsoft.com/en-us/5243f018-2713-45e3-9b61-39b2a57401b9) rubrique.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation `VIEW SERVER STATE`.  
  
## <a name="see-also"></a>Voir aussi  
 [Entrepôt de données SQL et les données en parallèle de l’entrepôt de vues de gestion dynamique &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  
