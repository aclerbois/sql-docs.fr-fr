---
title: Restaurations fragmentaires (SQL Server) | Microsoft Docs
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
- partial updates [SQL Server]
- staged restores [SQL Server]
- piecemeal restores [SQL Server]
- restoring [SQL Server], piecemeal restore scenario
ms.assetid: 208f55e0-0762-4cfb-85c4-d36a76ea0f5b
caps.latest.revision: 74
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 2c0af1af6758a9520d36398dfd34dc56430dd392
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="piecemeal-restores-sql-server"></a>Restaurations fragmentaires (SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique concerne uniquement les bases de données de l'édition Enterprise de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui contiennent plusieurs fichiers ou groupes de fichiers et, dans le mode simple, seulement des groupes de fichiers en lecture seule.  
  
 Pour plus d’informations sur les restaurations fragmentaires et les tables optimisées en mémoire, consultez [Restauration fragmentaire de bases de données avec des tables optimisées en mémoire](../../relational-databases/in-memory-oltp/piecemeal-restore-of-databases-with-memory-optimized-tables.md).  
  
 Une*restauration fragmentaire* autorise la restauration et la récupération par étapes des bases de données contenant plusieurs groupes de fichiers. La restauration fragmentaire implique une suite de séquences de restauration, en commençant par le groupe de fichiers primaire et, dans certains cas, un ou plusieurs groupes de fichiers secondaires. La restauration fragmentaire maintient des contrôles pour garantir la cohérence de la base de données au final. À l'issue de la séquence de restauration, les fichiers récupérés (s'ils sont valides et cohérents avec la base de données) peuvent être mis en ligne directement.  
  
 La restauration fragmentaire fonctionne dans tous les modes de récupération, mais elle offre davantage de souplesse dans le mode de restauration complète et dans le mode de récupération utilisant les journaux de transactions que dans le mode de récupération simple.  
  
 Chaque restauration fragmentaire commence par une séquence de restauration initiale appelée *séquence de restauration partielle*. Au minimum, la séquence de restauration partielle restaure et récupère le groupe de fichiers primaire et, en mode de récupération simple, tous les groupes de fichiers qui sont en lecture/écriture. Pendant la séquence de restauration fragmentaire, la totalité de la base de données doit se trouver hors connexion. Après l'étape initiale, la base de données est en ligne et les groupes de fichiers restaurés sont disponibles. Toutefois, les groupes de fichiers non restaurés restent hors connexion et inaccessibles. Les groupes de fichiers hors connexion peuvent cependant être restaurés et mis en ligne ultérieurement via une restauration de fichiers.  
  
 Sans tenir compte du mode de récupération utilisé par la base de données, la séquence de restauration partielle commence par une instruction RESTORE DATABASE qui restaure une sauvegarde complète et spécifie l'option PARTIAL. Une nouvelle restauration fragmentaire commence toujours par cette option ; par conséquent, il vous suffit de spécifier PARTIAL une seule fois dans l'instruction initiale de la séquence de restauration partielle. Lorsque la séquence de restauration partielle est terminée et que la base de données est mise en ligne, les fichiers restants sont dans l'état « récupération en attente » du fait que la récupération des fichiers est reportée.  
  
 Par la suite, une restauration fragmentaire comprend généralement une ou plusieurs séquences de restauration appelées *séquences de restauration de groupe de fichiers*. Vous pouvez retarder l'exécution d'une séquence de restauration de groupe de fichiers aussi longtemps que vous le souhaitez. Chaque séquence de restauration de groupe de fichiers restaure et récupère un ou plusieurs groupes de fichiers hors connexion jusqu'à un point cohérent avec la base de données. La fréquence et le nombre de séquences de restauration de groupe de fichiers dépendent de votre objectif de récupération, du nombre de groupes de fichiers hors connexion que vous souhaitez restaurer et du nombre d'entre eux que vous restaurez par séquence de restauration de groupe de fichiers.  
  
 Les conditions requises qui correspondent précisément à l'exécution d'une restauration fragmentaire dépendent du mode de récupération de la base de données. Pour plus d'informations, consultez les sections « Restauration fragmentaire en mode de récupération simple » et « Restauration fragmentaire en mode de récupération complète » plus loin dans cette rubrique.  
  
## <a name="piecemeal-restore-scenarios"></a>Scénarios de restauration fragmentaire  
 Toutes les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prennent en charge les restaurations fragmentaires hors connexion. Dans l'édition Enterprise, les restaurations fragmentaires peuvent être en ligne ou hors connexion. L'impact des restaurations en ligne et hors connexion est le suivant :  
  
