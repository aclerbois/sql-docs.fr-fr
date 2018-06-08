---
title: Afficher ou modifier le mode de récupération d’une base de données (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 08/05/2016
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.suite: sql
ms.technology: backup-restore
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- database backups [SQL Server], recovery models
- recovery [SQL Server], recovery model
- backing up databases [SQL Server], recovery models
- recovery models [SQL Server], switching
- recovery models [SQL Server], viewing
- database restores [SQL Server], recovery models
- modifying database recovery models
ms.assetid: 94918d1d-7c10-4be7-bf9f-27e00b003a0f
caps.latest.revision: 40
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: d48fb1394319e9f6e43f59ecf35903b6cb1320f5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="view-or-change-the-recovery-model-of-a-database-sql-server"></a>Afficher ou modifier le mode de récupération d'une base de données (SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Cette rubrique explique comment afficher ou modifier la base de données à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. 
  
  Un *mode de récupération* est une propriété de base de données qui contrôle la façon dont les transactions sont journalisées, précise si le journal des transactions nécessite (et permet) une sauvegarde et spécifie les types d’opérations de restauration disponibles. Il existe trois modes de récupération : simple, complète et utilisant les journaux de transactions. En règle générale, une base de données utilise le mode de restauration complète ou le mode de récupération simple. Il est possible de modifier le mode de récupération d'une base de données à tout moment. La base de données **model** définit le mode de récupération par défaut des nouvelles bases de données.  
  
  Pour obtenir une explication plus approfondie des [modèles de récupération](recovery-models-sql-server.md), consultez le site web sur les [modèles de récupération](https://www.mssqltips.com/sqlservertutorial/2/sql-server-recovery-models/) de SQL Server de l’équipe [MSSQLTips!](https://www.mssqltips.com/)
  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  

-   **Avant** de passer en [mode de récupération complète ou en mode de récupération utilisant les journaux de transactions](recovery-models-sql-server.md), [sauvegardez le journal des transactions](back-up-a-transaction-log-sql-server.md).  
  
-   La récupération jusqu'à une date et heure n'est pas possible dans le mode de récupération utilisant les journaux de transactions. Si vous exécutez des transactions en mode de récupération utilisant les journaux de transactions, pouvant nécessiter une restauration du journal des transactions, ces transactions peuvent être exposées à des pertes de données. Pour optimiser la possibilité de récupérer les données dans un scénario de récupération après sinistre, passez au mode de récupération utilisant les journaux de transactions dans les conditions suivantes :  
  
    -   Les utilisateurs ne sont pas actuellement autorisés dans la base de données.  
  
    -   Toutes modifications effectuées au cours du traitement en bloc sont récupérables sans une restauration du journal en réexécutant, par exemple, les processus en bloc.  
  
     Si ces deux conditions sont satisfaites, vous ne serez pas exposé à des pertes de données lors d'une restauration du journal des transactions sauvegardé en mode de récupération utilisant les journaux de transactions.  
  
**Remarque :** Si vous adoptez le mode de récupération complète pendant une opération en bloc, la journalisation des opérations en bloc passe de la journalisation minimale à la journalisation complète, et inversement.  
  
###  <a name="Security"></a> Autorisations requises  
   Nécessite l'autorisation ALTER sur la base de données.  
  
##  <a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-view-or-change-the-recovery-model"></a>Pour afficher ou modifier le mode de récupération  
  
1.  Après vous être connecté à l'instance appropriée du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], dans l'Explorateur d'objets, cliquez sur le nom du serveur pour développer son arborescence.  
  
2.  Développez **Bases de données**puis, selon la base de données, sélectionnez une base de données utilisateur ou développez **Bases de données système** et sélectionnez une base de données système.  
  
3.  Cliquez avec le bouton droit de la souris sur la base de données, puis cliquez sur **Propriétés**pour ouvrir la boîte de dialogue **Propriétés de la base de données** .  
  
4.  Dans le volet **Sélectionner une page** , cliquez sur **Options**.  
  
5.  Le mode de récupération actuel s'affiche dans la zone de liste **Mode de récupération** .  
  
6.  Au besoin, pour modifier le mode de récupération, sélectionnez un autre mode dans la liste. Les choix sont **Complet**, **Journalisé en bloc**ou **Simple**.  
  
7.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-view-the-recovery-model"></a>Pour afficher le mode de récupération  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple montre comment interroger l'affichage catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) pour connaître le mode de récupération de la base de données **model** .  
  
```sql  
SELECT name, recovery_model_desc  
   FROM sys.databases  
      WHERE name = 'model' ;  
GO  
  
```  
  
#### <a name="to-change-the-recovery-model"></a>Pour modifier le mode de récupération  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple montre comment modifier le mode de récupération de la base de données `model` en `FULL` à l'aide de l'option `SET RECOVERY` de l'instruction [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-set-options.md) .  
  
```sql  
USE master ;  
ALTER DATABASE model SET RECOVERY FULL ;  
```  
  
##  <a name="FollowUp"></a> Recommandations : Après avoir modifié le mode de récupération  
  
-   **Après un changement de mode de récupération complète ou de mode de récupération utilisant les journaux de transactions**  
  
    -   Repassez immédiatement en mode de récupération complète après avoir effectué les opérations en bloc.  
  
    -   Après être passé du mode de récupération utilisant les journaux de transactions au mode de récupération complète, sauvegardez le journal.  
  
        >**REMARQUE :** votre stratégie de sauvegarde ne change pas : continuez à effectuer régulièrement des sauvegardes des bases de données, des sauvegardes des journaux et des sauvegardes différentielles.  
  
-   **Après basculement à partir du mode de récupération simple**  
  
    -   Aussitôt après être passé en mode de restauration complète ou en mode de récupération utilisant les journaux de transactions, procédez à une sauvegarde de base de données complète ou différentielle pour lancer la séquence de journaux.  
  
        >**REMARQUE :** le passage au mode de restauration complète ou mode de récupération utilisant les journaux de transactions n’est effectif qu’après la première sauvegarde de base de données.  
  
    -   Planifiez des sauvegardes de journaux régulières et mettez à jour votre plan de restauration en conséquence.  
  
        > **IMPORTANT !** Sauvegardez vos journaux ! Si vous ne sauvegardez pas assez souvent le journal, il est susceptible de s’étendre jusqu’à manquer de l’espace disque nécessaire !  
  
-   **Après basculement en mode de récupération simple**  
  
    -   Mettez fin à tous les travaux planifiés afin de sauvegarder le journal des transactions.  
  
    -   Assurez-vous que des sauvegardes des bases de données régulières sont planifiées. La sauvegarde de votre base de données est essentielle pour protéger vos données et tronquer la partie inactive du journal des transactions.  
  
##  <a name="RelatedTasks"></a> Tâches associées  
  
-   [Créer une sauvegarde complète de base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)  
  
-   [Sauvegarder un journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
  
-   [Créer un travail](http://msdn.microsoft.com/library/b35af2b6-6594-40d1-9861-4d5dd906048c)  
  
-   [Activer ou désactiver un travail](http://msdn.microsoft.com/library/5041261f-0c32-4d4a-8bee-59a6c16200dd)  
  
##  <a name="RelatedContent"></a> Contenu associé  
  
-   [Plans de maintenance de base de données](http://msdn.microsoft.com/library/ms187658.aspx) (dans la documentation en ligne de [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] )  
  
## <a name="see-also"></a> Voir aussi  
 [Modes de récupération &#40;SQL Server&#41;](../../relational-databases/backup-restore/recovery-models-sql-server.md)   
 [Journal des transactions &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [Modes de récupération &#40;SQL Server&#41;](../../relational-databases/backup-restore/recovery-models-sql-server.md)  
  
  
