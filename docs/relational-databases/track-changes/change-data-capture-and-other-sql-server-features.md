---
title: Capture de données modifiées et autres fonctionnalités de SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 05/03/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: track-changes
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- change data capture [SQL Server], other SQL Server features and
ms.assetid: 7dfcb362-1904-4578-8274-da16681a960e
caps.latest.revision: 14
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 29c9a2ee3c323c9689e7ece0b2b1318a7051321e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="change-data-capture-and-other-sql-server-features"></a>Capture de données modifiées et autres fonctionnalités de SQL Server
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Cette rubrique décrit comment les fonctionnalités suivantes interagissent avec la capture de données modifiées :  
  
-   [Suivi des modifications](#ChangeTracking)  
  
-   [Mise en miroir de bases de données](#DatabaseMirroring)  
  
-   [Réplication transactionnelle](#TransReplication)  
  
-   [Restauration ou attachement d'une base de données activée pour la capture de données modifiées](#RestoreOrAttach)  
  
##  <a name="ChangeTracking"></a> Change Tracking  
 La capture de données modifiées et le [suivi des modifications](../../relational-databases/track-changes/about-change-tracking-sql-server.md) peuvent être activés sur la même base de données. Aucune considération particulière ne s'applique. Pour plus d’informations, consultez [Utiliser le suivi des modifications &#40;SQL Server&#41;](../../relational-databases/track-changes/work-with-change-tracking-sql-server.md).  
  
##  <a name="DatabaseMirroring"></a> Mise en miroir de bases de données  
 Une base de données prenant en charge la capture de données modifiées peut être mise en miroir. Pour faire en sorte que la capture et le nettoyage s'exécutent automatiquement après un basculement, suivez ces étapes :  
  
1.  Vérifiez que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent s'exécute sur la nouvelle instance de serveur principal.  
  
2.  Créez le travail de capture et le travail de nettoyage sur la nouvelle base de données principale (base de données miroir initiale). Pour créer les travaux, utilisez la procédure stockée [sp_cdc_add_job](../../relational-databases/system-stored-procedures/sys-sp-cdc-add-job-transact-sql.md) .  
  
 Pour consulter la configuration actuelle d’un travail de capture ou de nettoyage, utilisez la procédure stockée [sys.sp_cdc_help_jobs](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-jobs-transact-sql.md) sur la nouvelle instance de serveur principal. Pour une base de données spécifique, le travail de capture est nommé cdc.*nom_base_de_données*_capture, tandis que le travail de nettoyage est nommé cdc.*nom_base_de_données*_cleanup, où *nom_base_de_données* est le nom de la base de données.  
  
 Pour modifier la configuration d’un travail, utilisez la procédure stockée [sys.sp_cdc_change_job](../../relational-databases/system-stored-procedures/sys-sp-cdc-change-job-transact-sql.md).  
  
 Pour plus d’informations sur la mise en miroir des bases de données, consultez [Mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md).  
  
##  <a name="TransReplication"></a> Transactional Replication  
 La capture de données modifiées et la réplication transactionnelle peuvent coexister dans la même base de données, mais le remplissage des tables de modifications est géré différemment lorsque les deux fonctionnalités sont activées. La capture de données modifiées et la réplication transactionnelle utilisent toujours la même procédure, [sp_replcmds](../../relational-databases/system-stored-procedures/sp-replcmds-transact-sql.md), pour lire les modifications dans le journal des transactions. Quand la capture de données modifiées est la seule fonctionnalité activée, un travail de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent appelle **sp_replcmds**. Quand les deux fonctionnalités sont activées sur la même base de données, l’Agent de lecture du journal appelle **sp_replcmds**. Cet agent remplit à la fois les tables de modifications et les tables de bases de données de distribution. Pour plus d’informations, voir [Replication Log Reader Agent](../../relational-databases/replication/agents/replication-log-reader-agent.md).  
  
 Considérez un scénario dans lequel la capture de données modifiées est activée sur la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] , et deux tables sont activées pour la capture. Pour remplir les tables de modifications, le travail de capture appelle **sp_replcmds**. La base de données est activée pour la réplication transactionnelle, et une publication est créée. Ensuite, l'Agent de lecture du journal est créé pour la base de données et le travail de capture est supprimé. L'Agent de lecture du journal continue à analyser le journal à partir du dernier numéro séquentiel dans le journal qui été validé dans la table de modifications. Cela garantit la cohérence des données dans les tables de modifications. Si la réplication transactionnelle est désactivée dans cette base de données, l'Agent de lecture du journal est supprimé et le travail de capture est recréé.  
  
> [!NOTE]  
>  Lorsque l'Agent de lecture du journal est utilisé à la fois pour la capture de données modifiées et la réplication transactionnelle, les modifications répliquées sont écrites en premier dans la base de données de distribution. Puis, les modifications capturées sont écrites dans les tables de modifications. Les deux opérations sont validées ensemble. Si l'écriture dans la base de données de distribution s'effectue avec une latence, la même latence est observée avant l'affichage des modifications dans les tables de modifications.  
  
 L'option **proc exec** de réplication transactionnelle n'est pas disponible lorsque la capture de données modifiées est activée.  
  
##  <a name="RestoreOrAttach"></a> Restauration ou attachement d'une base de données activée pour la capture de données modifiées  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise la logique suivante pour déterminer si la capture de données modifiées reste activée après qu'une base de données a été restaurée ou attachée :  
  
-   Si une base de données est restaurée sur le même serveur avec le même nom de base de données, la capture de données modifiées reste activée.  
  
-   Si une base de données est restaurée sur un autre serveur, par défaut, la capture de données modifiées est désactivée et toutes les métadonnées connexes sont supprimées.  
  
     Pour conserver la fonction de capture de données modifiées, utilisez l’option **KEEP_CDC** lors de la restauration de la base de données. Pour plus d'informations sur cette option, consultez [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md).  
  
-   Si une base de données est détachée puis attachée au même serveur ou à un autre serveur, la capture de données modifiées reste activée.  
  
-   Si une base de données est attachée ou restaurée avec l’option **KEEP_CDC** à toute édition autre qu’Enterprise, l’opération est bloquée parce que la capture de données modifiées requiert [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise. Le message d’erreur 934 est affiché :  
  
     `SQL Server cannot load database '%.*ls' because Change Data Capture is enabled. The currently installed edition of SQL Server does not support Change Data Capture. Either restore database without KEEP_CDC option, or upgrade the instance to one that supports Change Data Capture.`  
  
 Vous pouvez utiliser [sys.sp_cdc_disable_db](../../relational-databases/system-stored-procedures/sys-sp-cdc-disable-db-transact-sql.md) pour supprimer la capture de données modifiées d’une base de données restaurée ou attachée.  
  
## <a name="change-data-capture-and-always-on"></a>Capture de données modifiées et Always On  
 Lorsque vous utilisez Always On, l’énumération des modifications doit être effectuée sur la réplication secondaire afin de réduire la charge du disque sur le serveur principal.  
  
## <a name="see-also"></a> Voir aussi  
 [Administrer et surveiller la capture de données modifiées &#40;SQL Server&#41;](../../relational-databases/track-changes/administer-and-monitor-change-data-capture-sql-server.md)  
  
  
