---
title: Récupération de bases de données associées contenant une transaction marquée | Microsoft Docs
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
- transaction logs [SQL Server], marks
- STOPBEFOREMARK option [RESTORE statement]
- STOPATMARK option [RESTORE statement]
- point in time recovery [SQL Server]
- restoring databases [SQL Server], point in time
- recovery [SQL Server], databases
- restoring [SQL Server], point in time
- transactions [SQL Server], recovering to a mark
- database recovery [SQL Server]
- marked transactions [SQL Server], restoring
- database restores [SQL Server], point in time
ms.assetid: 77a0d9c0-978a-4891-8b0d-a4256c81c3f8
caps.latest.revision: 37
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: cea5152d55c393bcae73232cc08a958ba955a00a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="recovery-of-related--databases-that-contain-marked-transaction"></a>Récupération de bases de données associées contenant une transaction marquée
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique s'applique uniquement aux bases de données qui contiennent des transactions marquées et qui utilisent le mode de récupération complète ou le mode de récupération utilisant les journaux de transactions.  
  
 Pour plus d’informations sur la configuration requise pour la restauration à un point de récupération spécifique, consultez [Restaurer une base de données SQL Server jusqu’à une limite dans le temps &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md).  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge l'insertion de marques nommées dans le journal des transactions afin de permettre la récupération jusqu'à une marque particulière. Les marques de journal sont transactionnelles et ne sont insérées que si leurs transactions associées sont validées. Par conséquent, des marques peuvent être liées à un travail spécifique, et vous pouvez récupérer jusqu'à un point incluant ou excluant ce travail.  
  
 Avant d'insérer des marques nommées dans le journal des transactions, prenez en compte les éléments suivants :  
  
-   les marques de transaction occupant de l'espace dans le journal, utilisez-les seulement pour les transactions ayant un rôle significatif dans la stratégie de récupération de la base de données ;  
  
-   Après la validation d'une transaction marquée, une ligne est insérée dans la table [logmarkhistory](../../relational-databases/system-tables/logmarkhistory-transact-sql.md) de la base de données **msdb**.  
  
-   si une transaction marquée s'étend à plusieurs bases de données sur le même serveur de bases de données ou sur différents serveurs, les marques doivent être enregistrées dans les journaux de toutes les bases de données concernées. Pour plus d’informations, consultez [Utiliser les transactions marquées pour récupérer des bases de données associées uniformément &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/use-marked-transactions-to-recover-related-databases-consistently.md).  
  
> [!NOTE]  
>  Pour plus d’informations sur la façon de marquer les transactions, consultez [Utiliser les transactions marquées pour récupérer des bases de données associées uniformément &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/use-marked-transactions-to-recover-related-databases-consistently.md).  
  
## <a name="transact-sql-syntax-for-inserting-named-marks-into-a-transaction-log"></a>Syntaxe Transact-SQL permettant d'insérer des marques nommées dans un journal des transactions  
 Pour insérer des marques dans les journaux des transactions, utilisez l’instruction [BEGIN TRANSACTION](../../t-sql/language-elements/begin-transaction-transact-sql.md) et la clause WITH MARK [*description*]. La marque a le même nom que la transaction. La *description* facultative est une description textuelle de la marque, pas le nom de la marque. Par exemple, le nom de la transaction et de la marque créé dans l'instruction `BEGIN TRANSACTION` suivante est `Tx1`:  
  
```wmimof  
BEGIN TRANSACTION Tx1 WITH MARK 'not the mark name, just a description'    
```  
  
 Le journal des transactions enregistre le nom de la marque (nom de la transaction), la description, la base de données, l’utilisateur, les informations de **datetime** et le numéro séquentiel dans le journal (LSN, Log Sequence Number). Les informations de **datetime** sont utilisées conjointement avec le nom de la marque pour identifier celle-ci de façon univoque.  
  
 Pour plus d’informations sur la façon d’insérer une marque dans une transaction qui s’étend sur plusieurs bases de données, consultez [Utiliser les transactions marquées pour récupérer des bases de données associées uniformément &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/use-marked-transactions-to-recover-related-databases-consistently.md).  
  
