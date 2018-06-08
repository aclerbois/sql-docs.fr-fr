---
title: Stratégies de sauvegarde et de restauration de la réplication de fusion | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
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
- recovery [SQL Server replication], merge replication
- backups [SQL Server replication], merge replication
- restoring [SQL Server replication], merge replication
- merge replication [SQL Server replication], backup and restore
ms.assetid: b8ae31c6-d76f-4dd7-8f46-17d023ca3eca
caps.latest.revision: 48
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 0c3e727b15d9e963996cfa4d9616ecf723bcff5e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="strategies-for-backing-up-and-restoring-merge-replication"></a>Stratégies de sauvegarde et de restauration de la réplication de fusion
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Pour la réplication de fusion, effectuez régulièrement des sauvegardes des bases de données suivantes :  
  
-   La base de données de publication au niveau du serveur de publication  
  
-   La base de données de distribution au niveau du serveur de distribution  
  
-   La base de données d'abonnement sur chaque Abonné  
  
-   Bases de données système **master** et **msdb** sur le serveur de publication, sur le serveur de distribution et sur tous les Abonnés. Il est recommandé de sauvegarder ces bases de données en même temps, ainsi que la base de données de réplication appropriée. Par exemple, sauvegardez les bases de données **master** et **msdb** au niveau du serveur de publication en même temps que la base de données de publication. Si la base de données de publication est restaurée, vérifiez que les bases de données **master** et **msdb** sont cohérentes avec la base de données de publication en termes de configuration de la réplication et de paramètres.  
  
 Si vous effectuez des sauvegardes régulières des journaux, toutes les modifications liées à la réplication doivent être capturées dans les sauvegardes des journaux. Si vous n'effectuez pas de sauvegardes des journaux, une sauvegarde doit être effectuée si un paramètre concernant la réplication est modifié. Pour en savoir plus, voir [Actions courantes nécessitant une sauvegarde mise à jour](../../../relational-databases/replication/administration/common-actions-requiring-an-updated-backup.md).  
  
 Choisissez une des approches détaillées ci-dessous pour la sauvegarde et la restauration de la base de données de publication, puis suivez les recommandations indiquées pour la base de données de distribution et les bases de données d'abonnement.  
  
## <a name="backing-up-and-restoring-the-publication-database"></a>Sauvegarde et restauration de la base de données de publication  
 Il y a deux approches pour la restauration d'une base de données de publication de fusion. Après la restauration de la base de données de publication à partir d'une sauvegarde, il est recommandé d'effectuer l'une des deux actions suivantes :  
  
-   Synchroniser la base de données de publication avec une base de données d'abonnement.  
  
-   Réinitialiser tous les abonnements associés aux publications figurant dans la base de données de publication.  
  
 L'utilisation de l'une de ces deux méthodes garantit que, après une restauration, le serveur de publication et tous les Abonnés sont synchronisés.  
  
> [!NOTE]  
>  Si des tables contiennent des colonnes d'identité, vous devez vérifier que les plages d'identité correctes sont attribuées après une restauration. Pour plus d’informations, consultez [ Répliquer des colonnes d’identité](../../../relational-databases/replication/publish/replicate-identity-columns.md).  
  
### <a name="synchronizing-the-publication-database"></a>Synchronisation de la base de données de publication  
 La synchronisation d'une base de données de publication avec une base de données d'abonnement vous permet de charger à partir d'une ou plusieurs bases de données d'abonnement les modifications précédemment effectuées dans la base de données de publication, mais qui ne sont pas représentées dans la sauvegarde restaurée. Les données qui peuvent être chargées dépendent de la façon dont une publication est filtrée :  
  
-   Si la publication n'est pas filtrée, vous devriez pouvoir mettre à jour la base de données de publication en la synchronisant avec l'Abonné le plus à jour.  
  
-   Si la publication est filtrée, il est possible que vous ne puissiez pas mettre à jour la base de données de publication. Supposons une table qui est partitionnée de façon telle que chaque abonnement reçoit des données client seulement pour une région : Nord, Est, Sud et Ouest. S'il y a au moins un Abonné pour chaque partition de données, la synchronisation avec un Abonné pour chaque partition doit permettre la mise à jour de la base de données de publication. Cependant, si par exemple des données de la partition Ouest n'ont été répliquées vers aucun des Abonnés, ces données ne peuvent pas être mises à jour au niveau du serveur de publication.  
  
> [!IMPORTANT]  
>  La synchronisation d'une base de données de publication avec une base de données d'abonnement peut aboutir à des tables publiées restaurées à un point dans le temps plus récent que celui d'autres tables non publiées restaurées à partir de la sauvegarde.  
  
 Si vous effectuez une synchronisation avec un Abonné qui exécute une version de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] antérieure à [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], l'abonnement ne peut pas être anonyme ; il doit être un abonnement client ou un abonnement serveur (qui s'appelaient « abonnement local » et « abonnement global » dans les versions antérieures).  
  
 Pour synchroniser un abonnement, consultez [Synchronize a Push Subscription](../../../relational-databases/replication/synchronize-a-push-subscription.md) et [Synchronize a Pull Subscription](../../../relational-databases/replication/synchronize-a-pull-subscription.md).  
  
