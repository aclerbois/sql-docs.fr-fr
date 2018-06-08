---
title: Sys.sp_xtp_control_query_exec_stats (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 10/13/2015
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.sp_xtp_control_query_exec_stats_TSQL
- sys.sp_xtp_control_query_exec_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_xtp_control_query_exec_stats
ms.assetid: 4838125d-ad1e-479e-b7d2-42655e8f4f02
caps.latest.revision: 16
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 23301458684008945c057aa1d7f9f97380a9cb68
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysspxtpcontrolqueryexecstats-transact-sql"></a>sys.sp_xtp_control_query_exec_stats (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2014-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2014-xxxx-xxxx-xxx-md.md)]

  Active la collection de statistiques par requête pour toutes les procédures stockées compilées en mode natif pour l'instance, ou pour des procédures stockées compilées en mode natif spécifiques.  
  
 Les performances sont altérées lorsque vous activez la collection de statistiques. Si vous devez uniquement dépanner une ou quelques procédures stockées compilées en mode natif, activez la collection de statistiques uniquement pour ces dernières.  
  
 Pour activer la collection de statistiques au niveau de la procédure pour toutes les procédures stockées compilées en mode natif, consultez [sys.sp_xtp_control_proc_exec_stats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-proc-exec-stats-transact-sql.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sp_xtp_control_query_exec_stats [ [ @new_collection_value = ] collection_value ],  
[ [ @database_id = ] database_id   
[ , [ @xtp_object_id = ] procedure_id ] ,   
[ @old_collection_value] ]  
```  
  
## <a name="arguments"></a>Arguments  
 @new_collection_value = *valeur*  
 Détermine si la collection de statistiques au niveau de la procédure est activée (1) ou désactivée (0).  
  
 @new_collection_value a la valeur zéro lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] démarre.  
  
 @database_id == *database_id*, @xtp_object_id = *procedure_id*  
 ID de base de données et ID d'objet pour la procédure stockée compilée en mode natif. Si la collection de statistiques est activée pour l’instance ([sys.sp_xtp_control_proc_exec_stats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-proc-exec-stats-transact-sql.md)), les statistiques sur une procédure stockée compilée en mode natif sont collectées. Le fait de désactiver la collection de statistiques sur l'instance ne désactive pas la collection de statistiques pour les procédures stockées individuelles compilées en mode natif.  
  
 Utilisez [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md), [sys.procedures &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-procedures-transact-sql.md), [DB_ID &#40;Transact-SQL&#41;](../../t-sql/functions/db-id-transact-sql.md), ou [OBJECT_ID &#40;Transact-SQL&#41; ](../../t-sql/functions/object-id-transact-sql.md) pour obtenir un ID pour une base de données et de la procédure stockée.  
  
 @old_collection_value = *valeur*  
 Retourne l'état actuel.  
  
## <a name="return-code"></a>Code de retour  
 0 pour réussite. Une valeur différente de zéro pour un échec.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle sysadmin fixe.  
  
## <a name="code-sample"></a>Exemple de code  
 Cet exemple de code montre comment activer la collection de statistiques par requête pour toutes les procédures stockées compilées en mode natif pour l'instance, puis pour une procédure stockée compilée en mode natif spécifique.  
  
```sql   
DECLARE @c bit  
  
EXEC [sys].[sp_xtp_control_query_exec_stats] @new_collection_value = 1;  
  
EXEC sp_xtp_control_query_exec_stats @old_collection_value=@c output;  
SELECT @c AS 'collection status';  
  
EXEC [sys].[sp_xtp_control_query_exec_stats] @new_collection_value = 1,   
@database_id = 5, @xtp_object_id = 341576255;  
  
EXEC sp_xtp_control_query_exec_stats @database_id = 5,   
@xtp_object_id = 341576255, @old_collection_value=@c output;  
  
SELECT @c AS 'collection status';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [OLTP en mémoire &#40;Optimisation en mémoire&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
  
