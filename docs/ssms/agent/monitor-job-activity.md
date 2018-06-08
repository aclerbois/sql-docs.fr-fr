---
title: Surveiller l’activité des travaux | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssms-agent
ms.reviewer: ''
ms.suite: sql
ms.technology: ssms
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent, monitoring
- jobs [SQL Server Agent], monitoring
- monitoring [SQL Server], jobs
- activity monitoring [SQL Server Agent]
- monitoring [SQL Server], SQL Server Agent
- monitoring [SQL Server Agent]
- SQL Server Agent Job Activity Monitor
- SQL Server Agent jobs, monitoring
- performance [SQL Server], jobs
- current activity
ms.assetid: 71cb432b-631d-4b8b-9965-e731b3d8266d
caps.latest.revision: 6
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 37fb0194a7971809acc8227d8ac7a9cda95d73f2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="monitor-job-activity"></a>Surveiller l'activité des travaux
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance), la plupart des fonctionnalités SQL Server Agent sont prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Database Managed Instance et SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Vous pouvez surveiller l'activité en cours de tous les travaux définis sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] à l'aide du moniteur d'activité des travaux de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent.  
  
## <a name="sql-server-agent-sessions"></a>Sessions de SQL Server Agent  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent crée une session à chaque démarrage du service. Quand une session est créée, la table **sysjobactivity** de la base de données **msdb** est remplie avec tous les travaux définis existants. Cette table conserve la dernière activité des travaux lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent est redémarré. Chaque session enregistre l'activité normale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent du début à la fin du travail. Les informations relatives à ces sessions sont stockées dans la table **syssessions** de la base de données **msdb** .  
  
## <a name="job-activity-monitor"></a>Moniteur d'activité des travaux  
Le moniteur d’activité des travaux vous permet de visualiser la table **sysjobactivity** à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull_md.md)]. Vous pouvez visualiser tous les travaux sur le serveur ou définir des filtres afin de limiter le nombre de travaux affichés. Vous pouvez également trier les informations sur les travaux en cliquant sur un en-tête de colonne dans la grille **Activité du travail de l’Agent** . Par exemple, quand vous sélectionnez l’en-tête de colonne **Dernière exécution** , vous pouvez visualiser les travaux dans l’ordre de leur dernière exécution. Vous pouvez cliquer sur l'en-tête de la colonne pour classer les travaux en fonction de la date de leur dernière exécution, dans l'ordre croissant ou décroissant.  
  
Le moniteur d'activité des travaux vous permet d'effectuer les tâches suivantes :  
  
-   Démarrer et arrêter des travaux.  
  
-   Visualiser les propriétés des travaux.  
  
-   Visualiser l'historique d'un travail spécifique.  
  
-   Actualiser les informations de la grille **Activité du travail de l’Agent** manuellement ou définir un intervalle d’actualisation automatique en cliquant sur **Afficher les paramètres d’actualisation**.  
  
Utilisez le moniteur d'activité des travaux pour connaître les travaux dont l'exécution a été planifiée, le dernier résultat des travaux exécutés pendant la session actuelle et les travaux en cours d'exécution ou inactifs. Si le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent échoue de manière inattendue, vous pouvez déterminer quels étaient les travaux en cours d'exécution en consultant la session précédente dans le moniteur d'activité des travaux.  
  
Pour ouvrir le moniteur d’activité des travaux, développez **SQL Server Agent** dans l’Explorateur d’objets de [!INCLUDE[ssManStudio](../../includes/ssmanstudio_md.md)] , cliquez avec le bouton droit sur **Moniteur d’activité des travaux**, puis cliquez sur **Afficher l’activité du travail**.  
  
Vous pouvez également visualiser l’activité des travaux de la session actuelle à l’aide de la procédure stockée **sp_help_jobactivity**.  
  
## <a name="related-tasks"></a>Related Tasks  
  
|||  
|-|-|  
|**Description**|**Rubrique**|  
|Explique comment afficher l'état d'exécution des travaux de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] .|[Afficher l’activité du travail](../../ssms/agent/view-job-activity.md)|  
  
## <a name="see-also"></a> Voir aussi  
[Afficher l’activité du travail](../../ssms/agent/view-job-activity.md)  
[sysjobactivity (Transact-SQL)](http://msdn.microsoft.com/en-us/fd17cac9-5d1f-4b44-b2dc-ee9346d8bf1e)  
[syssessions (Transact-SQL)](http://msdn.microsoft.com/en-us/187819b6-c7f4-4a26-b74c-0a89e96695cf)  
[sp_help_jobactivity (Transact-SQL)](http://msdn.microsoft.com/en-us/d344864f-b4d3-46b1-8933-b81dec71f511)  
  
