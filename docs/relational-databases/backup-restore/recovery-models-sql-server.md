---
title: Modes de récupération (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 07/16/2016
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.suite: sql
ms.technology: backup-restore
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- database backups [SQL Server], recovery models
- bulk-logged recovery model [SQL Server]
- recovery [SQL Server], recovery model
- restoring transaction logs [SQL Server], recovery models
- backing up databases [SQL Server], recovery models
- recovery models [SQL Server], about
- transaction log backups [SQL Server]
- simple recovery model [SQL Server]
- backups [SQL Server], recovery models
- recovery models [SQL Server]
- recovery models [SQL Server], transaction log management
- database restores [SQL Server], recovery models
- transaction log restores [SQL Server]
- logs [SQL Server], recovery models
- restoring databases [SQL Server], recovery models
- full recovery model [SQL Server]
- backing up transaction logs [SQL Server], recovery models
ms.assetid: 8cfea566-8f89-4581-b30d-c53f1f2c79eb
caps.latest.revision: 70
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: f86cc963e5328247b1ee883abb658dcc57e81efc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="recovery-models-sql-server"></a>Modes de récupération (SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] interviennent dans le cadre du mode de récupération de la base de données. Les modes de récupération sont conçus pour contrôler la maintenance des journaux de transactions. Un *mode de récupération* est une propriété de base de données qui contrôle la façon dont les transactions sont journalisées, précise si le journal des transactions nécessite (et permet) une sauvegarde et spécifie les types d’opérations de restauration disponibles. Il existe trois modes de récupération : simple, complète et utilisant les journaux de transactions. En règle générale, une base de données utilise le mode de restauration complète ou le mode de récupération simple. Il est possible de modifier le mode de récupération d'une base de données à tout moment.  
  
 **Dans cette rubrique :**  
  
-   [Vue d'ensemble du mode de récupération](#RMov)  
  
-   [Tâches associées](#RelatedTasks)  
  
##  <a name="RMov"></a> Vue d'ensemble du mode de récupération  
 Le tableau suivant récapitule les trois modes de récupération.  
  
|mode de récupération|Description|Risque de perte de travail|Récupération à un point précis dans le temps ?|  
|--------------------|-----------------|------------------------|-------------------------------|  
|**Simple**|Aucune sauvegarde de journal.<br /><br /> Recycle automatiquement l'espace du journal afin de minimiser l'espace nécessaire, ce qui élimine principalement le besoin de gérer l'espace du journal des transactions. Pour plus d’informations sur les sauvegardes de base de données en mode de récupération simple, consultez [Sauvegardes complètes de bases de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/full-database-backups-sql-server.md).<br /><br /> Les opérations qui nécessitent des sauvegardes du journal des transactions ne sont pas prises en charge par le mode de récupération simple. Les fonctionnalités suivantes ne peuvent pas être utilisées en mode de récupération simple :<br /><br /> - Copie des journaux des transactions<br /><br /> - Always On ou mise en miroir de bases de données<br /><br /> - Récupération des supports sans perte de données<br /><br /> - Restaurations dans le temps|Les modifications postérieures à la sauvegarde la plus récente ne sont pas protégées. En cas de sinistre, ces modifications doivent être apportées de nouveau.|La récupération est possible seulement jusqu'à la fin d'une sauvegarde. Pour plus d’informations, consultez [Restaurations complètes de bases de données &#40;mode de récupération simple&#41;](../../relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md). <br><br> Pour obtenir une explication plus approfondie du mode de récupération simple, consultez le site web sur le [mode de récupération simple de SQL Server](https://www.mssqltips.com/sqlservertutorial/4/sql-server-simple-recovery-model/) de l’équipe [MSSQLTips!](https://www.mssqltips.com)|  
|**Complet**|Exige des sauvegardes de journal.<br /><br /> Aucun travail n'est perdu suite à la perte ou à l'endommagement d'un fichier de données.<br /><br /> La récupération est possible jusqu'à un point arbitraire dans le temps (par exemple, avant l'erreur de l'application ou de l'utilisateur). Pour plus d’informations sur les sauvegardes de bases de données en mode de récupération complète, consultez [Sauvegardes complètes de bases de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/full-database-backups-sql-server.md) et [Restaurations complètes de bases de données &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/complete-database-restores-full-recovery-model.md).|Normalement aucun.<br /><br /> Si la fin du journal est endommagée, les modifications postérieures à la sauvegarde la plus récente du journal doivent être effectuées de nouveau.|La récupération est possible jusqu'à un point spécifique dans le temps, en supposant que vos sauvegardes ont été effectuées jusqu'à ce point. Pour plus d’informations sur l’utilisation de sauvegardes de journaux pour restaurer jusqu’à un point de défaillance, consultez [Restaurer une base de données SQL Server jusqu’à une limite dans le temps &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md).<br /><br /> Remarque : si vous disposez d’au moins deux bases de données en mode de restauration complète qui doivent être logiquement cohérentes, vous devez implémenter des procédures spéciales pour assurer la récupérabilité de ces bases de données. Pour plus d’informations, consultez [Récupération de bases de données associées contenant une transaction marquée](../../relational-databases/backup-restore/recovery-of-related-databases-that-contain-marked-transaction.md).|  
|**Utilisant les journaux de transactions**|Exige des sauvegardes de journal.<br /><br /> Complément au mode de restauration complète qui permet des opérations de copie en bloc avec des performances élevées.<br /><br /> Réduit l'espace du journal utilisé en utilisant un enregistrement minimal pour la plupart des opérations en bloc. Pour plus d’informations sur les opérations pouvant faire l’objet d’une journalisation minimale, consultez [Journal des transactions &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md).<br /><br /> Pour plus d’informations sur les sauvegardes de bases de données en mode de récupération utilisant les journaux de transactions, consultez [Sauvegardes complètes de bases de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/full-database-backups-sql-server.md) et [Restaurations complètes de bases de données &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/complete-database-restores-full-recovery-model.md).|Si le journal est endommagé ou si des opérations utilisant les journaux de transactions ont été effectuées depuis la sauvegarde de journal la plus récente, les modifications postérieures à la sauvegarde la plus récente du journal doivent être effectuées de nouveau.<br /><br /> À part cela, aucun travail n'est perdu.|La récupération est possible jusqu'à la fin de n'importe quelle sauvegarde. La récupération jusqu'à une date et heure n'est pas prise en charge.|  
  
##  <a name="RelatedTasks"></a> Tâches associées  
  
-   [Afficher ou modifier le mode de récupération d’une base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server.md)  
  
-   [Résoudre les problèmes liés à un journal des transactions saturé &#40;erreur SQL Server 9002&#41;](../../relational-databases/logs/troubleshoot-a-full-transaction-log-sql-server-error-9002.md)  
  
## <a name="see-also"></a> Voir aussi  
 [backupset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupset-transact-sql.md)   
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [Options ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [Sauvegarde et restauration des bases de données SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
 [Journal des transactions &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md)   
 [Tâches d’administration automatisée &#40;SQL Server Agent&#41;](http://msdn.microsoft.com/library/541ee5ac-2c9f-4b74-b4f0-13b7bd5920b0)   
 [Vue d’ensemble de la restauration et de la récupération &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)  
  
  
