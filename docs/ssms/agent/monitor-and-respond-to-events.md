---
title: Surveiller et répondre aux événements | Microsoft Docs
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
- notifications [SQL Server], alert
- events [SQL Server], alerts
- alerts [SQL Server]
- notifications [SQL Server]
- events [SQL Server], automatically responding to
- administrator notifications [SQL Server Agent]
- automatic event responses
- SQL Server Agent alerts
- monitoring [SQL Server], events
- responding to events automatically
ms.assetid: f7fbe155-5b68-4777-bc71-a47637471f32
caps.latest.revision: 5
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 21c40862e43cd7f5c4e883c6482d83faa4175ede
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="monitor-and-respond-to-events"></a>Surveiller et répondre aux événements
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance), la plupart des fonctionnalités SQL Server Agent sont prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Database Managed Instance et SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent peut surveiller les *événements*et répondre automatiquement à ces derniers, par exemple des messages de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], des conditions de performances particulières et des événements WMI (Windows Management Instrumentation).  
  
## <a name="in-this-section"></a>Dans cette section  
[Alertes](../../ssms/agent/alerts.md)  
Contient des informations sur la façon de nommer une alerte et de sélectionner les événements ou conditions de performances auxquels les alertes répondent.  
  
[Créer un événement défini par l'utilisateur](../../ssms/agent/create-a-user-defined-event.md)  
Contient des informations sur la façon de créer des événements autres que ceux qui sont prédéfinis par [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
[Opérateurs](../../ssms/agent/operators.md)  
Contient des informations sur la création d'alias pour les administrateurs, alias que l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] peut utiliser pour envoyer des notifications lorsque les travaux échouent ou réussissent.  
  
## <a name="about-monitoring-and-responding-to-events"></a>À propos de la surveillance et de la réponse aux événements  
Les réponses automatiques aux événements sont appelées *alertes*. Vous pouvez définir une alerte sur un ou plusieurs événements pour spécifier la façon dont vous voulez que l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] y réponde lorsqu'ils se produisent. Une alerte peut répondre à un événement en notifiant un administrateur ou en exécutant un travail, ou les deux. Une alerte peut également transmettre un événement au journal des applications Microsoft Windows d'un autre ordinateur. Par exemple, vous pouvez spécifier qu'un opérateur soit notifié immédiatement si un événement de gravité 19 se produit. En définissant des alertes, les administrateurs de bases de données peuvent surveiller et gérer plus efficacement [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent ne répond qu’aux événements pour lesquels une alerte a été définie. La méthode qu'utilise l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] pour surveiller les événements dépend du type d'événement.  
  
Lorsqu'une alerte de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] est définie sur un compteur de performance, l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] le surveille directement. Pour un événement WMI, l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] inscrit une requête d'événement pour cet événement.  
  
Pour répondre aux messages de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] surveille le journal des applications Windows. [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent ne peut répondre qu’aux messages qui apparaissent dans ce journal. Par défaut, SQL Server enregistre les événements suivants dans le journal des applications Windows :  
  
-   Erreurs sysmessages dont le niveau de gravité est supérieur ou égal à 19.  
  
    Si vous souhaitez également journaliser des erreurs sysmessages particulières dont le niveau de gravité est inférieur à 19, utilisez la procédure stockée sp_altermessage pour désigner ces erreurs comme étant « toujours journalisées ».  
  
-   Toute instruction RAISERROR appelée à l'aide de la syntaxe WITH LOG.  
  
    RAISERROR WITH LOG est la méthode conseillée pour écrire dans le journal des applications Windows à partir d'une instance de SQL Server.  
  
-   Tout événement d'application consigné dans le journal à l'aide de xp_logevent.  
  
    > [!NOTE]  
    > La journalisation des événements d'applications consomme de l'espace et peut conduire le journal des applications Windows à dépasser sa taille maximale. Assurez-vous que la taille du journal des applications Windows est suffisante pour éviter la perte d'informations liées aux événements SQL Server.  
  
Lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] journalise un message, le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent compare le message aux alertes définies par l'administrateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] .  
  
Quelle que soit la source de l'événement, le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent répond à l'événement en exécutant les tâches spécifiées dans l'alerte de cet événement.  
  
## <a name="see-also"></a> Voir aussi  
[sp_altermessage](http://msdn.microsoft.com/en-us/1b28f280-8ef9-48e9-bd99-ec14d79abaca)  
  
