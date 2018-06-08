---
title: Propriétés de l’abonnement - Serveur de publication | Microsoft Docs
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
- sql13.rep.newsubwizard.subproperties.publisher.f1
helpviewer_keywords:
- Subscription Properties dialog box
ms.assetid: d4b2bc8b-0431-4331-8305-8992c96d0d34
caps.latest.revision: 22
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: a393e209483b93b0181bc57f4eabac1727c967fa
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="subscription-properties---publisher"></a>Propriétés de l'abonnement - Serveur de publication
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  La boîte de dialogue **Propriétés de l'abonnement** du serveur de publication permet d'afficher et de configurer les propriétés des abonnements par envoi de données. Vous pouvez également afficher certaines propriétés des abonnements par extraction. Cependant, la boîte de dialogue **Propriétés de l'abonnement** de l'abonné affiche des propriétés supplémentaires que vous pouvez modifier.  
  
 Chaque propriété de **cette boîte de dialogue** comporte une description. Cliquez sur une propriété pour afficher sa description au bas de la boîte de dialogue. Cette rubrique fournit des informations supplémentaires sur diverses propriétés, dont la plupart sont affichées dans le serveur de publication uniquement pour les abonnements par envoi de données. Les propriétés sont regroupées selon les catégories suivantes :  
  
-   Propriétés appliquées à tous les abonnements.  
  
-   Propriétés appliquées aux abonnements transactionnels.  
  
-   Propriétés appliquées aux abonnements de fusion.  
  
 Si une option est affichée en lecture seule, vous pouvez la configurer uniquement lorsque l'abonnement est créé. Si vous voulez configurer des options non disponibles dans l'Assistant Nouvel abonnement, créez l'abonnement avec des procédures stockées. Pour plus d'informations, consultez [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md) et [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md).  
  
## <a name="options-for-all-subscriptions"></a>Options pour tous les abonnements  
 **Sécurité**  
 Cliquez sur la ligne **Compte de processus de l'agent** , puis sur le bouton des propriétés (**...**) pour modifier le compte sous lequel l'Agent de distribution ou de fusion s'exécute sur le serveur de distribution. Pour modifier le compte sous lequel l'Agent de distribution ou de fusion établit les connexions avec l'abonné, cliquez sur **Connexion de l'Abonné**, puis sur le bouton des propriétés (**...**).  
  
 Pour plus d'informations sur les autorisations indispensables pour chaque agent, consultez [Replication Agent Security Model](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
## <a name="options-for-transactional-subscriptions"></a>Options des abonnements transactionnels  
 **Empêcher le bouclage de la transaction**  
 Détermine si l'Agent de distribution retourne à l'Abonné les transactions créées sur ce dernier. Cette option est utilisée pour la réplication transactionnelle bidirectionnelle. Pour plus d’informations, voir [Bidirectional Transactional Replication](../../relational-databases/replication/transactional/bidirectional-transactional-replication.md).  
  
 **Abonnement pouvant être mis à jour**  
 Détermine si les modifications de l'Abonné sont répliquées dans le serveur de publication. Vous pouvez répliquer les modifications en utilisant la mise à jour immédiate ou en file d'attente. L'option **Méthode de mise à jour de l'Abonné** détermine la méthode à utiliser. Pour plus d’informations, voir [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md).  
  
## <a name="options-for-merge-subscriptions"></a>Options des abonnements de fusion  
 **Définition de la partition (HOST_NAME)**  
 Pour une publication qui utilise des filtres paramétrables, la publication de fusion évalue une des deux fonctions système (ou les deux si le filtre fait référence aux deux) pendant la synchronisation pour déterminer les données qu'un abonné doit recevoir : **SUSER_SNAME()** ou **HOST_NAME()**. Par défaut, **HOST_NAME()** retourne le nom de l'ordinateur sur lequel s'exécute l'Agent de fusion, mais vous pouvez l'ignorer dans l'Assistant Nouvel abonnement. Pour plus d'informations sur les filtres paramétrés et l'annulation de la fonction **HOST_NAME()**, consultez [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
 **Type d'abonnement** et **Priorité**  
 Indique si l'abonnement est un abonnement de client ou de serveur (cette option n'est pas modifiable après la création de l'abonnement). Les abonnements de serveur peuvent republier les données vers d'autres abonnés. Il est possible de leur affecter une priorité pour la résolution des conflits.  
  
 Si vous avez sélectionné un abonnement de type serveur dans l'Assistant Nouvel abonnement, l'abonné a la priorité utilisée lors de la résolution des conflits.  
  
 **Résoudre les conflits interactivement**  
 Détermine s'il faut utiliser l'interface utilisateur du Résolveur interactif pour résoudre les conflits pendant la synchronisation de fusion. Pour cela, l'option **Utiliser le Gestionnaire de synchronisation Windows** doit être active ( **Activer**). Pour plus d’informations, consultez [Interactive Conflict Resolution](../../relational-databases/replication/merge/advanced-merge-replication-conflict-interactive-resolution.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Afficher et modifier les propriétés d’un abonnement par extraction](../../relational-databases/replication/view-and-modify-pull-subscription-properties.md)   
 [Afficher et modifier les propriétés d’un abonnement par émission de données](../../relational-databases/replication/view-and-modify-push-subscription-properties.md)   
 [S'abonner à des publications](../../relational-databases/replication/subscribe-to-publications.md)  
  
  
