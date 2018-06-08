---
title: Rétablir une base de données dans l’état d’un instantané de base de données | Microsoft Docs
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: databases
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- database snapshots [SQL Server], reverting to
- reverting databases
ms.assetid: 8f74dd31-c9ca-4537-8760-0c7648f0787d
caps.latest.revision: 58
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 8a6bdd055cc32d6f01ec017f72c7caa8f503754f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="revert-a-database-to-a-database-snapshot"></a>Rétablir une base de données dans l'état d'un instantané de base de données
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Si les données d'une base de données en ligne sont endommagées, dans certains cas, rétablir la base de données dans l'état d'un instantané précédant le problème peut être une bonne solution plutôt que de restaurer la base de données à partir d'une sauvegarde. Par exemple, rétablir une base de données peut être utile pour annuler une grave erreur de l'utilisateur, telle que la suppression d'une table. Toutefois, toutes les modifications apportées depuis la création de l'instantané sont perdues.  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Configuration requise](#Prerequisites)  
  
     [Sécurité](#Security)  
  
-   **Pour rétablir une base de données dans l’état d’un instantané de base de données à l’aide de :**  [Transact-SQL](#TsqlProcedure)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Restrictions"></a> Limitations et restrictions  
 Le rétablissement n'est pas pris en charge dans les conditions suivantes :  
  
-   La base de données doit actuellement posséder un seul instantané de base de données, que vous prévoyez de rétablir.  
  
-   Des groupes de fichiers compressés ou en lecture seule existent dans la base de données.  
  
-   Des fichiers sont maintenant hors connexion alors qu'ils étaient en ligne lors de la création de l'instantané.  
  
 Avant de rétablir une base de données, tenez compte des limitations suivantes :  
  
-   Le rétablissement n'est pas destiné à la récupération des supports. . L'instantané de base de données est une copie incomplète des fichiers de base de données, donc si la base de données ou l'instantané de base de données est endommagé, le rétablissement à partir d'un instantané sera probablement impossible. En outre, même lorsque le rétablissement est possible, il est peu probable qu'il permette de corriger le problème en cas de corruption. Par conséquent, effectuer des sauvegardes régulières et tester votre plan de restauration sont essentiels pour la protection d'une base de données. Pour plus d’informations, consultez [Back Up and Restore of SQL Server Databases](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md).  
  
    > [!NOTE]  
    >  Si vous devez restaurer la base de données source jusqu'à la date et l'heure où vous avez créé un instantané de base de données, utilisez le mode de récupération complète et implémentez une stratégie de sauvegarde qui vous permette une telle opération.  
  
-   La base de données source d'origine est remplacée par la base de données rétablie, donc toutes les mises à jour de la base de données depuis la création de l'instantané sont perdues.  
  
-   L'opération de retour à un état antérieur remplace également l'ancien fichier journal puis le reconstruit. De ce fait, vous ne pouvez plus restaurer la base de données par progression jusqu'à l'erreur de l'utilisateur. Aussi, nous vous recommandons de sauvegarder le journal avant de rétablir une base de données.  
  
    > [!NOTE]  
    >  Bien que vous ne puissiez pas restaurer le journal original pour restaurer la base de données par progression, les informations qu'il contient peuvent être utiles pour reconstituer les données perdues.  
  
-   Le retour à un état antérieur brise la chaîne de sauvegarde du journal. Aussi, avant d'effectuer les sauvegardes du journal de la base de données ainsi récupérée, vous devez d'abord effectuer une sauvegarde de base de données complète ou une sauvegarde de fichiers. Nous recommandons une sauvegarde de base de données complète.  
  
-   Au cours d'une opération de retour à un état antérieur, l'instantané et la base de données source sont tous deux inaccessibles. Elles sont marquées comme étant en cours de restauration. En cas d'erreur pendant cette opération, lorsque la base de données redémarrera, la restauration tentera de se terminer.  
  
-   Les métadonnées d'une base de données ainsi restituée sont les mêmes qu'au moment de l'instantané.  
  
-   Le rétablissement supprime tous les catalogues de texte intégral.  
  
###  <a name="Prerequisites"></a> Conditions préalables  
 Vérifiez que la base de données source et l'instantané de base de données remplissent les conditions préalables suivantes :  
  
-   Vérifiez que la base de données n'a pas été endommagée.  
  
    > [!NOTE]  
    >  Si la base de données a été endommagée, vous devez la restaurer à partir des sauvegardes. Pour plus d’informations, consultez [Restaurations complètes de bases de données &#40;mode de récupération simple&#41;](../../relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md) ou [Restaurations complètes de bases de données &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/complete-database-restores-full-recovery-model.md).  
  
-   Identifiez un instantané récent, créé avant l'erreur. Pour plus d’informations, consultez [Afficher un instantané de base de données &#40;SQL Server&#41;](../../relational-databases/databases/view-a-database-snapshot-sql-server.md).  
  
-   Supprimez tous les autres instantanés qui existent actuellement sur la base de données. Pour plus d’informations, consultez [Supprimer un instantané de base de données &#40;Transact-SQL&#41;](../../relational-databases/databases/drop-a-database-snapshot-transact-sql.md).  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Tout utilisateur qui dispose des autorisations RESTORE DATABASE sur la base de données source peut la rétablir dans l'état qui était le sien au moment où l'instantané a été créé.  
  
##  <a name="TsqlProcedure"></a> Comment rétablir une base de données dans l'état d'un instantané de base de données (à l'aide de Transact-SQL)  
 **Pour rétablir une base de données dans l'état d'un instantané de base de données**  
  
> [!NOTE]  
>  Pour obtenir un exemple de cette procédure, consultez [Exemples (Transact-SQL)](#TsqlExample), plus loin dans cette section.  
  
1.  Identifiez l'instantané de base de données auquel vous souhaitez restaurer la base de données. Vous pouvez afficher les instantanés sur une base de données dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (consultez [Afficher un instantané de base de données &#40;SQL Server&#41;](../../relational-databases/databases/view-a-database-snapshot-sql-server.md)). En outre, vous pouvez identifier la base de données source d’un affichage à partir de la colonne **source_database_id** de l’affichage catalogue [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) .  
  
2.  Supprimez tous les autres instantanés de base de données.  
  
     Pour plus d’informations sur la suppression de captures instantanées, consultez [Supprimer un instantané de base de données &#40;Transact-SQL&#41;](../../relational-databases/databases/drop-a-database-snapshot-transact-sql.md). Si la base de données utilise le mode de restauration complète, vous devez sauvegarder le journal avant de rétablir la base de données. Pour plus d’informations, consultez [Sauvegarder un journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md) ou [Sauvegarder le journal des transactions lorsque la base de données est endommagée &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-the-transaction-log-when-the-database-is-damaged-sql-server.md).  
  
3.  Effectuez l'opération de restauration.  
  
     Une opération de restauration nécessite des autorisations RESTORE DATABASE sur la base de données source. Pour restaurer la base de données, utilisez l'instruction Transact-SQL suivante :  
  
     RESTORE DATABASE *database_name* FROM DATABASE_SNAPSHOT **=***database_snapshot_name*  
  
     Où *nom_base_de_données* est la base de données source et *nom_instantané_base_de_données* le nom de l’instantané auquel vous souhaitez rétablir la base de données. Notez que dans cette instruction, vous devez spécifier un nom d'instantané et non un périphérique de sauvegarde.  
  
     Pour plus d’informations, consultez [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).  
  
    > [!NOTE]  
    >  Pendant l'opération de restauration, l'instantané et la base de données source ne sont pas disponibles. La base de données source et l'instantané sont tous deux signalés « In restore (en restauration) ». Si une erreur se produit pendant l'opération de restauration, cette dernière tentera d'aboutir lors du redémarrage de la base de données.  
  
4.  Si le propriétaire de la base de données a changé depuis la création de l'instantané de base de données, il convient de mettre à jour le propriétaire de la base de données restaurée.  
  
    > [!NOTE]  
    >  La base de données restaurée conserve les autorisations et la configuration (par exemple, le propriétaire de la base de données et le mode de récupération) de l'instantané de base de données.  
  
5.  Démarrez la base de données.  
  
6.  En option, sauvegardez la base de données restaurée, notamment si elle utilise le mode de récupération complète (ou utilisant les journaux de transactions). Pour sauvegarder une base de données, consultez [Créer une sauvegarde complète de base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md).  
  
###  <a name="TsqlExample"></a> Exemples (Transact-SQL)  
 Cette section présente les exemples suivants de rétablissement d'une base de données à un état antérieur par le biais d'un instantané de base de données :  
  
-   A. [Rétablissement d'un instantané sur la base de données AdventureWorks](#Reverting_AW)  
  
-   B. [Rétablissement d'un instantané sur la base de données Sales (Ventes)](#Reverting_Sales)  
  
####  <a name="Reverting_AW"></a> A. Rétablissement d'un instantané sur la base de données AdventureWorks  
 L'exemple suivant part du principe qu'un seul instantané existe actuellement sur la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] . Pour l’exemple qui crée l’instantané auquel la base de données est ici rétablie, consultez [Créer un instantané de base de données &#40;Transact-SQL&#41;](../../relational-databases/databases/create-a-database-snapshot-transact-sql.md).  
  
```  
USE master;  
-- Reverting AdventureWorks to AdventureWorks_dbss1800  
RESTORE DATABASE AdventureWorks from   
DATABASE_SNAPSHOT = 'AdventureWorks_dbss1800';  
GO  
```  
  
####  <a name="Reverting_Sales"></a> B. Rétablissement d'un instantané sur la base de données Sales (Ventes)  
 Cet exemple suppose que deux instantanés existent actuellement sur la base de données **Sales** : **sales_snapshot0600** et **sales_snapshot1200**. Cet exemple supprime l'instantané le plus ancien et rétablit la base de données au moyen de l'instantané le plus récent.  
  
 Pour voir le code servant à créer la base de données et les instantanés donnés en exemple ici, consultez :  
  
-   Pour la base de données **Sales** et l’instantané **sales_snapshot0600**, consultez « Création d’une base de données avec des groupes de fichiers » et « Création d’un instantané de base de données » dans [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-sql-server-transact-sql.md).  
  
-   Pour la base de données **sales_snapshot1200** , consultez « Création d’un instantané de la base de données Sales (Ventes) » dans [Créer un instantané de base de données &#40;Transact-SQL&#41;](../../relational-databases/databases/create-a-database-snapshot-transact-sql.md).  
  
```  
--Test to see if sales_snapshot0600 exists and if it   
-- does, delete it.  
IF EXISTS (SELECT dbid FROM sys.databases  
    WHERE NAME='sales_snapshot0600')  
    DROP DATABASE SalesSnapshot0600;  
GO  
-- Reverting Sales to sales_snapshot1200  
USE master;  
RESTORE DATABASE Sales FROM DATABASE_SNAPSHOT = 'sales_snapshot1200';  
GO  
```  
  
##  <a name="RelatedTasks"></a> Tâches associées  
  
-   [Créer un instantané de base de données &#40;Transact-SQL&#41;](../../relational-databases/databases/create-a-database-snapshot-transact-sql.md)  
  
-   [Afficher un instantané de base de données &#40;SQL Server&#41;](../../relational-databases/databases/view-a-database-snapshot-sql-server.md)  
  
-   [Supprimer un instantané de base de données &#40;Transact-SQL&#41;](../../relational-databases/databases/drop-a-database-snapshot-transact-sql.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Instantanés de base de données &#40;SQL Server&#41;](../../relational-databases/databases/database-snapshots-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [Mise en miroir et instantanés de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-and-database-snapshots-sql-server.md)  
  
  
