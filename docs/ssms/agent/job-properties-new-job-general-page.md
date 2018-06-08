---
title: Propriétés du travail - Nouveau travail (page Général) | Microsoft Docs
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
- sql13.ag.job.general.f1
ms.assetid: b6832840-1c18-4db8-94fc-080db880ae9f
caps.latest.revision: 4
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: ab0fe35fcaec40b96132c2e51ef07b17438f137d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="job-properties---new-job-general-page"></a>Propriétés du travail - Nouveau travail (page Général)
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance), la plupart des fonctionnalités SQL Server Agent sont prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Database Managed Instance et SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Utilisez cette page pour afficher et modifier les propriétés générales d’un travail [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent.  
  
## <a name="options"></a>Options  
**Nom**  
Permet de modifier le nom du travail.  
  
**Propriétaire**  
Permet de sélectionner le propriétaire du travail.  
  
**Catégorie**  
Permet de sélectionner la catégorie du travail.  
  
**...**  
Affiche les travaux de la catégorie sélectionnée.  
  
**Description**  
Permet de modifier la description du travail.  
  
**Activé**  
Permet d'activer le travail. Quand le travail n’est pas activé, il ne s’exécute pas en réponse à une planification ou une alerte, mais vous pouvez toujours le démarrer à l’aide de la procédure stockée **sp_start_job** .  
  
**Source**  
Affiche le serveur principal du travail. Cette option est disponible uniquement dans la page **Général** des Propriétés du travail.  
  
**Créé le**  
Affiche la date et l'heure de création du travail. Cette option est disponible uniquement dans la page **Général** des Propriétés du travail.  
  
**Date de la dernière modification**  
Affiche la date et l'heure de la dernière modification du travail. Cette option est disponible uniquement dans la page **Général** des Propriétés du travail.  
  
**Dernière exécution**  
Affiche la date et l'heure de la dernière exécution du travail. Cette option est disponible uniquement dans la page **Général** des Propriétés du travail.  
  
**Afficher l'historique des travaux**  
Affiche l'historique des travaux. Cette option est disponible uniquement dans la page **Général** des Propriétés du travail.  
  
## <a name="see-also"></a> Voir aussi  
[Implémenter des travaux](../../ssms/agent/implement-jobs.md)  
[Catégories de travaux - Gérer les catégories de travaux](../../ssms/agent/job-categories-manage-job-categories.md)  
  