### <a name="reinitializing-all-subscriptions"></a>Réinitialisation de tous les abonnements  
 La réinitialisation de tous les abonnements garantit que tous les Abonnés sont dans un état cohérent avec la base de données de publication restaurée. Cette approche doit être utilisée si vous voulez replacer une topologie entière à l'état précédent représenté par une sauvegarde donnée de la base de données d'abonnement. Par exemple, vous pouvez souhaiter réinitialiser tous les abonnements si vous effectuez la restauration d'une base de données de publication à un point antérieur dans le temps, dans le but de récupérer d'une opération de traitement effectuée incorrectement.  
  
 Si vous choisissez cette option, générez un nouvel instantané en vue de la communiquer aux Abonnés réinitialisés dès la fin de la restauration de la base de données de publication.  
  
 Pour réinitialiser un abonnement, consultez [Reinitialize a Subscription](../../../relational-databases/replication/reinitialize-a-subscription.md).  
  
 Pour créer et appliquer un instantané, consultez [Create et Apply the Initial Snapshot](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md) et [Créer un instantané d'une publication de fusion avec des filtres paramétrés](../../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md).  
  
## <a name="backing-up-and-restoring-the-distribution-database"></a>Sauvegarde et restauration de la base de données de distribution  
 Avec la réplication de fusion, la base de données de distribution doit être sauvegardée régulièrement, et peut être restaurée sans considérations particulières aussi longtemps que la sauvegarde n'est pas plus ancienne que la période de rétention la plus courte de tous les abonnements qui utilisent le serveur de distribution. Par exemple, s'il y a trois publications avec des périodes de rétention de 10, 20 et 30 jours respectivement, la sauvegarde utilisée pour restaurer la base de données ne doit pas être vieille de plus de 10 jours. La base de données de distribution a un rôle limité dans la réplication de fusion : elle ne stocke aucune donnée utilisée dans le suivi des modifications et ne constitue pas un support de stockage temporaire pour les modifications de réplication de fusion destinées aux bases de données d'abonnement (comme elle le fait dans la réplication transactionnelle).  
  
## <a name="backing-up-and-restoring-a-subscription-database"></a>Sauvegarde et restauration d'une base de données d'abonnement  
 Pour garantir la récupération réussie d'une base de données d'abonnement, les Abonnés doivent se synchroniser avec le serveur de publication avant que la base de données d'abonnement soit sauvegardée ; ils doivent aussi se synchroniser après la restauration de la base de données d'abonnement :  
  
-   La synchronisation avec le serveur de publication avant une sauvegarde de la base de données d'abonnement aide à garantir que si un Abonné est restauré à partir d'une sauvegarde, l'abonnement se trouve toujours dans la période de rétention d'abonnement. Supposons par exemple qu'une publication ait une période de conservation de 10 jours. La dernière synchronisation date de 8 jours et la sauvegarde est effectuée maintenant. Si elle est restaurée 4 jours plus tard, la dernière synchronisation datera de 12 jours, ce qui est supérieur à la période de rétention. Dans ce cas, vous devez réinitialiser l'Abonné. Si l'Abonné avait effectué la synchronisation juste avant la sauvegarde, la base de données d'abonnement se trouverait dans la période de rétention.  
  
     La sauvegarde ne doit pas être plus ancienne que la période de rétention la plus courte pour toutes les publications auxquelles l'Abonné souscrit. Par exemple, si un abonné adhère à trois publications avec des périodes de conservation de 10, 20 et 30 jours respectivement, la sauvegarde utilisée pour restaurer la base de données ne doit pas être âgée de plus de 10 jours.  
  
-   La synchronisation de la base de données d'abonnement avec chacun de ses abonnements après une restauration garantit que l'Abonné est à jour avec toutes les modifications au niveau du serveur de publication.  
  
 Pour définir la période de rétention de publication, consultez [Définir la période d’expiration des abonnements](../../../relational-databases/replication/publish/set-the-expiration-period-for-subscriptions.md).  
  
 Pour synchroniser un abonnement, consultez [Synchroniser un abonnement par émission (push)](../../../relational-databases/replication/synchronize-a-push-subscription.md) et [Synchroniser un abonnement par extraction (pull)](../../../relational-databases/replication/synchronize-a-pull-subscription.md).  
  
## <a name="backing-up-and-restoring-a-republishing-database"></a>Sauvegarde et restauration d'une base de données de réédition  
 Une base de données de réédition désigne une base de données qui s'abonne à des données auprès d'un éditeur et qui, à son tour, diffuse ces données à d'autres bases de données d'abonnement. Lorsque vous restaurez une base de données de réédition, suivez les recommandations données dans les sections « Sauvegarde et restauration d'une base de données de publication » et « Sauvegarde et restauration d'une base de données d'abonnement » de cette rubrique.  
  
## <a name="see-also"></a> Voir aussi  
 [Sauvegarde et restauration des bases de données SQL Server](../../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
 [Sauvegarder et restaurer des bases de données répliquées](../../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md)  
  
  
