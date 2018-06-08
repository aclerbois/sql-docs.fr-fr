---
title: Créer des planifications et les attacher à des travaux | Microsoft Docs
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
- jobs [SQL Server]
- scheduling jobs [SQL Server]
- jobs [SQL Server], scheduling
- CPU [SQL Server], idle conditions
- automatic job processing
- SQL Server Agent jobs, scheduling
- idle time [SQL Server]
ms.assetid: 079c2984-0052-4a37-a2b8-4ece56e6b6b5
caps.latest.revision: 5
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 9aa5c92c0579fdcda7ca336e4893b1a5417dfaa0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-and-attach-schedules-to-jobs"></a>Créer des planifications et les attacher à des travaux
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance), la plupart des fonctionnalités SQL Server Agent sont prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Database Managed Instance et SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

La planification des travaux [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent consiste à définir la ou les conditions qui déclenchent leur exécution sans intervention de l'utilisateur. Vous pouvez planifier l'exécution automatique d'un travail en lui créant une planification ou en lui attachant une planification existante.  
  
Il existe deux méthodes pour créer une planification :  
  
-   Créez la planification en même temps que vous créez un travail.  
  
-   Créez la planification dans l'Explorateur d'objets.  
  
Une fois la planification créée, vous pouvez l'attacher à plusieurs travaux, même si elle a été conçue pour un travail spécifique. Vous pouvez également détacher une planification d'un travail.  
  
Une planification peut être définie par rapport à une heure ou un événement. Par exemple, vous pouvez planifier l'exécution d'un travail aux moments suivants :  
  
-   au moment où l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] démarre ;  
  
-   au moment où l'utilisation de l'UC atteint le niveau d'inactivité que vous avez défini ;  
  
-   ponctuellement, à une date et une heure spécifiques ;  
  
-   Selon une planification récurrente.  
  
Vous pouvez remplacer vos planifications du travail par des alertes qui répondent à un événement par l'exécution d'un travail.  
  
> [!NOTE]  
> Une seule instance du travail peut être exécutée à la fois. Si vous exécutez un travail manuellement alors qu'il est en train de s'exécuter d'après une planification, l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] refuse la demande.  
  
Pour empêcher l'exécution d'un travail planifié, vous devez effectuer l'une des opérations suivantes :  
  
-   désactiver la planification ;  
  
-   désactiver le travail ;  
  
-   détacher la planification du travail ;  
  
-   arrêter le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent ;  
  
-   supprimer la planification.  
  
Si la planification n'est pas activée, le travail peut toujours s'exécuter en réponse à une alerte ou lorsqu'un utilisateur l'exécute manuellement. Lorsqu'une planification du travail n'est pas activée, elle ne l'est pas pour aucun travail qui l'utilise normalement.  
  
Une planification désactivée doit être réactivée de manière explicite. Le simple fait de modifier une planification ne la réactive pas automatiquement.  
  
## <a name="scheduling-start-dates"></a>Planification de dates de début  
La date de début d'une planification doit être supérieure ou égale à 19900101.  
  
Lorsque vous attachez une planification à un travail, vous devez examiner la date de début utilisée par la planification la première fois qu'elle exécute le travail. La date de début dépend du jour et de l'heure auxquels vous attachez la planification au travail. Par exemple, supposons que vous créez une planification qui s'exécute un lundi sur deux à 8h00. Si vous créez un travail à 10h00 le lundi 3 mars 2008, la date de début de la planification est le lundi 17 mars 2008. Si vous créez un autre travail le mardi 4 mars 2008, la date de début de la planification est le lundi 10 mars 2008.  
  
Une fois la planification attachée à un travail, vous pouvez modifier sa date de début.  
  
## <a name="cpu-idle-schedules"></a>Planifications pendant l'inactivité de l'UC  
Pour augmenter les ressources de l'UC, vous pouvez définir une condition d'inactivité de l'UC pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent. [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent utilise ce paramètre pour déterminer le meilleur moment pour l’exécution des travaux. Par exemple, vous pouvez planifier un travail de reconstruction des index lorsque l'UC est inactive et pendant des périodes de production moins chargées.  
  
Avant de définir des travaux à exécuter pendant l'inactivité de l'UC, déterminez la charge de l'UC en situation d'utilisation normale. Pour ce faire, utilisez le [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler_md.md)] ou l'Analyseur de performances pour surveiller le trafic serveur et rassembler les statistiques. Ensuite, à l'aide des informations rassemblées, déterminez le pourcentage et la durée d'inactivité de l'UC.  
  
Définissez l'inactivité de l'UC comme un pourcentage en dessous duquel l'utilisation de l'UC doit se maintenir pendant une période donnée. Définissez ensuite la durée. Lorsque le taux d'utilisation de l'UC reste inférieur au pourcentage spécifié pendant un certain temps (défini par l'utilisateur), l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] démarre tous les travaux dont l'exécution est planifiée aux moments d'inactivité de l'UC. Pour plus d’informations sur l’utilisation de [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler_md.md)] ou de l’Analyseur de performances visant à surveiller le taux d’utilisation de l’UC, consultez [Surveillance de l’utilisation du processeur](http://msdn.microsoft.com/en-us/2a02a3b6-07b2-4ad0-8a24-670414d19812).  
  
## <a name="related-tasks"></a>Related Tasks  
  
|||  
|-|-|  
|**Description**|**Rubrique**|  
|Décrit la méthode à suivre pour créer la planification d'un travail [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent.|[Créer une planification](../../ssms/agent/create-a-schedule.md)|  
|Décrit la méthode à suivre pour planifier un travail [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent.|[Planifier un travail](../../ssms/agent/schedule-a-job.md)|  
|Indique comment définir la condition d'inactivité de l'UC pour votre serveur.|[Définir le seuil et la durée d’inactivité de l’UC &#40;SQL Server Management Studio&#41;](../../ssms/agent/set-cpu-idle-time-and-duration-sql-server-management-studio.md)|  
  
## <a name="see-also"></a> Voir aussi  
[sp_help_jobschedule](http://msdn.microsoft.com/en-us/2cded902-9272-4667-ac4b-a4f95a9f008e)  
[sysjobschedules](http://msdn.microsoft.com/en-us/ccdafec7-2a9b-4356-bffb-1caa3a12db59)  
  
