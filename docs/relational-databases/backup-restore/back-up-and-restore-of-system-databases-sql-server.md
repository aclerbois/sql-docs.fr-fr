---
title: Sauvegarder et restaurer des bases de données système (SQL Server) | Microsoft Docs
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
- system databases [SQL Server], backing up and restoring
- restoring system databases [SQL Server]
- backing up [SQL Server], system databases
- database backups [SQL Server], system databases
- servers [SQL Server], backup
ms.assetid: aef0c4fa-ba67-413d-9359-1a67682fdaab
caps.latest.revision: 57
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 7cdf3056f71609a33910a6c4812127fc78861be5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="back-up-and-restore-of-system-databases-sql-server"></a>Sauvegarder et restaurer des bases de données système (SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] assure la maintenance d’un jeu de bases de données au niveau système, les*bases de données système*, qui sont essentielles au fonctionnement d’une instance de serveur. Il est nécessaire de sauvegarder plusieurs bases de données système après chaque mise à jour importante. Les bases de données système qui doivent toujours êtres sauvegardées sont les suivantes : **msdb**, **master**, et **model**. Si une base de données utilise la réplication sur l'instance de serveur, vous devez également sauvegarder la base de données système **distribution** . La sauvegarde de ces bases de données système permet de restaurer et de récupérer le système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en cas d'incident système, comme le dysfonctionnement du disque dur.  
  
 Le tableau suivant récapitule l'ensemble des bases de données système :  
  
|Base de données système|Description|Des sauvegardes sont-elles nécessaires ?|mode de récupération|Commentaires|  
|---------------------|-----------------|---------------------------|--------------------|--------------|  
|[master](../../relational-databases/databases/master-database.md)|Base de données qui contient l'intégralité des informations système relatives à un système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|Oui|Simple|Sauvegardez la base de données **master** aussi souvent que nécessaire pour protéger suffisamment les données en fonction de vos besoins. Nous vous recommandons de définir une planification de sauvegarde régulière complétée d'une sauvegarde supplémentaire après une mise à jour substantielle.|  
|[model](../../relational-databases/databases/model-database.md)|Modèle de toutes les bases de données créées dans l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|Oui|Configurable par l’utilisateur*|Sauvegardez la base de données **model** aussi souvent que nécessaire en fonction de vos besoins ; par exemple, immédiatement après avoir personnalisé ses options de base de données.<br /><br /> **Méthode recommandée :** Nous recommandons d'effectuer uniquement des sauvegardes complètes de **mode**, selon les besoins. Étant donné que **mode** est petit et change rarement, il n'est pas nécessaire de sauvegarder le journal.|  
|[msdb](../../relational-databases/databases/msdb-database.md)|La base de données est utilisée par l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour planifier les alertes et les travaux et pour enregistrer les opérateurs. La base de données**msdb** contient aussi les tables d'historique, telles que les tables d'historique de restauration et de sauvegarde.|Oui|Simple (par défaut)|Sauvegardez la base de données **msdb** chaque fois qu'elle est mise à jour.|  
|[Resource](../../relational-databases/databases/resource-database.md) (RDB)|Base de données en lecture seule qui contient les copies de tous les objets système fournis avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|non|—|La base de données **Resource** réside dans le fichier mssqlsystemresource.mdf, qui contient uniquement du code. Par conséquent, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas sauvegarder la base de données **Resource** .<br /><br /> Remarque : vous pouvez effectuer une sauvegarde sur fichiers ou sur disque sur le fichier mssqlsystemresource.mdf en le traitant comme s’il s’agissait d’un fichier binaire (.exe), et non d’un fichier de base de données. Toutefois, vous ne pouvez pas utiliser la restauration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur les sauvegardes. La restauration d’une copie de sauvegarde du fichier mssqlsystemresource.mdf peut uniquement être effectuée manuellement et vous devez alors veiller à ne pas remplacer la version actuelle de la base de données **Resource** par une version obsolète ou potentiellement instable.|  
|[tempdb](../../relational-databases/databases/tempdb-database.md)|Espace de travail qui contient les ensembles de résultats temporaires et intermédiaires. Cette base de données est recréée chaque fois qu'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] démarre. Lors de l'arrêt de l'instance du serveur, toutes les données dans **tempdb** sont supprimées définitivement.|non|Simple|Vous ne pouvez pas sauvegarder la base de données système **tempdb** .|  
|[Configurer la distribution](../../relational-databases/replication/configure-distribution.md)|Base de données qui existe uniquement si le serveur est configuré comme serveur de distribution de réplication. Cette base de données contient les métadonnées et les données historiques de tous les types de réplications, ainsi que les transactions de la réplication transactionnelle.|Oui|Simple|Pour savoir quand vous devez sauvegarder la base de données **distribution**, consultez [Sauvegarder et restaurer des bases de données répliquées](../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md).|  
  
 * Pour plus d’informations sur le mode de récupération actuel du modèle, consultez [Afficher ou modifier le mode de récupération d’une base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server.md) ou [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).  
  
## <a name="limitations-on-restoring-system-databases"></a>Limitations sur la restauration des bases de données système  
  
-   Les bases de données système peuvent être restaurées uniquement à partir de sauvegardes créées dans la version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans laquelle s'exécute actuellement l'instance de serveur. Par exemple, pour restaurer une base de données système sur une instance de serveur qui s'exécute sur [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1, vous devez utiliser une sauvegarde de base de données créée après la mise à niveau de l'instance de serveur vers [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1.  
  
-   Pour restaurer une base de données, l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être active. Pour pouvoir démarrer une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , la base de données **master** doit être accessible et partiellement utilisable. Si la base de données **master** devient inutilisable, vous pouvez la ramener à un état utilisable de deux manières :  
  
    -   en restaurant la base de données **master** depuis une sauvegarde actuelle.  
  
         Si vous pouvez démarrer l'instance du serveur, vous pouvez restaurer la base de données **master** depuis une sauvegarde complète.  
  
    -   Recréez complètement la base de données **master** .  
  
         Si la base de données **master** est gravement endommagée et ne vous permet pas de démarrer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous devez recréer la base de données **master**. Pour plus d’informations, consultez [Reconstruire des bases de données système](../../relational-databases/databases/rebuild-system-databases.md).  
  
        > [!IMPORTANT]  
        >  Lorsque vous recréez la base de données **master** , vous recréez toutes les bases de données système.  
  
-   Dans certains cas, les problèmes de récupération de la base de données model peuvent nécessiter la reconstruction des bases de données système ou le remplacement des fichiers mdf et ldf de la base de données model. Pour plus d’informations, consultez [Reconstruire des bases de données système](../../relational-databases/databases/rebuild-system-databases.md).  
  
##  <a name="RelatedTasks"></a> Tâches associées  
  
-   [Créer une sauvegarde complète de base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)  
  
-   [Restaurations complètes de bases de données &#40;mode de récupération simple&#41;](../../relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md)  
  
-   [Restaurer la base de données MASTER &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/restore-the-master-database-transact-sql.md)  
  
-   [Afficher ou modifier le mode de récupération d’une base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server.md)  
  
-   [Déplacer des bases de données système](../../relational-databases/databases/move-system-databases.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Base de données de distribution](../../relational-databases/replication/distribution-database.md)   
 [Base de données master](../../relational-databases/databases/master-database.md)   
 [Base de données msdb](../../relational-databases/databases/msdb-database.md)   
 [Base de données model](../../relational-databases/databases/model-database.md)   
 [Base de données Resource](../../relational-databases/databases/resource-database.md)   
 [Base de données tempdb](../../relational-databases/databases/tempdb-database.md)  
  
  
