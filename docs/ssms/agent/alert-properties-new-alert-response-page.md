---
title: Propriétés de l’alerte - Nouvelle alerte (page Réponse) | Microsoft Docs
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
- sql13.ag.alert.response.f1
ms.assetid: 72daf008-f9ea-4077-b217-5048e7759d3e
caps.latest.revision: 4
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 3b6fc99f0eb6266979e122d82cbc2d2ce2578498
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="alert-properties---new-alert-response-page"></a>Propriétés de l’alerte - Nouvelle alerte (page Réponse)
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance), la plupart des fonctionnalités SQL Server Agent sont prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Database Managed Instance et SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Utilisez cette page pour spécifier un travail que vous souhaitez exécuter et pour obtenir une liste d'opérateurs à notifier en réponse à une alerte de l'Agent [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] .  

## <a name="options"></a>Options  
**Exécuter le travail**  
Active les options **Liste des travaux**, **Nouveau travail** et **Afficher le travail** .  
  
**Nouveau travail**  
Ouvre la boîte de dialogue **Nouveau travail** . Ce bouton n'est pas disponible lorsque **Exécuter le travail** n'est pas sélectionné.  
  
**Afficher le travail**  
Permet d'afficher et de modifier le travail sélectionné. Cette option n'est pas disponible lorsque **Exécuter le travail** n'est pas sélectionné.  
  
**Notifier les opérateurs**  
Active les contrôles qui vous permettent d'ajouter, de supprimer ou de modifier des opérateurs.  
  
**Liste d'opérateurs**  
Répertorie les opérateurs à notifier lorsqu'une alerte se produit. Pour spécifier une méthode de notification, cochez la case **Messagerie électronique**, **Radiomessagerie**ou **Envoi réseau** affichée après le nom de l’opérateur. Cette option n’est pas disponible quand **Notifier les opérateurs** n’est pas sélectionné.  
  
**Messagerie électronique**  
Utilisez un courrier électronique pour notifier l'opérateur.  
  
**Radiomessagerie**  
Utilisez une adresse de radiomessagerie pour notifier l'opérateur.  
  
**Envoi réseau**  
Utilisez **net send** pour notifier l’opérateur.  
  
**Nouvel opérateur**  
Affiche la boîte de dialogue **Nouvel opérateur** , que vous pouvez utiliser pour créer un opérateur.  
  
**Afficher l'opérateur**  
Affiche la boîte de dialogue **Propriétés** pour l’opérateur sélectionné. Vous pouvez afficher et modifier les propriétés d'opérateur dans la boîte de dialogue **Propriétés Opérateur** .  
  
## <a name="see-also"></a> Voir aussi  
[Alertes](../../ssms/agent/alerts.md)  
[Create an Alert Using Severity Level](../../ssms/agent/create-an-alert-using-severity-level.md)  
[Alertes](../../ssms/agent/alerts.md)  
[Edit an Alert](../../ssms/agent/edit-an-alert.md)  
[Delete an Alert](../../ssms/agent/delete-an-alert.md)  
  
