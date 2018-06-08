---
title: Configurer la stratégie de basculement automatique flexible | Microsoft Docs
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: high-availability
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], flexible failover policy
- Availability Groups [SQL Server], failover
- failover [SQL Server], AlwaysOn Availability Groups
ms.assetid: 1ed564b4-9835-4245-ae35-9ba67419a4ce
caps.latest.revision: 24
author: MashaMSFT
ms.author: mathoma
manager: craigg
monikerRange: '>= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 7980cc5f02b5766ef30e06440cb6dfa30fdc5560
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34768715"
---
# <a name="configure-flexible-automatic-failover-policy"></a>Configurer la stratégie de basculement automatique flexible

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Cette rubrique explique comment configurer la stratégie de basculement flexible pour un groupe de disponibilité Always On à l’aide de [!INCLUDE[tsql](../../../includes/tsql-md.md)] ou PowerShell dans [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]. Une stratégie de basculement flexible vous offre un contrôle granulaire sur les conditions qui entraînent un basculement automatique d'un groupe de disponibilité. En changeant les conditions d'échec qui déclenchent un basculement automatique et la fréquence des contrôles d'intégrité, vous pouvez augmenter ou diminuer la probabilité d'un basculement automatique pour assurer le contrat de niveau de service relatif à la haute disponibilité.  
  
