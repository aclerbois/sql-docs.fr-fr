---
title: DBCC SQLPERF (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/07/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- SQLPERF
- DBCC_SQLPERF_TSQL
- SQLPERF_TSQL
- DBCC SQLPERF
dev_langs:
- TSQL
helpviewer_keywords:
- statistical information [SQL Server], transaction logs
- transaction logs [SQL Server], space usage
- space [SQL Server], transaction logs
- DBCC SQLPERF statement
ms.assetid: ec9225ce-e20f-4b03-8b3a-7bcad8a649df
caps.latest.revision: 43
author: uc-msft
ms.author: umajay
manager: craigg
ms.openlocfilehash: 4f8229b7765655bd75c2e0bfa2dc804c56a6fffa
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="dbcc-sqlperf-transact-sql"></a>DBCC SQLPERF (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

Fournit des statistiques sur le taux d'utilisation de l'espace du journal des transactions pour toutes les bases de données. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], permet également de réinitialiser les statistiques des verrous et d’attente.
  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu’à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]), [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] ([préversion dans certaines régions](http://azure.microsoft.com/documentation/articles/sql-database-preview-whats-new/?WT.mc_id=TSQL_GetItTag))
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```
DBCC SQLPERF   
(  
     [ LOGSPACE ]  
     | [ "sys.dm_os_latch_stats" , CLEAR ]  
     | [ "sys.dm_os_wait_stats" , CLEAR ]  
)   
     [WITH NO_INFOMSGS ]  
```  
  
## <a name="arguments"></a>Arguments  
LOGSPACE  
Retourne la taille actuelle du journal des transactions et le pourcentage d'espace du journal utilisé pour chaque base de données. Utilisez ces informations pour surveiller la quantité d’espace utilisée dans un journal des transactions.

> [!IMPORTANT]
> Pour plus d’informations sur l’utilisation de l’espace pour le journal des transactions à compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], consultez la section [Notes](#Remarks) dans cette rubrique.
  
**"sys.dm_os_latch_stats"**, CLEAR  
Réinitialise les statistiques des verrous. Pour plus d’informations, consultez [sys.dm_os_latch_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-latch-stats-transact-sql.md). Cette option n'est pas disponible dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
**"sys.dm_os_wait_stats"**, CLEAR  
Réinitialise les statistiques d'attente. Pour plus d’informations, consultez [sys.dm_os_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md). Cette option n'est pas disponible dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
WITH NO_INFOMSGS  
Supprime tous les messages d'information dont les niveaux de gravité sont compris entre 0 et 10.  
  
## <a name="result-sets"></a>Jeux de résultats  
 Le tableau suivant décrit les colonnes du jeu de résultats.  
  
|Nom de colonne|Définition|  
|---|---|
|**Database Name**|Nom de la base de données pour les statistiques du journal affichées.|  
|**Taille du journal (Mo)**|Taille actuelle allouée au journal. Cette valeur est toujours inférieure à la quantité initialement allouée pour l’espace du journal, car le [!INCLUDE[ssDE](../../includes/ssde-md.md)] réserve une petite quantité d’espace disque pour les informations d’en-tête internes.|  
|**Espace journal utilisé (%)**|Pourcentage du fichier journal en cours d’utilisation pour stocker les informations du journal des transactions.|  
|**État**|État du fichier journal. Toujours 0.|  
  
## <a name="Remarks"></a> Notes  
À compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], utilisez la vue de gestion dynamique [sys.dm_db_log_space_usage](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-space-usage-transact-sql.md) à la place de `DBCC SQLPERF(LOGSPACE)` afin de retourner des informations sur l’utilisation de l’espace pour le journal des transactions par base de données.    
 
Le journal de transactions enregistre chaque transaction effectuée dans une base de données. Pour plus d’informations, consultez [Journal des transactions &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md) et [Guide d’architecture et gestion du journal des transactions SQL Server](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md).
  
## <a name="permissions"></a>Autorisations  
Sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], l’exécution de `DBCC SQLPERF(LOGSPACE)` nécessite l’autorisation `VIEW SERVER STATE` sur le serveur. La réinitialisation des statistiques des verrous et d’attente nécessite l’autorisation `ALTER SERVER STATE` sur le serveur.
  
Pour les niveaux [!INCLUDE[ssSDS](../../includes/sssds-md.md)] Premium et Critique pour l’entreprise, l’autorisation `VIEW DATABASE STATE` est requise dans la base de données. Pour les niveaux [!INCLUDE[ssSDS](../../includes/sssds-md.md)] Standard, De base et Usage général, le compte administrateur [!INCLUDE[ssSDS](../../includes/sssds-md.md)] est requis. La réinitialisation des statistiques de verrous et d’attente n’est pas prise en charge.
  
## <a name="examples"></a>Exemples  
  
### <a name="a-displaying-log-space-information-for-all-databases"></a>A. Affichage des informations relatives à l'utilisation de l'espace du journal pour toutes les bases de données  
L'exemple suivant affiche les informations `LOGSPACE` pour toutes les bases de données contenues dans l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
  
```sql  
DBCC SQLPERF(LOGSPACE);  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Database Name Log Size (MB) Log Space Used (%) Status        
------------- ------------- ------------------ -----------   
master         3.99219      14.3469            0   
tempdb         1.99219      1.64216            0   
model          1.0          12.7953            0   
msdb           3.99219      17.0132            0   
AdventureWorks 19.554688    17.748701          0  
```  
  
### <a name="b-resetting-wait-statistics"></a>B. Réinitialisation des statistiques d'attente  
L'exemple suivant réinitialise les statistiques d'attente pour l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
  
```sql  
DBCC SQLPERF("sys.dm_os_wait_stats",CLEAR);  
```  
  
## <a name="see-also"></a> Voir aussi  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)   
[sys.dm_os_latch_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-latch-stats-transact-sql.md)    
[sys.dm_os_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md)     
[sp_spaceused &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)    
[sys.dm_db_log_info &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-info-transact-sql.md)    
[sys.dm_db_log_space_usage &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-space-usage-transact-sql.md)     
[sys.dm_db_log_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-stats-transact-sql.md)     

