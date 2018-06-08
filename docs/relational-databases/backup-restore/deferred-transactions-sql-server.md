---
title: Transactions différées (SQL Server) | Microsoft Docs
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
- I/O [SQL Server], database recovery
- restoring pages [SQL Server]
- deferred transactions
- modifying transaction deferred state
ms.assetid: 6fc0f9b6-d3ea-4971-9f27-d0195d1ff718
caps.latest.revision: 45
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: dbfd471b4f6e5a7d7fa4f06ec317d8c2259e7928
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="deferred-transactions-sql-server"></a>Transactions différées (SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise, une transaction endommagée est différée si les données requises par la restauration (annulation) sont hors connexion durant le démarrage de la base de données. Une *transaction différée* est une transaction qui n’est pas validée lorsque la phase de restauration par progression s’achève et dont la restauration ne peut pas être annulée en raison d’une erreur. Si la transaction ne peut pas être annulée, elle est différée.  
  
> [!NOTE]  
>  Les transactions endommagées sont différées uniquement dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise. Pour les autres éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], une transaction endommagée peut entraîner l'échec du démarrage.  
  
 Généralement, une transaction différée peut se produire si, au cours d'une séquence de restauration de la base de données, une erreur d'E/S a empêché la lecture d'une page requise par la transaction. Cependant, une erreur au niveau des fichiers peut également engendrer des transactions différées. Une transaction différée peut aussi se produire lorsqu'une séquence de restauration partielle s'arrête à un point où l'annulation de la transaction est nécessaire et où une transaction requiert des données hors connexion.  
  
 Les transactions utilisateur en cours d'annulation qui rencontrent une erreur d'E/S provoquent la mise hors connexion de la totalité de la base de données. Lorsque la base de données est remise en ligne, la restauration par progression acquiert de nouveau tous les verrous qu'elle possédait et tente de restaurer toutes les transactions non validées. Toutes les données modifiées par une transaction demeurent correctement verrouillées jusqu'à ce que la transaction ait été restaurée. Les transactions qui ne peuvent pas être restaurées abandonneront leurs verrous une fois l'altération corrigée et la base de données redémarrée ou, après une restauration en ligne, une fois que sont résolues les transactions différées tandis que la base de données est en ligne. À ce stade, une transaction différée peut contenir des verrous qui empêchent certaines opérations sur la base de données dans sa globalité. Par exemple, si une transaction différée contient une instruction CREATE TABLE, aucun utilisateur ne peut créer de table tant que la transaction différée n'a pas été résolue.  
  
 Une transaction différée peut aussi se produire lorsqu'une restauration fragmentaire récupère une base de données à un point où une ou plusieurs transactions actives affectent un groupe de fichiers hors connexion et n'ayant pas encore été restaurés. Si les transactions ne peuvent pas être restaurées, elles deviennent différées.  
  
 Le tableau suivant répertorie les actions qui amènent une base de données à réaliser une récupération et le résultat en cas de problème d'E/S.  
  
|Action|Résolution (en cas de problèmes d'E/S ou de données requises hors connexion)|  
|------------|-----------------------------------------------------------------------|  
|Démarrage du serveur|transaction différée|  
|Restaurer|Transaction différée|  
|Attacher|Échec de l'attachement|  
|Redémarrage automatique|transaction différée|  
|Création d'une base de données ou d'un instantané de base de données|Échec de la création|  
|Restauration par progression sur mise en miroir de bases de données|transaction différée|  
|Groupe de fichiers hors connexion|transaction différée|  
  
## <a name="moving-a-transaction-out-of-the-deferred-state"></a>Mise d'une transaction hors de l'état DEFERRED  
  
> [!IMPORTANT]  
>  Une transaction différée maintient actif le journal des transactions. Un fichier journal virtuel qui contient des transactions différées ne peut pas être tronquées tant que ces transactions n'ont pas quitté l'état différé. Pour plus d’informations sur la troncation de journal, consultez [Journal des transactions &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md).  
  
 Pour que la transaction ne soit plus dans un état différé, la base de données doit démarrer correctement sans erreurs d'E/S. Si des transactions différées existent, vous devez corriger la source des erreurs d'E/S. Les solutions possibles sont répertoriées ci-dessous dans l'ordre dans lequel elles sont essayées habituellement :  
  
-   Redémarrez la base de données. Si le problème était temporaire, la base de données devrait démarrer sans transactions différées.  
  
-   Si les transactions ont été différées parce qu'un groupe de fichiers se trouvait hors connexion, placez le groupe de fichiers en ligne.  
  
     Pour remettre en ligne un groupe de fichiers hors connexion, utilisez l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante :  
  
    ```  
    RESTORE DATABASE database_name FILEGROUP=<filegroup_name>  
    ```  
  
-   Restaurez la base de données. Après une restauration en ligne, toutes les transactions différées sont résolues.  
  
     En mode de restauration complète ou en mode de récupération utilisant les journaux de transactions, si les transactions différées sont dues à quelques pages endommagées, une restauration de pages en ligne peut éventuellement résoudre les erreurs (là où elle est prise en charge).  
  
-   Si vous n'avez plus besoin d'un groupe de fichiers dont l'état hors connexion entraîne des transactions différées, rendez-les obsolètes. Les transactions qui étaient différées en raison du groupe de fichiers hors connexion quittent l'état différé une fois que le groupe de fichiers est obsolète.  
  
    > [!IMPORTANT]  
    >  Un groupe de fichiers obsolète ne peut jamais être récupéré.  
  
     Pour plus d’informations, consultez [Supprimer des groupes de fichiers obsolètes &#40;SQL Server&#41;](../../relational-databases/backup-restore/remove-defunct-filegroups-sql-server.md).  
  
-   Si les transactions ont été différées en raison d'une page erronée et s'il n'existe pas de bonne sauvegarde de la base de données, procédez comme suit pour réparer la base de données :  
  
    -   Commencez par mettre la base de données en mode urgence en exécutant l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante :  
  
        ```  
        ALTER DATABASE <database_name> SET EMERGENCY  
        ```  
  
         Pour plus d’informations sur le mode urgence, consultez [États d’une base de données](../../relational-databases/databases/database-states.md).  
  
    -   Ensuite, réparez la base de données à l’aide de l’option DBCC REPAIR_ALLOW_DATA_LOSS dans l’une des instructions DBCC suivantes : [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md), [DBCC CHECKALLOC](../../t-sql/database-console-commands/dbcc-checkalloc-transact-sql.md)ou [DBCC CHECKTABLE](../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md).  
  
         Lorsque la commande DBCC rencontre la page erronée, elle la désalloue et répare les éventuelles erreurs associées. Cette approche permet de remettre en ligne la base de données dans un état physique cohérent. Il se peut néanmoins que d'autres données soient perdues ; c'est pourquoi cette approche ne doit être adoptée qu'en dernier recours.  
  
## <a name="see-also"></a> Voir aussi  
 [Vue d’ensemble de la restauration et de la récupération &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)   
 [Supprimer des groupes de fichiers obsolètes &#40;SQL Server&#41;](../../relational-databases/backup-restore/remove-defunct-filegroups-sql-server.md)   
 [Restaurations de fichiers &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/file-restores-full-recovery-model.md)   
 [Restaurations de fichiers &#40;mode de récupération simple&#41;](../../relational-databases/backup-restore/file-restores-simple-recovery-model.md)   
 [Restaurer des pages &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-pages-sql-server.md)   
 [Restaurations fragmentaires &#40;SQL Server&#41;](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
  
