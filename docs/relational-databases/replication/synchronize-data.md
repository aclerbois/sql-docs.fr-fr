---
title: Synchroniser les données | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- synchronization [SQL Server replication], about synchronization
- merge replication synchronization [SQL Server replication]
- scripts [SQL Server replication], synchronization and
- synchronization [SQL Server replication]
- snapshot replication [SQL Server], synchronization
- transactional replication, synchronization
- subscriptions [SQL Server replication], synchronizing
- on demand script execution
- replication [SQL Server], synchronization
- scripts [SQL Server replication]
ms.assetid: 724802f7-7d69-46d3-a330-bd8aa7f53114
caps.latest.revision: 36
author: MashaMSFT
ms.author: mathoma
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 0a6d8e40a3fe7a3dea24ff31957048222d65d20d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="synchronize-data"></a>Synchroniser les données
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  La synchronisation des données est le processus de propagation des modifications de données et de schéma entre le serveur de publication et les abonnés après l'application de l'instantané initial sur les abonnés. La synchronisation peut se produire :  
  
-   En continu, ce qui est le mode normal pour la réplication transactionnelle.  
  
-   À la demande, ce qui est le mode normal pour la réplication de fusion.  
  
-   Suivant une planification, ce qui est le mode normal pour la réplication d'instantané.  
  
 Lorsqu'un abonnement est synchronisé, différents processus se produisent en fonction de votre type de réplication :  
  
-   Réplication d'instantané. La synchronisation signifie que l'Agent de distribution applique de nouveau l'instantané sur l'Abonné pour que le schéma et les données de la base de données d'abonnement soient cohérents avec ceux de la base de données de publication.  
  
     Si des modifications de données ou de schéma ont été effectuées sur le serveur de publication, un nouvel  instantané doit être généré afin de propager les modifications sur l'Abonné.  
  
-   Réplication transactionnelle. La synchronisation signifie que l'Agent de distribution transfère les mises à jour, les insertions, les suppressions et toute autre modification de la base de données de distribution sur l'Abonné.  
  
-   Réplication de fusion. La synchronisation signifie que l'Agent de distribution charge les modifications de l'Abonné sur le serveur de publication, puis télécharge les modifications du serveur de publication sur l'Abonné. S'il existe des conflits, ils sont détectés et résolus. Dès que la convergence des données est assurée, le serveur de publication et tous les abonnés disposent des mêmes valeurs de données. Si des conflits ont été détectés et corrigés, le travail validé par certains utilisateurs est modifié pour résoudre le conflit en fonction de vos stratégies définies.  
  
 Chaque fois que la synchronisation se produit, les publications d'instantanés actualisent complètement le schéma de l'Abonné, les modifications de schéma sont donc toutes appliquées sur l'Abonné. La réplication transactionnelle et la réplication de fusion prennent également en charge les modifications de schéma les plus courantes. Pour plus d’informations, consultez [Modifier le schéma dans les bases de données de publication](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md).  
  
 Pour synchroniser un abonnement par émission de données (push), consultez [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md).  
  
 Pour synchroniser un abonnement par extraction de données (pull), consultez [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md).  
  
 Pour définir des plannings de synchronisation, consultez [Specify Synchronization Schedules](../../relational-databases/replication/specify-synchronization-schedules.md).  
  
 **Pour afficher et résoudre les conflits de synchronisation**  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] : [Afficher et résoudre les conflits de données pour les publications de fusion &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/view-and-resolve-data-conflicts-for-merge-publications.md)  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] : [Afficher les conflits de données pour les publications transactionnelles &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/view-data-conflicts-for-transactional-publications-sql-server-management-studio.md)  
  
## <a name="executing-code-during-synchronization"></a>Exécution de code pendant la synchronisation  
 La réplication prend en charge deux méthodes d'exécution de code pendant la synchronisation :  
  
-   L'exécution de script à la demande est prise en charge pour la réplication transactionnelle et la réplication de fusion. Vous pouvez, lors de l'utilisation de script à la demande, spécifier un script SQL à exécuter pendant la synchronisation. Le script est copié sur l'Abonné et exécuté par la commande **sqlcmd** au début du processus de synchronisation. Le script n'a pas accès aux modifications répliquées car elles sont appliquées à l'Abonné. Pour plus d’informations, consultez [Exécuter des scripts pendant la synchronisation &#40;programmation Transact-SQL de la réplication&#41;](../../relational-databases/replication/execute-scripts-during-synchronization-replication-transact-sql-programming.md).  
  
-   Les gestionnaires de logique métier sont pris en charge par la réplication de fusion. Vous pouvez, à l'aide de l'infrastructure du gestionnaire de logique métier, écrire un assembly de code managé qui est appelé pendant le processus de synchronisation de fusion. L'assembly comprend une logique métier qui peut répondre à un certain nombre de conditions au cours de la synchronisation : les modifications de données, les conflits et les erreurs. Pour plus d’informations, consultez [Exécuter la logique métier lors de la synchronisation de fusion](../../relational-databases/replication/merge/execute-business-logic-during-merge-synchronization.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Détecter et résoudre les conflits de réplication de fusion](../../relational-databases/replication/merge/advanced-merge-replication-resolve-merge-replication-conflicts.md)  
  
  
