---
title: Sauvegardes partielles (SQL Server) | Microsoft Docs
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
- partial backups [SQL Server]
- READ_WRITE_FILEGROUPS option
- database backups [SQL Server], about backing up databases
ms.assetid: fe6b6bb1-38d0-46c4-bab8-31df14e8999c
caps.latest.revision: 46
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 33251a84013e47546ab149e4744eee9918f744f1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="partial-backups-sql-server"></a>Sauvegardes partielles (SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Tous les modes de récupération de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prennent en charge les sauvegardes partielles ; par conséquent, cette rubrique concerne toutes les bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Toutefois, les sauvegardes partielles sont conçues pour le mode de récupération simple et permettent d'améliorer la souplesse des sauvegardes de bases de données très volumineuses qui contiennent un ou plusieurs groupes de fichiers en lecture seule.  
  
 Les sauvegardes partielles sont utiles à chaque fois que vous souhaitez exclure des groupes de fichiers en lecture seule. Une *sauvegarde partielle* s’apparente à une sauvegarde complète de base de données, mais la première ne contient pas tous les groupes de fichiers. En revanche, pour une base de données accessible en lecture/écriture, une sauvegarde partielle contient toutes les données du groupe de fichiers primaire, de chaque groupe de fichiers en lecture/écriture et, éventuellement, d'un ou de plusieurs fichiers en lecture seule. La sauvegarde partielle d'une base de données en lecture seule contient uniquement le groupe de fichiers primaire.  
  
> [!NOTE]  
>  Si la base de données en lecture seule est convertie ensuite en base de données en lecture-écriture après une sauvegarde partielle, des groupes de fichiers secondaires en lecture-écriture peuvent ne pas figurer dans la sauvegarde partielle. Dans ce cas, si vous tentez d'effectuer une sauvegarde partielle différentielle, la sauvegarde échoue. Pour pouvoir créer une sauvegarde partielle différentielle de la base de données, vous devez créer une autre sauvegarde partielle. La nouvelle sauvegarde partielle contient tous les groupes de fichiers secondaires en lecture-écriture qui peuvent servir de base pour les sauvegardes partielles différentielles.  
  
 Des sauvegardes de fichiers de groupes de fichiers en lecture seule peuvent être associées à des sauvegardes partielles. Pour plus d’informations sur les sauvegardes de fichiers, consultez [Sauvegardes de fichiers complètes &#40;SQL Server&#41;](../../relational-databases/backup-restore/full-file-backups-sql-server.md).  
  
 Une sauvegarde partielle peut servir de *base différentielle* pour des sauvegardes partielles différentielles. Pour plus d’informations, consultez [Sauvegardes différentielles &#40;SQL Server&#41;](../../relational-databases/backup-restore/differential-backups-sql-server.md).  
  
##  <a name="RelatedTasks"></a> Tâches associées  
  
> [!NOTE]  
>  Les sauvegardes partielles ne sont pas prises en charge par [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou par l'Assistant Plan de maintenance.  
  
 **Pour créer une sauvegarde partielle**  
  
-   [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md) (READ_WRITE_FILEGROUPS ; option FILEGROUP, si nécessaire)  
  
 **Pour utiliser une sauvegarde partielle dans une séquence de restauration**  
  
-   [Exemple : restauration fragmentaire d’une base de données &#40;mode de récupération simple&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-database-simple-recovery-model.md)  
  
-   [Exemple : restauration fragmentaire de quelques groupes de fichiers uniquement &#40;mode de récupération simple&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-only-some-filegroups-simple-recovery-model.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Vue d’ensemble de la sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md)   
 [Restaurations de fichiers &#40;mode de récupération simple&#41;](../../relational-databases/backup-restore/file-restores-simple-recovery-model.md)   
 [Restaurations fragmentaires &#40;SQL Server&#41;](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md)  
  
  
