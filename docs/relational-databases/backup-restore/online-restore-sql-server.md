---
title: Restauration en ligne (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.suite: sql
ms.technology: backup-restore
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- online restores [SQL Server]
- online restores [SQL Server], about online restores
ms.assetid: 7982a687-980a-4eb8-8e9f-6894148e7d8c
caps.latest.revision: 45
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 93b882f13d9c665198abbcf72fe809398d274f62
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="online-restore-sql-server"></a>Restauration en ligne (SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  La restauration en ligne est prise en charge uniquement dans l'édition [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise. Par défaut, une restauration fragmentaire, de fichiers ou de pages est en ligne dans cette édition. Cette rubrique concerne les bases de données qui contiennent plusieurs fichiers ou groupes de fichiers et, en mode de récupération simple, seulement des groupes de fichiers en lecture seule.  
  
 Le processus de restauration des données lorsque la base de données est en ligne est appelé *restauration en ligne*. Une base de données est considérée en ligne lorsque le groupe de fichiers primaire est en ligne, même si un ou plusieurs de ses groupes de fichiers secondaires sont hors connexion. Quel que soit le mode de récupération, vous pouvez restaurer un fichier qui est hors connexion quand la base de données est en ligne. En mode de restauration complète, vous pouvez aussi restaurer les pages quand la base de données est en ligne.  
  
> [!NOTE]  
>  La restauration en ligne a lieu automatiquement dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise, et ne nécessite aucune action de l'utilisateur. Si vous ne souhaitez pas utiliser la restauration en ligne, mettez une base de données hors connexion avant de démarrer une restauration. Pour plus d'informations, consultez [Mise hors connexion d'une base de données ou d'un fichier](#taking_db_or_file_offline), plus loin dans cette rubrique.  
  
 Au cours d'une restauration de fichiers en ligne, les fichiers en cours de restauration et leurs groupes de fichiers sont hors connexion. Si ces fichiers sont en ligne au démarrage d'une restauration en ligne, la première instruction de restauration met le groupe de fichiers du fichier hors connexion. À l'inverse, au cours d'une restauration de pages en ligne, seule la page est hors connexion.  
  
 Chaque scénario de restauration en ligne comprend les étapes de base suivantes :  
  
1.  Restaurer les données.  
  
2.  Effectuer la dernière restauration du journal à l'aide de WITH RECOVERY. Cette opération met les données restaurées en ligne.  
  
 Il arrive parfois qu'une transaction non validée ne puisse être restaurée parce que les données requises par la restauration sont hors connexion au démarrage. Dans ce cas, la transaction est différée. Pour plus d’informations, consultez [Transactions différées &#40;SQL Server&#41;](../../relational-databases/backup-restore/deferred-transactions-sql-server.md).  
  
> [!NOTE]  
>  Si la base de données utilise le mode de récupération utilisant les journaux de transactions, il est recommandé de basculer en mode de restauration complète avant de démarrer une restauration en ligne. Pour plus d’informations, consultez [Afficher ou modifier le mode de récupération d’une base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server.md).  
  
> [!IMPORTANT]  
>  Si les sauvegardes ont été effectuées sur plusieurs unités reliées au serveur, un nombre identique d'unités doit être disponible pendant la restauration en ligne.  
  
> [!CAUTION]  
>  Lorsque vous utilisez des sauvegardes de captures instantanées, vous ne pouvez pas effectuer de **restauration en ligne**. Pour plus d’informations sur la **sauvegarde de captures instantanées**, consultez [Sauvegarde d’instantanés de fichiers pour les fichiers de base de données dans Azure](../../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md).  
  
## <a name="log-backups-for-online-restore"></a>Sauvegardes de journal pour une restauration en ligne  
 Pour une restauration en ligne, le point de récupération est le point où les données en cours de restauration ont été mises hors connexion ou mises en lecture seule pour la dernière fois. Les sauvegardes des journaux de transactions jusqu'à ce point de récupération compris doivent toutes être disponibles. Généralement, une sauvegarde de fichier journal est requise après ce point pour couvrir le point de récupération du fichier. La seule exception concerne une restauration en ligne de données en lecture seule à partir une sauvegarde de données effectuée après la mise en lecture seule des données. Dans ce cas, vous n'avez pas besoin d'une sauvegarde de journal.  
  
 En général, vous pouvez effectuer des sauvegardes des journaux de transactions pendant que la base de données est en ligne, même après le début de la séquence de restauration. Le moment de la dernière sauvegarde de journal dépend des propriétés du fichier en cours de restauration :  
  
-   Pour un fichier en lecture seule en ligne, vous pouvez effectuer la dernière sauvegarde de journal nécessaire à la récupération avant ou pendant la première séquence de restauration. Un groupe de fichiers en lecture seule peut se passer de sauvegardes de journal si une sauvegarde de données ou différentielle a été effectuée après la mise en lecture seule du groupe de fichiers.  
  
    > [!NOTE]  
    >  Les informations précédentes s'appliquent aussi à chaque fichier hors connexion.  
  
-   Un cas particulier concerne un fichier en lecture-écriture qui était en ligne lorsque la première instruction de restauration a été envoyée, et qui a ensuite été mis automatiquement hors ligne par cette instruction de restauration. Dans ce cas, vous devez effectuer une sauvegarde de journal pendant la première *séquence de restauration* (la séquence d’une ou plusieurs instructions RESTORE qui restaurent, restaurent par progression et récupèrent les données). En règle générale, cette sauvegarde de journal doit avoir lieu après la restauration de toutes les sauvegardes complètes et avant la récupération des données. Toutefois, dans le cas de plusieurs sauvegardes de fichiers pour un groupe de fichiers spécifique, le point minimal de la sauvegarde de journal est le moment après la mise hors connexion du groupe de fichiers. Cette sauvegarde de journal après restauration des données capture le point où le fichier a été mis hors connexion. La sauvegarde de journal après restauration des données est nécessaire car le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] ne peut pas exploiter le journal en ligne pour une restauration en ligne.  
  
    > [!NOTE]  
    >  D'une autre manière, vous pouvez mettre manuellement le fichier hors connexion avant la séquence de restauration. Pour plus d'informations, consultez « Mise hors connexion d'une base de données ou d'un fichier », plus loin dans cette rubrique.  
  
##  <a name="taking_db_or_file_offline"></a> Mise hors connexion d'une base de données ou d'un fichier  
 Si vous ne souhaitez pas utiliser la restauration en ligne, mettez la base de données hors connexion avant de démarrer la séquence de restauration au moyen de l'une des méthodes suivantes :  
  
-   Quel que soit le mode de récupération, vous pouvez mettre la base de données hors connexion à l'aide de l'instruction [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md) suivante :  
  
     ALTER DATABASE *nom_base_de_données* SET OFFLINE  
  
-   En mode de restauration complète, vous pouvez aussi forcer la mise hors connexion d'une restauration de pages ou de fichiers à l'aide de l'instruction suivante [BACKUP LOG](../../t-sql/statements/backup-transact-sql.md) pour mettre la base de données dans l'état de restauration :  
  
     BACKUP LOG *nom_base_de_données* WITH NORECOVERY  
  
 Tant qu'une base de données demeure hors connexion, toutes les restaurations sont des restaurations hors connexion.  
  
## <a name="examples"></a>Exemples  
  
> [!NOTE]  
>  La syntaxe pour une séquence de restauration en ligne est la même que pour une séquence de restauration hors connexion.  
  
-   [Exemple : restauration fragmentaire d’une base de données &#40;mode de récupération simple&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-database-simple-recovery-model.md)  
  
-   [Exemple : restauration fragmentaire de quelques groupes de fichiers uniquement &#40;mode de récupération simple&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-only-some-filegroups-simple-recovery-model.md)  
  
-   [Exemple : restauration en ligne d’un fichier en lecture seule &#40;Mode de récupération simple&#41;](../../relational-databases/backup-restore/example-online-restore-of-a-read-only-file-simple-recovery-model.md)  
  
-   [Exemple : restauration fragmentaire d’une base de données &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-database-full-recovery-model.md)  
  
-   [Exemple : restauration fragmentaire de quelques groupes de fichiers &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-only-some-filegroups-full-recovery-model.md)  
  
-   [Exemple : restauration en ligne d’un fichier en lecture/écriture &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/example-online-restore-of-a-read-write-file-full-recovery-model.md)  
  
-   [Exemple : restauration en ligne d’un fichier en lecture seule &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/example-online-restore-of-a-read-only-file-full-recovery-model.md)  
  
##  <a name="RelatedTasks"></a> Tâches associées  
  
-   [Restaurer des fichiers et des groupes de fichiers &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-files-and-filegroups-sql-server.md)  
  
-   [Restaurer des pages &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-pages-sql-server.md)  
  
-   [Gérer la table suspect_pages &#40;SQL Server&#41;](../../relational-databases/backup-restore/manage-the-suspect-pages-table-sql-server.md)  
  
-   [Récupérer une base de données sans restaurer les données &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/recover-a-database-without-restoring-data-transact-sql.md)  
  
-   [Supprimer des groupes de fichiers obsolètes &#40;SQL Server&#41;](../../relational-databases/backup-restore/remove-defunct-filegroups-sql-server.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Restaurations de fichiers &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/file-restores-full-recovery-model.md)   
 [Restaurations de fichiers &#40;mode de récupération simple&#41;](../../relational-databases/backup-restore/file-restores-simple-recovery-model.md)   
 [Restaurer des pages &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-pages-sql-server.md)   
 [Restaurations fragmentaires &#40;SQL Server&#41;](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md)   
 [Vue d’ensemble de la restauration et de la récupération &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)  
  
  
