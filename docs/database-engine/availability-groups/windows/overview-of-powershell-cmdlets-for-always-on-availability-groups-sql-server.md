---
title: Vue d’ensemble des applets de commande PowerShell pour les groupes de disponibilité Always On (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 08/30/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: high-availability
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], PowerShell cmdlets
- Availability Groups [SQL Server], about
- PowerShell [SQL Server], cmdlets
ms.assetid: b3fef0d5-b6d7-4386-a0f0-d06c165ad4de
caps.latest.revision: 36
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 21ec89f0a33e3170eed83f3a1ed0333a5bc67dac
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34769215"
---
# <a name="overview-of-powershell-cmdlets-for-always-on-availability-groups-sql-server"></a>Vue d’ensemble des applets de commande PowerShell pour les groupes de disponibilité Always On (SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  [!INCLUDE[msCoName](../../../includes/msconame-md.md)] PowerShell est un interpréteur de ligne de commande et un langage de script basé sur des tâches tout spécialement conçu pour l’administration système. [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] fournit un ensemble d'applets de commande PowerShell dans [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] qui vous permet de déployer, gérer et surveiller des groupes de disponibilité, des réplicas de disponibilité et des bases de données de disponibilité.  
  
> [!NOTE]  
>  Une applet de commande PowerShell peut s'effectuer en initiant correctement une action. Cela n'indique pas si le travail prévu, tel que le basculement d'un groupe de disponibilité, est terminé. Lors de l'écriture sous forme de script d'une séquence d'actions, vous devrez peut-être vérifier l'état des actions et attendre la fin de leur exécution.  
  
 Cette rubrique présente les applets de commande pour les ensembles de tâches suivants :  
  