-   Scénario de restauration fragmentaire hors connexion  
  
     Dans une restauration fragmentaire hors ligne, la base de données est en ligne après la séquence de restauration partielle. Les groupes de fichiers qui n'ont pas encore été restaurés demeurent hors connexion mais si cela est nécessaire, ils peuvent être restaurés une fois la base de données mise hors connexion.  
  
-   Scénario de restauration fragmentaire en ligne  
  
     En cas de restauration fragmentaire en ligne, au terme de la séquence de restauration partielle, la base de données est en ligne et le groupe de fichiers primaire ainsi que tout groupe de fichiers secondaire récupéré sont mis à votre disposition. Les groupes de fichiers qui ne sont pas encore restaurés restent hors ligne, mais ils peuvent être restaurés le cas échéant alors que la base de données reste en ligne.  
  
     Les restaurations fragmentaires en ligne peuvent contenir des transactions différées. Lorsque la restauration ne concerne qu'un sous-ensemble de groupes de fichiers, les transactions de la base de données qui dépendent des groupes de fichiers en ligne peuvent être différées. Ceci est normal car l'ensemble de la base de données doit être cohérent. Pour plus d’informations, consultez [Transactions différées &#40;SQL Server&#41;](../../relational-databases/backup-restore/deferred-transactions-sql-server.md).  
  
-   [!INCLUDE[hek_1](../../includes/hek-1-md.md)] scénario de restauration fragmentaire  
  
     Pour plus d’informations sur les restaurations fragmentaires des bases de données de l’OLTP en mémoire, consultez [Sauvegarde et restauration fragmentaire de bases de données avec des tables optimisées en mémoire](../../relational-databases/in-memory-oltp/piecemeal-restore-of-databases-with-memory-optimized-tables.md).  
  
## <a name="restrictions"></a>Restrictions  
 Si une séquence de restauration partielle exclut tout groupe de fichiers [FILESTREAM](../../relational-databases/blob/filestream-sql-server.md) , la limite de restauration dans le temps n’est pas prise en charge. Vous pouvez forcer la séquence de restauration à continuer. Cependant, les groupes de fichiers FILESTREAM omis de l'instruction RESTORE ne peuvent jamais être restaurés. Pour forcer une limite de restauration dans le temps, spécifiez l'option CONTINUE_AFTER_ERROR avec l'option STOPAT, STOPATMARK ou STOPBEFOREMARKx, que vous devez également spécifier dans vos instructions RESTORE LOG suivantes. Si vous spécifiez l'option CONTINUE_AFTER_ERROR, la séquence de restauration partielle réussit et le groupe de fichiers FILESTREAM devient irrécupérable.  
  
## <a name="piecemeal-restore-under-the-simple-recovery-model"></a>Restauration fragmentaire en mode de récupération simple  
 En mode de récupération simple, la séquence de restauration fragmentaire doit commencer par une sauvegarde de base de données complète ou partielle. Puis, si la sauvegarde restaurée est une base différentielle, restaurez ensuite la dernière sauvegarde différentielle.  
  
 Au cours de la première séquence de restauration partielle, si vous restaurez uniquement un sous-ensemble de groupes de fichiers en lecture/écriture, les groupes de fichiers non restaurés deviennent obsolètes lorsque vous récupérez la base de données restaurée partiellement. L'omission d'un groupe de fichiers en lecture/écriture dans une séquence de restauration partielle convient uniquement dans les cas suivants :  
  
-   Vous prévoyez que les groupes de fichiers non restaurés soient obsolètes.  
  
-   La séquence de restauration parviendra au point de récupération auquel chaque groupe de fichiers non restauré est passé en lecture seule, supprimé ou obsolète (lors d'une restauration antérieure dans la séquence de restauration partielle).  
  
-   La sauvegarde complète a eu lieu lorsque la base de données utilisait le mode de récupération simple, mais le point de récupération utilisait le mode de récupération complète. Pour plus d'informations, consultez « Exécution d'une restauration fragmentaire d'une base de données dont le mode de récupération est passé du mode de récupération complète au mode de récupération simple », plus loin dans cette rubrique.  
  
### <a name="requirements-for-piecemeal-restore-under-the-simple-recovery-model"></a>Conditions requises pour la restauration fragmentaire en mode de récupération simple  
 En mode de récupération simple, l'étape initiale restaure et récupère le groupe de fichiers primaire et tous les groupes de fichiers secondaires en lecture/écriture. Au terme de l'étape initiale, les fichiers récupérés (s'ils sont valides et cohérents avec la base de données) peuvent être mis en ligne directement.  
  
 Ensuite, les groupes de fichiers en lecture seule peuvent être restaurés en une ou plusieurs étapes supplémentaires.  
  
 La restauration fragmentaire est disponible pour un groupe de fichiers secondaires en lecture seule uniquement si les conditions suivantes sont vérifiées :  
  
-   était en lecture seule lors de sa sauvegarde ;  
  
-   est resté en lecture seule (et logiquement cohérent avec le groupe de fichiers primaire).  
  
 Pour effectuer une restauration fragmentaire, observez les instructions suivantes :  
  
-   Un ensemble complet de sauvegardes pour la restauration fragmentaire d'une base de données utilisant le mode de récupération simple doit contenir les éléments suivants :  
  
    -   Une sauvegarde partielle ou complète de base de données contenant le groupe de fichiers primaire et tous les groupes de fichiers qui étaient en lecture/écriture au moment de la sauvegarde.  
  
    -   Une sauvegarde de chaque fichier en lecture seule.  
  
-   Pour que la sauvegarde d'un fichier en lecture seule soit cohérente avec le groupe de fichiers primaire, le groupe de fichiers secondaire doit se trouver en lecture seule depuis le moment où il a été sauvegardé jusqu'à l'achèvement de la sauvegarde contenant le groupe de fichiers primaire. Vous pouvez utiliser des sauvegardes différentielles de fichiers, sous réserve qu'elles aient été réalisées une fois le groupe de fichiers devenu accessible en lecture seule.  
  
### <a name="piecemeal-restore-stages-simple-recovery-model"></a>Procédure de restauration fragmentaire (mode de récupération simple)  
 Le scénario de restauration fragmentaire comprend les étapes suivantes :  
  
-   Étape initiale (restaurer et récupérer le groupe de fichiers primaire et tous les groupes de fichiers en lecture/écriture)  
  
     L'étape initiale effectue une restauration partielle. La séquence de restauration partielle restaure le groupe de fichiers primaire, tous les groupes de fichiers secondaires en lecture/écriture et, éventuellement, certains groupes de fichiers en lecture seule. Pendant l'étape initiale, la totalité de la base de données doit se trouver hors connexion. Après l'étape initiale, la base de données est en ligne et les groupes de fichiers restaurés sont disponibles. Toutefois, tous les groupes de fichiers en lecture seule qui n'ont pas encore été restaurés demeurent hors connexion.  
  
     La première instruction RESTORE de l'étape initiale doit :  
  
    -   utiliser une sauvegarde partielle ou complète de base de données contenant le groupe de fichiers primaire et tous les groupes de fichiers qui étaient en lecture/écriture au moment de la sauvegarde ; il est courant de commencer une séquence de restauration partielle en restaurant une sauvegarde partielle ;  
  
    -   spécifier l'option PARTIAL qui indique le démarrage d'une restauration fragmentaire ;  
  
    > [!NOTE]  
    >  L'option PARTIAL effectue des contrôles de sécurité qui garantissent l'adéquation de la base de données obtenue à son usage en production.  
  
    -   spécifier l'option READ_WRITE_FILEGROUPS si la sauvegarde est une sauvegarde complète de base de données.  
  
-   Si la base de données est en ligne, vous pouvez faire appel à une ou plusieurs restaurations de fichiers en ligne pour restaurer et récupérer les fichiers en lecture seule hors connexion qui étaient dans cet état au moment de la sauvegarde. La fréquence des restaurations de fichiers en ligne dépend du moment où vous souhaitez que les données soient en ligne.  
  
     La nécessité de restaurer des données dans un fichier dépend des éléments suivants :  
  
    -   Les fichiers en lecture seule valides et cohérents avec la base de données peuvent être mis en ligne directement s'ils sont récupérés sans restaurer les données.  
  
    -   Les fichiers endommagés ou incohérents avec la base de données doivent être restaurés avant d'être récupérés.  
  
### <a name="examples"></a>Exemples  
  
-   [Exemple : restauration fragmentaire d’une base de données &#40;mode de récupération simple&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-database-simple-recovery-model.md)  
  
-   [Exemple : restauration fragmentaire de quelques groupes de fichiers uniquement &#40;mode de récupération simple&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-only-some-filegroups-simple-recovery-model.md)  
  
## <a name="piecemeal-restore-under-the-full-recovery-model"></a>Restauration fragmentaire en mode de récupération complète  
 En mode de récupération complète ou de récupération utilisant les journaux de transactions, une restauration fragmentaire est disponible pour les bases de données qui contiennent plusieurs groupes de fichiers et vous pouvez restaurer une base de données à n'importe quel point dans le temps. Les séquences de restauration d'une restauration fragmentaire se déroulent comme suit :  
  
-   séquence de restauration partielle  
  
     La séquence de restauration partielle restaure le groupe de fichiers primaire et éventuellement certains des groupes de fichiers secondaires.  
  
     La première instruction RESTORE DATABASE doit :  
  
    -   spécifier l'option PARTIAL ; celle-ci indique le début d'une restauration fragmentaire.  
  
    -   utiliser n'importe quelle sauvegarde de base de données complète contenant le groupe de fichiers primaire. Il est conseillé de commencer une séquence de restauration partielle en restaurant une sauvegarde partielle.  
  
    -   Pour restaurer à un point précis dans le temps, vous devez spécifier l'heure dans la séquence de restauration partielle. Chaque étape successive de la séquence de restauration doit spécifier le même point dans le temps.  
  
-   Les séquences de restauration de groupe de fichiers mettent en ligne les groupes de fichiers supplémentaires jusqu'à un point cohérent avec la base de données.  
  
     Dans l'édition Enterprise, tout groupe de fichiers secondaire hors connexion peut être restauré et récupéré tant que la base de données reste en ligne. Un fichier en lecture seule spécifique ne doit être restauré que s'il est endommagé ou s'il est incohérent avec la base de données. Pour plus d’informations, consultez [Récupérer une base de données sans restauration des données &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/recover-a-database-without-restoring-data-transact-sql.md).  
  
### <a name="applying-log-backups"></a>Application des sauvegardes de journaux  
 Si un groupe de fichiers est en lecture seule avant la création de la sauvegarde de fichiers, l'application des sauvegardes de journaux au groupe de fichiers n'est pas nécessaire et n'est pas effectuée par la restauration de fichiers. Si le groupe de fichiers est en lecture/écriture, une séquence non rompue de sauvegardes de journaux doit être appliquée à la dernière restauration complète ou différentielle pour restaurer par progression le groupe de fichiers jusqu'au fichier journal actuel.  
  
### <a name="examples"></a>Exemples  
  
-   [Exemple : restauration fragmentaire d’une base de données &#40;mode de restauration complète&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-database-full-recovery-model.md)  
  
-   [Exemple : restauration fragmentaire de quelques groupes de fichiers &#40;mode de restauration complète&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-only-some-filegroups-full-recovery-model.md)  
  
## <a name="performing-a-piecemeal-restore-of-a-database-whose-recovery-model-has-been-switched-from-simple-to-full"></a>Exécution d'une restauration fragmentaire d'une base de données dont le mode de récupération est passé du mode de récupération complète au mode de récupération simple  
 Vous pouvez effectuer une restauration fragmentaire d'une base de données qui est passée du mode de récupération simple au mode de récupération complète depuis la sauvegarde de base de données complète ou la sauvegarde partielle. Exemple : considérons une base de données sur laquelle vous effectuez les opérations ci-après :  
  
1.  Créez une sauvegarde partielle (sauvegarde_1) d'une base de données dans le mode simple.  
  
2.  Après quelque temps, changez le mode de récupération en mode complet.  
  
3.  Créez une sauvegarde différentielle.  
  
4.  Commencez par effectuer des sauvegardes des journaux.  
  
 Par la suite, la séquence suivante est correcte :  
  
1.  Restauration partielle qui omet certains groupes de fichiers secondaires.  
  
2.  Restauration différentielle suivie d'autres restaurations nécessaires.  
  
3.  Ultérieurement, restauration de fichiers d'un groupe de fichiers secondaire en lecture/écriture avec l'option WITH NORECOVERY à partir de la sauvegarde partielle backup_1.  
  
4.  Sauvegarde différentielle suivie d'autres sauvegardes restaurées dans la séquence de restauration fragmentaire d'origine pour restaurer les données jusqu'au point de récupération d'origine.  
  
## <a name="see-also"></a> Voir aussi  
 [Appliquer les sauvegardes du journal de transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Restaurer une base de données SQL Server jusqu’à une limite dans le temps &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md)   
 [Vue d’ensemble de la restauration et de la récupération &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)   
 [Planifier et exécuter des séquences de restauration &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/plan-and-perform-restore-sequences-full-recovery-model.md)  
  
  
