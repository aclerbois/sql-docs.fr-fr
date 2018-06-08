---
title: Réduire une base de données | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: databases
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.shrinkdatabase.f1
helpviewer_keywords:
- shrinking databases
- databases [SQL Server], shrinking
- decreasing database size
- database shrinking [SQL Server]
- reducing database size
ms.assetid: 83afbf74-fd50-4c39-831c-b1f473a50620
caps.latest.revision: 42
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 1df06190df852cab19c5d3c846e588e43b337c0b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="shrink-a-database"></a>Réduire une base de données
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Cette rubrique explique comment réduire une base de données au moyen de l'Explorateur d'objets dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 La réduction des fichiers de données permet de récupérer de l'espace en déplaçant des pages de données de la fin du fichier vers un espace inoccupé plus proche de l'avant du fichier. Quand une quantité d'espace libre suffisante est créée à la fin du fichier, des pages de données à la fin du fichier peuvent être désallouées et retournées au système de fichiers.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Recommandations](#Recommendations)  
  
     [Sécurité](#Security)  
  
-   **Pour réduire une base de données, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Follow Up:**  [You shrink a database](#FollowUp)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Restrictions"></a> Limitations et restrictions  
  
-   La base de données ne peut pas être réduite à une taille inférieure à la taille minimale de la base de données. La taille minimale d'une base de données correspond à la taille spécifiée lors de sa création ou lors de la dernière définition de taille explicite lors d'une opération de modification de taille, notamment au moyen de DBCC SHRINKFILE. Par exemple, la plus petite taille que pourrait avoir une base de données de 10 Mo initialement et de 100 Mo avant réduction, même si toutes les données qu'elle contient ont été supprimées, est de 10 Mo après réduction.  
  
-   Vous ne pouvez pas réduire la taille d'une base de données en cours de sauvegarde. Inversement, vous ne pouvez pas sauvegarder une base de données alors qu'elle fait l'objet d'une opération de réduction.  
  
-   DBCC SHRINKDATABASE échoue lorsqu'il rencontre un index columnstore optimisé en mémoire xVelocity. Le travail effectué avant de rencontrer l'index columnstore réussit, de sorte que la base de données peut être plus petite. Pour exécuter DBCC SHRINKDATABASE, désactivez tous les index columnstore avant d'exécuter DBCC SHRINKDATABASE, puis reconstruisez les index.  
  
###  <a name="Recommendations"></a> Recommandations  
  
-   Pour visualiser la quantité d'espace actuellement libre (non allouée) dans la base de données. Pour plus d'informations, consultez [Display Data and Log Space Information for a Database](../../relational-databases/databases/display-data-and-log-space-information-for-a-database.md)  
  
-   Prenez en compte les informations suivantes lorsque vous envisagez de réduire une base de données :  
  
    -   Une opération de réduction de taille de fichier est plus efficace après l'exécution d'une opération qui crée une grande quantité d'espace inutilisé, telle qu'une troncature de table ou une suppression de table.  
  
    -   Un certain espace libre doit exister pour les opérations quotidiennes courantes pour la plupart des bases de données. Si vous réduisez plusieurs fois la taille d'une base de données et que vous constatez que la taille augmente de nouveau, cela indique que l'espace qui a été réduit est nécessaire pour les opérations courantes. Dans ce cas, la réduction de la taille de la base de données ne sert à rien.  
  
    -   Une opération de réduction ne conserve pas l'état de fragmentation des index de la base de données, et augmente généralement la fragmentation. Il s'agit là d'une raison supplémentaire pour ne pas réduire la taille de la base de données de manière répétitive.  
  
    -   Sauf en cas de besoin précis, n'attribuez pas la valeur ON à l'option de base de données AUTO_SHRINK.  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Nécessite l’appartenance au rôle de serveur fixe **sysadmin** ou au rôle de base de données fixe **db_owner** .  
  
##  <a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-shrink-a-database"></a>Pour réduire une base de données  
  
1.  Dans **l’Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], puis développez-la.  
  
2.  Développez **Bases de données**, puis cliquez avec le bouton droit sur la base de données à réduire.  
  
3.  Pointez sur **Tâches**puis sur **Réduire**, puis cliquez sur **Base de données**.  
  
     **Sauvegarde de la base de données**  
     Affiche le nom de la base de données sélectionnée.  
  
     **Espace actuellement alloué**  
     Affiche l'espace total, utilisé et non utilisé, pour la base de données sélectionnée.  
  
     **Espace libre disponible**  
     Affiche la totalité de l'espace disponible dans les fichiers de données et les fichiers journaux de la base de données sélectionnée.  
  
     **Réorganiser les fichiers avant de libérer de l'espace inutilisé**  
     Activer cette option équivaut à exécuter DBCC SHRINKDATABASE en spécifiant une option de pourcentage cible. Désactiver cette option équivaut à exécuter DBCC SHRINKDATABASE avec l'option TRUNCATEONLY. Par défaut, cette option n'est pas sélectionnée à l'ouverture de la boîte de dialogue. Si cette option est sélectionnée, l'utilisateur doit spécifier une option de pourcentage cible.  
  
     **Espace libre maximal dans les fichiers après réduction**  
     Entrez le pourcentage maximal d'espace disponible à conserver dans les fichiers de base de données après la réduction de la base de données. Les valeurs autorisées sont comprises entre 0 et 99.  
  
4.  Cliquez sur **OK**.  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-shrink-a-database"></a>Pour réduire une base de données  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. L'exemple suivant utilise [DBCC SHRINKDATABASE](../../t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql.md) pour diminuer la taille des fichiers de données et journaux de la base de données utilisateur `UserDB` pour obtenir `10` % d'espace libre dans la base de données.  
  
 [!code-sql[DBCC#DBCC_SHRINKDB1](../../relational-databases/databases/codesnippet/tsql/shrink-a-database_1.sql)]  
  
##  <a name="FollowUp"></a> Suivi : Après avoir réduit une base de données  
 Les données qui sont déplacées pour réduire un fichier peuvent être dispersées à n'importe quel emplacement disponible dans le fichier. Cela provoque la fragmentation de l'index et peut ralentir les performances des requêtes qui recherchent une plage de l'index. Pour éliminer la fragmentation, reconstruisez les index dans le fichier après réduction.  
  
## <a name="see-also"></a> Voir aussi  
 [Réduire un fichier](../../relational-databases/databases/shrink-a-file.md)   
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)   
 [DBCC SHRINKFILE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-shrinkfile-transact-sql.md)   
 [Groupes de fichiers et fichiers de base de données](../../relational-databases/databases/database-files-and-filegroups.md)  
  
  
