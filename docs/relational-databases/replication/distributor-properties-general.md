---
title: Propriétés du serveur de distribution, Général | Microsoft Docs
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
- sql13.rep.configdistwizard.distproperties.general.f1
helpviewer_keywords:
- Distributor Properties dialog box
ms.assetid: ab4120ec-e524-4c0c-8b48-f2f40adb1a3b
caps.latest.revision: 22
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: e06ffa4473bf9f92509252b851e3e93a6b16beae
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="distributor-properties-general"></a>Propriétés du serveur de distribution, Général
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  La page **Général** de la boîte de dialogue **Propriétés du serveur de distribution** vous permet d'ajouter et de supprimer des bases de données de distribution et d'en définir des propriétés.  
  
 La base de données de distribution stocke les métadonnées et les données d'historique pour tous les types de réplications, et les transactions pour la réplication transactionnelle. Dans de nombreux cas, une seule base de données de distribution est suffisante. Mais si plusieurs serveurs de publication n'utilisent qu'un seul serveur de distribution, étudiez l'éventualité de créer une base de données de distribution propre à chaque serveur de publication. Ainsi, vous vous assurerez que les données transitant entre chaque base de données de distribution seront bien séparées.  
  
## <a name="options"></a>Options  
 **Bases de données**  
 La grille de propriétés **Bases de données** répertorie le nom et les propriétés de rétention des bases de données de distribution se trouvant sur le serveur de distribution. La**rétention des transactions** est la durée pendant laquelle les transactions sont stockées en vue de leur réplication transactionnelle (la rétention des transactions est également connue sous le nom de rétention de distribution). La**rétention des historiques** est la durée pendant laquelle les métadonnées des historiques sont stockées en vue de leur réplication de quelque type que ce soit. Pour plus d’informations sur la rétention de la distribution, consultez [Expiration et désactivation des abonnements](../../relational-databases/replication/subscription-expiration-and-deactivation.md).  
  
 Cliquez sur le bouton représenté par des points de suspension (**...**) dans la grille des propriétés **Bases de données** pour ouvrir la boîte de dialogue **Propriétés de la base de données de distribution** .  
  
 **Nouveau**  
 Permet de créer une base de données de distribution.  
  
 **Supprimer**  
 Sélectionnez une base de données de distribution dans la grille des propriétés **Bases de données** , puis cliquez sur **Supprimer** pour supprimer la base de données. Vous ne pouvez pas supprimer la base de données de distribution s'il n'existe qu'une seule base de données de ce type. Chaque serveur de distribution doit en effet disposer d'au moins une base de données de distribution. Pour les supprimer toutes, vous devez dans ce cas désactiver la distribution sur l'ordinateur. Pour plus d’informations, consultez [Désactiver la publication et la distribution](../../relational-databases/replication/disable-publishing-and-distribution.md).  
  
 **Valeurs par défaut du profil**  
 Permet d'accéder aux profils des agents de réplication dans la boîte de dialogue **Profils de l'Agent** . Pour plus d'informations sur ces profils, consultez [Replication Agent Profiles](../../relational-databases/replication/agents/replication-agent-profiles.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Configurer la distribution](../../relational-databases/replication/configure-distribution.md)   
 [Afficher et modifier les propriétés d’un serveur de distribution ou d’un serveur de publication](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)  
  
  
