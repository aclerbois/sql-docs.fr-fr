---
title: Sauvegarder et restaurer des bases de données répliquées | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- backups [SQL Server replication]
- administering replication, restoring
- backing up replicated databases
- backups [SQL Server replication], about backups
- restoring replicated databases [SQL Server replication]
- recovery [SQL Server replication], about recovery
- restoring databases [SQL Server], replicated databases
- backing up databases [SQL Server], replicated databases
- restoring [SQL Server replication], about restoring
- recovery [SQL Server replication]
- replication [SQL Server], administering
- distribution databases [SQL Server replication], backing up
- restoring [SQL Server replication]
- administering replication, backing up
ms.assetid: 04588807-21e7-4bbe-9727-b72f692cffa7
caps.latest.revision: 40
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: c750cf586127bc098d8444fa79823c10a1f248c8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="back-up-and-restore-replicated-databases"></a>Sauvegarder et restaurer des bases de données répliquées
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Les bases de données répliquées nécessitent une attention toute particulière en ce qui concerne la sauvegarde et la restauration de données. Cette rubrique fournit des informations introductives et des liens vers des informations plus complètes sur les stratégies de sauvegarde et de restauration pour chaque type de réplication.  
  
 La réplication prend en charge la restauration de bases de données répliquées sur le même serveur et dans la même base de données à partir desquels la sauvegarde a été créée. Si vous restaurez une sauvegarde d'une base de données répliquée sur un autre serveur ou dans une autre base de données, les paramètres de réplication ne peuvent pas être conservés. Dans ce cas, vous devez recréer toutes les publications et tous les abonnements après la restauration des sauvegardes.  
  
> [!NOTE]  
>  Il est possible de restaurer une base de données répliquée vers un serveur mis en veille à condition que la copie des journaux de transaction soit utilisée. Pour plus d’informations, consultez [Copie des journaux de transaction et réplication &#40;SQL Server&#41;](../../../database-engine/log-shipping/log-shipping-and-replication-sql-server.md).  
  
 Les bases de données répliquées et leurs bases de données système associées doivent être sauvegardées régulièrement. Sauvegardez les bases de données suivantes :  
  
-   La base de données de publication au niveau du serveur de publication  
  
-   La base de données de distribution au niveau du serveur de distribution  
  
-   La base de données d'abonnement sur chaque Abonné  
  
-   Bases de données système **master** et **msdb** sur le serveur de publication, sur le serveur de distribution et sur tous les Abonnés. Il est recommandé de sauvegarder ces bases de données en même temps, ainsi que la base de données de réplication appropriée. Par exemple, sauvegardez les bases de données **master** et **msdb** au niveau du serveur de publication en même temps que la base de données de publication. Si la base de données de publication est restaurée, vérifiez que les bases de données **master** et **msdb** sont cohérentes avec la base de données de publication en termes de configuration de la réplication et de paramètres.  
  
 Si vous effectuez des sauvegardes régulières des journaux, toutes les modifications liées à la réplication doivent être capturées dans les sauvegardes des journaux. Si vous n'effectuez pas de sauvegardes de fichiers journaux, une sauvegarde doit être effectuée chaque fois qu'une modification portant sur un paramètre de réplication a lieu. Pour plus d’informations, consultez [Common Actions Requiring an Updated Backup](../../../relational-databases/replication/administration/common-actions-requiring-an-updated-backup.md).  
  
## <a name="backup-and-restore-strategies"></a>Stratégies de sauvegarde et de restauration  
 Les stratégies de sauvegarde et de restauration de chaque nœud d'une topologie de réplication sont différentes selon le type de réplication utilisé. Pour plus d'informations sur les stratégies de sauvegarde et de restauration pour chaque type de réplication, consultez les rubriques suivantes :  
  
-   [Stratégies de sauvegarde et de restauration de la réplication transactionnelle et d’instantané](../../../relational-databases/replication/administration/strategies-for-backing-up-and-restoring-snapshot-and-transactional-replication.md)  
  
-   [Stratégies de sauvegarde et de restauration de la réplication de fusion](../../../relational-databases/replication/administration/strategies-for-backing-up-and-restoring-merge-replication.md)  
  
 Quelle que soit votre stratégie de récupération, conservez toujours dans un endroit sûr un script à jour de vos paramètres de réplication. En cas de panne sur un serveur, ou si vous avez besoin de créer un environnement de test, vous pouvez modifier ce script en changeant des références de nom de serveur et l'utiliser pour recréer vos paramètres de réplication. Outre vos paramètres de réplication courants, prévoyez d'intégrer dans un script l'activation et la désactivation de la réplication. Pour plus d'informations sur les scripts d'objets de réplication, consultez [Scripting Replication](../../../relational-databases/replication/scripting-replication.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Sauvegarde et restauration des bases de données SQL Server](../../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
 [Best Practices for Replication Administration](../../../relational-databases/replication/administration/best-practices-for-replication-administration.md)  
  
  
