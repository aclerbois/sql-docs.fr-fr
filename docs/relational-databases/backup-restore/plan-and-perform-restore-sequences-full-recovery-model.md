---
title: Planifier et exécuter des séquences de restauration (mode de récupération complète) | Microsoft Docs
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
- restore sequences [SQL Server], planning for
- full recovery model [SQL Server], planning restore sequences
ms.assetid: 9cbefaf8-d2b6-41c9-83fc-b3807a841fe2
caps.latest.revision: 33
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 9b76cce7b1944da1e00c423de448628ae42614a3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="plan-and-perform-restore-sequences-full-recovery-model"></a>Planifier et exécuter des séquences de restauration (mode de récupération complète)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Cette rubrique explique comment planifier et effectuer une séquence de restauration pour les bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui utilisent habituellement le mode de restauration complète. Une *séquence de restauration* est une séquence contenant une ou plusieurs instructions [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md) . En règle générale, une séquence de restauration initialise le contenu de la base de données, des fichiers et/ou des pages en cours de restauration (phase de copie des données), restaure les transactions journalisées (phase de restauration par progression) et annule les transactions non validées (phase de restauration).  
  
 Dans les cas simples, une séquence de restauration nécessite uniquement une sauvegarde complète de base de données, une sauvegarde différentielle de base de données et les sauvegardes du journal réalisées ultérieurement. Dans ces cas, la construction d'une séquence de restauration correcte est facile. Par exemple, pour restaurer une base de données dans son ensemble jusqu’au moment de la défaillance, commencez par sauvegarder le journal des transactions actives (la *fin* du journal). Puis, restaurez la sauvegarde complète de base de données la plus récente, la sauvegarde différentielle la plus récente (si elle existe) et toutes les sauvegardes du journal réalisées ultérieurement dans l'ordre dans lequel elles ont été effectuées.  
  
 Dans les cas plus complexes, la construction d'une séquence de restauration correcte peut constituer un processus complexe. Par exemple, une séquence de restauration peut nécessiter plusieurs sauvegardes de fichiers ou la restauration de données à un point dans le temps spécifique. Dans les cas très complexes, vous serez peut-être obligé de traverser un chemin de récupération ramifié comportant un ou plusieurs branchements de récupération.  
  
> [!NOTE]  
>  Un *chemin de récupération* correspond à la séquence de sauvegardes de données et de fichiers journaux qui a amené une base de données à un point particulier dans le temps (appelé point de récupération). Un chemin de récupération est un jeu spécifique de transformations qui ont fait évoluer la base de données au cours du temps, tout en maintenant sa cohérence. Un chemin de récupération décrit une plage de NSE entre un point de départ (NSE,GUID) et un point final (NSE,GUID). Le jeu de NSE d'un chemin de récupération peut traverser une ou plusieurs branches de récupération entre le début et la fin.  
  
## <a name="to-plan-a-restore-sequence"></a>Pour planifier une séquence de restauration  
 Avant de démarrer une séquence de restauration, procédez comme suit :  
  
1.  Créez une sauvegarde de la fin du journal de la base de données, si possible. Pour plus d’informations, consultez [Sauvegardes de la fin du journal &#40;SQL Server&#41;](../../relational-databases/backup-restore/tail-log-backups-sql-server.md).  
  
2.  Déterminez le point de récupération cible.  
  
     Il peut s'agir de n'importe quel point dans le temps ou d'une marque au sein d’une sauvegarde du journal des transactions. Pour plus d’informations, consultez [Restaurer une base de données SQL Server jusqu’à une limite dans le temps &#40;Mode de récupération complète&#41;](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md) ou [Utiliser les transactions marquées pour récupérer des bases de données associées uniformément &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/use-marked-transactions-to-recover-related-databases-consistently.md).  
  
3.  Déterminez le type de restauration à effectuer. Pour plus d’informations, consultez [Vue d’ensemble de la restauration et de la récupération &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md).  
  
4.  Identifiez les sauvegardes dont vous avez besoin et vérifiez que les jeux de supports de sauvegarde et unités de sauvegarde nécessaires sont disponibles. Pour plus d’informations, consultez [Unités de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md) et [Jeux de supports, familles de supports et jeux de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md).  
  
## <a name="to-perform-a-restore-sequence"></a>Pour exécuter une séquence de restauration  
 Pour exécuter une séquence de restauration, procédez comme suit :  
  
1.  Pour commencer la séquence, restaurez une ou plusieurs sauvegardes de données, telles que : une sauvegarde de base de données, une sauvegarde partielle, une ou plusieurs sauvegardes de fichiers.  
  
2.  Restaurez éventuellement les dernières sauvegardes différentielles qui sont basées sur ces sauvegardes complètes.  
  
     Pour chaque sauvegarde complète que vous envisagez de restaurer, déterminez si elle est la base d'une sauvegarde différentielle. Si tel est le cas, restaurez la sauvegarde différentielle la plus récente, si possible. Pour plus d’informations, consultez [Sauvegardes différentielles &#40;SQL Server&#41;](../../relational-databases/backup-restore/differential-backups-sql-server.md).  
  
3.  Restaurez la base de données par progression en restaurant les sauvegardes de journal de transactions en séquence, en terminant par la sauvegarde contenant le point de récupération. L'application ou non de toutes les sauvegardes du journal dépend de la sauvegarde du journal qui contient le point de récupération cible, comme suit :  
  
    -   Si ce point correspond au point de défaillance, vous devez restaurer chaque sauvegarde du journal ayant été créée depuis la dernière sauvegarde de données (complète ou différentielle) que vous avez restaurée. Pour plus d’informations, consultez [Appliquer les sauvegardes du journal de transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md).  
  
    -   Pour une restauration limitée dans le temps, vous n'aurez sans doute pas besoin des sauvegardes du journal les plus récentes. Si vous utilisez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], l'Assistant Récupération de base de données garantit que seules les sauvegardes requises pour la restauration de votre limite dans le temps spécifique sont sélectionnées. Ces sauvegardes constituent le plan de restauration recommandé pour votre limite de restauration dans le temps. Pour plus d’informations, consultez [Restaurer une base de données SQL Server jusqu’à une limite dans le temps &#40;Mode de récupération complète&#41;](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md).  
  
## <a name="restarting-a-restore-sequence"></a>Redémarrage d'une séquence de restauration  
 Si vous rencontrez un problème avec le résultat d'une séquence de restauration, vous pouvez l'arrêter et la redémarrer à partir du début. Par exemple, si vous restaurez accidentellement un trop grand nombre de sauvegardes de journal et allez au-delà du point de récupération souhaité, vous devez redémarrer la séquence de restauration jusqu'à la sauvegarde du journal qui contient le point de récupération cible.  
  
## <a name="see-also"></a> Voir aussi  
 [Vue d’ensemble de la sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md)   
 [Vue d’ensemble de la restauration et de la récupération &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)   
 [Restaurations complètes de bases de données &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/complete-database-restores-full-recovery-model.md)   
 [Restauration en ligne &#40;SQL Server&#41;](../../relational-databases/backup-restore/online-restore-sql-server.md)   
 [Restaurations de fichiers &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/file-restores-full-recovery-model.md)   
 [Restaurer des pages &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-pages-sql-server.md)   
 [Restaurations fragmentaires &#40;SQL Server&#41;](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md)  
  
  
