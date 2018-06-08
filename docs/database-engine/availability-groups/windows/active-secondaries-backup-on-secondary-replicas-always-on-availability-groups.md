---
title: Secondaires actifs - Sauvegarde sur les réplicas secondaires - Disponibilité Always On | Microsoft Docs
ms.custom: ''
ms.date: 09/01/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: high-availability
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- backup priority
- backup on secondary replicas
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], backup on secondary replicas
- active secondary replicas [SQL Server], backup on
- automated backup preference
- Availability Groups [SQL Server], active secondary replicas
ms.assetid: 82afe51b-71d1-4d5b-b20a-b57afc002405
caps.latest.revision: 34
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 57eedc29ea74b102db8d25573b376041def228c5
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34770195"
---
# <a name="active-secondaries-backup-on-secondary-replicas-always-on-availability-groups"></a>Secondaires actifs : sauvegarde sur les réplicas secondaires (groupes de disponibilité Always On)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Les fonctions secondaires actives [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] incluent la prise en charge des opérations de sauvegarde sur les réplicas secondaires. Les opérations de sauvegarde peuvent solliciter de manière significative les E/S et l'UC (avec la compression de sauvegarde). Le déchargement des sauvegardes vers un réplica secondaire synchronisé ou en cours de synchronisation vous permet d'utiliser les ressources sur l'instance de serveur qui héberge le réplica principal pour vos charges de travail de niveau 1.  

> [!NOTE]  
>  Les instructions RESTORE ne sont pas autorisées sur les bases de données primaire ou secondaire d'un groupe de disponibilité.  
  
-   [Types de sauvegarde pris en charge](#SupportedBuTypes)  
  
-   [CConfiguration de l'emplacement d'exécution des travaux de sauvegarde](#WhereBuJobsRun)  
  
-   [Tâches associées](#RelatedTasks)  
  
##  <a name="SupportedBuTypes"></a> Types de sauvegardes pris en charge sur les réplicas secondaires  
  
-   **BACKUP DATABASE** prend en charge uniquement la copie seule des sauvegardes complètes de la base de données, des fichiers ou des groupes de fichiers lorsqu’elle est exécutée sur les réplicas secondaires. Notez que les sauvegardes de type copie seule n'affectent pas la séquence de journaux de transactions consécutifs ou n'effacent pas la bitmap différentielle.  
  
-   Les sauvegardes différentielles ne sont pas prises en charge sur les réplicas secondaires.  
  
-   **BACKUP LOG** prend uniquement en charge les sauvegardes de journaux régulières (l’option COPY_ONLY n’est pas prise en charge pour les sauvegardes de fichiers journaux sur des réplicas secondaires).  
  
     Une séquence de journaux de transactions consécutifs cohérente est garantie sur les sauvegardes des journaux effectuées sur les réplicas (principaux ou secondaires), quel que soit leur mode de disponibilité (avec validation synchrone ou validation asynchrone).  
  
-   Pour sauvegarder une base de données secondaire, un réplica secondaire doit pouvoir communiquer avec le réplica principal et doit être **SYNCHRONIZED** ou **SYNCHRONIZING**.  

Dans un groupe de disponibilité distribué, les sauvegardes peuvent être effectuées sur les réplicas secondaires dans le même groupe de disponibilité que le réplica principal actif, ou sur le réplica principal d’un groupe de disponibilité secondaire. Les sauvegardes ne peuvent pas être effectuées sur un réplica secondaire dans un groupe de disponibilité secondaire, car les réplicas secondaires communiquent uniquement avec le réplica principal dans leur propre groupe de disponibilité. Seuls les réplicas qui communiquent directement avec le réplica principal global peuvent effectuer des opérations de sauvegarde.

##  <a name="WhereBuJobsRun"></a> CConfiguration de l'emplacement d'exécution des travaux de sauvegarde  
 L'exécution de sauvegardes sur un réplica secondaire pour décharger la charge de travail de sauvegarde du serveur de production principal constitue un énorme avantage. Cependant, les sauvegardes sur des réplicas secondaires compliquent considérablement la détermination de l'emplacement d'exécution des travaux de sauvegarde. Pour résoudre ce problème, configurez l'emplacement d'exécution des travaux de sauvegarde comme suit :  
  
1.  Configurez le groupe de disponibilité pour spécifier les réplicas de disponibilité de votre choix pour effectuer les sauvegardes. Pour plus d’informations, consultez les paramètres *AUTOMATED_BACKUP_PREFERENCE* et *BACKUP_PRIORITY* dans [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/create-availability-group-transact-sql.md) ou [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-availability-group-transact-sql.md).  
  
2.  Créez les travaux de sauvegarde par script pour chaque base de données de disponibilité sur chaque instance de serveur qui héberge un réplica de disponibilité candidat pour effectuer des sauvegardes. Pour plus d’informations, consultez la section « Suivi : Après la configuration de la sauvegarde sur les réplicas secondaires » dans [Configurer la sauvegarde sur des réplicas de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md).  
  
##  <a name="RelatedTasks"></a> Tâches associées  
 **Pour configurer la sauvegarde sur les réplicas secondaires**  
  
-   [Configurer la sauvegarde sur des réplicas de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
 **Pour déterminer si le réplica actuel est le réplica de sauvegarde par défaut**  
  
-   [sys.fn_hadr_backup_is_preferred_replica](../../../relational-databases/system-functions/sys-fn-hadr-backup-is-preferred-replica-transact-sql.md)  
  
 **Pour créer un travail de sauvegarde**  
  
-   [Utiliser l'Assistant Plan de maintenance](../../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md)  
  
-   [Implémenter des travaux](http://msdn.microsoft.com/library/69e06724-25c7-4fb3-8a5b-3d4596f21756)  
  
## <a name="see-also"></a> Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Sauvegardes de type copie seule &#40;SQL Server&#41;](../../../relational-databases/backup-restore/copy-only-backups-sql-server.md)   
 [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/create-availability-group-transact-sql.md)   
 [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-availability-group-transact-sql.md)  
  
  
