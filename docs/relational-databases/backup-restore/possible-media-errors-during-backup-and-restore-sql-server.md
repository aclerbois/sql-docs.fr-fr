---
title: Erreurs de support possibles pendant les opérations de sauvegarde et de restauration (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.suite: sql
ms.technology: backup-restore
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- media errors [SQL Server]
- CONTINUE_AFTER_ERROR option
- errors [SQL Server], backups
- backups [SQL Server], errors
- RESTORE VERIFYONLY statement
- backup media [SQL Server], error management
- page checksums [SQL Server]
- backup checksums [SQL Server]
- backing up [SQL Server], media errors
- RESTORE statement, media errors
- NO_CHECKSUM option
- checksums [SQL Server]
ms.assetid: 83a27b29-1191-4f8d-9648-6e6be73a9b7c
caps.latest.revision: 37
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: db783f8235ec3216c46dbd886680c0c4cbf9238d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="possible-media-errors-during-backup-and-restore-sql-server"></a>Erreurs de support possibles pendant les opérations de sauvegarde et de restauration (SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] vous donne la possibilité de récupérer une base de données en dépit des erreurs détectées. Un nouveau et important mécanisme de détection d'erreur est la création facultative d'une somme de contrôle de sauvegarde qui peut être créée par une opération de sauvegarde et validée par une opération de restauration. Vous pouvez déterminer si une opération recherche la présence d'erreurs et si elle s'arrête ou si elle continue en présence d'une erreur. Si une sauvegarde contient une somme de contrôle de sauvegarde, les instructions RESTORE et RESTORE VERIFYONLY peuvent rechercher la présence d'erreurs.  
  
> [!NOTE]  
>  Les sauvegardes en miroir offrent jusqu'à quatre copies (miroirs) d'un support de sauvegarde, auxquelles vous pouvez recourir pour effectuer une récupération à partir d'erreurs causées par un support endommagé. Pour plus d'informations, consultez [Jeux de supports de sauvegarde en miroir &#40;SQL Server&#41;](../../relational-databases/backup-restore/mirrored-backup-media-sets-sql-server.md).  
  
  
##  <a name="BckChecksums"></a> Sommes de contrôle de sauvegarde  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge trois types de sommes de contrôle : une somme de contrôle sur les pages, une somme de contrôle dans les blocs de journal et une somme de contrôle de sauvegarde. Lorsqu'elle génère une somme de contrôle de sauvegarde, l'instruction BACKUP vérifie que les données lues depuis la base de données sont cohérentes avec toute indication de somme de contrôle ou de page endommagée présente dans la base de données.  
  
 L'instruction BACKUP peut aussi calculer une somme de contrôle de sauvegarde sur le flux de sauvegarde ; si des informations de page endommagée ou de somme de contrôle de page sont présentes dans une page donnée, lors de la sauvegarde de la page, l'instruction BACKUP vérifie également l'état de somme de contrôle et de page endommagée, ainsi que l'ID de la page. Lorsqu'elle crée une somme de contrôle de sauvegarde, une opération de sauvegarde n'ajoute aucune somme de contrôle aux pages. Les pages sont sauvegardées telles qu'elles existent dans la base de données et ne sont pas modifiées par la sauvegarde.  
  
 En raison de la charge inhérente à la vérification et à la génération des sommes de contrôle de sauvegarde, l'utilisation de ces sommes peut avoir des répercussions sur les performances. La charge de travail et le débit de sauvegarde peuvent être affectés. Par conséquent, l'utilisation de sommes de contrôle de sauvegarde est facultative. Lorsque vous décidez de générer des sommes de contrôle pendant une sauvegarde, surveillez attentivement la charge processeur induite, ainsi que l'impact sur les charges de travail simultanées du système.  
  
 L'instruction BACKUP ne modifie jamais la page source sur le disque, ni le contenu d'une page.  
  
 Lorsque les sommes de contrôle de sauvegarde sont activées, une opération de sauvegarde effectue les étapes suivantes :  
  
1.  Avant d'écrire une page sur le support de sauvegarde, l'opération de sauvegarde vérifie les informations de niveau page : somme de contrôle de page ou détection de page endommagée, si elles existent. En l'absence d'informations, la sauvegarde ne peut pas vérifier la page. Non vérifiées, les pages sont incluses en l'état, et leur contenu est ajouté à la somme de contrôle de sauvegarde globale.  
  
     Si l'opération de sauvegarde rencontre une erreur de page pendant la vérification, la sauvegarde échoue.  
  
    > [!NOTE]  
    >  Pour plus d'informations sur les sommes de contrôle de page et sur la détection de page endommagée, consultez l'option PAGE_VERIFY de l'instruction ALTER DATABASE. Pour plus d’informations, consultez [Options ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md).  
  
2.  Que la somme de contrôle de la page soit disponible ou non, BACKUP génère une somme de contrôle de sauvegarde séparée pour le flux de sauvegardes. Les opérations de restauration peuvent éventuellement utiliser cette somme de contrôle pour vérifier que la sauvegarde n'est pas endommagée. La somme de contrôle de sauvegarde est stockée sur le support de sauvegardes et non pas dans les pages de base de données. Elle peut en option être utilisée lors de la restauration.  
  
3.  Le jeu de sauvegarde est signalé comme contenant des sommes de contrôle de sauvegarde (dans la colonne **has_backup_checksums** de **msdb..backupset)**. Pour plus d’informations, consultez [backupset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupset-transact-sql.md).  
  
 Au cours d'une opération de restauration, si des sommes de contrôle de sauvegarde sont présentes sur le support de sauvegarde, par défaut, les instructions RESTORE et RESTORE VERIFYONLY vérifient les sommes de contrôle de sauvegarde et les sommes de contrôle de page. En l'absence de somme de contrôle de sauvegarde, les deux opérations de restauration se poursuivent sans vérification ; en effet, sans somme de contrôle de sauvegarde, la restauration ne peut pas vérifier les sommes de contrôle de page de façon fiable.  
  
## <a name="response-to-page-checksum-errors-during-a-backup-or-restore-operation"></a>Réponse aux erreurs de somme de contrôle de page pendant une opération de sauvegarde ou de restauration  
 Par défaut, après la rencontre d'une erreur de somme de contrôle de page, une opération de sauvegarde BACKUP ou de restauration RESTORE échoue et une opération RESTORE VERIFYONLY se poursuit. Toutefois, vous pouvez contrôler si une opération donnée échoue en présence d'une erreur ou continue du mieux possible.  
  
 Si une opération BACKUP continue après la survenue d'erreurs, l'opération effectue les étapes suivantes :  
  
1.  Marque le jeu de sauvegarde sur le support de sauvegarde comme contenant des erreurs et trace la page dans la table **suspect_pages** de la base de données **msdb**. Pour plus d’informations, consultez [suspect_pages &#40;Transact-SQL&#41;](../../relational-databases/system-tables/suspect-pages-transact-sql.md).  
  
2.  Consigne l'erreur dans le journal des erreurs SQL Server.  
  
3.  Marque le jeu de sauvegarde comme contenant ce type d’erreur (dans la colonne **is_damaged** de **msdb.backupset**). Pour plus d’informations, consultez [backupset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupset-transact-sql.md).  
  
4.  Émet un message indiquant que la sauvegarde a été correctement générée, mais qu'elle contient des erreurs de page.  
  
##  <a name="RelatedTasks"></a> Tâches associées  
 **Pour activer ou désactiver les sommes de contrôle de sauvegarde**  
  
-   [Activer ou désactiver des sommes de contrôle de sauvegarde au cours d’opérations de sauvegarde ou de restauration &#40;SQL Server&#41;](../../relational-databases/backup-restore/enable-or-disable-backup-checksums-during-backup-or-restore-sql-server.md)  
  
 **Pour contrôler la réponse à une erreur lors d'une opération de sauvegarde**  
  
-   [Spécifier si une opération de sauvegarde ou de restauration continue ou s’arrête après la survenue d’une erreur &#40;SQL Server&#41;](../../relational-databases/backup-restore/specify-if-backup-or-restore-continues-or-stops-after-error.md)  
  
## <a name="see-also"></a> Voir aussi  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [backupset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupset-transact-sql.md)   
 [Jeux de supports de sauvegarde en miroir &#40;SQL Server&#41;](../../relational-databases/backup-restore/mirrored-backup-media-sets-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [RESTORE VERIFYONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)  
  
  