## <a name="transact-sql-syntax-for-recovering-to-a-mark"></a>Syntaxe Transact-SQL permettant de réaliser une récupération jusqu'à une marque  
 Quand vous ciblez une transaction marquée à l’aide d’une instruction[RESTORE LOG](../../t-sql/statements/restore-statements-transact-sql.md), vous pouvez utiliser l’une des clauses suivantes pour vous arrêter à la marque ou juste avant celle-ci :  
  
-   Utilisez la clause WITH STOPATMARK = **'***<nom_marque>***'** pour spécifier que la transaction marquée est le point de récupération.  
  
     L'option STOPATMARK effectue une restauration par progression jusqu'à la marque et inclut la transaction marquée dans cette restauration.  
  
-   Utilisez la clause WITH STOPBEFOREMARK = **'***<nom_marque>***'** pour spécifier que l’enregistrement de journal situé juste avant la marque est le point de récupération.  
  
     L'option STOPBEFOREMARK effectue une restauration par progression jusqu'à la marque et exclut la transaction marquée de cette restauration.  
  
 Les options STOPATMARK et STOPBEFOREMARK prennent toutes les deux en charge une clause AFTER *datetime* facultative. Lorsqu'ils sont utilisés avec *datetime* , il n'est pas nécessaire que les noms de marque soient uniques.  
  
 Si la clause AFTER *datetime* est omise, la restauration par progression s'arrête à la première marque portant le nom spécifié. Si une valeur est spécifiée pour AFTER *datetime* , la récupération par progression s'arrête à la première marque portant le nom spécifié, à *datetime*exactement ou après.  
  
> [!NOTE]  
>  Comme dans le cas de toute opération de restauration dans le temps, la récupération jusqu'à une marque n'est pas permise pendant les périodes où la base de données subit des opérations journalisées en bloc.  
  
 **Pour effectuer une restauration jusqu'à une transaction marquée**  
  
 [Restaurer une base de données jusqu’à une transaction marquée &#40;SQL Server Management Studio&#41;](../../relational-databases/backup-restore/restore-a-database-to-a-marked-transaction-sql-server-management-studio.md)  
  
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
### <a name="preparing-the-log-backups"></a>Préparation des sauvegardes de journaux  
 Dans cet exemple, il serait parfaitement envisageable d'appliquer la stratégie de sauvegarde appropriée suivante aux bases de données :  
  
1.  Utilisation du mode de récupération complète pour les deux bases de données.  
  
2.  Création d'une sauvegarde complète de chaque base de données.  
  
     Les bases de données peuvent être sauvegardées l'une après l'autre ou de façon simultanée.  
  
3.  Préalablement à la sauvegarde du journal des transactions, le marquage d'une transaction qui s'exécute dans toutes les bases de données. Pour plus d’informations sur la façon de créer les transactions marquées, consultez [Utiliser les transactions marquées pour récupérer des bases de données associées uniformément &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/use-marked-transactions-to-recover-related-databases-consistently.md).  
  
4.  Sauvegarde du journal des transactions sur chaque base de données.  
  
### <a name="recovering-the-database-to-a-marked-transaction"></a>Récupération de la base de données jusqu'à une transaction marquée  
 **Pour restaurer la sauvegarde**  
  
1.  Créez des [sauvegardes de la fin du journal](../../relational-databases/backup-restore/tail-log-backups-sql-server.md) pour les bases de données non endommagées, si possible.  
  
2.  Restaurez la sauvegarde complète de base de données la plus récente pour chaque base de données.  
  
3.  Identifiez la transaction marquée la plus récente disponible dans toutes les sauvegardes des journaux de transactions. Ces informations sont stockées dans la table **logmarkhistory** de la base de données **msdb** de chaque serveur.  
  
4.  Identifiez les sauvegardes de journaux de toutes les bases de données associées contenant cette marque.  
  
5.  Restaurez chaque sauvegarde de journal en vous arrêtant à la transaction marquée.  
  
6.  Récupérez chaque base de données.  
  
## <a name="see-also"></a> Voir aussi  
 [BEGIN TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-transaction-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Appliquer les sauvegardes du journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)   
 [Utiliser les transactions marquées pour récupérer des bases de données associées uniformément &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/use-marked-transactions-to-recover-related-databases-consistently.md)   
 [Vue d’ensemble de la restauration et de la récupération &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)   
 [Restaurer une base de données SQL Server jusqu’à une limite dans le temps &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md)   
 [Planifier et exécuter des séquences de restauration &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/plan-and-perform-restore-sequences-full-recovery-model.md)  
  
  
