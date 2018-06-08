---
title: Résoudre une opération d’ajout de fichier ayant échoué (Groupes de disponibilité AlwaysOn) | Microsoft Docs
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: high-availability
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- secondary databases [SQL Server], Availability Groups
- Availability Groups [SQL Server], troubleshooting
ms.assetid: 31ceaebf-864b-4dd0-9112-0d047b0316ad
caps.latest.revision: 11
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 55cb45b70259dde919ec33bb01d1e1bd30fbf483
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34769975"
---
# <a name="troubleshoot-a-failed-add-file-operation-always-on-availability-groups"></a>Résoudre une opération d’ajout de fichier ayant échoué (groupes de disponibilité Always On)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Pour certains déploiements de groupes de disponibilité Always On, les chemins des fichiers varient entre le système qui héberge le réplica principal et les systèmes qui hébergent un réplica secondaire. Si le chemin d'accès aux fichiers d'une opération d'ajout de fichier n'existe pas sur le réplica secondaire, l'opération d'ajout de fichier réussit sur la base de données principale. Cependant, l'opération d'ajout e fichier entraîne l'interruption de la base de données secondaire. Par voie de conséquence, le réplica secondaire passe à l'état NOT SYNCHRONIZING.  
  
> [!NOTE]  
>  Nous recommandons, si possible, que le chemin d'accès au fichier (y compris la lettre de lecteur) d'une base de données secondaire donnée soit identique au chemin d'accès de la base de données primaire correspondante.  
  
## <a name="problem-resolution"></a>Dépannage  
 Pour résoudre ce problème, le propriétaire de la base de données doit effectuer les opérations suivantes :  
  
1.  Supprimer la base de données secondaire du groupe de disponibilité. Pour plus d’informations, consultez [Supprimer une base de données secondaire d’un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-database-from-an-availability-group-sql-server.md).  
  
2.  Sur la base de données secondaire existante, restaurer une sauvegarde complète du groupe de fichiers contenant le fichier ajouté à la base de données secondaire à l'aide de WITH NORECOVERY et de WITH MOVE (en spécifiant le chemin d'accès au fichier sur l'instance de serveur qui héberge le réplica secondaire). Pour plus d’informations, consultez [Restaurer une base de données à un nouvel emplacement &#40;SQL Server&#41;](../../../relational-databases/backup-restore/restore-a-database-to-a-new-location-sql-server.md).  
  
3.  Sauvegarder le journal des transactions qui contient l'opération d'ajout de fichier sur la base de données primaire, puis restaurer manuellement la sauvegarde du journal sur la base de données secondaire à l'aide de WITH NORECOVERY et de WITH MOVE.  
  
4.  Préparer la base de données secondaire pour rejoindre le groupe de disponibilité, en restaurant, à l'aide de WITH NORECOVERY, toutes les autres sauvegardes de journal en attente de la base de données primaire.  
  
5.  Réintégrer la base de données secondaire au groupe de disponibilité. Pour plus d’informations, consultez [Joindre une base de données secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Préparer manuellement une base de données secondaire pour un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)   
 [Dépanner des utilisateurs orphelins &#40;SQL Server&#41;](../../../sql-server/failover-clusters/troubleshoot-orphaned-users-sql-server.md)   
 [Résoudre des problèmes de configuration des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md)
