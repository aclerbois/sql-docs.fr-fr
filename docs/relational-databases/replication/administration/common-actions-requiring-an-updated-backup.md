---
title: Actions courantes nécessitant une sauvegarde mise à jour | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- recovery [SQL Server replication], actions requiring a backup
- restoring [SQL Server replication], actions requiring a backup
- backups [SQL Server replication], actions requiring a backup
ms.assetid: a5975bf4-183e-42e3-b7d1-ad02f89d2e1d
caps.latest.revision: 32
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 70ce579ad6548d0fee933291fdb6c8f9b4e03f42
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="common-actions-requiring-an-updated-backup"></a>Actions courantes nécessitant une sauvegarde mise à jour
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Si vous effectuez des sauvegardes régulières des journaux, toutes les modifications liées à la réplication doivent être capturées dans les sauvegardes des journaux. Si vous ne sauvegardez pas les journaux, effectuez une sauvegarde des bases de données de publication, de distribution, d'abonnement ainsi que des bases de données **msdb**et **master** après avoir apporté des modifications à votre schéma ou topologie de réplication.  
  
## <a name="publication-database"></a>Base de données de publication  
 Sauvegardez la base de données de publication après avoir :  
  
-   créé de nouvelles de publications ;  
  
-   modifié toute propriété de publication, notamment le filtrage ;  
  
-   ajouté des articles à une publication existante ;  
  
-   réalisé une réinitialisation des abonnements au niveau de la publication ;  
  
-   effectué une modification de schéma sur une table publiée ;  
  
-   Exécution de script à la demande avec [sp_addscriptexec &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addscriptexec-transact-sql.md).  
  
-   modifié toute propriété d'article ;  
  
-   supprimé une publication ;  
  
-   supprimé un article ;  
  
-   désactivé la réplication.  
  
## <a name="distribution-database"></a>Base de données de distribution  
 Sauvegardez la base de données de distribution après avoir :  
  
-   créé ou modifié des profils d'Agent de réplication ;  
  
-   modifié les paramètres de profils d'Agent de réplication ;  
  
-   modifié les propriétés d'Agent de réplication (notamment les planifications) d'abonnements envoyés.  
  
-   Une nouvelle plage d'identité est assignée par la fonctionnalité de gestion des plages d'identité.  
  
## <a name="subscription-database"></a>Base de données d'abonnement  
 Sauvegardez la base de données d'abonnement après avoir :  
  
-   modifié toute propriété d'abonnement ;  
  
-   modifié la priorité d'un abonnement de fusion sur le serveur de publication ;  
  
-   supprimé un abonnement ;  
  
-   désactivé la réplication.  
  
## <a name="msdb-database"></a>Base de données msdb  
 Sauvegardez la base de données système **msdb** sur le nœud approprié après avoir :  
  
-   activé ou désactivé la réplication ;  
  
-   ajouté ou supprimé une base de données de distribution (sur le serveur de distribution) ;  
  
-   activé ou désactivé une base de données destinée à être publiée (sur le serveur de publication) ;  
  
-   créé ou modifié des profils d'Agent de réplication (sur le serveur de distribution) ;  
  
-   modifié des paramètres de profils d'Agent de réplication (sur le serveur de distribution) ;  
  
-   modifié les propriétés d'Agent de réplication (notamment les planifications) d'un abonnement envoyé (sur le serveur de distribution) ;  
  
-   modifié les propriétés d'Agent de réplication (notamment les planifications) d'un abonnement extrait (sur l'Abonné) ;  
  
-   créé un package DTS associé à une publication transactionnelle qui utilise des abonnements transformables (sur le serveur de distribution et sur le serveur de publication) ;  
  
-   ajouté ou supprimé un abonnement transformable (sur le serveur de distribution et sur le serveur de publication) ;  
  
## <a name="master-database"></a>Base de données master  
 Sauvegardez la base de données système **master** sur le nœud approprié après avoir :  
  
-   activé ou désactivé la réplication ;  
  
-   ajouté ou supprimé une base de données de distribution (sur le serveur de distribution) ;  
  
-   activé ou désactivé une base de données destinée à être publiée (sur le serveur de publication) ;  
  
-   ajouté la première publication, ou supprimé la dernière, dans une base de données (sur le serveur de publication) ;  
  
-   ajouté le premier abonnement, ou supprimé le dernier, dans une base de données (sur l'Abonné) ;  
  
-   activé ou désactivé un serveur de publication sur un serveur de publication de distribution (sur le serveur de publication et le serveur de distribution).  
  
## <a name="see-also"></a> Voir aussi  
 [Sauvegarde et restauration des bases de données SQL Server](../../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
 [Sauvegarder et restaurer des bases de données répliquées](../../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md)  
  
  
