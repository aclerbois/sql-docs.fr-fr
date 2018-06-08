---
title: Abonnements pouvant être mis à jour | Microsoft Docs
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
f1_keywords:
- sql13.rep.newsubwizard.updatablesubscriptions.f1
ms.assetid: 8e9a13a0-6b24-47c6-9d83-3cbaf08f673d
caps.latest.revision: 22
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: e8be8df0ca94a2c335d5dbdb24503d6c62a31211
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="updatable-subscriptions"></a>Abonnements pouvant être mis à jour
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Avec la réplication transactionnelle, les données répliquées doivent être traitées en lecture seule. Toutefois, vous pouvez modifier les données répliquées au niveau d'un abonné [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en utilisant des abonnements pouvant être mis à jour. Si vous devez modifier des données sur l'Abonné, vous pouvez choisir l'une des options suivantes selon vos besoins.  
  
|Type d'abonnement pouvant être mis à jour|Spécifications|  
|---------------------------------|------------------|  
|Mise à jour immédiate|Le serveur de publication et l'Abonné doivent être connectés pour mettre à jour les données sur l'Abonné.|  
|Mise à jour en attente|Le serveur de publication et l'Abonné n'ont pas besoin d'être connectés pour mettre à jour les données sur l'Abonné. Les mises à jour peuvent être effectuées hors ligne et synchronisées ensuite entre le serveur de publication et l'Abonné.|  
  
## <a name="options"></a>Options  
 **Répliquer les modifications de l'Abonné**  
 Activez la case à cocher dans la colonne **Répliquer** pour chaque abonné qui doit pouvoir effectuer des mises à jour. Pour les abonnés qui peuvent effectuer des mises à jour, sélectionnez l'option appropriée dans la zone de liste déroulante de la colonne **Valider sur le serveur de publication** :  
  
-   Sélectionnez **Enregistrer les modifications simultanément** pour un abonnement mis à jour immédiatement.  
  
-   Sélectionnez **Mettre les modifications en file d'attente et valider dès que possible** pour un abonnement mis à jour en file d'attente.  
  
## <a name="see-also"></a> Voir aussi  
 [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md)   
 [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md)   
 [Subscribe to Publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)  
  
  
