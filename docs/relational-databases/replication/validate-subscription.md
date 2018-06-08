---
title: Valider l’abonnement | Microsoft Docs
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
- sql13.rep.validate.validateandresynch.f1
helpviewer_keywords:
- Validate Subscription dialog box
ms.assetid: 74bdf5e1-b886-4284-b5fb-332bf79ae083
caps.latest.revision: 19
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 9dd7793586e2fde1289c61cc070f7c329d6d3db5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="validate-subscription"></a>Valider l'abonnement
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Utilisez la boîte de dialogue **Valider l'abonnement** pour indiquer qu'un abonnement à une publication de fusion doit être validée lors de la prochaine exécution de l'Agent de fusion de l'abonnement. Le résultat de la validation figure dans le Moniteur de réplication. Pour plus d'informations, voir [Validate Data at the Subscriber](../../relational-databases/replication/validate-data-at-the-subscriber.md).  
  
 Vous pouvez également valider tous les abonnements à une publication de fusion en cliquant avec le bouton droit de la souris sur une publication dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] et en cliquant sur **Valider tous les abonnements**.  
  
## <a name="options"></a>Options  
 **Date de la dernière tentative de validation**  
 Date de la dernière session de l'Agent de fusion qui incluait la validation d'abonnement, que la validation ait abouti ou non.  
  
 **Date de la dernière validation réussie**  
 Date de la dernière session de l'Agent de fusion qui incluait une validation d'abonnement réussie.  
  
 **Valider cet abonnement**  
 Sélectionnez cette option pour valider l'abonnement.  
  
 **Options**  
 Cliquez pour accéder à la boîte de dialogue **Options de validation d'abonnement** qui permet d'indiquer si vous voulez utiliser la validation du nombre de lignes ou la validation de somme de contrôle.  
  
## <a name="see-also"></a> Voir aussi  
 [Valider des données répliquées](../../relational-databases/replication/validate-replicated-data.md)  
  
  
