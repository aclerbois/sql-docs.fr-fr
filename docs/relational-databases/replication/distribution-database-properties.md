---
title: Propriétés de la base de données de distribution | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
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
- sql13.rep.configdistwizard.distdbproperties.f1
helpviewer_keywords:
- Distribution Database Properties dialog box
ms.assetid: 0f404ab9-1237-4936-8df5-888baab6a245
caps.latest.revision: 23
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 2ec47bad429a79f1995461e7a73f06bdad735acd
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="distribution-database-properties"></a>Propriétés de la base de données de distribution
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  La boîte de dialogue **Propriétés de la base de données de distribution** vous permet d'afficher certaines propriétés et de définir la période de rétention de transaction ainsi que la période de rétention d'historique pour la base de données.  
  
## <a name="options"></a>Options  
 **Nom**  
 Nom de la base de données de distribution, dont la valeur par défaut est « distribution » (en lecture seule).  
  
 **Emplacements des fichiers**  
 Emplacement du fichier de base de données et du fichier journal (en lecture seule).  
  
 **Période de rétention des transactions**  
 Également appelée période de rétention de distribution. Durée de stockage des transactions pour une réplication transactionnelle. Pour plus d’informations, voir [Subscription Expiration and Deactivation](../../relational-databases/replication/subscription-expiration-and-deactivation.md).  
  
 **Période de rétention de l'historique**  
 Durée de stockage des métadonnées de l'historique pour tous les types de réplications.  
  
 **Sécurité de l'Agent de lecture de la file d'attente**  
 L'Agent de lecture de la file d'attente est utilisé par la réplication transactionnelle avec les abonnements mis à jour en file d'attente. Un Agent de lecture de la file d'attente est créé automatiquement si vous sélectionnez l'option **Publication transactionnelle avec abonnements pouvant être mis à jour** dans la page **Type de publication** de l'Assistant Nouvelle publication. Cliquez sur **Paramètres de sécurité…** pour modifier le compte sous lequel l'Agent est exécuté et effectue des connexions avec le serveur de distribution.  
  
 Un Agent de lecture de la file d'attente peut également être créé en sélectionnant l'option **Créer un Agent de lecture de la file d'attente** dans cette page (cette option est désactivée si l'Agent a déjà été créé).  
  
 Des informations de connexion supplémentaires pour l'Agent de lecture de la file d'attente sont spécifiées dans deux endroits :  
  
-   L'Agent se connecte au serveur de publication à l'aide des informations d'identification spécifiées dans la boîte de dialogue **Propriétés du serveur de publication** , qui est accessible à partir de la page **Serveurs de publication** de la boîte de dialogue **Propriétés du serveur de distribution** .  
  
-   L'Agent se connecte à l'Abonné à l'aide des informations d'identification spécifiées pour l'Agent de distribution dans l'Assistant Nouvel abonnement.  
  
 Pour plus d’informations, consultez  [Replication Agent Security Model](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Configurer la distribution](../../relational-databases/replication/configure-distribution.md)   
 [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md)   
 [Créer un abonnement par émission de données](../../relational-databases/replication/create-a-push-subscription.md)   
 [Afficher et modifier les propriétés d’un serveur de distribution ou d’un serveur de publication](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)  
  
  