-   **Avant de commencer :**  
  
     [Limitations relatives aux basculements automatiques](#Limitations)  
  
     [Conditions préalables](#Prerequisites)  
  
     [Sécurité](#Security)  
  
-   **Pour configurer la stratégie de basculement flexible à l'aide de :**  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [PowerShell](#PowerShellProcedure)  
  
    > [!NOTE]  
    >  La stratégie de basculement flexible d'un groupe de disponibilité ne peut pas être configurée à l'aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)].  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Limitations"></a> Limitations relatives aux basculements automatiques  
  
-   Pour qu'un basculement automatique se produise, le réplica principal et un réplica secondaire doivent être configurés pour le mode de disponibilité avec validation synchrone et basculement automatique et le réplica secondaire doit être synchronisé avec le réplica principal.  
  
-   Seuls trois réplicas sont pris en charge dans le cadre du basculement automatique.  
  
-   Si un groupe de disponibilité dépasse le seuil d'échec WSFC, le cluster WSFC ne tente pas un basculement automatique du groupe de disponibilité. En outre, le groupe de ressources WSFC du groupe de disponibilité reste à l'état d'échec jusqu'à ce que l'administrateur de cluster mette manuellement le groupe de ressources en ligne ou jusqu'à ce que l'administrateur de base de données exécute un basculement manuel du groupe de disponibilité. Le *seuil d'échec WSFC* est le nombre maximal d'échecs autorisés pour le groupe de disponibilité au cours d'une période donnée. La période par défaut est de six heures, et la valeur par défaut du nombre maximal d’échecs au cours de cette période est *n*-1, où *n* est le nombre de nœuds de WSFC. Pour modifier les valeurs de seuil/d'échec pour un groupe de disponibilité donné, utilisez la console du gestionnaire de basculement WSFC.  
  
###  <a name="Prerequisites"></a> Conditions préalables  
  
-   Vous devez être connecté à l'instance de serveur qui héberge le réplica principal.  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
  
|Tâche|Autorisations|  
|----------|-----------------|  
|Pour configurer la stratégie de basculement flexible pour un nouveau groupe de disponibilité|Requiert l’appartenance au rôle serveur fixe **sysadmin** et l’autorisation de serveur CREATE AVAILABILITY GROUP, l’autorisation ALTER ANY AVAILABILITY GROUP ou l’autorisation CONTROL SERVER.|  
|Pour modifier la stratégie d'un groupe de disponibilité existant|Requiert l'autorisation ALTER AVAILABILITY GROUP sur le groupe de disponibilité, l'autorisation CONTROL AVAILABILITY GROUP, l'autorisation ALTER ANY AVAILABILITY GROUP ou l'autorisation CONTROL SERVER.|  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 **Pour configurer la stratégie de basculement flexible**  
  
1.  Connectez-vous à l'instance de serveur qui héberge le réplica principal.  
  
2.  Pour un nouveau groupe de disponibilité, utilisez l’instruction [CREATE AVAILABILITY GROUP](../../../t-sql/statements/create-availability-group-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] . Si vous modifiez un groupe de disponibilité existant, utilisez l’instruction [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] .  
  
    -   Pour définir le niveau de condition de basculement, utilisez l’option FAILURE_CONDITION_LEVEL = *n* , où *n* est un entier compris entre 1 et 5.  
  
         Par exemple, l'instruction [!INCLUDE[tsql](../../../includes/tsql-md.md)] suivante modifie le niveau de condition d'échec d'un groupe de disponibilité existant, `AG1`, sur le niveau un :  
  
        ```  
  
        ALTER AVAILABILITY GROUP AG1 SET (FAILURE_CONDITION_LEVEL = 1);  
        ```  
  
         La relation de ces valeurs entières avec les niveaux de condition d'échec est la suivante :  
  
        |[!INCLUDE[tsql](../../../includes/tsql-md.md)] Valeur|Level|Le basculement automatique démarre lorsque…|  
        |------------------------------|-----------|-------------------------------------------|  
        | 1|Un|Le serveur est arrêté. Le service SQL Server s'arrête à cause d'un basculement ou d'un redémarrage.|  
        |2|Deux|Le serveur ne répond pas. Toutes les conditions qui correspondent à une valeur inférieure sont remplies, le service SQL Server est connecté au cluster et le seuil du délai d'attente de contrôle d'intégrité est dépassé, ou le réplica principal actuel est dans un état d'échec.|  
        |3|Trois|Erreur critique du serveur. Toutes les conditions qui correspondent à une valeur inférieure sont remplies ou une erreur interne et critique de serveur est survenue.<br /><br /> C'est le niveau par défaut.|  
        |4|Quatre|Erreur de serveur modérée. Toutes les conditions qui correspondent à une valeur inférieure sont remplies ou une erreur modérée de serveur s'est produite.|  
        |5|Cinq|Conditions d'échec qualifiées. Toutes les conditions qui correspondent à une valeur inférieure sont remplies ou une condition d'échec qualifiée s'est produite.|  
  
         Pour plus d’informations sur les niveaux de condition de basculement, consultez [Stratégie flexible pour le basculement automatique d’un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/flexible-automatic-failover-policy-availability-group.md).  
  
    -   Pour configurer le seuil du délai d’attente de contrôle d’intégrité, utilisez l’optionHEALTH_CHECK_TIMEOUT = *n*, où *n* est un entier compris entre 15000 millisecondes (15 secondes) et 4294967295 millisecondes. La valeur par défaut est 30 000 millisecondes (ou 30 secondes).  
  
         Par exemple, l'instruction [!INCLUDE[tsql](../../../includes/tsql-md.md)] suivante modifie le seuil du délai d'attente de contrôle d'intégrité d'un groupe de disponibilité existant, `AG1`, sur 60 000 millisecondes (une minute).  
  
        ```  
  
        ALTER AVAILABILITY GROUP AG1 SET (HEALTH_CHECK_TIMEOUT = 60000);  
        ```  
  
##  <a name="PowerShellProcedure"></a> Utilisation de PowerShell  
 **Pour configurer la stratégie de basculement flexible**  
  
1.  Définissez la valeur par défaut (**cd**) sur l’instance de serveur qui héberge le réplica principal.  
  
2.  Quand vous ajoutez un réplica de disponibilité à un groupe de disponibilité, utilisez l’applet de commande **New-SqlAvailabilityGroup** . Quand vous modifiez un réplica de disponibilité existant, utilisez l’applet de commande **Set-SqlAvailabilityGroup** .  
  
    -   Pour définir le niveau de condition de basculement, utilisez le paramètre **FailureConditionLevel***niveau*, où *niveau* est une des valeurs suivantes :  
  
        |Valeur|Niveau|Le basculement automatique démarre lorsque…|  
        |-----------|-----------|-------------------------------------------|  
        |**OnServerDown**|Un|Le serveur est arrêté. Le service SQL Server s'arrête à cause d'un basculement ou d'un redémarrage.|  
        |**OnServerUnresponsive**|Deux|Le serveur ne répond pas. Toutes les conditions qui correspondent à une valeur inférieure sont remplies, le service SQL Server est connecté au cluster et le seuil du délai d'attente de contrôle d'intégrité est dépassé, ou le réplica principal actuel est dans un état d'échec.|  
        |**OnCriticalServerError**|Trois|Erreur critique du serveur. Toutes les conditions qui correspondent à une valeur inférieure sont remplies ou une erreur interne et critique de serveur est survenue.<br /><br /> C'est le niveau par défaut.|  
        |**OnModerateServerError**|Quatre|Erreur de serveur modérée. Toutes les conditions qui correspondent à une valeur inférieure sont remplies ou une erreur modérée de serveur s'est produite.|  
        |**OnAnyQualifiedFailureConditions**|Cinq|Conditions d'échec qualifiées. Toutes les conditions qui correspondent à une valeur inférieure sont remplies ou une condition d'échec qualifiée s'est produite.|  
  
         Pour plus d’informations sur les niveaux de condition de basculement, consultez [Stratégie flexible pour le basculement automatique d’un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/flexible-automatic-failover-policy-availability-group.md).  
  
         Par exemple, la commande suivante modifie le niveau de condition d'échec d'un groupe de disponibilité existant, `AG1`, sur le niveau un.  
  
        ```  
        Set-SqlAvailabilityGroup `   
        -Path SQLSERVER:\Sql\PrimaryServer\InstanceName\AvailabilityGroups\MyAg `   
        -FailureConditionLevel OnServerDown  
        ```  
  
    -   Pour définir le seuil du délai d’attente de contrôle d’intégrité, utilisez le paramètre **HealthCheckTimeout***n*, où *n* est un entier compris entre 15 000 millisecondes (15 secondes) et 4 294 967 295 millisecondes. La valeur par défaut est 30 000 millisecondes (ou 30 secondes).  
  
         Par exemple, la commande suivante modifie le seuil du délai d'attente de contrôle d'intégrité d'un groupe de disponibilité existant, `AG1`, sur 120 000 millisecondes (deux minutes).  
  
        ```  
        Set-SqlAvailabilityGroup `   
        -Path SQLSERVER:\Sql\PrimaryServer\InstanceName\AvailabilityGroups\MyAG `   
        -HealthCheckTimeout 120000  
        ```  
  
> [!NOTE]  
>  Pour voir la syntaxe d’une applet de commande, utilisez l’applet de commande **Get-Help** dans l’environnement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell. Pour en savoir plus, voir [Get Help SQL Server PowerShell](../../../relational-databases/scripting/get-help-sql-server-powershell.md).  
  
 **Pour configurer et utiliser le fournisseur SQL Server PowerShell**  
  
-   [Fournisseur SQL Server PowerShell](../../../relational-databases/scripting/sql-server-powershell-provider.md)  
  
-   [Get Help SQL Server PowerShell](../../../relational-databases/scripting/get-help-sql-server-powershell.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Modes de disponibilité &#40;groupes de disponibilité Always On&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md)   
 [Basculement et modes de basculement &#40;groupes de disponibilité AlwaysOn&#41;](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md)   
 [Clustering de basculement Windows Server &#40;WSFC&#41; avec SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md)   
 [Stratégie de basculement pour les instances de cluster de basculement](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md)   
 [sp_server_diagnostics &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md)  
  
  
