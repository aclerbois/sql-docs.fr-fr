---
title: Sauvegardes du journal des transactions (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 01/05/2018
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.suite: sql
ms.technology: backup-restore
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- backing up [SQL Server], transaction logs
- transaction log backups [SQL Server], creating
- log backups [SQL Server]
- transaction log backups [SQL Server], sequencing
ms.assetid: f4a44a35-0f44-4a42-91d5-d73ac658a3b0
caps.latest.revision: 52
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 36142710470ed5fab6721d4fba8075dd056960fa
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="transaction-log-backups-sql-server"></a>Sauvegardes du journal des transactions (SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique s'applique uniquement aux bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] employant le mode de récupération complète ou le mode de récupération utilisant les journaux de transactions. Cette rubrique décrit la sauvegarde du journal des transactions d'une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Vous devez au moins avoir créé une sauvegarde complète pour pouvoir créer des sauvegardes de journaux. Après cela, le journal des transactions peut être sauvegardé à tout moment, à moins qu'il ne soit déjà en cours de sauvegarde. 
 
Nous vous recommandons d’effectuer des sauvegardes de journaux fréquemment, à la fois pour réduire les risques de perte de travail et pour tronquer le journal des transactions. 
 
En règle générale, un administrateur de base de données crée une sauvegarde complète de base de données de temps en temps, par exemple chaque semaine et, éventuellement, crée une série de sauvegardes de base de données différentielles à un intervalle plus court, par exemple tous les jours. Indépendamment des sauvegardes de base de données, l’administrateur de base de données sauvegarde le journal des transactions à intervalles fréquents. Pour un type donné de sauvegarde, l'intervalle optimal varie en fonction de divers facteurs tels que l'importance des données, la taille de la base de données et la charge de travail du serveur. Pour plus d’informations sur l’implémentation d’une bonne stratégie, consultez [Recommandations](#Recommendations) dans cette rubrique. 
   
##  <a name="LogBackupSequence"></a> Fonctionnement des sauvegardes de journal  
 La séquence de sauvegarde des journaux des transactions ( *séquence de journaux* ) ne dépend pas des sauvegardes de données. Supposons, par exemple, que la séquence des événements est la suivante.  
  
|Time|Événement|  
|----------|-----------|  
|8h00|Sauvegarde de la base de données.|  
|Midi|Sauvegarde du journal des transactions.|  
|16h00|Sauvegarde du journal des transactions.|  
|18h00|Sauvegarde de la base de données.|  
|20h00|Sauvegarde du journal des transactions.|  
  
 La sauvegarde du journal des transactions lancée à 20h00 contient les enregistrements du journal des transactions effectués entre 16h00 et 20h00, ce qui couvre le moment de la création de la sauvegarde complète de la base de données intervenant à 18h00. La séquence de sauvegardes du journal des transactions est continue depuis la sauvegarde complète de la base de données initiale créée à 8h00 jusqu’à la dernière sauvegarde du journal des transactions effectuée à 20h00. Pour plus d’informations sur l’application de ces sauvegardes du journal, reportez-vous à l’exemple cité dans [Appliquer les sauvegardes du journal de transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md).  
  
##  <a name="Recommendations"></a> Recommandations  
  
-   Si un journal des transactions est endommagé, vous perdez alors les travaux réalisés depuis la sauvegarde valide la plus récente. Par conséquent, nous vous recommandons vivement de placer vos fichiers journaux sur une unité de stockage à tolérance de pannes.  
  
-   Si une base de données est endommagée ou si vous êtes sur le point de restaurer la base de données, nous vous recommandons de créer une [sauvegarde de la fin du journal](../../relational-databases/backup-restore/tail-log-backups-sql-server.md) pour vous permettre de restaurer la base de données au moment actuel.  
  
-   Par défaut, chaque opération de sauvegarde réussie ajoute une entrée au journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et au journal des événements système. Si vous sauvegardez très fréquemment le journal, ces messages de réussite peuvent rapidement s'accumuler, créer des journaux d'erreurs très volumineux et compliquer la recherche d'autres messages. Dans de tels cas, vous pouvez supprimer ces entrées de journal en utilisant l'indicateur de trace 3226 si aucun de vos scripts ne dépend de ces entrées. Pour plus d’informations, consultez [Indicateurs de trace &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).  

-   Effectuez des sauvegardes de journaux suffisamment fréquentes pour répondre à vos besoins, en particulier votre tolérance des pertes de données comme celles causées par un stockage de journal endommagé. 
   -   La fréquence appropriée des sauvegardes de journaux dépend de votre gestion des risques liés aux pertes de données et du nombre de sauvegardes de journaux qu'il vous est possible de stocker, gérer et potentiellement restaurer. Pensez à [l’objectif de délai de récupération](http://wikipedia.org/wiki/Recovery_time_objective) et à [l’objectif de point de récupération](http://wikipedia.org/wiki/Recovery_point_objective) quand vous implémentez votre stratégie de récupération, en particulier la cadence des sauvegardes de fichier journal.
   -   Réaliser une sauvegarde de journal tous les 15 à 30 minutes peut être suffisant. Si vos besoins nécessitent de minimiser les risques de perte de travail, vous devez envisager des sauvegardes de journaux plus fréquentes. Une meilleure fréquence pour les sauvegardes de fichiers journaux offre l'avantage d'augmenter la fréquence de la troncation des journaux qui produit des fichiers journaux plus petits.  
  
> [!IMPORTANT]
> Pour limiter le nombre des sauvegardes de fichiers journaux à restaurer, il est essentiel de sauvegarder vos données régulièrement. Vous pouvez, par exemple, planifier une sauvegarde complète hebdomadaire et des sauvegardes différentielles quotidiennes de la base de données.  
> Là encore, pensez à [l’objectif de délai de récupération](http://wikipedia.org/wiki/Recovery_time_objective) et à [l’objectif de point de récupération](http://wikipedia.org/wiki/Recovery_point_objective) quand vous implémentez votre stratégie de récupération, en particulier la cadence des sauvegardes différentielles et complètes de base de données.
  
##  <a name="RelatedTasks"></a> Tâches associées  
 **Pour créer une sauvegarde du journal des transactions**  
  
-   [Sauvegarder un journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Backup.SqlBackup%2A> (SMO)  
  
 Pour planifier des travaux de sauvegarde, consultez [Use the Maintenance Plan Wizard](../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md).  
  

## <a name="see-also"></a> Voir aussi  
 [Journal des transactions &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md)   
 [Sauvegardes de fichier journal dans le guide d’architecture et gestion du journal des transactions SQL Server](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Backups)     
 [Sauvegarde et restauration des bases de données SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
 [Sauvegardes de la fin du journal &#40;SQL Server&#41;](../../relational-databases/backup-restore/tail-log-backups-sql-server.md)   
 [Appliquer les sauvegardes du journal de transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)  
  
  
