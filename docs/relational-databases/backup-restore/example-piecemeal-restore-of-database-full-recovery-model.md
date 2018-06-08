---
title: 'Exemple : restauration fragmentaire d’une base de données (Mode de restauration complète) | Microsoft Docs'
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
- full recovery model [SQL Server], RESTORE example
- piecemeal restores [SQL Server], full recovery model
- restore sequences [SQL Server], piecemeal
ms.assetid: 0a84892d-2f7a-4e77-b2d0-d68b95595210
caps.latest.revision: 30
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: d90d95868f28cdc55140e8d291eead48a743d040
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="example-piecemeal-restore-of-database-full-recovery-model"></a>Exemple : restauration fragmentaire d'une base de données (Mode de restauration complète)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Une séquence de restauration fragmentaire restaure et récupère une base de données par étape au niveau des groupes de fichiers, en commençant par le groupe de fichiers primaire, suivi des groupes fichiers secondaires en lecture-écriture.  
  
 Dans cet exemple, la base de données `adb` est restaurée sur un nouvel ordinateur après un problème grave. La base de données utilise le mode de restauration complète ; par conséquent, avant le début de la restauration, il convient d'effectuer une sauvegarde de la fin du journal sur la base de données. Avant le sinistre, tous les groupes de fichiers sont en ligne. Groupe de fichiers `B` en lecture seule. Tous les groupes de fichiers secondaires doivent être restaurés, mais dans l'ordre de priorité suivant : `A` (le plus élevé), `C`et enfin `B` Dans cet exemple, il y a quatre sauvegardes de journal, dont la sauvegarde de la fin du journal.  
  
## <a name="tail-log-backup"></a>Sauvegarde de la fin du journal  
 Avant de restaurer la base de données, l'administrateur de la base de données doit sauvegarder la fin du journal. La base de données étant endommagée, la création de la sauvegarde de la fin du journal requiert l'utilisation de l'option NO_TRUNCATE :  
  
```  
BACKUP LOG adb TO tailLogBackup WITH NORECOVERY, NO_TRUNCATE  
```  
  
 La sauvegarde de la fin du journal est la dernière sauvegarde appliquée dans les séquences de restauration qui suivent.  
  
## <a name="restore-sequences"></a>Séquences de restauration  
  
> [!NOTE]  
>  La syntaxe pour une séquence de restauration en ligne est la même que pour une séquence de restauration hors connexion.  
  
1.  Restauration partielle du groupe de fichiers primaire et secondaire `A`.  
  
    ```  
    RESTORE DATABASE adb FILEGROUP='Primary' FROM backup1   
       WITH PARTIAL, NORECOVERY  
    RESTORE DATABASE adb FILEGROUP='A' FROM backup2   
       WITH NORECOVERY  
    RESTORE LOG adb FROM backup3 WITH NORECOVERY  
    RESTORE LOG adb FROM backup4 WITH NORECOVERY  
    RESTORE LOG adb FROM backup5 WITH NORECOVERY  
    RESTORE LOG adb FROM tailLogBackup WITH RECOVERY  
    ```  
  
2.  Restauration en ligne du groupe de fichiers `C`.  
  
     À ce stade, le groupe de fichiers primaire et le groupe de fichiers secondaire `A` sont en ligne. Tous les fichiers dans les groupes de fichiers `B` et `C` sont en attente de récupération, et les groupes de fichiers sont hors connexion.  
  
     Les messages de la dernière instruction `RESTORE LOG` (étape 1) indiquent que la restauration des transactions impliquant le groupe de fichiers `C` a été différée parce que ce groupe de fichiers n'est pas disponible. Les opérations courantes peuvent continuer, mais des verrous sont détenus par ces transactions et la troncation du fichier journal ne peut pas avoir lieu tant que la restauration n'est pas terminée.  
  
     Dans la deuxième séquence de restauration, l'administrateur de base de données restaure le groupe de fichiers `C`:  
  
    ```  
    RESTORE DATABASE adb FILEGROUP='C' FROM backup2a WITH NORECOVERY  
    RESTORE LOG adb FROM backup3 WITH NORECOVERY  
    RESTORE LOG adb FROM backup4 WITH NORECOVERY  
    RESTORE LOG adb FROM backup5 WITH NORECOVERY  
    RESTORE LOG adb FROM tailLogBackup WITH RECOVERY  
    ```  
  
     À ce stade, le groupe de fichiers primaire et les groupes de fichiers `A` et `C` sont en ligne. Les fichiers du groupe de fichiers `B` restent en attente de récupération et le groupe de fichiers est déconnecté. Les transactions différées ont été résolues et la troncation du fichier journal a lieu.  
  
3.  Restauration en ligne du groupe de fichiers `B`.  
  
     Dans la troisième séquence de restauration, l'administrateur de base de données restaure le groupe de fichiers `B`. La sauvegarde du groupe de fichiers `B` a été effectuée après que le groupe de fichiers soit passé en lecture seule ; ces fichiers n'ont donc pas besoin d'être restaurés par progression au cours de la récupération.  
  
    ```  
    RESTORE DATABASE adb FILEGROUP='B' FROM backup2b WITH RECOVERY  
    ```  
  
     Tous les groupes de fichiers sont maintenant en ligne.  
  
## <a name="additional-examples"></a>Autres exemples  
  
-   [Exemple : restauration fragmentaire d’une base de données &#40;Mode de récupération simple&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-database-simple-recovery-model.md)  
  
-   [Exemple : restauration fragmentaire de quelques groupes de fichiers uniquement &#40;mode de récupération simple&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-only-some-filegroups-simple-recovery-model.md)  
  
-   [Exemple : restauration en ligne d’un fichier en lecture seule &#40;Mode de récupération simple&#41;](../../relational-databases/backup-restore/example-online-restore-of-a-read-only-file-simple-recovery-model.md)  
  
-   [Exemple : restauration fragmentaire de quelques groupes de fichiers &#40;mode de restauration complète&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-only-some-filegroups-full-recovery-model.md)  
  
-   [Exemple : restauration en ligne d’un fichier en lecture/écriture &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/example-online-restore-of-a-read-write-file-full-recovery-model.md)  
  
-   [Exemple : restauration en ligne d’un fichier en lecture seule &#40;mode de restauration complète&#41;](../../relational-databases/backup-restore/example-online-restore-of-a-read-only-file-full-recovery-model.md)  
  
## <a name="see-also"></a> Voir aussi  
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [Restauration en ligne &#40;SQL Server&#41;](../../relational-databases/backup-restore/online-restore-sql-server.md)   
 [Appliquer les sauvegardes du journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Restaurations fragmentaires &#40;SQL Server&#41;](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md)  
  
  
