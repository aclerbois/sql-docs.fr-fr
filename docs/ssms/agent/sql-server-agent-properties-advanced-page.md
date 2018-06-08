---
title: Propriétés de SQL Server Agent (page Avancé) | Microsoft Docs
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
- sql13.ag.agent.advanced.f1
ms.assetid: a4d798ee-4c18-40d4-b6af-63d17503738c
caps.latest.revision: 3
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: c0c79369a1486f175fb23321e706a799059c92c9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-agent-properties-advanced-page"></a>Propriétés de l'Agent SQL Server (page Avancé)
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance), la plupart des fonctionnalités SQL Server Agent sont prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Database Managed Instance et SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Utilisez cette page pour consulter et modifier les propriétés avancées du service [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent.  
  
## <a name="options"></a>Options  
**Transfert d'événements SQL Server**  
Les options de cette catégorie permettent d'activer et de configurer le transfert d'événements.  
  
**Transférer les événements sur un autre serveur**  
Transfère les événements [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent sur un autre serveur.  
  
**Server**  
Sélectionnez le nom du serveur sur lequel vous voulez transférer les événements.  
  
**Événements non gérés**  
Transfère uniquement les événements non gérés sur le serveur spécifié. [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent transfère uniquement les événements auxquels aucune alerte ne répond.  
  
**Tous les événements**  
Transfère tous les événements. Si une alerte répond à l'événement dans l'instance locale, [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent transfère l'événement et traite l'alerte.  
  
**Si l'événement a une gravité au moins égale à**  
Transfère uniquement les événements dont le niveau de gravité est supérieur ou égal au niveau spécifié.  
  
**Condition d'inactivité de l'UC**  
Les options de cette catégorie définissent les conditions dans lesquelles [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent exécute les travaux dont l'exécution est planifiée pendant l'inactivité de l'UC.  
  
**Définir la condition d'inactivité de l'UC**  
Définit les conditions dans lesquelles [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent considère que l'UC est inactive.  
  
**La moyenne d'utilisation de l'UC tombe en dessous de**  
Pourcentage d'utilisation de l'UC au-dessous duquel l'UC est considérée comme inactive.  
  
**Et reste sous ce niveau pendant**  
Durée pendant laquelle la moyenne d'utilisation de l'UC doit rester au-dessous du niveau spécifié avant que [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent démarre l'exécution des travaux planifiés pendant l'inactivité de l'UC.  
  
## <a name="see-also"></a> Voir aussi  
[Créer des planifications et les attacher à des travaux](../../ssms/agent/create-and-attach-schedules-to-jobs.md)  
[Gérer les événements](../../ssms/agent/manage-events.md)  
  
