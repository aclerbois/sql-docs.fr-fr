---
title: Choisir une planification pour le travail | Microsoft Docs
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
f1_keywords:
- sql13.ag.job.pickscheduleforjob.f1
helpviewer_keywords:
- Pick Schedule for Job dialog box
ms.assetid: 6de2025d-c25c-47b9-9a25-18c294935c15
caps.latest.revision: 3
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: c561bb6100bb06318195f4c7a461056f35c65647
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="pick-schedule-for-job"></a>Choisir une planification pour le travail
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance), la plupart des fonctionnalités SQL Server Agent sont prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Database Managed Instance et SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Cette boîte de dialogue vous permet de choisir une planification existante pour le travail de l'Agent [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] .  
  
## <a name="options"></a>Options  
**Planifications disponibles**  
Répertorie les planifications disponibles pour ce travail. Comme un travail et une planification doivent avoir le même propriétaire, cette liste inclut uniquement les planifications possédées par le propriétaire du travail.  
  
**Nom**  
Affiche le nom de la planification.  
  
**Activé**  
Option sélectionnée si la planification est activée.  
  
**Description**  
Décrit les conditions dans lesquelles la planification déclenche l'exécution du travail.  
  
**Travaux planifiés**  
Répertorie les numéros des travaux associés à la planification. Cliquez sur un numéro pour afficher les propriétés du travail correspondant.  
  
**Propriétés**  
Affiche la boîte de dialogue **Propriétés de la planification du travail** qui inclut des informations sur la planification.  
  
## <a name="see-also"></a> Voir aussi  
[Créer des planifications et les attacher à des travaux](../../ssms/agent/create-and-attach-schedules-to-jobs.md)  
  
