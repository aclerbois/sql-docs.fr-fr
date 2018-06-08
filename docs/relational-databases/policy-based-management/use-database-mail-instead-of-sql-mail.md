---
title: Utiliser la messagerie de base de données plutôt que SQL Mail | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: performance-monitor
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Best Practices [Database Engine]
ms.assetid: b08df7be-d8be-4184-a661-38ec0ac85cd1
caps.latest.revision: 9
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 091e8278e18cd5c4545bad31e0d5fec0a5c4c7fa
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="use-database-mail-instead-of-sql-mail"></a>Utiliser la messagerie de base de données plutôt que SQL Mail
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette règle vérifie dans l'affichage catalogue sys.configurations si l'option de configuration au niveau serveur sqlmail XPs est définie sur ON.  
  
## <a name="best-practices-recommendations"></a>Meilleures pratiques recommandées  
 SQL Mail sera supprimé dans une version ultérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Pour envoyer du courrier, utilisez la messagerie de base de données.  
  
 SQL Mail s'exécute dans un processus interne à un service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si SQL Mail arrête de fonctionner, il en sera de même pour le serveur. La messagerie de base de données s'exécute en dehors de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans un processus distinct, est évolutive et ne requiert pas l'installation des composants clients MAPI étendus sur le serveur de production.  
  
## <a name="for-more-information"></a>Pour plus d'informations  
 [Messagerie de base de données](../../relational-databases/database-mail/database-mail.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Contrôler et appliquer les meilleures pratiques à l'aide de la Gestion basée sur des stratégies](../../relational-databases/policy-based-management/monitor-and-enforce-best-practices-by-using-policy-based-management.md)  
  
  
