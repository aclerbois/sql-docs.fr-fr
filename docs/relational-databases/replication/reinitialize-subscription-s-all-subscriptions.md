---
title: Réinitialiser les abonnements - Tous les abonnements | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
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
- sql13.rep.reinit.all.f1
helpviewer_keywords:
- Reinitialize Subscription(s) dialog box
ms.assetid: e1122018-9f74-43e3-8489-7eae33ff23d9
caps.latest.revision: 11
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 4bc95a8cc7b8b7b0b7e7dd59a2448e7fbc4bef81
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="reinitialize-subscriptions---all-subscriptions"></a>Réinitialiser les abonnements - Tous les abonnements
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  La boîte de dialogue **Réinitialiser les abonnements** permet de marquer tous les abonnements à une publication qui doivent être réinitialisés. La réinitialisation implique l'application d'un instantané à chaque Abonné ; l'Agent de distribution l'effectue pour les abonnements aux publications transactionnelles, l'Agent de fusion pour les abonnements aux publications de fusion.  
  
## <a name="options"></a>Options  
 **Utiliser l'instantané actuel**  
 Applique l'instantané actif à chaque Abonné lors de la prochaine exécution de l'Agent de distribution ou de fusion pour l'abonnement. Si aucun instantané valide n'est disponible, vous ne pouvez pas sélectionner cette option.  
  
 **Utiliser un nouvel instantané**  
 Réinitialise tous les abonnements avec un nouvel instantané. Il est possible d'appliquer l'instantané à chaque Abonné uniquement lorsqu'il a été créé par l'Agent d'instantané. Si l'exécution de l'Agent est planifiée, les abonnements ne sont pas réinitialisés avant la prochaine exécution planifiée de l'Agent d'instantané.  
  
 Sélectionnez **Générer le nouvel instantané maintenant** pour lancer immédiatement l'Agent d'instantané.  
  
 **Télécharger les modifications non synchronisées avant la réinitialisation**  
 Réplication de fusion uniquement. Cette option télécharge les modifications en attente à partir des bases de données d'abonnement avant que les données pour les Abonnés ne soient remplacées par un instantané.  
  
 Si vous ajoutez, supprimez ou modifiez un filtre paramétré, les modifications en attente chez l'abonné ne peuvent pas être chargées sur le serveur de publication pendant la réinitialisation. Si vous voulez télécharger les modifications en attente, synchronisez tous les abonnements avant de modifier le filtre.  
  
 **Marquer pour réinitialisation**  
 Cliquez pour marquer chaque abonnement à réinitialiser. Lorsqu'un instantané est disponible, lors de la prochaine exécution de l'Agent de distribution ou de fusion, l'instantané est appliqué à l'Abonné.  
  
## <a name="see-also"></a> Voir aussi  
 [Réinitialiser des abonnements](../../relational-databases/replication/reinitialize-subscriptions.md)  
  
  
