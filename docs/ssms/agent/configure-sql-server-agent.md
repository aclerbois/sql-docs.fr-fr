---
title: Configurer SQL Server Agent | Microsoft Docs
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
- SQL Server Agent, configuring
- accounts [SQL Server], SQL Server Agent
- SQL Server Agent, permissions
- security [SQL Server], SQL Server Agent
ms.assetid: 2e361a62-9e92-4fcd-80d7-d6960f127900
caps.latest.revision: 5
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 6b3e0a5c3738c703e28ab9ad3f6888ae330a189f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configure-sql-server-agent"></a>Configurer SQL Server Agent
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance), la plupart des fonctionnalités SQL Server Agent sont prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Database Managed Instance et SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Cette rubrique explique comment spécifier certaines options de configuration de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent lors de l'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. L’ensemble complet des options de configuration de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent n’est disponible que dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull_md.md)], les objets SMO ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Management Objects) ou dans les procédures stockées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent.  
  
**Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
    [Limitations et restrictions](#Restrictions)  
  
    [Sécurité](#Security)  
  
-   [Pour configurer SQL Server Agent](#SSMSProcedure)  
  
## <a name="BeforeYouBegin"></a>Avant de commencer  
  
### <a name="Restrictions"></a>Limitations et restrictions  
  
-   Cliquez sur **SQL Server Agent** dans l'Explorateur d'objets de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull_md.md)] pour administrer des travaux, des opérateurs, des alertes et le service de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent. Cependant, l'Explorateur d'objets n'affiche le nœud [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent que si vous avez l'autorisation de l'utiliser.  
  
-   Le redémarrage automatique ne doit pas être activé pour le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou le service de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent sur les instances de cluster de basculement.  
  
### <a name="Security"></a>Sécurité  
  
#### <a name="Permissions"></a>Permissions  
Pour exécuter ses fonctions, [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent doit être configuré pour utiliser les informations d'identification d'un compte qui est membre du rôle serveur fixe **sysadmin** dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Le compte doit avoir les autorisations Windows suivantes :  
  
-   Ouvrir une session en tant que service (SeServiceLogonRight)  
  
-   Remplacer un jeton de niveau processus (SeAssignPrimaryTokenPrivilege)  
  
-   Outrepasser le contrôle de parcours (SeChangeNotifyPrivilege)  
  
-   Changer les quotas de mémoire d'un processus (SeIncreaseQuotaPrivilege)  
  
Pour plus d’informations sur les autorisations Windows nécessaires pour le compte de service de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent, consultez [Sélectionner un compte pour le service SQL Server Agent](../../ssms/agent/select-an-account-for-the-sql-server-agent-service.md) et [Setting Up Windows Service Accounts](http://msdn.microsoft.com/en-us/309b9dac-0b3a-4617-85ef-c4519ce9d014)(Configuration des comptes de service Windows).  
  
## <a name="SSMSProcedure"></a>Utilisation de SQL Server Management Studio  
  
#### <a name="to-configure-sql-server-agent"></a>Pour configurer SQL Server Agent  
  
1.  Cliquez sur le bouton **Démarrer** , puis, dans le menu **Démarrer**  , cliquez sur **Panneau de configuration**.  
  
2.  Dans le Panneau de configuration, cliquez sur **Système et sécurité**, puis sur **Outils d'administration**et sélectionnez **Stratégie de sécurité locale**.  
  
3.  Dans Stratégie de sécurité locale, cliquez sur le chevron pour développer le dossier **Stratégies locales** , puis cliquez sur le dossier **Attribution des droits utilisateur** .  
  
4.  Cliquez avec le bouton droit sur l’autorisation à configurer pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] et sélectionnez **Propriétés**.  
  
5.  Dans la boîte de dialogue des propriétés de l'autorisation, vérifiez que le compte sous lequel [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent s'exécute est répertorié. Sinon, cliquez sur **Ajouter un utilisateur ou un groupe**, entrez le compte sous lequel [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent s'exécute dans la boîte de dialogue **Choisir des utilisateurs, des ordinateurs ou des groupes** , puis cliquez sur **OK**.  
  
6.  Répétez ces étapes pour chaque autorisation à ajouter pour l'exécution de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent. Lorsque vous avez terminé, cliquez sur **OK**.  
  
