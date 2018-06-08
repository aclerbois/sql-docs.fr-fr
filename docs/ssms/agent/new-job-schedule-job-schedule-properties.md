---
title: Nouvelle planification du travail - Propriétés de la planification du travail | Microsoft Docs
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
- sql13.ag.job.scheduleproperties.f1
- sql13.swb.maint.editrecurringjobsched.f1
ms.assetid: 5c0b1bc9-dd87-49cc-b0dd-75d0d922b177
caps.latest.revision: 3
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 8ece6ac37eabdaa3a0c92035eda3ac880480fa6a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="new-job-schedule---job-schedule-properties"></a>Nouvelle planification du travail - Propriétés de la planification du travail
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance), la plupart des fonctionnalités SQL Server Agent sont prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Database Managed Instance et SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Cette page vous permet d'afficher et de modifier les propriétés de la planification.  
  
## <a name="options"></a>Options  
**Nom**  
Tapez un nouveau nom pour la planification.  
  
**Travaux planifiés**  
Indique les travaux qui utilisent la planification.  
  
**Type de planification**  
Permet de sélectionner le type de planification.  
  
**Activé**  
Cliquez pour activer ou désactiver la planification.  
  
## <a name="recurring-schedule-types-options"></a>Options des types de planification périodique  
**Périodicité**  
Permet de sélectionner l'intervalle après lequel la planification se répète.  
  
**Répéter à chaque**  
Permet de sélectionner le nombre de jours ou de semaines entre deux occurrences de la planification. Cette option n'est pas disponible pour les planifications périodiques mensuelles.  
  
**Lundi**  
Exécute le travail le lundi. Disponible uniquement pour les planifications périodiques hebdomadaires.  
  
**Mardi**  
Exécute le travail le mardi. Disponible uniquement pour les planifications périodiques hebdomadaires.  
  
**Mercredi**  
Exécute le travail le mercredi. Disponible uniquement pour les planifications périodiques hebdomadaires.  
  
**Jeudi**  
Exécute le travail le jeudi. Disponible uniquement pour les planifications périodiques hebdomadaires.  
  
**Vendredi**  
Exécute le travail le vendredi. Disponible uniquement pour les planifications périodiques hebdomadaires.  
  
**Samedi**  
Exécute le travail le samedi. Disponible uniquement pour les planifications périodiques hebdomadaires.  
  
**Dimanche**  
Exécute le travail le dimanche. Disponible uniquement pour les planifications périodiques hebdomadaires.  
  
**Jour**  
Sélectionnez le jour du mois pour la planification. Disponible uniquement pour les planifications périodiques mensuelles.  
  
**de chaque**  
Sélectionnez le nombre de mois entre deux occurrences de la planification. Disponible uniquement pour les planifications périodiques mensuelles.  
  
**Le**  
Spécifiez une planification pour un jour de la semaine spécifique, dans une semaine spécifique dans le mois. Disponible uniquement pour les planifications périodiques mensuelles.  
  
**Une fois à**  
Définit l'heure à laquelle le travail est exécuté chaque jour.  
  
**Toutes les**  
Définit le nombre d'heures, de minutes ou de secondes entre deux occurrences.  
  
**Date de début**  
Définit la date à laquelle la planification est effective.  
  
**Date de fin**  
Définit la date à laquelle la planification cessera d'être appliquée.  
  
**Aucune date de fin**  
Spécifie que la planification doit être appliquée indéfiniment.  
  
## <a name="one-time-schedule-types-options"></a>Options des types de planification unique  
**Date**  
Sélectionnez la date d'exécution du travail.  
  
**Time**  
Sélectionnez l'heure d'exécution du travail.  
  
## <a name="see-also"></a> Voir aussi  
[Créer des planifications et les attacher à des travaux](../../ssms/agent/create-and-attach-schedules-to-jobs.md)  
[Planifier un travail](../../ssms/agent/schedule-a-job.md)  
  
