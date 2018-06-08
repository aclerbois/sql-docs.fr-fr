---
title: Propriétés de l’opérateur - Nouvel opérateur (page Général) | Microsoft Docs
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
- sql13.ag.operator.general.f1
ms.assetid: c036d1c9-83d1-4a95-b67e-29d283b1a046
caps.latest.revision: 4
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 5c85048f2e5fb02fd060d3c813b1b4cdbc23d637
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="operator-properties---new-operator-general-page"></a>Propriétés de l’opérateur - Nouvel opérateur (page Général)
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance), la plupart des fonctionnalités SQL Server Agent sont prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Database Managed Instance et SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Utilisez cette page pour afficher et modifier les propriétés générales des opérateurs de l'Agent [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] .  
  
## <a name="options"></a>Options  
**Nom**  
Permet de modifier le nom de l'opérateur.  
  
**Activé**  
Permet d'activer l'opérateur. Aucune notification n'est envoyée à l'opérateur lorsque cette option est désactivée.  
  
**Nom de messagerie électronique**  
Spécifie l'adresse de messagerie de l'opérateur.  
  
**Adresse d'envoi réseau**  
Spécifie l’adresse à utiliser pour **net send**.  
  
**Nom de l'adresse de radiomessagerie**  
Spécifie l'adresse de messagerie à utiliser pour la radiomessagerie de l'opérateur.  
  
**Planification de la radiomessagerie active**  
Définit les périodes d'activité de la radiomessagerie.  
  
**Lundi - Dimanche**  
Permet de sélectionner les jours d'activité de la radiomessagerie.  
  
**Début de journée**  
Sélectionne l’heure après laquelle [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent envoie des messages à la radiomessagerie.  
  
**Fin de journée**  
Sélectionne l’heure après laquelle l’Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] n’envoie plus de messages à la radiomessagerie.  
  
## <a name="see-also"></a> Voir aussi  
[Opérateurs](../../ssms/agent/operators.md)  
  
