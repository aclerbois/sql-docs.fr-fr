---
title: Créer un événement défini par l’utilisateur | Microsoft Docs
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
- SQL Server Agent alerts, user-defined events
- user-defined events [SQL Server]
- multiple language support [SQL Server], alerts
- languages [SQL Server], alerts
- severity levels [SQL Server]
- global considerations [SQL Server], alerts
- events [SQL Server], user-defined
- SQL Server Agent alerts, multiple-language environments
- alerts [SQL Server], user-defined events
- alerts [SQL Server], multiple-language environments
- custom events [SQL Server Agent]
- international considerations [SQL Server], alerts
ms.assetid: 03d71a35-97fa-4bba-aa9a-23ac9c9cf879
caps.latest.revision: 5
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 5f1783fd0a0326ca2b74b3199033545e9b5d96d8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-a-user-defined-event"></a>Créer un événement défini par l'utilisateur
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance), la plupart des fonctionnalités SQL Server Agent sont prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Database Managed Instance et SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Vous pouvez créer des événements définis par l'utilisateur si vous voulez surveiller d'autres événements que ceux qui sont prédéfinis par [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Vous pouvez également attribuer un niveau de sévérité à chaque événement défini par l'utilisateur.  
  
> [!NOTE]  
> Quand vous utilisez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull_md.md)], sélectionnez l’option **Enregistrer dans le journal des événements** pour chaque message d’événement défini par l’utilisateur, pour vous assurer que les messages seront journalisés. Par défaut, les messages définis par l'utilisateur de sévérité inférieure à 19 ne sont pas envoyés au journal des applications [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows lorsqu'ils se produisent. Par conséquent, les messages définis par l'utilisateur de sévérité inférieure à 19 ne déclenchent pas d'alertes au niveau de l'Agent SQL Server.  
  
Les événements définis par l'utilisateur doivent posséder un numéro de message unique. Les numéros de message liés à un événement défini par l'utilisateur doivent être supérieurs à 50 000. Vous pouvez définir des messages pour l'événement dans plusieurs langues. Cependant, un message **En-US** doit exister avant que des messages correspondant à d’autres langues puissent être ajoutés.  
  
Si vous administrez un environnement [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] multilingue, créez des messages définis par l'utilisateur dans chacune des langues prises en charge par le système. Par exemple, si vous créez un nouveau message d'événement qui sera utilisé aussi bien sur un serveur anglais que sur un serveur allemand, vous devez utiliser le même numéro d'événement et le même niveau de sévérité pour les deux, mais leur affecter un message dans une langue différente.  
  
Les tâches suivantes apportent des informations sur la façon de créer des événements définis par l'utilisateur, ainsi que des alertes qui leur répondent :  
  
**Pour créer une alerte en fonction d'un numéro de message**  
  
-   [SQL Server Management Studio](../../ssms/agent/create-an-alert-using-an-error-number.md)  
  
-   [Transact-SQL](http://msdn.microsoft.com/en-us/d9b41853-e22d-4813-a79f-57efb4511f09)  
  
**Pour créer une alerte en fonction de niveaux de gravité**  
  
-   [SQL Server Management Studio](../../ssms/agent/create-an-alert-using-severity-level.md)  
  
-   [Transact-SQL](http://msdn.microsoft.com/en-us/d9b41853-e22d-4813-a79f-57efb4511f09)  
  
**Pour définir la réponse à une alerte**  
  
-   [SQL Server Management Studio](../../ssms/agent/define-the-response-to-an-alert-sql-server-management-studio.md)  
  
-   [Transact-SQL](http://msdn.microsoft.com/en-us/0525e0a2-ed0b-4e69-8a4c-a9e3e3622fbd)  
  
**Pour créer un message d'erreur d'événement défini par l'utilisateur**  
  
-   [Transact-SQL](http://msdn.microsoft.com/en-us/54746d30-f944-40e5-a707-f2d9be0fb9eb)  
  
**Pour modifier un message d'erreur d'événement défini par l'utilisateur**  
  
-   [Transact-SQL](http://msdn.microsoft.com/en-us/1b28f280-8ef9-48e9-bd99-ec14d79abaca)  
  
**Pour supprimer un message d'erreur d'événement défini par l'utilisateur**  
  
-   [Transact-SQL](http://msdn.microsoft.com/en-us/17287a15-cdde-43d1-bb18-9f920bc15db8)  
  
**Pour désactiver ou réactiver une alerte**  
  
-   [SQL Server Management Studio](../../ssms/agent/disable-or-reactivate-an-alert.md)  
  
-   [Transact-SQL](http://msdn.microsoft.com/en-us/4bbaeaab-8aca-4c9e-abc1-82ce73090bd3)  
  
## <a name="see-also"></a> Voir aussi  
[sp_update_alert (Transact-SQL)](http://msdn.microsoft.com/en-us/4bbaeaab-8aca-4c9e-abc1-82ce73090bd3)  
  
