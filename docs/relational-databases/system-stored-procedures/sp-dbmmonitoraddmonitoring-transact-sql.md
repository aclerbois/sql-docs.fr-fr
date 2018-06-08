---
title: sp_dbmmonitoraddmonitoring (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_dbmmonitoraddmonitoring
- sp_dbmmonitoraddmonitoring_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database mirroring [SQL Server], monitoring
- sp_dbmmonitoraddmonitoring
ms.assetid: 9489dc30-af29-4363-a172-4645947fc95e
caps.latest.revision: 36
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: ef20d563325a2aebf5490c7b4a389042fa61a559
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spdbmmonitoraddmonitoring-transact-sql"></a>sp_dbmmonitoraddmonitoring (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Crée un travail de surveillance de la mise en miroir de bases de données qui met régulièrement à jour l'état de mise en miroir de chaque base de données en miroir sur l'instance du serveur.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_dbmmonitoraddmonitoring [ update_period ]  
```  
  
## <a name="arguments"></a>Arguments  
 *update_period*  
 Spécifie, en minutes, l'intervalle entre les mises à jour. Cette valeur peut être comprise entre 1 et 120. La valeur par défaut est de 1 minute.  
  
> [!NOTE]  
>  Si la valeur attribuée à la période de mise à jour est trop basse, le temps de réponse peut augmenter pour les clients.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 Aucun  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun  
  
## <a name="remarks"></a>Notes  
 Cette procédure ne fonctionne que si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent est autorisé à s'exécuter sur l'instance du serveur. En outre, le travail de surveillance de la mise en miroir de bases de données ne peut s'exécuter que si l'agent est lui-même en cours d'exécution.  
  
 Si la mise en miroir de base de données est démarrée à partir de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], le **sp_dbmmonitoraddmonitoring** procédure est exécutée automatiquement. Si vous démarrez la mise en miroir manuellement à l’aide d’instructions ALTER DATABASE, pour surveiller la mise en miroir de base de données sur l’instance de serveur, vous devez exécuter **sp_dbmmonitoraddmonitoring** manuellement.  
  
> [!NOTE]  
>  Si vous exécutez **sp_dbmmonitoraddmonitoring** avant de définir la mise en miroir de base de données, le travail de surveillance fonctionnera, mais ne met pas à jour la table d’état dans la base de données qui est stocké l’historique du moniteur de mise en miroir.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant démarre la surveillance avec une période de mise à jour de `3` minutes.  
  
```  
EXEC sp_dbmmonitoraddmonitoring 3;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Surveillance de la mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [sp_dbmmonitorchangemonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangemonitoring-transact-sql.md)   
 [sp_dbmmonitordropmonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitordropmonitoring-transact-sql.md)   
 [sp_dbmmonitorhelpmonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorhelpmonitoring-transact-sql.md)   
 [sp_dbmmonitorresults &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorresults-transact-sql.md)  
  
  