-   [Configuration d’une instance de serveur pour les groupes de disponibilité Always On](#ConfiguringServerInstance)  
  
-   [Sauvegarde et restauration de bases de données et de journaux de transactions](#BnRcmdlets)  
  
-   [Création et gestion d'un groupe de disponibilité](#DeployManageAGs)  
  
-   [Création et gestion d'un écouteur de groupe de disponibilité](#AGlisteners)  
  
-   [Création et gestion d'un réplica de disponibilité](#DeployManageARs)  
  
-   [Ajout et gestion d'une base de données de disponibilité](#DeployManageDbs)  
  
-   [Surveillance de l'intégrité d'un groupe de disponibilité](#MonitorTblshtAGs)  
  
> [!NOTE]  
>  Pour obtenir la liste des rubriques de la documentation en ligne [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] qui expliquent comment utiliser les applets de commande pour effectuer des tâches [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] , consultez la section « Tâches associées » de l’article [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).  
  
##  <a name="ConfiguringServerInstance"></a> Configuring a Server Instance for Always On Availability Groups  
  
|Applets de commande|Description|Prise en charge sur|  
|-------------|-----------------|------------------|
|[**Disable-SqlAlwaysOn**](/powershell/module/sqlserver/disable-sqlalwayson)|Désactive la fonctionnalité [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] sur une instance de serveur.|Instance de serveur spécifiée par le paramètre **Path**, **InputObject**ou **Name** . (Il doit s'agir d'une édition de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui prend en charge [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)].)|  
|[**Enable-SqlAlwaysOn**](/powershell/module/sqlserver/enable-sqlalwayson)|Active [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] sur une instance de [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] qui prend en charge la fonctionnalité [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] . Pour plus d’informations sur la prise en charge des [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], consultez [Conditions préalables requises, restrictions et recommandations pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).|Toute édition de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui prend en charge [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)].|  
|[**New-SqlHadrEndPoint**](/powershell/module/sqlserver/new-sqlhadrendpoint)|Crée un nouveau point de terminaison de mise en miroir de bases de données sur une instance de serveur. Ce point de terminaison est requis pour le déplacement des données entre des bases de données primaires et secondaires.|Toute instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]|  
|[**Set-SqlHadrEndpoint**](/powershell/module/sqlserver/set-sqlhadrendpoint)|Modifie les propriétés d'un point de terminaison de mise en miroir de bases de données existant, telles que le nom, l'état ou les propriétés d'authentification.|Instance de serveur qui prend en charge [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] et ne dispose pas d'un point de terminaison de mise en miroir de bases de données|  

  
##  <a name="BnRcmdlets"></a> Backing Up and Restoring Databases and Transaction Logs  
  
|Applets de commande|Description|Prise en charge sur|  
|-------------|-----------------|------------------|  
|[**Backup-SqlDatabase**](/powershell/module/sqlserver/backup-sqldatabase)|Crée une sauvegarde de données ou de journal.|Base de données en ligne (pour [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], base de données sur l'instance de serveur qui héberge le réplica principal)|  
|[**Restore-SqlDatabase**](/powershell/module/sqlserver/restore-sqldatabase)|Restaure une sauvegarde.|Instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (pour [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], instance de serveur qui héberge un réplica secondaire)<br /><br />

  >[!Important]
  >Quand vous préparez une base de données secondaire, vous devez utiliser le paramètre **-NoRecovery** dans chaque commande **Restore-SqlDatabase**. 
  
 Pour plus d’informations sur l’utilisation de ces applets de commande pour préparer une base de données secondaire, consultez [Préparer manuellement une base de données secondaire pour un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md).  
  
##  <a name="DeployManageAGs"></a> Creating and Managing an Availability Group  
  
|Applets de commande|Description|Prise en charge sur|  
|-------------|-----------------|------------------|  
|[**New-SqlAvailabilityGroup**](/powershell/module/sqlserver/new-sqlavailabilitygroup)|Crée un groupe de disponibilité.|Instance de serveur pour héberger le réplica principal|  
|[**Remove-SqlAvailabilityGroup**](/powershell/module/sqlserver/remove-sqlavailabilitygroup)|Supprime un groupe de disponibilité|Instance de serveur activée pour le service HADR|  
|[**Set-SqlAvailabilityGroup**](/powershell/module/sqlserver/set-sqlavailabilitygroup)|Définit les propriétés d'un groupe de disponibilité ; met un groupe de disponibilité en ligne/hors connexion|Instance de serveur qui héberge le réplica principal|  
|[**Switch-SqlAvailabilityGroup**](/powershell/module/sqlserver/switch-sqlavailabilitygroup)|Démarre l'une des formes de basculement suivantes :<br /><br /> Basculement forcé d'un groupe de disponibilité (avec possible perte de données).<br /><br /> Basculement manuel d'un groupe de disponibilité.|Instance de serveur qui héberge le réplica secondaire cible.|  
  
##  <a name="AGlisteners"></a> Creating and Managing an Availability Group Listener  
  
|Applet de commande|Description|Prise en charge sur|  
|------------|-----------------|------------------|  
|[**New-SqlAvailabilityGroupListener**](/powershell/module/sqlserver/new-sqlavailabilitygrouplistener)|Crée un écouteur de groupe de disponibilité et l'attache à un groupe de disponibilité existant.|Instance de serveur qui héberge le réplica principal|  
|[**Set-SqlAvailabilityGroupListener**](/powershell/module/sqlserver/set-sqlavailabilitygrouplistener)|Modifie le paramètre de port sur un écouteur de groupe de disponibilité existant.|Instance de serveur qui héberge le réplica principal|  
|[**Add-SqlAvailabilityGroupListenerStaticIp**](/powershell/module/sqlserver/add-sqlavailabilitygrouplistenerstaticip)|Ajoute une adresse IP statique à une configuration existante d'écouteur de groupe de disponibilité. L'adresse IP peut être une adresse IPv4 avec sous-réseau ou une adresse IPv6.|Instance de serveur qui héberge le réplica principal|  
  
##  <a name="DeployManageARs"></a> Creating and Managing an Availability Replica  
  
|Applets de commande|Description|Prise en charge sur|  
|-------------|-----------------|------------------|  
|[**New-SqlAvailabilityReplica**](/powershell/module/sqlserver/new-sqlavailabilityreplica)|Crée un réplica de disponibilité. Vous pouvez utiliser le paramètre **-AsTemplate** pour créer un objet en mémoire du réplica de disponibilité pour chaque nouveau réplica de disponibilité.|Instance de serveur qui héberge le réplica principal|  
|[**Join-SqlAvailabilityGroup**](/powershell/module/sqlserver/join-sqlavailabilitygroup)|Joint un réplica secondaire au groupe de disponibilité.|Instance de serveur qui héberge le réplica secondaire.|  
|[**Remove-SqlAvailabilityReplica**](/powershell/module/sqlserver/remove-sqlavailabilityreplica)|Supprime un réplica de disponibilité.|Instance de serveur qui héberge le réplica principal|  
|[**Set-SqlAvailabilityReplica**](/powershell/module/sqlserver/set-sqlavailabilityreplica)|Définit les propriétés d'un réplica de disponibilité.|Instance de serveur qui héberge le réplica principal|  
  
##  <a name="DeployManageDbs"></a> Adding and Managing an Availability Database  
  
|Applets de commande|Description|Prise en charge sur|  
|-------------|-----------------|------------------|  
|[**Add-SqlAvailabilityDatabase**](/powershell/module/sqlserver/add-sqlavailabilitydatabase)|Sur le réplica principal, ajoute une base de données à un groupe de disponibilité.<br /><br /> Sur un réplica secondaire, joint une base de données secondaire à un groupe de disponibilité.|Instance de serveur qui héberge un réplica de disponibilité (le comportement diffère pour les réplicas principal et secondaire)|  
|[**Remove-SqlAvailabilityDatabase**](/powershell/module/sqlserver/remove-sqlavailabilitydatabase)|Sur le réplica principal, supprime la base de données du groupe de disponibilité.<br /><br /> Sur un réplica secondaire, supprime la base de données secondaire locale du réplica secondaire local.|Instance de serveur qui héberge un réplica de disponibilité (le comportement diffère pour les réplicas principal et secondaire)|  
|[**Resume-SqlAvailabilityDatabase**](/powershell/module/sqlserver/resume-sqlavailabilitydatabase)|Reprend le déplacement des données pour une base de données de disponibilité interrompue.|Instance de serveur sur laquelle la base de données a été interrompue.|  
|[**Suspend-SqlAvailabilityDatabase**](/powershell/module/sqlserver/suspend-sqlavailabilitydatabase)|Interrompt le déplacement des données pour une base de données de disponibilité.|Instance de serveur qui héberge un réplica de disponibilité.|  
  
##  <a name="MonitorTblshtAGs"></a> Monitoring Availability Group Health  
 Les applets de commande [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] suivantes vous permettent de surveiller l'intégrité d'un groupe de disponibilité et de ses réplicas et bases de données.  
  
> [!IMPORTANT]  
>  Vous devez disposer des autorisations CONNECT, VIEW SERVER STATE et VIEW ANY DEFINITION pour exécuter ces applets de commande.  
  
|Applet de commande|Description|Prise en charge sur|  
|------------|-----------------|------------------|  
|[**Test-SqlAvailabilityGroup**](/powershell/module/sqlserver/test-sqlavailabilitygroup)|Évalue l'intégrité d'un groupe de disponibilité lors de l'évaluation des stratégies de gestion basées sur des stratégies SQL Server.|Toute instance de serveur qui héberge un réplica de disponibilité.*|  
|[**Test-SqlAvailabilityReplica**](/powershell/module/sqlserver/test-sqlavailabilityreplica)|Évalue l'intégrité des réplicas de disponibilité lors de l'évaluation des stratégies de gestion basées sur des stratégies SQL Server.|Toute instance de serveur qui héberge un réplica de disponibilité.*|  
|[**Test-SqlDatabaseReplicaState**](/powershell/module/sqlserver/test-sqldatabasereplicastate)|Évalue l'intégrité d'une base de données de disponibilité sur tous les réplicas de disponibilité joints par l'évaluation des stratégies de gestion basées sur des stratégies SQL Server.|Toute instance de serveur qui héberge un réplica de disponibilité.*|  
  
 *Pour afficher des informations sur tous les réplicas de disponibilité d’un groupe de disponibilité, utilisez l’instance de serveur qui héberge le réplica principal.  
  
 Pour plus d’informations, consultez [Utiliser les stratégies Always On pour afficher l’intégrité d’un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/use-always-on-policies-to-view-the-health-of-an-availability-group-sql-server.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Obtenir de l’aide sur SQL Server PowerShell](../../../relational-databases/scripting/get-help-sql-server-powershell.md)  
  
  
