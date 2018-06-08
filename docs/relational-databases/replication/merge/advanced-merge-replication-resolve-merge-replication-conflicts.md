---
title: Détecter et résoudre les conflits de réplication de fusion | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
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
- merge replication conflict resolution [SQL Server replication], about conflict resolution
- default conflict resolver
- conflict resolution [SQL Server replication]
- viewing merge replication conflicts
- resolving merge replication conflicts
- articles [SQL Server replication], conflict resolution
- merge replication conflict resolution [SQL Server replication]
- conflict resolution [SQL Server replication], merge replication
ms.assetid: 0d033c76-e8c9-4e35-ab95-4d335abb18c1
caps.latest.revision: 37
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: f80dc6ce38117335f571903dcac9a124dcd106e1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="advanced-merge-replication---resolve-merge-replication-conflicts"></a>Réplication de fusion avancée - Résoudre les conflits de réplication de fusion
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Lorsqu'un serveur de publication et un Abonné sont connectés et que la synchronisation se produit, l'Agent de fusion détecte la présence d'éventuels conflits. Si tel est le cas, l'Agent de fusion utilise un programme de résolution de conflits pour déterminer les données qui doivent être acceptées et propagées aux autres sites.  
  
> [!NOTE]  
>  Bien qu'un Abonné se synchronise avec le serveur de publication, les conflits se produisent généralement entre les mises à jour des différents Abonnés plutôt qu'entre les mises à jour de l'Abonné et du serveur de publication.  
  
 La réplication de fusion propose plusieurs méthodes de détection et de résolution des conflits. Pour la plupart des applications, la méthode par défaut est la plus adaptée :  
  
-   Si un conflit se produit entre un serveur de publication et un Abonné, la modification du serveur de publication est conservée tandis que celle de l'Abonné est annulée.  
  
-   Si un conflit se produit entre deux Abonnés utilisant des abonnements client (le type par défaut des abonnements par extraction de données), la modification provenant du premier Abonné pour se synchroniser avec le serveur de publication est conservée tandis que celle provenant du second Abonné est annulée. Pour plus d’informations sur la spécification des abonnements client et serveur, consultez [Spécifier un type d’abonnement de fusion et une priorité pour la résolution des conflits &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/specify-a-merge-subscription-type-and-conflict-resolution-priority.md).  
  
-   Si un conflit se produit entre deux Abonnés utilisant des abonnements serveur (le type par défaut des abonnements par envoi de données), la modification provenant de l'Abonné ayant la valeur de priorité la plus élevée est conservée tandis que celle provenant du second Abonné est annulée. Si les valeurs de priorité sont identiques, la modification provenant du premier Abonné pour se synchroniser avec le serveur de publication est conservée.  
  
 Pour plus d'informations sur la détection et la résolution des conflits pour la réplication de fusion, consultez [Advanced Merge Replication Conflict Detection and Resolution](../../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Options d’articles pour la réplication de fusion](../../../relational-databases/replication/merge/article-options-for-merge-replication.md)   
 [S’abonner à des publications](../../../relational-databases/replication/subscribe-to-publications.md)  
  
  
