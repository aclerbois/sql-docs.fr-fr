---
title: Restaurer une sauvegarde de base de données en mode de récupération simple (Transact-SQL) | Microsoft Docs
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
- full backups [SQL Server]
- database restores [SQL Server], full backups
- backing up databases [SQL Server], full backups
- database backups [SQL Server], full backups
- restoring databases [SQL Server], full backups
ms.assetid: a928fa36-e285-476f-9a7b-6840a8bb7283
caps.latest.revision: 39
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 65594c3c0811245fa562493ba0faad75686f171a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="restore-a-database-backup-under-the-simple-recovery-model-transact-sql"></a>Restaurer une sauvegarde de base de données en mode de récupération simple (Transact-SQL)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Cette rubrique explique comment restaurer une sauvegarde complète de base de données.  
  
> [!IMPORTANT]  
>  L'administrateur système qui restaure la sauvegarde complète de base de données doit être la seule personne à utiliser la base de données à restaurer.  
  
## <a name="prerequisites-and-recommendations"></a>Conditions préalables et recommandations  
  
-   Pour restaurer une base de données chiffrée, vous devez avoir accès au certificat ou à la clé asymétrique qui a servi à chiffrer la base de données. Sans le certificat et la clé asymétrique, la base de données ne peut pas être restaurée. En conséquence, le certificat utilisé pour chiffrer la clé de chiffrement de base de données doit être conservé tant que la sauvegarde est utile. Pour plus d'informations, consultez [SQL Server Certificates and Asymmetric Keys](../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).  
  
-   Pour des raisons de sécurité, nous vous recommandons de ne pas attacher ni restaurer des bases de données provenant de sources inconnues ou non approuvées. Ces bases de données peuvent contenir du code malveillant susceptible d'exécuter du code [!INCLUDE[tsql](../../includes/tsql-md.md)] indésirable ou de provoquer des erreurs en modifiant le schéma ou la structure physique des bases de données. Avant d’utiliser une base de données issue d’une source inconnue ou non approuvée, exécutez [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) sur la base de données sur un serveur autre qu’un serveur de production et examinez également le code, notamment les procédures stockées ou tout autre code défini par l’utilisateur, de la base de données.  
  
## <a name="database-compatibility-level-after-upgrade"></a>Niveau de compatibilité des bases de données après une mise à niveau  
 Les niveaux de compatibilité des bases de données **tempdb**, **model**, **msdb** et **Resource** sont définis sur le niveau de compatibilité [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] après la mise à niveau. La base de données système **master** conserve le niveau de compatibilité qu’elle avait avant la mise à niveau, sauf si ce niveau était inférieur à 100. Si le niveau de compatibilité de la base de données **master** était inférieur à 100 avant la mise à niveau, il est défini sur 100 une fois celle-ci effectuée.  
  
 Si le niveau de compatibilité d'une base de données utilisateur est à 100 ou supérieur avant la mise à niveau, il reste le même après la mise à niveau. Si le niveau de compatibilité était à 90 avant la mise à niveau, dans la base de données mise à niveau, le niveau de compatibilité est défini à 100, ce qui correspond au niveau de compatibilité le plus bas pris en charge dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
> [!NOTE]  
>  Les nouvelles bases de données utilisateur héritent du niveau de compatibilité de la base de données **model** .  
  
## <a name="procedures"></a>Procédures  
  
#### <a name="to-restore-a-full-database-backup"></a>Pour restaurer une sauvegarde complète de base de données  
  
1.  Exécutez l'instruction RESTORE DATABASE pour restaurer la sauvegarde complète de la base de données, en spécifiant :  
  
    -   le nom de la base de données à restaurer ;  
  
    -   l'unité de sauvegarde à partir de laquelle sera restaurée la sauvegarde complète de la base de données ;  
  
    -   la clause NORECOVERY si vous devez appliquer la sauvegarde différentielle de base de données ou du journal des transactions après avoir restauré la sauvegarde complète de base de données.  
  
    > [!IMPORTANT]  
    >  Pour restaurer une base de données chiffrée, vous devez avoir accès au certificat ou à la clé asymétrique qui a servi à chiffrer la base de données. Sans le certificat et la clé asymétrique, la base de données ne peut pas être restaurée. En conséquence, le certificat utilisé pour chiffrer la clé de chiffrement de base de données doit être conservé tant que la sauvegarde est utile. Pour plus d'informations, consultez [SQL Server Certificates and Asymmetric Keys](../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).  
  
2.  Spécifiez éventuellement :  
  
    -   la clause FILE pour identifier le jeu de sauvegarde sur l'unité de sauvegarde à restaurer.  
  
> [!NOTE]  
>  Si vous restaurez une base de données de version antérieure à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], la base de données est automatiquement mise à niveau. En général, la base de données est immédiatement disponible. Toutefois, si une base de données [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] comprend des index de recherche en texte intégral, le processus de mise à niveau les importe, les réinitialise ou les reconstruit, selon la valeur de la propriété de serveur  **upgrade_option** . Si l’option de mise à niveau a la valeur Importer (**upgrade_option** = 2) ou Reconstruire (**upgrade_option** = 0), les index de recherche en texte intégral ne seront pas disponibles pendant la mise à niveau. Selon le volume de données indexé, l'importation peut prendre plusieurs heures et la reconstruction jusqu'à dix fois plus longtemps. Notez également que lorsque l'option de mise à niveau est Importer, les index de recherche en texte intégral associés sont reconstruits si aucun catalogue de texte intégral n'est disponible. Pour modifier le paramètre de la propriété de serveur **upgrade_option** , utilisez [sp_fulltext_service](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md).  
  
## <a name="example"></a> Exemple  
  
### <a name="description"></a>Description  
 Cet exemple restaure la sauvegarde complète de base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] à partir d'une bande.  
  
### <a name="example"></a> Exemple  
  
```  
USE master;  
GO  
RESTORE DATABASE AdventureWorks2012  
   FROM TAPE = '\\.\Tape0';  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Restaurations complètes de bases de données &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/complete-database-restores-full-recovery-model.md)   
 [Restaurations complètes de bases de données &#40;mode de récupération simple&#41;](../../relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md)   
 [Sauvegardes complètes de bases de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/full-database-backups-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Historique de sauvegarde et informations d’en-tête &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-history-and-header-information-sql-server.md)   
 [Reconstruire des bases de données système](../../relational-databases/databases/rebuild-system-databases.md)  
  
  
