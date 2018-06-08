---
title: Spécifier un type d’abonnement de fusion et une priorité pour la résolution des conflits | Microsoft Docs
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
- merge replication conflict resolution [SQL Server replication], merge subscription resolvers
- conflict resolution [SQL Server replication], merge replication
ms.assetid: 2b828d83-2341-4924-b92a-4f81a22246c0
caps.latest.revision: 35
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 00abc16a10addad0b4c690df637e610e4e24515e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="specify-a-merge-subscription-type-and-conflict-resolution-priority"></a>Spécifier un type d’abonnement de fusion et une priorité pour la résolution des conflits
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Spécifiez un type d'abonnement de fusion et une priorité pour la résolution des conflits dans la page **Type d'abonnement** de l'Assistant Nouvel abonnement. Pour plus d'informations sur l'utilisation de cet Assistant, consultez [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md) et [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md).  
  
 Le type d’abonnement ne peut pas être modifié après la création d’un abonnement, mais la priorité peut être modifiée pour le type d’abonnement serveur dans la boîte de dialogue **Propriétés de l’abonnement - \<Serveur_de_publication> : \<Base_de_données_de_publication>**. Pour plus d'informations sur l'accès à cette boîte de dialogue, consultez [View and Modify Push Subscription Properties](../../relational-databases/replication/view-and-modify-push-subscription-properties.md) et [View and Modify Pull Subscription Properties](../../relational-databases/replication/view-and-modify-pull-subscription-properties.md).  
  
### <a name="to-specify-a-merge-subscription-type-and-conflict-resolution-priority"></a>Pour spécifier un type d'abonnement de fusion et une priorité pour la résolution des conflits  
  
1.  Dans la page **Type d'abonnement** de l'Assistant Nouvel abonnement, sélectionnez **Client** ou **Serveur** pour l'option **Type d'abonnement** .  
  
2.  Si vous sélectionnez le type d'abonnement **Serveur**, affectez également une valeur (0,00 à 99,99) à l'option **Priorité pour la résolution des conflits** .  
  
### <a name="to-modify-the-conflict-resolution-priority"></a>Pour modifier la priorité pour la résolution des conflits  
  
1.  Dans la boîte de dialogue **Propriétés de l’abonnement - \<Serveur_de_publication> : \<Base_de_données_de_publication>** sur le serveur de publication, affectez une valeur (de 0,00 à 99,99) à l’option **Priorité**.  
  
2.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
## <a name="see-also"></a> Voir aussi  
 [Advanced Merge Replication Conflict Detection and Resolution](../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md)   
 [S'abonner à des publications](../../relational-databases/replication/subscribe-to-publications.md)  
  
  
